
# Update robots.txt and AI Crawler Access

**Summary:** Add, move, or remove `User-agent`, `Disallow`, `Allow`, and `Sitemap` lines in `/robots.txt` so named search and AI crawlers keep sharing one rule set.

**When to change:** When the lab decides to allow or block a search or AI crawler, a new private file should stay out of crawl lists, or the sitemap URL changes.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★</span>

**Estimated Time:** 10 minutes

---

## Visual Reference

**Real Code Snippet (`/robots.txt` — shared User-agent group, lines 7–37)**

```text
# OpenAI — ChatGPT search, live fetch, and model training
User-agent: OAI-SearchBot
User-agent: ChatGPT-User
User-agent: GPTBot

# Anthropic — Claude search, live fetch, and model training
User-agent: Claude-SearchBot
User-agent: Claude-User
User-agent: ClaudeBot

# Perplexity — search index and live fetch
User-agent: PerplexityBot
User-agent: Perplexity-User

# Google Gemini grounding / training (does not affect Search or AI Overviews)
User-agent: Google-Extended

# Apple search and Apple Intelligence training
User-agent: Applebot
User-agent: Applebot-Extended

# Microsoft Bing / Copilot
User-agent: bingbot

# Meta, Amazon, and Common Crawl training
User-agent: meta-externalagent
User-agent: Amazonbot
User-agent: CCBot

# Everyone else
User-agent: *
```

*All 16 `User-agent` lines share the one rule set that follows them (lines 39–69): the `Disallow` list, the `Allow` list and `Sitemap: https://nano.nau.edu/sitemap.php`.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/robots.txt` | The only file this task edits. It names the crawlers, lists paths they should skip or keep, and advertises the sitemap. |

> [!NOTE]
> **One shared rule set:** Consecutive `User-agent` lines form one group, and a crawler obeys only the group that names it most specifically ([RFC 9309](https://www.rfc-editor.org/rfc/rfc9309)). That is why the 15 named agents sit in the same group as `User-agent: *`. The file's own header comment (lines 1–5) says this: a named group replaces `*` for that agent, so those agents share this one rule set rather than inheriting it. A blank line or a `#` comment does not start a new group. The first `Allow` or `Disallow` ends the agent list and begins that group's rules. The next `User-agent` line after a rule starts another group. If you leave the same token in two groups, RFC 9309 combines those rules instead of picking one, so a block you thought was separate can stay open. That miss is silent.

> [!WARNING]
> **robots.txt is public, and crawlers may ignore it.** A `Disallow` line does not stop a request. Crawlers that ignore the file still fetch the path, and the site returns it with HTTP 200, so the miss is silent. The line also advertises the path, because anyone can open `https://nano.nau.edu/robots.txt`. To refuse a request, block the path in `/.htaccess` as described in [Update Security Headers and Blocked Paths](../Security/06-update-security-headers.md). PHP's built-in server (`php -S`) does not read `.htaccess`, so a blocked path can be confirmed only on Apache (production, staging, or a local Apache such as XAMPP).

