
# Update Rate Limits

**Summary:** Change how many submissions one IP address or email address may send, and where those counters are stored, in the shared PHP rate limiter and in `mpact_config.php` on the server.

**When to change:** When real visitors report "Too many submissions…", spam gets through, or the server's storage folder changes.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 15 minutes, plus time with the server administrator

---

## Visual Reference

**Real Code Snippet (`/includes/rate_limit_config.php` — defaults used only when the constant is still undefined, lines 23–40)**

```php
if (!defined('RATE_LIMIT_IP_MAX')) {
    define('RATE_LIMIT_IP_MAX', 5);
}
if (!defined('RATE_LIMIT_EMAIL_MAX')) {
    define('RATE_LIMIT_EMAIL_MAX', 2);
}
if (!defined('RATE_LIMIT_WINDOW_SEC')) {
    define('RATE_LIMIT_WINDOW_SEC', 300);
}
if (!defined('RATE_LIMIT_RETENTION_DAYS')) {
    define('RATE_LIMIT_RETENTION_DAYS', 7);
}
if (!defined('RATE_LIMIT_STORAGE')) {
    define('RATE_LIMIT_STORAGE', 'json');
}
if (!defined('RATE_LIMIT_DATA_DIR')) {
    define('RATE_LIMIT_DATA_DIR', dirname(__DIR__) . '/data/rate-limits');
}
```

