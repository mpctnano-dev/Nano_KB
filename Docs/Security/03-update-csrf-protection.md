
# Update CSRF Token Protection

**Summary:** Update how a form loads, sends, and checks the CSRF token in the page scripts, the submit code, and the PHP handlers.

**When to change:** When a form's submit code is rewritten, a new fetch-based form is added, or HTTPS or proxy settings change on the server.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 20 minutes

---

## Visual Reference

**Real Output (response of `/csrf-token.php` on a local copy, token shortened)**

```http
HTTP/1.1 200 OK
Content-Type: application/javascript; charset=UTF-8
Cache-Control: no-store
Set-Cookie: csrf_token=[64 hex characters]; path=/; HttpOnly; SameSite=Lax

window.MPCT_CSRF_TOKEN="[64 hex characters]";
```

*The same token goes into the HttpOnly cookie and the JS global. This sample is plain HTTP, so `secure` is absent. Over HTTPS the cookie also carries `secure`.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/includes/csrf.php` | Mints or reuses the token, sets the `csrf_token` cookie, and compares it with the POST field in `verifyCsrfToken()`. |
| `/csrf-token.php` | Sends `Cache-Control: no-store`, the cookie, and `window.MPCT_CSRF_TOKEN`. Does not use `mpact_config.php`. |
| `/JS/csrf.js` | Adds the hidden `csrf_token` field and copies the token onto `FormData`. |
| `/Contact_Us.html` | Loads `csrf-token.php` then `JS/csrf.js` (lines 208–209), before `JS/layout.js`. |
| `/ServiceRequest.html` | Same script order (lines 1939–1940). The inline submit calls are around lines 2679 and 2704. |
| `/Reserve_Equipment.html` | Same script order (lines 642–643). Submit code is in `/JS/booking.js`. |
| `/CHIPS_Scholars_Program.html` | Same script order (lines 641–642). The inline submit calls are around lines 970 and 983. |
| `/JS/script.js` | Contact form: `appendToFormData()` on line 1196, and `applyToForm()` after `form.reset()` on line 1208. |
| `/JS/booking.js` | Booking form: the same two calls, around lines 994 and 1007. |
| `/FormSubmission.php` | Requires `includes/csrf.php` on line 65. Calls `verifyCsrfToken()` on line 335 (guard lines 330–337). |
| `/ServiceRequestSubmission.php` | Requires `includes/csrf.php` on line 53. Calls `verifyCsrfToken()` on line 507 (guard lines 501–511). |
| `/EquipmentReservation.php` | Requires `includes/csrf.php` on line 45. Calls `verifyCsrfToken()` on line 184 (guard lines 178–188). |
| `/IntelScholarshipSubmission.php` | Requires `includes/csrf.php` on line 33. Calls `verifyCsrfToken()` on line 45 (guard lines 39–47). |

> [!WARNING]
> **`csrf-token.php` must load before `JS/csrf.js`, and every `fetch` must send the token!**
> If the token never arrives in the POST, every submit fails with "Unable to process your submission. Please refresh the page and try again." Refreshing does not help when the script order, the script path, or the submit code is what is wrong, and nothing is written to `error_log`. Skipping `verifyCsrfToken()` fails the other way: a forged post is accepted, and the visitor sees no error.

> [!NOTE]
> **Double-submit cookie, in four steps:** `csrfIssueToken()` mints a token or reuses a valid one, then the browser stores an HttpOnly `csrf_token` cookie and `window.MPCT_CSRF_TOKEN`, then the page sends that value as a hidden `csrf_token` field or on `FormData`, then `verifyCsrfToken()` compares the two with `hash_equals`.

> [!IMPORTANT]
> **Verify against your local codebase:** the two script tags and their order on each form page, the `appendToFormData()` and `applyToForm()` calls in that page's submit code, and the `verifyCsrfToken()` line in each handler. Re-check `MPCT_CSRF_BYTES` and the 64-hex test in `csrfTokenLooksValid()` before changing either one.

> [!TIP]
> **Adding a whole new form?** Follow [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md) so the honeypot, this token, Turnstile, and the rate limit stay one chain. This page is only the CSRF piece.

---

## Step-by-Step Instructions

---

### Part 1: Load the Token on the Page

1. To get started, please open `/Contact_Us.html` in your code editor. The same pair is on `/ServiceRequest.html` (lines 1939–1940), `/Reserve_Equipment.html` (lines 642–643), and `/CHIPS_Scholars_Program.html` (lines 641–642).
2. Search for `csrf-token.php`. You will find the security scripts, in this order:

   **Real Code Snippet (`/Contact_Us.html` — security script tags, lines 205–210)**
   ```html
    <script src="https://challenges.cloudflare.com/turnstile/v0/api.js?render=explicit" defer></script>
    <script src="turnstile-config.php"></script>
    <script src="JS/turnstile.js"></script>
    <script src="csrf-token.php"></script>
    <script src="JS/csrf.js"></script>
    <script src="JS/layout.js"></script>
   ```

3. Please leave `csrf-token.php` directly above `JS/csrf.js`. Do not add `defer` or `async` to either tag. Only `api.js` is deferred, and `JS/layout.js` stays underneath. `JS/csrf.js` fills each form immediately when `document.readyState` is not `"loading"`. If the token script has not run yet, the token is empty, no hidden field is added, and it does not try again. That miss is silent in the browser.
4. Open `/csrf-token.php` next. This file does not read `mpact_config.php`.

   **Real Code Snippet (`/csrf-token.php` — cookie and JS global, lines 11–16)**
   ```php
