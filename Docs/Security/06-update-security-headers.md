
# Update Security Headers and Blocked Paths

**Summary:** Add, change, or remove the site's Apache security headers and blocked paths in `/.htaccess` and in a folder file such as `/data/.htaccess`.

**When to change:** When a new private file or folder must be hidden from the web, or an audit asks for different headers.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★</span>

**Estimated Time:** 10 minutes

---

## Visual Reference

**Real Code Snippet (`/.htaccess` — site-wide Apache hardening, lines 1–23)**

```apache
# Site-wide Apache hardening: block sensitive endpoints and set basic security headers.
# Uses Apache 2.4 Require directives (see data/.htaccess).

# Block direct access to PHPMailer OAuth helper (must not be web-accessible).
<Files "get_oauth_token.php">
    Require all denied
</Files>

<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteRule (^|/)get_oauth_token\.php$ - [F,L]
    # GEO item #4: robots.txt used to advertise sitemap_mpact.xml. Keep that
    # URL (and the conventional sitemap.xml) resolving to sitemap.php so
    # crawlers that already cached the old address still get the page list.
    RewriteRule ^sitemap(_mpact)?\.xml$ sitemap.php [L]
</IfModule>

# Basic security headers (HSTS is configured separately on production).
<IfModule mod_headers.c>
    Header always set X-Content-Type-Options "nosniff"
    Header always set X-Frame-Options "SAMEORIGIN"
    Header always set Referrer-Policy "strict-origin-when-cross-origin"
</IfModule>
```

*The Files block and the first RewriteRule block one PHP filename in any folder. The three Header lines set the response headers. The comment on line 18 says HSTS is configured outside this file. Leave the sitemap rewrite on line 15 unchanged.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/.htaccess` | Sets the three response headers, and blocks the filename `get_oauth_token.php` in any folder. |
| `/data/.htaccess` | Denies every URL under `/data/` with Apache 2.4 `Require all denied`. |

> [!WARNING]
> **One syntax error in `/.htaccess` returns HTTP 500 for the whole site.**
> A missing quote, an invalid `RewriteRule` pattern, or a directive Apache will not allow takes down every URL, not only the path you meant to hide. Fix the file before you do anything else.
> If `mod_headers` or `mod_rewrite` is off, Apache skips that module's `<IfModule>` block and shows no error. The headers, or the rewrite block, quietly disappear while the rest of the site still loads. Always check with `curl`.

> [!IMPORTANT]
> **Verify against your local codebase:** Open `/.htaccess` and `/data/.htaccess` and confirm the directives still match the snippets on this page before you edit.
> PHP's built-in server (`php -S`) ignores `.htaccess`. Headers and blocks from this page are not applied there, and a page that loads under `php -S` does not prove a block works. Test on Apache: production, staging, or a local Apache such as XAMPP.

> [!NOTE]
> **HSTS is set on production outside this repo.**
> The comment on line 18 says HSTS is configured separately on production. Do not add a `Strict-Transport-Security` line in `/.htaccess`. If an audit asks to change HSTS, that is a question for the nano.nau.edu server administrator.
> The rule `RewriteRule ^sitemap(_mpact)?\.xml$ sitemap.php [L]` on line 15 belongs to the sitemap. Leave it in place. To change that URL, follow [Add or Unlist a Page in the Sitemap](../SEO-GEO/02-update-sitemap.md). This page does not document that rule.

> [!TIP]
> **Hiding a whole folder instead of one filename?** Jump to [Part 3: Deny a whole folder](#part-3-deny-a-whole-folder).

---

## Step-by-Step Instructions

---

### Part 1: Read the three security headers

1. To get started, please open `/.htaccess` in your code editor.
2. Go to the bottom of the file. The three headers sit inside `<IfModule mod_headers.c>`:

   **Real Code Snippet (`/.htaccess` — response headers, lines 18–23)**

   ```apache
   # Basic security headers (HSTS is configured separately on production).
   <IfModule mod_headers.c>
       Header always set X-Content-Type-Options "nosniff"
       Header always set X-Frame-Options "SAMEORIGIN"
       Header always set Referrer-Policy "strict-origin-when-cross-origin"
   </IfModule>
   ```

3. Read what each line does before you change a value. These lines are not a login wall, and they do not check form posts. A visitor blocked on submit is covered in [Troubleshoot Blocked Form Submissions](05-troubleshoot-blocked-submissions.md).

| Header | Value | What it prevents |
|---|---|---|
| `X-Content-Type-Options` | `nosniff` | Tells the browser to trust the server's `Content-Type` instead of guessing (MIME sniffing). A response that is not labeled as HTML or a script is less likely to be treated as one. It does not stop a file that is already served as a script. `nosniff` is the only defined value. Any other value is ignored. |
| `X-Frame-Options` | `SAMEORIGIN` | Stops other websites from putting a nano.nau.edu page in a frame. Pages on this same site may still frame each other. That reduces clickjacking from other sites. An unknown value is ignored, so the protection drops while the page still loads. |
| `Referrer-Policy` | `strict-origin-when-cross-origin` | Limits the `Referer` header. A same-origin request sends the full URL. A cross-origin request sends only the origin (`https://nano.nau.edu`), not the path or query string. A request from HTTPS to HTTP sends no referrer. An unknown policy is ignored. The page still loads, and the browser uses its default. |