*Each default applies only when `mpact_config.php` has not already defined the constant, so server overrides belong in `mpact_config.php`.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/includes/rate_limit_config.php` | Default `RATE_LIMIT_*` constants. Each one is set only when it is still undefined. |
| `/mpact_config.php` | Server-only overrides. Gitignored, and not in the repo. The nano.nau.edu server administrator edits this file. |
| `/includes/rate_limit.php` | `getClientIp()`, `normalizeEmail()`, `checkRateLimits()`, and the fail-open catch. |
| `/includes/rate_limit/RateLimitStoreInterface.php` | `RateLimitDecision::next()`, which decides a fixed window. |
| `/includes/rate_limit/RateLimitStoreFactory.php` | Picks the JSON directory or the temp fallback, and writes the store log lines. |
| `/includes/rate_limit/JsonFileRateLimitStore.php` | `flock`-locked JSON files named with `sha256` of the key, under a `json` subdirectory. |
| `/includes/rate_limit/SqliteRateLimitStore.php` | Alternate store. It is not opened when `RATE_LIMIT_STORAGE` is `json`. |
| `/includes/validation.php` | `respond()` (HTTP 200 JSON) and `mpactLogInternalError()`, which formats the skipped log line. |
| `/FormSubmission.php` | Contact Us handler. Rate-limit call at line 337, before field checks. |
| `/ServiceRequestSubmission.php` | Printing, laser, and scanning handler. Rate-limit call at line 511. |
| `/EquipmentReservation.php` | Reserve Equipment handler. Rate-limit call at line 188. |
| `/IntelScholarshipSubmission.php` | CHIPS Scholars handler. Rate-limit call at line 47. |
| `/data/.htaccess` | Denies web access to `/data/`, including the counter files. |
| `/data/rate-limits/.gitkeep` | Empty placeholder so git keeps the storage directory. |
| `/.gitignore` | Line 11 ignores `mpact_config.php`. Lines 13–15 ignore counter files and keep `.gitkeep`. |

> [!NOTE]
> **How counting works**
> Each submission that reaches `checkRateLimits()` can use two keys, `ip:` plus the address and `email:` plus the lowercased address, in one fixed window (`RATE_LIMIT_WINDOW_SEC`, 300 seconds by default). The window is not per form. `/FormSubmission.php` (line 337), `/ServiceRequestSubmission.php` (line 511), `/EquipmentReservation.php` (line 188), and `/IntelScholarshipSubmission.php` (line 47) call the same function, so Contact Us, the three Service Request forms, Reserve Equipment, and the CHIPS Scholars form share both counts. The call sits after the honeypot, the CSRF token check, and Cloudflare Turnstile, and before `requireFields()`, so a slot is used before field validation. Those earlier rejects do not use a slot. A storage error is caught and the submission is allowed through (fail open). About 1 request in 100 (`mt_rand(1, 100) === 1`) deletes records whose `last_seen` is older than `RATE_LIMIT_RETENTION_DAYS` (7 days unless you override it). That roll runs only after both keys allow the request. The visitor sees one of these exact sentences: `Too many submissions from your network. Please try again later.` or `Too many submissions for this email address. Please try again later.` The full guard order is in [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md).

> [!WARNING]
> **`getClientIp()` reads only `REMOTE_ADDR`. Behind an unwrapped proxy, the whole site shares one cap, and nothing is logged.**
> `getClientIp()` returns `$_SERVER['REMOTE_ADDR']`, or `0.0.0.0` when that value is missing. It does not read `X-Forwarded-For`, `X-Real-IP`, `CF-Connecting-IP`, or any other forwarded header. If the site sits behind a proxy or CDN that the server does not unwrap, every visitor shares one address, so the limit becomes 5 per 5 minutes for the whole site (the defaults `RATE_LIMIT_IP_MAX` 5 and `RATE_LIMIT_WINDOW_SEC` 300). If `REMOTE_ADDR` is missing, every such request shares the key `ip:0.0.0.0`, and that same site-wide cap applies. No log line says that this happened. Visitors only see "Too many submissions from your network. Please try again later." Do not point this function at a client-supplied header. A sender could then choose a new address on every request, the cap would never hold, and that would also write nothing to the log.

> [!IMPORTANT]
> **Verify against your local codebase:** Before you change a number, confirm the six `RATE_LIMIT_*` defaults in `/includes/rate_limit_config.php` (lines 23–40), the `checkRateLimits(getClientIp(), trim($_POST['email'] ?? ''))` call in all four handlers, `hash('sha256', $key)` in `/includes/rate_limit/JsonFileRateLimitStore.php` (line 189), and that `/.gitignore` still ignores `data/rate-limits/*` while keeping `data/rate-limits/.gitkeep`. `mpact_config.php` is not in the repo.

> [!TIP]
> **A visitor is already blocked?** Jump to [Part 3: Unblock a Visitor](#part-3-unblock-a-visitor). To match the sentence on screen to a layer, use [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).

---

## Step-by-Step Instructions

Please change thresholds in `mpact_config.php` on the server. The PHP files below show how a number is counted. You should not need to edit them for a normal cap change. The guard chain those files sit in is [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md).

---

### Part 1: Change the Thresholds

1. To get started, please open `/includes/rate_limit.php` and find `getClientIp()`.

   **Real Code Snippet (`/includes/rate_limit.php` — client address used for the IP cap, lines 16–19)**
   ```php
   function getClientIp(): string
   {
       return $_SERVER['REMOTE_ADDR'] ?? '0.0.0.0';
   }
   ```

   Please leave this function as it is. The warning above is the failure mode: one shared address, and no log line. Replacing `REMOTE_ADDR` with a forwarded header in this file would let a client pick the bucket, and that bypass would also be silent.

2. Please read `checkRateLimits()` before you change a number. This function is the whole policy: two keys, fail open, and a 1-in-100 cleanup.

   **Real Code Snippet (`/includes/rate_limit.php` — IP key, email key, cleanup, and fail-open catch, lines 31–75)**
   ```php
   function checkRateLimits(string $ip, string $emailRaw): void
   {
       $windowSec = (int) RATE_LIMIT_WINDOW_SEC;
       $retentionDays = (int) RATE_LIMIT_RETENTION_DAYS;
   
       // Fail open. A storage fault must never cost us a genuine enquiry from a
       // prospective student or research partner — losing those is worse than
       // letting spam through until someone reads the log. A real reject is not
       // affected: processRateLimitKey() calls respond(), which exits before
       // this catch can see it.
       try {
           $store = RateLimitStoreFactory::create();
   
           processRateLimitKey(
               $store,
               'ip:' . $ip,
               (int) RATE_LIMIT_IP_MAX,
               $windowSec,
               $retentionDays,
               'Too many submissions from your network. Please try again later.'
           );
   
           $email = normalizeEmail($emailRaw);
           if ($email !== null) {
               processRateLimitKey(
                   $store,
                   'email:' . $email,
                   (int) RATE_LIMIT_EMAIL_MAX,
                   $windowSec,
                   $retentionDays,
                   'Too many submissions for this email address. Please try again later.'
               );
           }
   
           if (mt_rand(1, 100) === 1) {
               $store->cleanupExpired($retentionDays);
           }
       } catch (Throwable $e) {
           if (function_exists('mpactLogInternalError')) {
               mpactLogInternalError('MPCT rate limit skipped', $e);
           } else {
               error_log('MPCT rate limit skipped');
           }
       }
   }
   ```

   A few rules fall straight out of that function. Each one fails quietly if you miss it:

   * The IP key is always counted. The email key is counted only when `normalizeEmail()` returns an address. An empty or invalid email does not get an email file. There is no log line and no visitor message for that skip. Only the IP cap applies.
   * The IP key is counted first. When the email key is over its cap, `respond()` exits, but this request's IP slot was already used. Further retries of the same email still walk toward the IP cap. The sentence on screen then changes from the email message to the network message. Neither reject writes a rate-limit line to `error_log`.
   * A normal reject calls `respond()` in `/includes/validation.php` (lines 48–59). The browser gets HTTP 200 and JSON `{"success":false,"message":"…"}`, then the script exits. Checking only the HTTP status looks like success. Read the JSON `success` field. The `catch` does not run for this reject, which is what the comment on lines 36–40 says.
   * A thrown storage error is fail open. The visitor is not told that the cap was skipped. The submission continues into field validation and can still be emailed. The log line in Part 4 is the only signal.
   * `mt_rand(1, 100) === 1` runs only after both keys have allowed the request. It deletes files whose `last_seen` is older than the retention period. It does not end a block that is still inside the 300 second window, and a rejected request never reaches it.

3. The window is fixed, not sliding. Please open `RateLimitDecision::next()` in `/includes/rate_limit/RateLimitStoreInterface.php`.

   **Real Code Snippet (`/includes/rate_limit/RateLimitStoreInterface.php` — retention reset, fixed window, and reject inside `RateLimitDecision::next()`, lines 40–70)**
   ```php
           if ($record !== null) {
               $retentionCutoff = $now - ($retentionDays * 86400);
               if ($record['last_seen'] < $retentionCutoff) {
                   $record = null;
               }
           }
   
           if ($record === null) {
               return [
                   'count' => 1,
                   'window_start' => $now,
                   'last_seen' => $now,
               ];
           }
   
           if (($now - $record['window_start']) >= $windowSec) {
               return [
                   'count' => 1,
                   'window_start' => $now,
                   'last_seen' => $now,
               ];
           }
   
           if ($record['count'] >= $max) {
               return null;
           }
   
           $record['count']++;
           $record['last_seen'] = $now;
   
           return $record;
   ```

   `window_start` stays on the first allowed hit. A new count of 1 starts only when `now - window_start` has reached `RATE_LIMIT_WINDOW_SEC`. A reject returns `null` and does not move `window_start` or `last_seen`. In the JSON store, that `null` is not written (`JsonFileRateLimitStore.php`, lines 36–39), so rejected clicks do not extend the block. Waiting means waiting from the first counted submission in the current window, not from the latest click, and not for 7 days. Seven days is retention: how long an idle file is kept, and when an old `last_seen` is treated as "no record."

   The max is not stored in the file. The next request compares `count` with the constants loaded for that request. Raising a max unblocks on the next submit without deleting a file. Lowering a max can block on the next submit when `count` is already at the new max. You do not edit the JSON to change the cap. With the defaults, the third submission for one email is rejected (`count` is already 2), and the sixth submission for one IP is rejected (`count` is already 5).

4. Please confirm the call sits in front of field validation. If it is moved below `requireFields()`, invalid posts stop using a slot. A script can then hammer the handler without spending the cap, and nothing is logged to say the call moved.

   **Real Code Snippet (`/FormSubmission.php` — rate limit after the other guards and before `requireFields()`, lines 330–342)**
   ```php
   if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
       respond(false, 'Invalid request method.');
   }
   
   rejectIfHoneypotFilled();
   verifyCsrfToken();
   verifyTurnstile();
   checkRateLimits(getClientIp(), trim($_POST['email'] ?? ''));
   
   // Name, email, and category are required for every form type.
   // Category tells us which form was submitted; without it we can't
   // look up the right fields or build a meaningful email.
   requireFields(['first_name', 'last_name', 'email', 'category']);
   ```

   The same `checkRateLimits(getClientIp(), trim($_POST['email'] ?? ''));` line is in `/ServiceRequestSubmission.php` at line 511, `/EquipmentReservation.php` at line 188, and `/IntelScholarshipSubmission.php` at line 47. Field checks start after that line in each file. A request rejected for a missing field has already used a slot. A request rejected by the honeypot, the CSRF token check, or Cloudflare Turnstile never reaches the line, so it does not.

5. Please open `normalizeEmail()` when you need to see which addresses share one file.

   **Real Code Snippet (`/includes/rate_limit.php` — lowercase and email-shape check before the email key is counted, lines 21–29)**
   ```php
   function normalizeEmail(string $email): ?string
   {
       $email = strtolower(trim($email));
       if ($email === '' || !filter_var($email, FILTER_VALIDATE_EMAIL)) {
           return null;
       }
   
       return $email;
   }
   ```

   `Name@Example.com` and `name@example.com` are one key. An address that fails `FILTER_VALIDATE_EMAIL` is not a key. When you test, use a normal address such as `name@example.com`. A blank or one-word value never creates an email record, and the third submit will not show the email sentence. That miss is silent.

6. Put the new numbers in `mpact_config.php` on the server, not in the defaults file. Every handler loads `mpact_config.php` first (`/FormSubmission.php` line 56, `/ServiceRequestSubmission.php` line 44, `/EquipmentReservation.php` line 36, `/IntelScholarshipSubmission.php` line 28). `/includes/rate_limit.php` then loads `rate_limit_config.php`. A constant that already exists is left alone. Changing the `5` or the `2` inside `rate_limit_config.php` does nothing when `mpact_config.php` already defined that constant. There is no error and no log line. The site keeps the server value.

   The file is not in the repository. `/.gitignore` line 11 ignores the name `mpact_config.php`. Please ask the nano.nau.edu server administrator to edit the copy on the server. Do not commit the file, and do not paste it into a ticket. It also holds the Turnstile keys. Use the placeholders below.

7. Add only the constants you want to change. A constant you leave undefined keeps the default from the Visual Reference.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `RATE_LIMIT_IP_MAX` | Allowed submissions for one IP address in the current window. The next one is rejected. Counted even when the email is missing or later field checks fail. | Default `5`. `define('RATE_LIMIT_IP_MAX', [number]);` |
| ② | `RATE_LIMIT_EMAIL_MAX` | Allowed submissions for one valid email in that same window, across all four handlers. The next one is rejected. Empty or invalid emails are not counted. | Default `2`. `define('RATE_LIMIT_EMAIL_MAX', [number]);` |
| ③ | `RATE_LIMIT_WINDOW_SEC` | Fixed window length in seconds, measured from `window_start` (the first allowed hit), not from the latest hit. | Default `300` (5 minutes). `define('RATE_LIMIT_WINDOW_SEC', [seconds]);` |
| ④ | `RATE_LIMIT_RETENTION_DAYS` | How long a file may sit after `last_seen` before it is treated as empty and, about 1 request in 100, deleted. Not the block timer. | Default `7`. `define('RATE_LIMIT_RETENTION_DAYS', [days]);` |
| ⑤ | `RATE_LIMIT_STORAGE` | Which store the factory opens. | Default `json`. Leave it as `json`. See Technical best practices. |
| ⑥ | `RATE_LIMIT_DATA_DIR` | Folder whose `json` subdirectory holds the counter files. | Default, from `/includes/rate_limit_config.php`: `dirname(__DIR__) . '/data/rate-limits'`. In `/mpact_config.php` use `__DIR__ . '/data/rate-limits'`, because that file is in the site root. Copying `dirname(__DIR__)` into the server file points above the site and fails silently: the factory still starts, then falls back to the temp folder. |

   **Example Snippet (server overrides in mpact_config.php)**
   ```php
   // Add to the existing mpact_config.php on the server. Do not replace the file.
   // Handlers load this file before /includes/rate_limit_config.php.
   // A constant you do not define here keeps the default.

   define('RATE_LIMIT_IP_MAX', [number]); // change this number
   define('RATE_LIMIT_EMAIL_MAX', [number]); // change this number
   define('RATE_LIMIT_WINDOW_SEC', [seconds]); // change this number
   ```

   Please leave every Turnstile constant that is already in the file. The new numbers are read on the next request, because each request loads `mpact_config.php` again. Existing record files are not rewritten. If the old cap is still in effect after a save, ask the server administrator whether PHP is still running the previous copy of the file.

---

### Part 2: Set Up the Storage Folder on the Server

1. Please open `/includes/rate_limit/RateLimitStoreFactory.php`. You should not need to edit it. You do need the path it will actually write.

   **Real Code Snippet (`/includes/rate_limit/RateLimitStoreFactory.php` — configured directory, temp fallback, and store log lines, lines 16–38)**
   ```php
           $baseDir = defined('RATE_LIMIT_DATA_DIR')
               ? RATE_LIMIT_DATA_DIR
               : dirname(__DIR__, 2) . '/data/rate-limits';
   
           $driver = defined('RATE_LIMIT_STORAGE')
               ? strtolower((string) RATE_LIMIT_STORAGE)
               : strtolower((string) (getenv('RATE_LIMIT_STORAGE') ?: 'sqlite'));
   
           $tempDir = rtrim(sys_get_temp_dir(), '/\\') . '/mpct-rate-limits';
           $lastError = null;
   
           foreach (self::candidates($driver, $baseDir, $tempDir) as $make) {
               try {
                   self::$instance = $make();
                   error_log('MPCT rate limit store ready: ' . self::$instance->describe());
                   return self::$instance;
               } catch (Throwable $e) {
                   $lastError = $e;
                   error_log('MPCT rate limit store init failed: ' . $e->getMessage());
               }
           }
   
           throw $lastError ?? new RuntimeException('Unable to initialize rate limit storage.');
   ```

   **Real Code Snippet (`/includes/rate_limit/RateLimitStoreFactory.php` — JSON candidates, then the sqlite list and the rejected-driver log, lines 49–68)**
   ```php
           $sqliteThenJson = [
               static fn () => new SqliteRateLimitStore($baseDir . '/rate_limits.sqlite'),
               static fn () => new JsonFileRateLimitStore($baseDir . '/json'),
               static fn () => new SqliteRateLimitStore($tempDir . '/rate_limits.sqlite'),
               static fn () => new JsonFileRateLimitStore($tempDir . '/json'),
           ];
   
           if ($driver === 'json') {
               return [
                   static fn () => new JsonFileRateLimitStore($baseDir . '/json'),
                   static fn () => new JsonFileRateLimitStore($tempDir . '/json'),
               ];
           }
   
           if ($driver === 'sqlite' || $driver === '') {
               return $sqliteThenJson;
           }
   
           error_log('MPCT rate limit storage driver rejected');
           throw new RuntimeException('Unable to initialize rate limit storage.');
   ```

   With `RATE_LIMIT_STORAGE` set to `json`, the factory tries two directories and does not open SQLite:

   * `[RATE_LIMIT_DATA_DIR]/json`, which by default is the site-root folder `data/rate-limits/json`
   * then `sys_get_temp_dir()` plus `/mpct-rate-limits/json`

   `JsonFileRateLimitStore` creates that directory with mode `0775` when the parent can be written (lines 12–13 of `/includes/rate_limit/JsonFileRateLimitStore.php`). The nano.nau.edu server administrator has to make `data/rate-limits/` writable by the account PHP-FPM runs as. If that account cannot create `data/rate-limits/json`, the first candidate throws `Unable to create rate limit storage directory`. The factory logs `MPCT rate limit store init failed: Unable to create rate limit storage directory` and opens the temp directory instead. Counting still works there. The comment at the top of `/includes/rate_limit_config.php` says those temp counters reset whenever PHP restarts. The factory has no line that deletes the temp folder on startup, so do not use a PHP restart as a way to clear a block, and do not leave production on the temp folder. Please do not copy server account or group names out of that config comment into a ticket or into this manual.

   Part 4 shows how to tell the two folders apart. A `store ready` path ending in `data/rate-limits/json` is the configured folder. A path containing `mpct-rate-limits` is the temp fallback. If you only look in the repository tree, a temp fallback looks like "no file was written." The browser does not say so: submits still succeed until the cap is hit, and the files are simply somewhere else.

   If both directories fail, the `throw` after the loop is caught by `checkRateLimits()`, and the submission is allowed. Spam is not blocked. The visitor sees no rate-limit sentence. Only the log shows it.

2. Please keep `/data/.htaccess`. It denies web access to the whole `data` folder, including the counter files.

   **Real Code Snippet (`/data/.htaccess` — deny web access to runtime data, lines 1–2)**
   ```apache
   # Deny direct HTTP access to rate-limit storage and other runtime data.
   Require all denied
   ```

   The filename is a hash, but the JSON body still contains the plain key (`email:…` or `ip:…`). If this file is removed, anyone who can request that path can download the address. That is not a silent code path: the file is served. Please leave both lines in place. A syntax error here makes Apache return HTTP 500 for paths under `/data/`. It does not, by itself, return 500 for the rest of the site.

   PHP's built-in server (`php -S`) does not read `.htaccess`, so it cannot prove this rule. On Apache (production, staging, or a local Apache such as XAMPP), the check in Let's Verify should return HTTP 403.

3. Please keep the empty placeholder and the ignore rules.

   **Real Code Snippet (`/.gitignore` — counter files stay out of git, lines 13–15)**
   ```text
   # Rate limit runtime data (SQLite DB / JSON counters — never commit)
   data/rate-limits/*
   !data/rate-limits/.gitkeep
   ```

   `/data/rate-limits/.gitkeep` is an empty file so git keeps the directory. Those three lines keep `json/*.json` and any SQLite file out of a normal `git add`. Do not `git add -f` them. The JSON body contains the visitor's email address or IP address, and committing one publishes it. Deleting `.gitkeep` drops the placeholder from the repo the next time git sees an empty folder. PHP recreates the folder at runtime only when it has permission to. Please leave both the placeholder and the ignore rules. `mpact_config.php` stays ignored as well (line 11).

---

### Part 3: Unblock a Visitor

1. You can wait out the fixed window, or you can delete that visitor's record file. Please do not wait `RATE_LIMIT_RETENTION_DAYS` (7 by default). Retention is how long an idle file is kept. The block ends when `window_start` is at least `RATE_LIMIT_WINDOW_SEC` seconds ago (300 by default), because the next request then starts a new count at 1. Rejected attempts do not push `window_start` forward.

   Raising the matching max in `mpact_config.php` also clears the block on the next request, without deleting the file, because the max is not stored in the record. Lowering it can block someone who was previously under the old cap, for the rest of that same window.

2. To delete one record, hash the key the same way the store names the file.

   **Real Code Snippet (`/includes/rate_limit/JsonFileRateLimitStore.php` — record filename is `sha256` of the key, lines 187–190)**
   ```php
       private function pathForKey(string $key): string
       {
           return $this->directory . '/' . hash('sha256', $key) . '.json';
       }
   ```

   `hash()` returns lowercase hex, so the filename is 64 lowercase hex characters plus `.json`. The directory is the one in the `store ready` log: `data/rate-limits/json/` when the configured folder is in use, or the `mpct-rate-limits/json` temp directory when it is not. Deleting a file in the other directory does nothing. The visitor stays blocked, and there is no error. Please read the log first (Part 4).

   Lowercase the email before you hash it. `normalizeEmail()` has already lowercased the stored key. For an IP, do not lowercase the address and do not add a port. The key is `ip:` plus the address exactly as `REMOTE_ADDR` had it.

   **Example Snippet (hash a rate-limit key)**
   ```text
   php -r "echo hash('sha256', 'email:[name@example.com]');"
   php -r "echo hash('sha256', 'ip:[address]');"
   ```

   Replace `[name@example.com]`, including the square brackets, with the lowercased address. Replace `[address]` with the exact `REMOTE_ADDR` string, with no port and no second `ip:` prefix. Do not leave the brackets in the string you hash. For the local test in Let's Verify, the email string inside the quotes is `email:name@example.com`. Run the one-liner on the server (PowerShell accepts the same quoting), then delete only that filename from the `json` directory the log named. Never paste a real visitor email or IP into a ticket, a chat, or this manual. Do not copy the JSON body out either. The file stores the plain key on purpose:

   **Real Code Snippet (`/includes/rate_limit/JsonFileRateLimitStore.php` — JSON body keeps the plain key, lines 142–147)**
   ```php
           $encoded = json_encode([
               'key' => $key,
               'count' => (int) $record['count'],
               'window_start' => (int) $record['window_start'],
               'last_seen' => (int) $record['last_seen'],
           ], JSON_PRETTY_PRINT);
   ```

3. Please do not hand-edit `count` to unblock someone. If the file is truncated or missing `count`, `window_start`, or `last_seen`, the next submission is treated as a new record and the file is rewritten.

   **Real Code Snippet (`/includes/rate_limit/JsonFileRateLimitStore.php` — a corrupt record is treated as empty, lines 172–178)**
   ```php
           // A truncated or corrupt file is treated as "no record" so the next
           // write repairs it. Throwing here would lock that email or IP out
           // permanently, with no way back short of deleting the file by hand.
           $data = json_decode($raw, true);
           if (!is_array($data) || !isset($data['count'], $data['window_start'], $data['last_seen'])) {
               return null;
           }
   ```

   Editing the file by hand can unblock by accident and can also throw away a real count. Deleting the file is the step that matches the code. A hash of the wrong string (an uppercase email, a missing `email:` prefix, or `email:` when you meant `ip:`) deletes nothing useful, and the block remains. That miss is silent.

4. The email file and the IP file are separate. Deleting the email file does not reset the IP file. After several retries, the IP cap can be the one still blocking, and the visitor still sees "Too many submissions from your network. Please try again later." Hash both keys when you need both cleared. In any notes, keep using `[name@example.com]` and `[address]`. Compute the real hashes only on the server, and do not paste them back into this page.

---

### Part 4: Read the Logs

1. Please read the PHP error log when you need the folder name or a storage failure. A normal cap hit is not logged. The visitor's JSON message is the only output, and the HTTP status stays 200. If you wait for an error line before you believe the block, you will think the cap is broken. It is not. It is quiet on purpose.

   On the server, use the PHP log the nano.nau.edu server administrator already collects. Locally, the Let's Verify steps start `php -S` from the site root. `error_log()` lines from that run appear in the terminal where the server is running, unless `php.ini` points `error_log` at a file. `php -S` does not read `.htaccess`, so that terminal will not show whether `/data/` is denied. It will show the rate-limit lines, because those come from PHP.

2. These are the exact strings. The factory writes the first three. `checkRateLimits()` writes the last one, through `mpactLogInternalError()` when that function exists.

   **Real Code Snippet (`/includes/rate_limit/JsonFileRateLimitStore.php` — text appended after `MPCT rate limit store ready: `, lines 131–134)**
   ```php
       public function describe(): string
       {
           return 'json:' . $this->directory;
       }
   ```

   **Real Code Snippet (`/includes/validation.php` — shape of `MPCT rate limit skipped` when the helper is loaded, lines 108–111)**
   ```php
   function mpactLogInternalError(string $label, Throwable $e): void
   {
       error_log($label . ': ' . $e->getMessage() . ' in ' . basename($e->getFile()) . ':' . $e->getLine());
   }
   ```

   The ready line is `MPCT rate limit store ready: ` plus `describe()`. For this store that is `json:` immediately followed by the directory, with no space. A configured folder ends in `data/rate-limits/json`. A temp fallback contains `mpct-rate-limits`. `create()` keeps the store in a static property (`/includes/rate_limit/RateLimitStoreFactory.php`, lines 8–14) and logs the ready line only the first time that PHP process builds it. Later requests in the same process, including a request that is then rejected for being over the cap, do not log ready again. There is not a second line that says "blocked."

   | Log text | When it is written | What you miss if you skip it |
   |---|---|---|
   | `MPCT rate limit store ready: json:<directory>` | The store opened (factory line 30 plus `describe()`). | If `<directory>` is not the folder you are watching, you will look at an empty directory and think nothing was counted. The cap is still enforced in the other folder. That confusion is silent in the browser. |
   | `MPCT rate limit store init failed: <message>` | A candidate threw (factory line 34). JSON creation fails with `Unable to create rate limit storage directory`. | The preferred folder was skipped. A following ready line that contains `mpct-rate-limits` means you are on the temp fallback. |
   | `MPCT rate limit storage driver rejected` | `RATE_LIMIT_STORAGE` is not `json`, not `sqlite`, and not an empty string (factory line 67). No extra words on this line. | The limiter then fails open. Submissions are allowed. The visitor sees no rate-limit error. |
   | `MPCT rate limit skipped: <message> in <file>:<line>` | The catch in `checkRateLimits()` ran, and `mpactLogInternalError()` was loaded. All four handlers load `/includes/validation.php` first, so this is the line you should expect. | Storage failed and the form was allowed through. Spam is not blocked until someone reads this line. The visitor is not told. |
   | `MPCT rate limit skipped` | The same catch, only when `mpactLogInternalError` is missing (rate_limit.php lines 71–72). | You should not see the bare sentence on these handlers. If you do, `validation.php` did not load, and the other guards are not loaded either. |

3. Honeypot and CSRF rejects also log nothing. Cloudflare Turnstile failures log under different prefixes (`MPCT Turnstile siteverify…`), which [Update Cloudflare Turnstile (CAPTCHA) Settings](02-update-turnstile-settings.md) covers. If the visitor sees "Too many submissions…" and the log shows only `store ready`, the cap is what stopped them. Match the full sentence in [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).

---

## Technical best practices and validation

* **Keep `RATE_LIMIT_STORAGE` set to `json`.** The default (lines 35–36 of `/includes/rate_limit_config.php`) is `json`. The header comment at lines 8–11 says JSON is the supported production mode because nano.nau.edu ships SQLite 3.7.17, which that comment says is older than the UPSERT syntax the driver needs:

   **Real Code Snippet (`/includes/rate_limit_config.php` — comment that still calls JSON the supported mode, lines 8–11)**
   ```php
    * Storage is 'json' — plain files guarded by flock. The SQLite driver is
    * kept as a fallback, but nano.nau.edu ships SQLite 3.7.17, which is older
    * than the UPSERT syntax the driver needs, so JSON is the supported mode
    * in production.
   ```

   The SQLite class does not send UPSERT. It uses `INSERT OR REPLACE`, and its own comment says `ON CONFLICT … DO UPDATE` is a syntax error on SQLite 3.7.17:

   **Real Code Snippet (`/includes/rate_limit/SqliteRateLimitStore.php` — `INSERT OR REPLACE` instead of UPSERT, lines 73–79)**
   ```php
           // INSERT OR REPLACE, not ON CONFLICT ... DO UPDATE: UPSERT needs
           // SQLite 3.24+ and nano.nau.edu ships 3.7.17, where it is a syntax
           // error. Every column is written here, so the two are equivalent.
           $stmt = $this->pdo->prepare(
               'INSERT OR REPLACE INTO rate_limits (rate_key, count, window_start, last_seen)
                VALUES (:rate_key, :count, :window_start, :last_seen)'
           );
   ```

   Leave the constant as `json` anyway. In the `json` branch the factory never opens SQLite. The only fallback is the temp JSON directory. Switching the constant to `sqlite` uses the other candidate list (a SQLite file first). That is not the path these defaults are written for, and this page does not cover running it. The version 3.7.17 is what those two comments state. An unrecognized value logs `MPCT rate limit storage driver rejected` and then fails open: submissions go through, and the visitor is not told. Deleting the `RATE_LIMIT_STORAGE` define is also a silent change of driver. If the constant is missing, factory lines 20–22 use the `RATE_LIMIT_STORAGE` environment variable, or `sqlite` when that variable is unset. Do not delete the define.

* **On a shared network, raise the IP limit before the email limit.** Campus Wi-Fi and VPNs put many visitors behind one `REMOTE_ADDR`. They share `RATE_LIMIT_IP_MAX` (5 per 300 seconds with the defaults). One group can spend that cap before any single person has sent two forms. They all see "Too many submissions from your network. Please try again later." Nothing is logged to separate "one shared address" from "one person sent five." Raising `RATE_LIMIT_EMAIL_MAX` does not help them. Raise `RATE_LIMIT_IP_MAX` first. The email cap is still what stops one address from posting through all four handlers. That window is shared on purpose.

* **Leave the fail-open catch in place.** The comment in `checkRateLimits()` (lines 36–40) says a storage fault must not drop a genuine enquiry. The catch allows the submission and logs `MPCT rate limit skipped…`. If you remove the catch, a permissions mistake on the storage folder throws on every submission. The exception handler in `/includes/validation.php` (lines 123–126) logs `MPCT uncaught error:` and the visitor sees `publicFormErrorMessage()` (lines 101–106), not a rate-limit sentence. Fix the folder (Part 2) instead of removing the catch.

---

## Let's Verify Your Changes

1. A local submit has to get past Cloudflare Turnstile and the CSRF token check before the cap is involved. Please use Turnstile test keys, Part 4 of [Update Cloudflare Turnstile (CAPTCHA) Settings](02-update-turnstile-settings.md). Without a passing token the handler stops first. You will see a Turnstile sentence, no counter file, and it will look as if the email cap did nothing. The order of those checks is in [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md). If the sentence is one you do not expect, use [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).

2. From the site root (the folder that contains `FormSubmission.php`), start PHP's built-in server and leave the terminal visible:

   ```text
   php -S 127.0.0.1:8080
   ```

   `error_log()` lines print in that terminal unless `php.ini` sends `error_log` to a file. This server does not read `.htaccess`. Do not use it to judge the `/data/` deny rule.

3. Open `http://127.0.0.1:8080/Contact_Us.html`. Pick a category, fill the required fields, and use the email `name@example.com`. Submit twice, inside 5 minutes. Both requests must get past Turnstile and the CSRF token check. They may still fail later with a mail error if mail is not set up on your machine. That is all right: the slot is used before mail is sent. Do not use a real person's email address.

4. In the `php -S` terminal, find `MPCT rate limit store ready: json:`. If the path ends with `data/rate-limits/json`, the configured folder is in use. If the path contains `mpct-rate-limits`, you are on the temp fallback, and an earlier line should read `MPCT rate limit store init failed:`. Look in the directory named on the ready line. After the first allowed submit, a `.json` file with a 64-character lowercase hex name should be there. Do not paste the file into a ticket. Its `key` field is the email address.

5. Submit a third time with the same email, still inside 5 minutes, and before you have sent five requests from this IP. In the browser's Network panel, open `FormSubmission.php`. The status is HTTP 200, not 429. The body is `{"success":false,"message":"Too many submissions for this email address. Please try again later."}`. The terminal does not add a skipped line or a rejected line for this hit. Do not expect a second `store ready` line. That line is written once per PHP process, on the first request that opens the store. Stop at three. A later request from the same IP can switch the sentence to "Too many submissions from your network. Please try again later.", because each email reject still counted toward the IP cap. If the body is a different sentence, stop and use [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).

6. Either wait until 300 seconds after the first counted submit (the window starts at `window_start`, not at the third click), or delete the email file with the Part 3 one-liner, using the directory from the ready line. Submit `name@example.com` again. The email sentence should be gone. If you already used five IP slots, you will see the network sentence until that window ends or you also delete the `ip:` file.

7. On Apache, not on `php -S`, confirm the counter folder is not public. Production, staging, or a local Apache such as XAMPP can all serve `.htaccess`. PHP's built-in server cannot.

   ```text
   curl -I https://nano.nau.edu/data/rate-limits/
   ```

   You should see HTTP 403, from `Require all denied` in `/data/.htaccess`. HTTP 200 means that file is not in effect on the server you queried. A syntax error in it returns HTTP 500 for `/data/` only, not a pass.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