header('Content-Type: application/javascript; charset=UTF-8');
header('Cache-Control: no-store');

$token = csrfIssueToken();

echo 'window.MPCT_CSRF_TOKEN=' . json_encode($token, JSON_UNESCAPED_SLASHES) . ';';
   ```

   The Visual Reference is that response with the token shortened. The cookie value and `window.MPCT_CSRF_TOKEN` match. Please do not paste a real token into a guide, a ticket, or a log.
5. For a new page in the site root, copy the pair below. A page in a subfolder needs `../` on both `src` values. A wrong path never sets the global, every submit shows the refresh message, and reloading does not help.

   **Example Snippet (token scripts on a root form page)**
   ```html
   <!-- [NewPage].html — keep this pair in order, with no defer or async -->
   <script src="csrf-token.php"></script>
   <script src="JS/csrf.js"></script>
   ```

---

### Part 2: Send the Token with the Form

1. To get started, please open `/JS/csrf.js` in your code editor.
2. Search for `function applyToForm`. This file writes the hidden field, survives `form.reset()`, and puts the token on `FormData`:

   **Real Code Snippet (`/JS/csrf.js` — hidden field and FormData helpers, lines 14–68)**
   ```javascript
    function applyToForm(form) {
        if (!form) {
            return;
        }

        var token = getToken();
        if (!token) {
            return;
        }

        var input = form.querySelector('input[name="csrf_token"]');
        if (!input) {
            input = document.createElement('input');
            input.type = 'hidden';
            input.name = 'csrf_token';
            form.appendChild(input);
        }

        input.value = token;
        // Keep reset() from wiping the token to an empty default.
        try {
            input.defaultValue = token;
        } catch (err) {
            /* ignore */
        }
    }

    function applyToAllForms() {
        var forms = document.querySelectorAll('form');
        for (var i = 0; i < forms.length; i += 1) {
            applyToForm(forms[i]);
        }
    }

    function appendToFormData(formData) {
        var token = getToken();
        if (token && formData && typeof formData.set === 'function') {
            formData.set('csrf_token', token);
        }
        return formData;
    }

    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', applyToAllForms);
    } else {
        applyToAllForms();
    }

    global.MPCT = global.MPCT || {};
    global.MPCT.Csrf = {
        getToken: getToken,
        applyToForm: applyToForm,
        applyToAllForms: applyToAllForms,
        appendToFormData: appendToFormData,
    };
   ```

3. `applyToAllForms()` runs on `DOMContentLoaded`, or right away when the document is already loaded. It calls `applyToForm()` for every `<form>`. An empty `window.MPCT_CSRF_TOKEN` makes `applyToForm()` return without adding an input. The browser shows no warning.
4. Lines 32–38 set `defaultValue` on purpose. Writing `input.value` does not change what `form.reset()` restores. A field built in JavaScript starts with an empty default, so `reset()` would clear `csrf_token` and the next submit would post an empty token. Copying the token into `defaultValue` makes `reset()` keep it. The `try/catch` ignores a browser that refuses `defaultValue`, so the rest of the script still runs. Each submit handler also calls `applyToForm()` after `form.reset()` so the field is written again even when `defaultValue` could not be set.
5. `appendToFormData()` uses `formData.set`, not `append`, so a second call replaces `csrf_token` instead of sending it twice. If the token or `FormData.set` is missing, the function returns the body unchanged and still does not warn.
6. Please open the contact submit handler next. The other three forms use the same two calls.

   **Real Code Snippet (`/JS/script.js` — contact submit sends the token, lines 1194–1209)**
   ```javascript
        const formData = new FormData(form);
        if (window.MPCT && MPCT.Csrf) {
            MPCT.Csrf.appendToFormData(formData);
        }
        const response = await fetch('FormSubmission.php', {
            method: 'POST',
            body: formData,
        });
        const result = await response.json();

        if (result.success) {
            setContactFeedback(result.message || 'Your message was sent.', 'success');
            form.reset();
            if (window.MPCT && MPCT.Csrf) {
                MPCT.Csrf.applyToForm(form);
            }
   ```

   `/JS/booking.js` does this around lines 994 and 1007. The inline script in `/ServiceRequest.html` does it around lines 2679 and 2704. The inline script in `/CHIPS_Scholars_Program.html` does it around lines 970 and 983.
7. Please keep both calls when you rewrite a submit function. `new FormData(form)` includes the hidden field only when that field is already filled. `appendToFormData()` writes the global's value at submit time, which still works if the field was cleared. `applyToForm()` after `form.reset()` fills the field for the next submit. The `if (window.MPCT && MPCT.Csrf)` check does not stop the request when the script failed to load. The POST goes out with no token, the handler rejects it, and the visitor sees the refresh message. The log stays empty.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `MPCT.Csrf.appendToFormData(formData)` | Puts `window.MPCT_CSRF_TOKEN` on the POST body with `formData.set`. | After `new FormData(...)`, before `fetch`. |
| ② | `MPCT.Csrf.applyToForm(form)` | Writes the hidden `csrf_token` input and its `defaultValue`. | After every `form.reset()`. |

   **Example Snippet (fetch submit that sends the token)**
   ```javascript
   const formData = new FormData(form);
   if (window.MPCT && MPCT.Csrf) {
       MPCT.Csrf.appendToFormData(formData); // send csrf_token on this fetch
   }
   const response = await fetch('[NewHandler].php', {
       method: 'POST',
       body: formData,
   });
   // ...after success:
   form.reset();
   if (window.MPCT && MPCT.Csrf) {
       MPCT.Csrf.applyToForm(form); // put the token back; reset() would clear an empty default
   }
   ```

---

### Part 3: Check the Token in PHP

1. To get started, please open `/FormSubmission.php`. The other three handlers use the same order. Some put a blank line between the calls. The order itself does not change.

| Handler | `validation.php` | `csrf.php` | `verifyCsrfToken()` |
|---|---|---|---|
| `/FormSubmission.php` | line 61 | line 65 | line 335 (guard lines 330–337) |
| `/ServiceRequestSubmission.php` | line 49 | line 53 | line 507 (guard lines 501–511) |
| `/EquipmentReservation.php` | line 41 | line 45 | line 184 (guard lines 178–188) |
| `/IntelScholarshipSubmission.php` | line 29 | line 33 | line 45 (guard lines 39–47) |

2. Search for `includes/csrf.php`. It is loaded after `includes/validation.php`:

   **Real Code Snippet (`/FormSubmission.php` — requires ending with csrf.php, lines 61–65)**
   ```php
require_once __DIR__ . '/includes/validation.php';
require_once __DIR__ . '/includes/turnstile.php';
require_once __DIR__ . '/includes/rate_limit.php';
require_once __DIR__ . '/includes/honeypot.php';
require_once __DIR__ . '/includes/csrf.php';
   ```

   `verifyCsrfToken()` calls `respond()`, which lives in `/includes/validation.php`. `/includes/csrf.php` does not load that file. Keep `validation.php` first so the check cannot run before `respond()` exists. A missing `respond()` is a PHP fatal, and these handlers set `display_errors` to `0`, so the visitor does not see the refresh message. If `validation.php` already registered its shutdown function, the log line is `MPCT fatal:` and the visitor sees the generic public error instead. CSRF itself logs nothing either way.
3. Search for `verifyCsrfToken()`. Call it after the honeypot and before Turnstile:

   **Real Code Snippet (`/FormSubmission.php` — guard chain, lines 330–337)**
   ```php
if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    respond(false, 'Invalid request method.');
}