> [!WARNING]
> **Three endpoints are missing from the Disallow list.** Today `/robots.txt` does not disallow `/IntelScholarshipSubmission.php`, `/csrf-token.php`, or `/turnstile-config.php`. Keep documenting the file as it is; adding those lines is a later edit, shown in [Part 4](#part-4-hide-a-private-file-from-crawlers), not a change already present in the file. Crawlers that follow the shared group may fetch all three, and nothing in the file reports that gap. Do not close the gap by denying those URLs in `/.htaccess`. The scholarship form on `/CHIPS_Scholars_Program.html` posts to `/IntelScholarshipSubmission.php`, and every form page loads `/csrf-token.php` and `/turnstile-config.php`. An Apache deny breaks those requests. The visitor sees a failed security check or a failed submit, and `robots.txt` will not say why.

> [!IMPORTANT]
> **Verify against your local codebase:** Before you edit, confirm `/robots.txt` still has the 16 `User-agent` lines (lines 7–37), the `Disallow` and `Allow` lines (lines 39–67), and `Sitemap: https://nano.nau.edu/sitemap.php` on line 69. A mistyped product token matches no group. That crawler then follows `User-agent: *` with no error on the site.

> [!TIP]
> **Jump to the change you need:** [Allow a crawler](#part-2-allow-a-new-crawler), [block a crawler](#part-3-block-a-crawler), [hide a private file](#part-4-hide-a-private-file-from-crawlers), or [keep the sitemap line last](#part-5-keep-the-sitemap-line-last).

---

## Step-by-Step Instructions

Titles, descriptions, and social tags are a different edit. Those live in each HTML file, covered in [Update Site-Wide Metadata or SEO](../General/update-site-wide-metadata.md). This page changes only `/robots.txt`, in the site root.

---

### Part 1: Read the File Layout

The file is one shared group (the agent lines), then a `Disallow` list, an `Allow` list, and one `Sitemap` line. Please keep that order. A rule placed between agent lines splits the group, and the crawlers above that rule silently lose the shared `Disallow` list.

1. To get started, please open `/robots.txt` in your code editor. Leave the filename in lowercase at the site root. A different name is a different URL. If the live URL returns HTTP 404, crawlers may treat the file as missing and crawl every path. That failure is silent: the public pages still load.
2. Read the header comment before you move any line. It is the reason the named agents are not in groups of their own.

   **Real Code Snippet (`/robots.txt` — header comment, lines 1–5)**
   ```text
   # robots.txt for https://nano.nau.edu
   #
   # AI agents are named so search, live fetch, and training are allowed on
   # purpose. A named group replaces User-agent: * for that agent, so they
   # share this one rule set rather than inheriting it.
   ```

3. The Visual Reference is the whole agent group: 15 named product tokens plus `User-agent: *` on line 37. Comments and blank lines between those tokens do not end the group. The blank line on line 38 does not end it either. The rules that start on line 39 still belong to all 16 tokens.
4. The rule set below that blank line is the `Disallow` list, the `Allow` list, and the sitemap line. Please leave existing `Disallow` lines in place unless you mean for that path to be crawled. Deleting one does not warn you; a well-behaved crawler simply starts requesting that path.

   **Real Code Snippet (`/robots.txt` — Disallow list, Allow list, and Sitemap line, lines 39–69)**
   ```text
   # Block PHP form processors — no indexable content
   Disallow: /FormSubmission.php
   Disallow: /EquipmentReservation.php
   Disallow: /ServiceRequestSubmission.php
   
   # Block third-party library source
   Disallow: /PHPMailer/
   
   # Block version control directory
   Disallow: /.git/
   
   # Block internal developer documentation
   Disallow: /README.md
   Disallow: /CSS_GUIDE.md
   Disallow: /JS_GUIDE.md
   
   # Block internal data files
   Disallow: /equipment.json
   Disallow: /rates.json
   
   # Allow all public content, including assets needed for rendering
   Allow: /*.html
   Allow: /CSS/
   Allow: /JS/
   Allow: /Images/
   Allow: /About_Equipment/
   Allow: /Manuals_and_Specs/
   Allow: /knowledge-base/
   Allow: /llms.txt
   
   Sitemap: https://nano.nau.edu/sitemap.php
   ```

5. Match the edit you came to make to one row below. Parts 2–5 each change one row.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `User-agent` | Product token for one crawler. Consecutive lines share every rule that follows them. `*` is only the fallback for a token this file does not name. | `User-agent: [CrawlerName]` inside the shared group, or alone above it with `Disallow: /` |
| ② | `Disallow` | Path prefix a well-behaved crawler should skip. It does not block a browser, and it publishes the path. | `Disallow: /[NewHandler].php` |
| ③ | `Allow` | Path prefix that stays crawlable. A longer `Allow` overrides a shorter `Disallow`. In a path, `*` means any characters, which is why `/*.html` is not a literal asterisk. | `Allow: /llms.txt` |
| ④ | `Sitemap` | Absolute sitemap URL. It is not a crawl rule, and it does not end the group. Keep this file's one Sitemap line as the last line. | `Sitemap: https://nano.nau.edu/sitemap.php` |

---

### Part 2: Allow a New Crawler

Allowing a crawler means adding its `User-agent` line to the shared group, above any `Allow` or `Disallow`. A line placed after the rules, with nothing under it, starts a new group at the end of the file. That group allows every path for that token, including the form handlers, and it does not inherit the shared `Disallow` list. The site still looks normal.

1. Copy the product token from the vendor's robots.txt documentation. Use the token only, not the full HTTP `User-Agent` string. Matching is case-insensitive, but please copy the token as published so the next diff stays obvious. A token the vendor does not use matches nobody, so that bot follows `User-agent: *` and you get no error. That is harmless for an allow, because `*` is this same group, and it is a silent failure for a block (Part 3).
2. Above the `# Everyone else` comment (line 36), add a comment and one `User-agent` line. The comment should name the company and what the bot does: search, live fetch, or training. Use the same shape as the OpenAI, Anthropic, and Perplexity comments in the Visual Reference. Do not put `Allow` or `Disallow` under the new line. A rule there ends the group. Every token above that rule stops seeing the shared `Disallow` list and may fetch the form processors. Nothing on the site reports the split.

   **Example Snippet (new crawler inside the shared group)**
   ```text
   # [Company] — [search, live fetch, or training]  # company, and what this bot does
   User-agent: [CrawlerName]  # vendor product token; leave all rules below User-agent: *
   ```

3. Leave `User-agent: *` as the last agent line in the group (it is line 37 today) so unnamed crawlers still receive the same rules.

---

### Part 3: Block a Crawler

Leaving a crawler in the shared group keeps it allowed, except for the shared `Disallow` paths. Blocking it means moving its line into a group that contains only `Disallow: /`.

1. Find the token in the shared group and cut it. Do not copy it. If the comment above it names only that crawler, move the comment too. If the comment covers several tokens (the OpenAI, Anthropic, and Perplexity blocks), leave the comment and move only the one line. A second copy left in the shared group is combined with the block group under RFC 9309. The longer `Allow` lines (lines 60–67) then reopen `/*.html`, `/CSS/`, `/JS/`, `/Images/`, `/About_Equipment/`, `/Manuals_and_Specs/`, `/knowledge-base/`, and `/llms.txt`. The file looks blocked, and those URLs stay crawlable. That failure is silent.
2. Paste the token into its own group after the header comment (after line 5) and before line 7. The only rule in that group is `Disallow: /`. Paths are prefixes, so `/` matches every path. `/robots.txt` itself stays implicitly allowed.

   **Example Snippet (crawler moved into its own block group)**
   ```text
   # [Company] — blocked ([search, live fetch, or training])  # place this group after line 5, before the shared group
   User-agent: [CrawlerName]  # remove this same line from the shared group
   Disallow: /  # only rule for this token; do not add the shared Allow lines here
   ```

3. Please treat `Google-Extended` as a special case of the same move, not as a different kind of file edit. It is a control token that Google's crawlers read. It is not a separate bot: Google's crawler list says it has no HTTP user-agent string of its own, and crawling is still done with Google's existing user agents. Blocking it does not affect Google Search (inclusion or ranking). The comment on line 21 says the same for Search and for AI Overviews.

   **Real Code Snippet (`/robots.txt` — Google-Extended comment and token, lines 21–22)**
   ```text
   # Google Gemini grounding / training (does not affect Search or AI Overviews)
   User-agent: Google-Extended
   ```

   `Googlebot` is not named in this file, so Google Search follows `User-agent: *`. Rules for `Googlebot` cover Google Search, including its search features. Please do not add `User-agent: Googlebot` with `Disallow: /` unless the lab means to stop Google Search from crawling the site. Visitors would still see every page. Search would stop crawling, with no error in the browser. Vendor detail: [Google's common crawlers](https://developers.google.com/crawling/docs/crawlers-fetchers/google-common-crawlers).
4. To allow that crawler again, delete its separate group and put its `User-agent` line back in the shared group, using Part 2. A `User-agent` line with no rules allows every path for that token. That is not a block, and the site will not tell you the difference.

---

### Part 4: Hide a Private File from Crawlers

Use this when a new form handler, or one of the three endpoints named in the warning above, should be left off crawl lists. The handler itself is wired in [Add Security Checks to a New Form](../Security/01-add-security-checks-to-a-form.md). This part only adds the `Disallow` line.

1. Add the line in the shared group's `Disallow` list, with the other form processors. Do not add it after the `Sitemap` line, and do not add it between `User-agent` lines.

   **Real Code Snippet (`/robots.txt` — form-processor Disallow lines, lines 39–42)**
   ```text
   # Block PHP form processors — no indexable content
   Disallow: /FormSubmission.php
   Disallow: /EquipmentReservation.php
   Disallow: /ServiceRequestSubmission.php
   ```

2. Insert new `Disallow` lines in that block. A new handler uses your real filename. The three lines under it are optional and are not in the file today: `/IntelScholarshipSubmission.php` (the scholarship form handler), `/csrf-token.php` (the CSRF token script), and `/turnstile-config.php` (the public Turnstile site-key script). `Disallow` only asks a well-behaved crawler to skip them. Browsers can still load them, which the forms need. Denying any of the three in `/.htaccess` breaks submission or the security scripts, and the failure shows up in the browser, not in this file.

   **Example Snippet (Disallow lines for a new handler and the three current gaps)**
   ```text
   Disallow: /[NewHandler].php  # new form handler; keep it with the other processors
   Disallow: /IntelScholarshipSubmission.php  # not listed today; crawl hint only
   Disallow: /csrf-token.php  # not listed today; do not deny this URL in Apache
   Disallow: /turnstile-config.php  # not listed today; do not deny this URL in Apache
   ```

3. Copy the path exactly, including capitals. Path matching is case-sensitive, so `Disallow: /formsubmission.php` does not match `/FormSubmission.php`, and the crawler may fetch the real file with no warning. A `Disallow` path is a prefix: `/[NewHandler].php` also matches a longer path that starts with that text. Directory rules in this file keep the trailing slash, as in `Disallow: /PHPMailer/`. Dropping the slash also matches a different file that merely begins with the same letters, such as a future page you did not mean to hide.
4. A private file that must not be downloaded at all still needs `/.htaccess`, because this file only asks. Follow [Update Security Headers and Blocked Paths](../Security/06-update-security-headers.md) for that block. Remember that listing the path here tells the world the name.

---

### Part 5: Keep the Sitemap Line Last

The page list itself is built by `/sitemap.php`. Change that list in [Add or Unlist a Page in the Sitemap](02-update-sitemap.md). This part only protects the one line that points crawlers at it.

1. Leave this block at the bottom of the file. Line 69 must stay the last line.

   **Real Code Snippet (`/robots.txt` — knowledge-base and llms.txt Allow lines, then Sitemap, lines 66–69)**
   ```text
   Allow: /knowledge-base/
   Allow: /llms.txt
   
   Sitemap: https://nano.nau.edu/sitemap.php
   ```

2. Please do not add rules below `Sitemap`. A Sitemap line does not end the group, so a rule under it still applies, but the next editor will look for rules up with lines 40–67 and can miss the stray one. The missed rule fails silently until somebody compares the live file with this guide.
3. If the sitemap address changes, change line 69 to the same absolute URL and keep it last. A stale line does not redirect crawlers to the new address. They keep requesting the old URL, and the site does not report the mismatch. Do not point this line at `sitemap.xml` or `sitemap_mpact.xml`. On Apache, `/.htaccess` rewrites those names to `sitemap.php`. `php -S` does not read `.htaccess`, so those two names 404 on the built-in server even when `sitemap.php` is fine. The line in this file is the PHP URL on purpose.
4. `Allow: /llms.txt` (line 67) keeps the root index crawlable when a shorter `Disallow` would otherwise cover it, because the longer match wins. `/knowledge-base/llms.txt` is already covered by `Allow: /knowledge-base/` (line 66). When either copy of the index changes, follow [Update llms.txt](05-update-llms-txt.md). This file does not store the index text. It only keeps those two paths allowed.

---

## Technical best practices and validation

* **Do not split the agent list.** The group runs from the first `User-agent` line until the first `Allow` or `Disallow`. A rule inserted between tokens ends the group early. Tokens above that rule never see the form-processor `Disallow` lines. The public site does not change, so the mistake is silent.

   **Example Snippet (a rule that silently splits the group)**
   ```text
   User-agent: [CrawlerName]
   Disallow: /private.html  # ends the group; this crawler never sees the shared Disallow list
   User-agent: *  # a new group starts here, not a continuation
   ```

* **Move a block. Do not copy the token.** One group per token is unambiguous. Two groups that name the same token have their rules combined (RFC 9309). Combined with `Disallow: /`, the longer `Allow` lines reopen the public paths.
* **Never put `Disallow: /` on the shared group.** That one line asks all 15 named agents, and every unnamed crawler using `*`, to skip the whole site. `Googlebot` is not named, so Google Search uses `*`. Visitors still see the site. Crawling stops, with no on-page error.
* **Longest path wins.** When an `Allow` and a `Disallow` both match, the longer path is the one that counts. If they are the same length, `Allow` should win. In a path, `*` means any run of characters. Please do not "fix" `Allow: /*.html` into `Allow: /.html`.
* **Paths are case-sensitive prefixes.** `User-agent` matching is case-insensitive. Path matching is not. Directory rules keep the trailing slash already used in the file.
* **The file is a request, and it is cached.** It is not access control. Crawlers may reuse a copy for up to 24 hours, and longer if the URL becomes unreachable. HTTP 404 on `/robots.txt` lets crawlers fetch every path. HTTP 500 (and other unreachable results) makes them assume a full disallow. Either failure is invisible in a browser tab pointed at an HTML page. After deploy, confirm HTTP 200.
* **Disallow does not remove a URL from an index.** A crawler that already listed a URL can keep showing the address without a snippet. This file cannot delete that listing. To stop a download, use `/.htaccess` (see the warning above). To stop indexing of a page that remains crawlable, add `noindex` as described in [Add or Unlist a Page in the Sitemap](02-update-sitemap.md).
* **Keep the rendering assets allowed.** Do not add `Disallow` lines for `/CSS/`, `/JS/`, `/Images/`, or `/*.html`. The `Allow` list exists so a later, broader `Disallow` does not strip the files a crawler needs to render a page. Blocking them does not break the live site for visitors. Search can store a page that looks empty.
* **One Sitemap line, and it stays last.** Keep `Sitemap: https://nano.nau.edu/sitemap.php`. This line only publishes the URL. It does not build the page list.

---

## Let's Verify Your Changes

1. In the editor, search for any token you added or moved. It should appear once. If you blocked a crawler, that token's only rule is `Disallow: /`, and it is no longer in the shared group. Every other token still shares the original `Disallow` and `Allow` lists. The last line of the file should still be `Sitemap: https://nano.nau.edu/sitemap.php`.
2. From the website root (the folder that contains `robots.txt`), start PHP's built-in server, then request the file. `php -S` serves `robots.txt` as a static file. It does not read `.htaccess`, so this check does not prove that a private path is denied. Stop the server with Ctrl+C when you are done. If port 8080 is taken, pick another free port and use it in both commands.

   ```powershell
   php -S 127.0.0.1:8080
   ```

   ```powershell
   curl.exe -sS -i http://127.0.0.1:8080/robots.txt
   ```

   In PowerShell, `curl` is an alias for `Invoke-WebRequest`. Please use `curl.exe` so you see the raw status line and body. Expect HTTP 200 and a body that matches the file you edited, including your new line and the Sitemap line. A 404 means the server was started in the wrong folder. Crawlers treat a missing file as "allow all," so do not read a 404 as a successful block.
3. After the file is deployed, run the same request against the public site. Expect HTTP 200 and the same body. A change can take up to a day to replace a crawler's cached copy.

   ```powershell
   curl.exe -sS -i https://nano.nau.edu/robots.txt
   ```

4. In Google Search Console, open **Settings** and then the **robots.txt** report. Confirm Google fetched `https://nano.nau.edu/robots.txt` without an error. The older robots.txt Tester, which accepted any URL in a box of its own, has been removed. To test one URL, use the inspection bar at the top, enter the full URL, and run **Test live URL**. Read **Crawl allowed?**. `https://nano.nau.edu/FormSubmission.php` should not be allowed to be crawled. `https://nano.nau.edu/index.html` should be allowed. If you just deployed a block, the report can still show the previous file until Google refetches it. Use the report's recrawl action for `robots.txt`, then test the URL again. The report is described in [Google's robots.txt report help](https://support.google.com/webmasters/answer/6062598).
5. Confirm the sitemap URL loads. Expect HTTP 200 and a body that starts with `<?xml`. `/sitemap.php` sends `Content-Type: application/xml; charset=UTF-8`. An HTML error page means the generator failed, even if `robots.txt` is fine.

   ```powershell
   curl.exe -sS -i https://nano.nau.edu/sitemap.php
   ```

   Locally, the same `php -S` window will run `sitemap.php`. A 404 for `http://127.0.0.1:8080/sitemap.xml` on that server is expected: the rewrite from `sitemap.xml` and `sitemap_mpact.xml` lives in `/.htaccess`, which `php -S` does not read. Check `sitemap.php` itself, or check the rewritten names on Apache.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