4. When an audit asks for a different value, change only the quoted string on that line. Keep the header name, the double quotes, and the words `Header always set`. The word `always` is why the header is still present on error responses, including the 403 from a blocked path. If you delete `always`, a normal page can still show the header while a 403 or a redirect drops it. That miss is silent unless you check both with `curl`.
5. Keep the `<IfModule mod_headers.c>` wrapper. It is what makes a missing `mod_headers` skip these lines instead of returning HTTP 500. The skip has no error on the page. The site loads without the three headers.
6. Do not add `Strict-Transport-Security` in this block. Production already sends HSTS from outside the repo. A second copy in this file is not how that policy is managed.
7. An unknown header value fails silently, as the table says. A missing quote does not: Apache returns HTTP 500 for the whole site.

   **Example Snippet (replace a header value only when an audit names a new one)**

   ```apache
   <IfModule mod_headers.c>
       # Replace [nosniff], brackets included. The only defined value today is nosniff.
       Header always set X-Content-Type-Options "[nosniff]"
       # Replace [SAMEORIGIN], brackets included. Keep SAMEORIGIN unless the audit names another value.
       Header always set X-Frame-Options "[SAMEORIGIN]"
       # Replace the bracketed policy, brackets included. Do not add Strict-Transport-Security here.
       Header always set Referrer-Policy "[strict-origin-when-cross-origin]"
   </IfModule>
   ```

   Replace each `[Placeholder]`, including the square brackets. Leaving the brackets in the file does not break the site. Browsers ignore the bad value, and the protection is gone.

---

### Part 2: Block one file

The file blocked today is `/PHPMailer/get_oauth_token.php`. It is PHPMailer's browser helper for requesting an OAuth token, and the page asks for a client id and secret. It must not be reachable on the web. You do not edit that PHP file for this task. You block its filename.

The root file blocks that name twice. Keep both layers on the same filename.

1. To get started, please open `/.htaccess` and find the filename block:

   **Real Code Snippet (`/.htaccess` — basename deny for get_oauth_token.php, lines 4–7)**

   ```apache
   # Block direct access to PHPMailer OAuth helper (must not be web-accessible).
   <Files "get_oauth_token.php">
       Require all denied
   </Files>
   ```

   In the root `/.htaccess`, `<Files "get_oauth_token.php">` matches that filename in any folder on the site, including `/PHPMailer/get_oauth_token.php`. The dot is literal here. It is not a regular expression. `Require all denied` is Apache 2.4 and returns 403. This block is outside `<IfModule>`, so it still runs when `mod_rewrite` is off.

2. Find the rewrite layer a few lines lower:

   **Real Code Snippet (`/.htaccess` — rewrite block, lines 9–16)**

   ```apache
   <IfModule mod_rewrite.c>
       RewriteEngine On
       RewriteRule (^|/)get_oauth_token\.php$ - [F,L]
       # GEO item #4: robots.txt used to advertise sitemap_mpact.xml. Keep that
       # URL (and the conventional sitemap.xml) resolving to sitemap.php so
       # crawlers that already cached the old address still get the page list.
       RewriteRule ^sitemap(_mpact)?\.xml$ sitemap.php [L]
   </IfModule>
   ```

   Lines 12–15 are the sitemap rewrite. Do not change those lines.
   In `/.htaccess`, Apache compares the pattern with the path under the site root, and that path has no leading slash. `PHPMailer/get_oauth_token.php` matches because of the slash before the filename. The same name in the site root matches because of the `^`. The `-` means "do not change the URL". `[F]` returns 403. Apache implies `[L]` for that flag, so later rewrite rules are not evaluated. This file still writes both flags. Copy both. A query string does not get past the rule.
   The check sits inside `<IfModule mod_rewrite.c>`. If `mod_rewrite` is off, Apache skips the whole block and does not say so. Only the `<Files>` block from step 1 still blocks the file. Do not put a leading slash on a new pattern. It will not match in this file. The URL stays public, with no error, unless the `<Files>` block names that same file.