rejectIfHoneypotFilled();
verifyCsrfToken();
verifyTurnstile();
checkRateLimits(getClientIp(), trim($_POST['email'] ?? ''));
   ```

   The whole chain is written up in [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md). Please do not move this call later, and do not delete it. A missing call does not show an error. The post is accepted.
4. The check uses one visitor message for every failure:

   **Real Code Snippet (`/includes/csrf.php` — verifyCsrfToken function, lines 78–90)**
   ```php
function verifyCsrfToken(): void
{
    $cookie = (string) ($_COOKIE[MPCT_CSRF_COOKIE] ?? '');
    $posted = (string) ($_POST[MPCT_CSRF_FIELD] ?? '');

    if ($cookie === '' || $posted === '' || !csrfTokenLooksValid($cookie) || !csrfTokenLooksValid($posted)) {
        respond(false, 'Unable to process your submission. Please refresh the page and try again.');
    }

    if (!hash_equals($cookie, $posted)) {
        respond(false, 'Unable to process your submission. Please refresh the page and try again.');
    }
}
   ```

   An empty value, a value that is not 64 lowercase hex characters, and a cookie that does not match the POST field all look the same to the visitor. `respond()` sends HTTP 200 and JSON with `success` false, then exits. The front end prints `message`. HTTP 200 does not mean the submit worked. Nothing is added to `error_log`. The honeypot sentence is the same up to "Please try again." and does not say "refresh the page", which is how you tell the two apart. Both are on [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).
5. On a new handler, copy the full chain from [Add Security Checks to a New Form](01-add-security-checks-to-a-form.md). The CSRF lines are these:

   **Example Snippet (CSRF lines in a new handler)**
   ```php
   // [NewHandler].php — validation.php must load before verifyCsrfToken() runs
   require_once __DIR__ . '/includes/validation.php';
   require_once __DIR__ . '/includes/csrf.php'; // after validation.php, so respond() exists

   rejectIfHoneypotFilled();
   verifyCsrfToken(); // after the honeypot, before verifyTurnstile()
   verifyTurnstile();
   ```

---

### Part 4: Change the Cookie Settings

1. To get started, please open `/includes/csrf.php` and search for `function csrfSetCookie`.

   **Real Code Snippet (`/includes/csrf.php` — csrfSetCookie options, lines 46–57)**
   ```php
