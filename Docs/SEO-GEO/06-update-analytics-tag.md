
# Update the Google Tag Manager Tag

**Summary:** Change the Google Tag Manager container ID in `/JS/gtm.js` and the two live `<noscript>` iframes, or add that same loader as the first tag in a new page's `<head>`.

**When to change:** When the lab's Google Tag Manager container changes, or a new HTML page is added without the analytics tag.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 5 minutes

---

## Visual Reference

**Real Code Snippet (`/JS/gtm.js` — shared Google Tag Manager loader, lines 1–7)**

```javascript
 //Google Tag Manager
(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-5PQKN9FD');
//<!-- End Google Tag Manager -->
```

*Line 1 begins with a space. That space does not affect the tag. The container ID is the last argument on line 6.*

**Real Code Snippet (`/About_Equipment/SEM.html` — the first tag in `<head>`, around line 4)**

   ```html
       <script src='../JS/gtm.js'></script>
   ```

*This is the first tag in `<head>` on an equipment page. The path is `../JS/gtm.js`. Root pages use `JS/gtm.js` in that same spot.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/JS/gtm.js` | Shared loader. The container ID on line 6 covers every page that includes this file. |
| `/index.html` | Loads `JS/gtm.js` on line 5. Holds a commented-out inline copy (lines 10–16) and the live `<noscript>` iframe (lines 54–55). |
| `/MPaCT.html` | Same pattern: loader on line 5, commented inline copy on lines 10–16, live `<noscript>` iframe on lines 52–53. |
| `/*.html` | All 23 root HTML pages, including the two files above. Each loads `JS/gtm.js` as the first tag in `<head>`. Only `/index.html` and `/MPaCT.html` also have the comment and the iframe. |
| `/About_Equipment/*.html` | All 42 equipment pages. Each loads `../JS/gtm.js` as the first tag in `<head>`. None of them has a `<noscript>` iframe. |

> [!WARNING]
> **Leave the old inline copy commented out!**
> `/index.html` and `/MPaCT.html` still contain a full copy of the loader inside an HTML comment (lines 11–15). If you remove that comment, or paste the inline snippet onto a page that already loads `/JS/gtm.js`, the container loads twice. Page views are counted twice, and the page looks normal. Nothing on the screen reports the extra hit.

> [!IMPORTANT]
> **Verify against your local codebase:** Before you edit, search for `GTM-5PQKN9FD`. On this branch the live ID is `/JS/gtm.js` line 6, `/index.html` line 54, and `/MPaCT.html` line 52. The same ID also appears inside the comments on line 15 of `/index.html` and `/MPaCT.html`. Re-count pages that reference `gtm.js` if files have been added since.

> [!NOTE]
> **One file, loaded first in `<head>` on the public pages.**
> 65 HTML pages load this script: 23 in the site root (`src='JS/gtm.js'`) and 42 in `/About_Equipment/` (`src='../JS/gtm.js'`). Whitespace may sit above it, but it is the first tag inside `<head>` on each of them. `/includes/header.html` and `/includes/footer.html` do not load it. `/JS/layout.js` injects those fragments with `innerHTML` (around line 20), and a `<script>` inserted that way does not run. Put the tag in the page `<head>`, not in the include. The 54 HTML files under `/knowledge-base/` are a separate MkDocs build and do not load it either.

> [!TIP]
> **Changing a container is Part 1. Adding a page is Part 2.**
> Jump to [Part 2: Add the Tag to a New Page](#part-2-add-the-tag-to-a-new-page). The rest of a new page is covered in [Add a New Page](../General/add-a-new-page.md).

---

## Step-by-Step Instructions

### Part 1: Change the Container ID

1. To get started, please open `/JS/gtm.js` in your code editor.
2. Change only the container ID on line 6. On that same line, `'script'` is the tag name the loader creates, and `'dataLayer'` is the array it uses on `window`. If you rename `dataLayer`, `window.dataLayer` never appears and every tag that reads it stops, with no error on the page.

   **Real Code Snippet (`/JS/gtm.js` — container ID argument, around line 6)**
   ```javascript
   })(window,document,'script','dataLayer','GTM-5PQKN9FD');
   ```

3. Search the website repo for `GTM-5PQKN9FD`. You should find five lines. Three are live. Two are inside comments and must stay inside comments:
   * `/JS/gtm.js` line 6, live. Change this ID.
   * `/index.html` line 54 and `/MPaCT.html` line 52, live iframes. Change these to the same ID.
   * `/index.html` line 15 and `/MPaCT.html` line 15, inside the comment. You may update the ID characters so a later search does not look like a missed tag. Do not remove `<!--` or `-->`. Uncommenting this block double-counts page views, silently, even when the ID matches `/JS/gtm.js`.

4. Please open `/index.html` and read lines 10–16 before you change anything. The loader on lines 11–15 is not running. `/MPaCT.html` lines 10–16 are the same text, including the container ID on line 15.

   **Real Code Snippet (`/index.html` — commented-out inline loader, lines 10–16)**
   ```html
       <!-- Google Tag Manager -->
       <!--<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
       new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
       j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
       'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
       })(window,document,'script','dataLayer','GTM-5PQKN9FD');</script>-->
       <!-- End Google Tag Manager -->
   ```

5. The `<noscript>` iframe directly under `<body>` is live. The comment lines above and below it are labels only. They do not disable the iframe. Update the `id` so it matches `/JS/gtm.js`. If you skip the iframe, visitors with JavaScript use the new container and visitors with JavaScript off still hit the old one. The page does not show an error, so the split is silent. Only these two pages have this iframe.

   **Real Code Snippet (`/index.html` — live no-JavaScript iframe, lines 53–56)**
   ```html
       <!-- Google Tag Manager (noscript) -->
       <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-5PQKN9FD"
       height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
       <!-- End Google Tag Manager (noscript) -->
   ```

   **Real Code Snippet (`/MPaCT.html` — live no-JavaScript iframe, lines 51–54)**
   ```html
       <!-- Google Tag Manager (noscript) -->
       <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-5PQKN9FD"
       height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
       <!-- End Google Tag Manager (noscript) -->
   ```

   The iframe markup matches. The line numbers do not: lines 54–55 in `/index.html`, and lines 52–53 in `/MPaCT.html`.

6. Paste the new ID using the guide below. Comments mark the lines you change.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `/JS/gtm.js` line 6 | Live ID for all 65 pages. Change only the quoted ID. | `'GTM-[CONTAINER_ID]'` |
| ② | `/index.html` line 54 and `/MPaCT.html` line 52 | Live `id` on the `<noscript>` iframe. Must match ①. A mismatch sends no-JavaScript hits to the old container, silently. | `id=GTM-[CONTAINER_ID]` |
| ③ | `/index.html` line 15 and `/MPaCT.html` line 15 | The same characters inside a comment. Optional to edit. The comment markers are not optional. | Leave `<!--` and `-->` in place. |

   **Example Snippet (new container ID in the loader)**
   ```javascript
   })(window,document,'script','dataLayer','GTM-[CONTAINER_ID]'); // ① Change only this ID.
   ```

   **Example Snippet (new container ID in the iframe)**
   ```html
   <noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-[CONTAINER_ID]" height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript> <!-- ② Same ID as /JS/gtm.js. Keep this element live. -->
   ```

---

### Part 2: Add the Tag to a New Page

A new HTML file does not inherit `/JS/gtm.js`. The header include does not load it. Add the tag while you build the page. [Add a New Page](../General/add-a-new-page.md) covers the rest of the file. Titles and descriptions are in [Update Site-Wide Metadata or SEO](../General/update-site-wide-metadata.md). Head examples on that page do not include this tag. Keep the `gtm.js` line when you copy a head from there. Dropping it does not change how the page looks. Analytics simply never starts.

1. To get started, please duplicate a page that already has the tag. Every root HTML page and every page in `/About_Equipment/` has it. After you rename the copy, confirm the tag is still the first tag in `<head>`. Do not hunt for one line number: root pages and equipment pages put the tag on different lines, but nothing except whitespace comes before it. Also confirm you did not uncomment or paste the inline snippet from `/index.html`.
2. If the tag is missing, add the one line from the case that matches the folder. A wrong `src` makes the browser return 404 for the local file. `window.dataLayer` stays undefined, Google's script is never requested, and the page still renders. That failure is silent.
3. Do not move this tag into `/includes/header.html` or `/includes/footer.html`. `/JS/layout.js` sets those fragments with `innerHTML` (around line 20), so a `<script>` there does not run. Any page that lost the tag in `<head>` stops sending analytics and still looks normal. Do not add the tag under `/knowledge-base/` either. That site is built separately and does not use this loader.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `src` | Relative path to the shared loader. Keep the single quotes used on the other pages so a search still finds the tag. | Root: `JS/gtm.js`. `/About_Equipment/`: `../JS/gtm.js`. |
| ② | Position | First tag inside `<head>`, before `<meta charset>`. A copy lower in the page still loads, but it starts later than on every other page, and earlier hits can be missed, with no error shown. | Copy Case A or Case B. |

#### Case A: A page in the site root

1. Copy the tag from `/index.html` line 5.

   **Real Code Snippet (`/index.html` — root loader tag, around line 5)**
   ```html
       <script src='JS/gtm.js'></script>
   ```

2. Keep it as the first tag in `<head>`.

   **Example Snippet (root page head)**
   ```html
   <head>
       <script src='JS/gtm.js'></script> <!-- ① First tag in head. Site root only. -->
       <meta charset="UTF-8">
   </head>
   ```

#### Case B: A page in `/About_Equipment/`

1. Copy the tag from `/About_Equipment/SEM.html` line 4. The page is one folder down, so the path needs `../`. Using the root path `JS/gtm.js` from this folder returns 404, and the tag never runs. The page looks the same either way.

   **Real Code Snippet (`/About_Equipment/SEM.html` — equipment loader tag, around line 4)**
   ```html
       <script src='../JS/gtm.js'></script>
   ```

2. Keep it as the first tag in `<head>`.

   **Example Snippet (equipment page head)**
   ```html
   <head>
       <script src='../JS/gtm.js'></script> <!-- ① First tag in head. /About_Equipment/ only. -->
       <meta charset="UTF-8">
   </head>
   ```

---

## Technical best practices and validation

* **One live ID:** The running container ID lives in three places: `/JS/gtm.js` line 6, `/index.html` line 54, and `/MPaCT.html` line 52. If those three disagree, no-JavaScript visits to the homepage and the MPaCT Lab page are recorded in a different container from every other visit. The pages do not show an error.
* **The comment is not a second install:** Lines 11–15 of `/index.html` and `/MPaCT.html` must stay commented out. The live loader is `/JS/gtm.js`. Both together double-count page views, silently.
* **No-JavaScript coverage stops at two pages:** The other 63 HTML pages have no `<noscript>` iframe. With JavaScript off, they send no hit. That matches the site today. Add the iframe to a new page only when you want that fallback, use the same container ID, and leave the element itself uncommented. The iframe is `height="0"`, so it does not change the layout.
* **Do not reflow the loader:** Change the ID string on line 6 and leave the rest of `/JS/gtm.js` as it is, including the space at the start of line 1. Do not change `'dataLayer'` or `'script'`. A different data-layer name fails silently: the array you check in the console is missing, and tags in the container that expect `dataLayer` do not see events.
* **The container ID is public:** `GTM-5PQKN9FD` is meant to be in the page source. It is not a secret. Do not put a Tag Manager API credential, or any other private key, in `/JS/gtm.js` or in a page.
* **Apache is not involved:** This tag is a static script. PHP's built-in server (`php -S`) does not read `.htaccess`, and that does not matter here. Rewrite rules and security headers are not part of this check.

---

## Let's Verify Your Changes

1. From the website root, open the page through a local HTTP server (Live Server, or `php -S localhost:8000`). Use `http://`, not a double-clicked file. `.htaccess` does not affect this tag, so `php -S` is enough.
2. Open DevTools, go to Network, reload, and filter on `gtm`. You should see the local file (`JS/gtm.js` or `../JS/gtm.js`) and exactly one request to `https://www.googletagmanager.com/gtm.js?id=GTM-…` using the ID you set. Two requests to `googletagmanager.com/gtm.js` mean the tag is on the page twice. Page views will be double-counted, and the page will not say so.
3. In the console, enter `window.dataLayer`. It should be an array that includes an object whose `event` is `"gtm.js"`. The local loader pushes that object as soon as it runs. If `window.dataLayer` is `undefined`, the local file did not run: the tag is missing, or the `src` path returns 404. The array only proves the loader ran. The `id=` on the network request is what proves which container the page asked for.
4. In [Google Tag Manager](https://tagmanager.google.com/), open the container whose ID you just put in the file, choose Preview, enter the same page URL, and click Connect. Tag Assistant opens that URL in your browser, so a local server URL works. The container should show as firing on that page. Preview uses the workspace draft, so you can check before you publish. If a different container fires, a second snippet is still live. If you cannot open the container, steps 2 and 3 are still the check you can run on the page.
5. To check the no-JavaScript iframe, open `/index.html` or `/MPaCT.html`, turn JavaScript off, and reload. Network should show one request to `https://www.googletagmanager.com/ns.html?id=GTM-…` with the new ID. The iframe is hidden, so the layout does not change. If the old ID appears, line 54 or line 52 was not updated. On any other page, JavaScript off means no Tag Manager request, which is what the markup does today.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