3. To block another file, add a new `<Files>` block under the existing one, and add one new `RewriteRule` on the line after line 11, still inside the same `<IfModule mod_rewrite.c>`. Do not add a second `RewriteEngine On`. Use the same filename in both places. `<Files>` will match that name in every folder, so do not reuse it for a public page.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `<Files>` filename | Basename to block in every folder. The dot is literal. A wrong name fails open: the file stays downloadable, and the browser shows no Apache error. | `<Files "[NewHandler].php">` |
| ② | `RewriteRule` on the next line after line 11 | The same basename as ①, inside the existing `mod_rewrite` block. A pattern that matches nothing fails open with no error. An invalid pattern returns HTTP 500 for the whole site. | See the example below. Do not put a leading slash on the pattern. |

   **Example Snippet (second blocked filename)**

   ```apache
   # Block direct access to [NewHandler].php (must not be web-accessible).
   <Files "[NewHandler].php">
       Require all denied
   </Files>
   ```

   ```apache
   # Paste inside the existing <IfModule mod_rewrite.c>, under the get_oauth_token rule.
   # Same [NewHandler] as the Files block. Escape the dot. No leading slash.
   # [F] returns 403. Apache implies [L]. Keep both flags, as the existing rule does.
   RewriteRule (^|/)[NewHandler]\.php$ - [F,L]
   ```

   Replace `[NewHandler]` in both fences, including the brackets. If the filename contains a regular-expression character such as `+` or `?`, stop and ask for help before saving. A broken pattern returns HTTP 500 for the whole site. A pattern that simply fails to match leaves the file on the web.

4. `robots.txt` is not protection. `/robots.txt` already contains `Disallow: /PHPMailer/`, which only asks polite crawlers to skip that folder. Anyone can still request the URL, and a `Disallow` line advertises the path. Hide the file with the pair above. To edit crawler rules, follow [Update robots.txt and AI Crawler Access](../SEO-GEO/01-update-robots-and-ai-crawlers.md).
5. To make a blocked file public again, remove both its `<Files>` block and its `RewriteRule`. Removing only one leaves the other in place, so the URL can stay 403. If the layer you leave is only the rewrite rule, and `mod_rewrite` is off, the file becomes public with no error. After either edit, check with `curl`. `403` means a layer is still blocking. `200` means the file is on the web.

---

### Part 3: Deny a whole folder

Use this when the whole folder is private. `/data/` is the folder that already works this way. It holds rate-limit storage and other runtime data. Changing how many submissions are allowed is a different task: [Update Rate Limits](04-update-rate-limits.md). This part only stops Apache from serving the folder.

1. To get started, please open `/data/.htaccess`:

   **Real Code Snippet (`/data/.htaccess` — deny the data folder, lines 1–2)**

   ```apache
   # Deny direct HTTP access to rate-limit storage and other runtime data.
   Require all denied
   ```

2. A `.htaccess` file applies to the folder it sits in and to every path under that folder. `Require all denied` is Apache 2.4. It returns 403 for those URLs. Line 2 of `/.htaccess` points at this file. There is no allow-list under the `Require` line, so a public page placed in `/data/` is blocked too. To hide one filename and leave the rest of a folder public, use Part 2 instead.
3. Do not replace this line with `Deny from all` or `Order deny,allow`. Those are the older Apache 2.2 forms. This site's root file says to use Apache 2.4 `Require` directives. The old form can return HTTP 500 where it is not accepted, or fail to block.
4. To deny a new folder, create `[PrivateFolder]` under the site root and put a `.htaccess` in that folder (not a new rule in the root file):

   **Example Snippet (deny a new folder)**

   ```apache
   # Deny direct HTTP access to [PrivateFolder].
   Require all denied
   ```

   Replace `[PrivateFolder]` in the comment, including the brackets. Leave the second line exactly `Require all denied`.