function csrfSetCookie(string $token): void
{
    setcookie(MPCT_CSRF_COOKIE, $token, [
        'expires'  => 0,
        'path'     => '/',
        'secure'   => csrfIsHttps(),
        'httponly' => true,
        'samesite' => 'Lax',
    ]);

    $_COOKIE[MPCT_CSRF_COOKIE] = $token;
}
   ```

2. Change a line only when the lab's HTTPS or proxy setup requires it. Each option below is the value to keep unless that reason applies.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `expires` | How long the browser keeps the cookie. | `0`. Session cookie, kept until the browser closes. PHP then sends no `Expires` and no `Max-Age`. |
| ② | `path` | Which paths receive the cookie. | `'/'`. A narrower path omits the cookie on the handler, and every submit shows the refresh message. |
| ③ | `secure` | Send the cookie only over HTTPS. | Leave `csrfIsHttps()`. Do not hard-code `true` or `false`. |
| ④ | `httponly` | Hide the cookie from `document.cookie`. | `true`. The page reads `window.MPCT_CSRF_TOKEN` instead. |
| ⑤ | `samesite` | When the browser attaches the cookie. | `'Lax'`. Do not switch this to `None` or `Strict`. |

3. `secure` follows `csrfIsHttps()`:

   **Real Code Snippet (`/includes/csrf.php` — csrfIsHttps checks, lines 21–34)**
   ```php
function csrfIsHttps(): bool
{
    if (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') {
        return true;
    }

    if (isset($_SERVER['SERVER_PORT']) && (int) $_SERVER['SERVER_PORT'] === 443) {
        return true;
    }

    $forwarded = strtolower((string) ($_SERVER['HTTP_X_FORWARDED_PROTO'] ?? ''));

    return $forwarded === 'https';
}
   ```

   The Visual Reference was captured on PHP's built-in server over plain HTTP, so the `Set-Cookie` line has no `secure` flag. That matches this function. When any one check is true, PHP adds `secure` to the same cookie. `X-Forwarded-Proto` must be exactly `https` after lowercasing. A longer value does not match, and `secure` stays off. Please do not replace `csrfIsHttps()` with `true`: on an `http://` local server the browser drops the cookie, every submit shows the refresh message, and a refresh does not help. Please do not hard-code `false` on production either, or the cookie is allowed over plain HTTP. If the live site is missing `secure`, ask the nano.nau.edu server administrator to pass `X-Forwarded-Proto: https`, or to turn `HTTPS` on or serve port 443. That fix is on the server, not in this file. PHP's built-in server does not read `.htaccess`, so it will not pick up Apache header rules while you test.
4. A valid cookie is reused, not rotated, on every page load:

   **Real Code Snippet (`/includes/csrf.php` — csrfIssueToken reuse, lines 62–73)**
   ```php
function csrfIssueToken(): string
{
    $existing = (string) ($_COOKIE[MPCT_CSRF_COOKIE] ?? '');
    if (csrfTokenLooksValid($existing)) {
        csrfSetCookie($existing);
        return $existing;
    }

    $token = csrfGenerateToken();
    csrfSetCookie($token);
    return $token;
}
   ```

   Reloading does not mint a new token while the cookie still matches the 64-hex test, including in another tab of the same browser. A missing or invalid cookie is replaced. `csrfSetCookie()` also copies the value into `$_COOKIE` for the rest of that PHP request.
5. If you do edit the array, keep the safe values and comment the line you touch:

   **Example Snippet (cookie options)**
   ```php
   setcookie(MPCT_CSRF_COOKIE, $token, [
       'expires'  => [Expires], // use 0 so the cookie ends when the browser closes
       'path'     => '[Path]', // use '/' so the root handlers receive the cookie
       'secure'   => csrfIsHttps(), // leave this call; do not hard-code true or false
       'httponly' => true, // keep true; the page uses window.MPCT_CSRF_TOKEN
       'samesite' => '[SameSite]', // use 'Lax'
   ]);
   ```

---

## Technical best practices and validation

These pairs have to change together. Updating one side still looks like a normal page load, then every submit fails, and `error_log` stays empty.