5. If the new file does not take effect, the server may not allow overrides for that directory. `curl` then returns the file, often with HTTP 200, and the page shows no error. The folder is public. That is a question for the nano.nau.edu server administrator. Ask whether `AllowOverride` for that directory includes `AuthConfig`, which is the class `Require` belongs to. Do not edit the server configuration yourself.
6. A directive the server reads but does not allow returns HTTP 500 for URLs in that folder, not a quiet skip. A syntax error in `/data/.htaccess` does the same, and only for URLs under `/data/`. The rest of the site can still load. A syntax error in the root `/.htaccess` is the one that returns HTTP 500 for every URL.
7. To open the folder again, remove its `.htaccess` (or the `Require` line). The folder becomes requestable on the next request. There is no Apache restart. If you do not run `curl`, that change is easy to miss.

---

## Technical best practices and validation

* **Apache only:** `php -S` never reads `.htaccess`. A successful local browse on that server does not mean the header or the block is in place. Use Apache, then `curl`.
* **Deploy before you curl production:** `https://nano.nau.edu/` serves the deployed files, not unsaved editor buffers. `.htaccess` is picked up on the next request. You do not restart Apache for a change in these files.
* **Loud vs silent:** A syntax error in `/.htaccess` is loud (HTTP 500 for the whole site). A skipped `<IfModule>` block, a header value the browser ignores, a rewrite pattern that matches nothing, and a folder `.htaccess` the server never reads are silent. The site still looks fine. `curl` is the check.
* **Keep the pair together:** The `<Files>` name and the `RewriteRule` must name the same file. Updating only the rewrite line leaves the new file public when `mod_rewrite` is off. Updating only `<Files>` leaves the old rewrite rule in place, so the old name can stay 403.
* **`<Files>` is the whole site:** From the root file it matches that basename in every folder. Blocking `index.html`, or any name a public page uses, hides every copy of that name.
* **Do not add HSTS here:** Production sends `Strict-Transport-Security` from outside the repo. You will see it in `curl` output. It is not a fourth header to copy into `/.htaccess`.
* **Leave line 15 alone:** The sitemap rewrite stays in this file so old sitemap URLs keep resolving. Change it only from [Add or Unlist a Page in the Sitemap](../SEO-GEO/02-update-sitemap.md).
* **`robots.txt` does not enforce a block:** A `Disallow` line is a request to crawlers, and it publishes the path. The blocks on this page are what return 403. See [Update robots.txt and AI Crawler Access](../SEO-GEO/01-update-robots-and-ai-crawlers.md).
* **New folder still 200:** Ask the nano.nau.edu server administrator about overrides. Do not "fix" it by weakening the root file.

---

## Let's Verify Your Changes

Run these checks on Apache only, after the edited files are on that server. `php -S` cannot confirm any step on this page. On Windows PowerShell, `curl` is an alias for a different command. Type `curl.exe`. On macOS or Linux, the same commands work with `curl` instead of `curl.exe`.

1. Check the three headers on the homepage:

   ```powershell
   curl.exe -I https://nano.nau.edu/
   ```

   You want status 200, and these three lines (other headers will be there too; the order can vary):

   ```http
   X-Content-Type-Options: nosniff
   X-Frame-Options: SAMEORIGIN
   Referrer-Policy: strict-origin-when-cross-origin
   ```

   You may also see `Strict-Transport-Security`. That is HSTS, set on the server outside this repo. Do not add it to `/.htaccess` to "match" production. If the status is `500`, the root `/.htaccess` has a syntax or override error and the whole site is affected. Restore or fix that file, then run this command again. If the status is `200` but one of the three lines is missing, the header line was dropped or `mod_headers` skipped its block. That failure is silent in the browser.

2. Check the blocked PHP file:

   ```powershell
   curl.exe -I https://nano.nau.edu/PHPMailer/get_oauth_token.php
   ```

   You want status 403. Use `-I` so you are reading status and headers, not the script. A `200` means the helper is reachable and the block failed open. The 403 response should still include the three headers from step 1, because the root file uses `Header always set`. If the homepage has them and this 403 does not, `always` was removed.

3. Check the denied folder, then one path under it:

   ```powershell
   curl.exe -I https://nano.nau.edu/data/
   curl.exe -I https://nano.nau.edu/data/rate-limits/
   ```

   Both should be status 403. A `200` means `/data/.htaccess` is not in effect and the server may not be allowing overrides. Ask the nano.nau.edu server administrator. Do not treat a `robots.txt` rule as a substitute.

4. On a local Apache (staging or XAMPP), run the same three commands against that server's base URL before you deploy. Pointing them at `https://nano.nau.edu/` before you deploy still shows the old files.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