* **Token length:** `MPCT_CSRF_BYTES` is 32. `bin2hex` of those bytes is 64 hex characters, and `csrfTokenLooksValid()` allows only `/^[a-f0-9]{64}$/`. Change the byte count and the `{64}` in the same edit (the new width is twice the byte count). If you change only one, every new token fails the test, a refresh mints another token of the wrong length, and the refresh message does not go away.

   **Real Code Snippet (`/includes/csrf.php` — cookie name and byte count, lines 17–19)**
   ```php
define('MPCT_CSRF_COOKIE', 'csrf_token');
define('MPCT_CSRF_FIELD', 'csrf_token');
define('MPCT_CSRF_BYTES', 32);
   ```

   **Real Code Snippet (`/includes/csrf.php` — mint and 64-hex test, lines 36–44)**
   ```php
function csrfGenerateToken(): string
{
    return bin2hex(random_bytes(MPCT_CSRF_BYTES));
}

function csrfTokenLooksValid(string $token): bool
{
    return (bool) preg_match('/^[a-f0-9]{64}$/', $token);
}
   ```

* **Do not cache `/csrf-token.php`:** line 12 sends `Cache-Control: no-store`. A stored copy can hand one browser's token to another, or pair a new cookie with an old script body. Leave `no-store` in place. Do not add a long cache rule for this URL.
* **Do not log the token:** the cookie is HttpOnly, but the JavaScript global and the POST field are the same value. An `error_log`, an analytics hit, or a `console.log` left in the submit code keeps that secret until the browser closes. The reject path writes nothing on purpose. Compare the values in your own DevTools window, and do not paste them into a ticket or a log.
* **Keep `SameSite=Lax`:** `Lax` sends the cookie on same-site form posts and withholds it on cross-site posts. `None` also sends it on cross-site posts, and browsers drop a `None` cookie that is not `Secure`, which then fails every submit with the refresh message. `Strict` can withhold the cookie when the visit started on another site, so the first submit fails until the visitor loads the page again. Leave `Lax`.
* **Keep the name `csrf_token` in three places:** lines 17–18 in PHP, and the literal `csrf_token` in `/JS/csrf.js` (`querySelector`, `input.name`, and `formData.set`). The JavaScript does not read the PHP constants. Rename one side only, and every submit shows the refresh message with an empty log. Leave `hash_equals` as the comparison. Please do not remove `window.MPCT_CSRF_TOKEN` to "hide" the token. The field would stay empty and every submit would fail the same way.

---

## Let's Verify Your Changes

1. Open any of the four form pages on a local server from the site root. PHP's built-in server is enough for this check. It does not read `.htaccess`, and this check does not need Apache.
2. In DevTools, open Application → Cookies and select this host. Confirm `csrf_token` is present, HttpOnly is set, SameSite is `Lax`, and Path is `/`. On HTTP, Secure is off. On HTTPS, Secure is on.
3. In the console, confirm `window.MPCT_CSRF_TOKEN` is 64 lowercase hex characters. Please do not copy that value out of DevTools.
4. Submit the form once. The request leaves the browser only after Turnstile passes. See [Update Cloudflare Turnstile (CAPTCHA) Settings](02-update-turnstile-settings.md) if you are using local test keys. In Network, open the POST to the handler (`FormSubmission.php` on the contact form) and confirm the payload includes `csrf_token` with the same value as `window.MPCT_CSRF_TOKEN`.
5. In Application → Cookies, delete `csrf_token` only. Do not reload. Submit again. The feedback box shows `Unable to process your submission. Please refresh the page and try again.` The response is HTTP 200 and JSON with `success` false and that `message`. `error_log` does not gain a CSRF line.
6. Reload the page. The cookie comes back. Submit again. This check passes. When Turnstile and the rate limit also pass, the success message shows. A different message means a later check failed. Use [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md). Another submit with the same email can be stopped by the rate limit instead. See [Update Rate Limits](04-update-rate-limits.md).

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
