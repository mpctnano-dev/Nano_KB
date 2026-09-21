
# Add or Unlist a Page in the Sitemap

**Summary:** Add a public page to the live sitemap, or unlist a page from `/sitemap.php` and, for an instrument, from the catalog, the rates table, and the booking lists as well.

**When to change:** When a page is added, retired, or duplicated, or when an instrument must stay off public listings, as the three TEM prep instruments were on September 17, 2026 (commit `57788ce`).

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★</span>

**Estimated Time:** 10 minutes (instrument unlisting: 30 minutes)

---

## Visual Reference

**Real Code Snippet (`/sitemap.php` — scan folders and excluded filenames, lines 32–45)**

```php
$scanDirs = array(
    '',
    'About_Equipment',
);

// Duplicate / superseded HTML that still sits in the web root. Each already
// canonicals to a live page; listing them here would split crawl attention.
$excludeFiles = array(
    'OLD_CHIPS_Scholars_Program.html' => true,
    'SCEAcademicPathways.html' => true,
    'TEM_Prep_Disk_Grinder.html' => true,
    'TEM_Prep_Dimple_Grinder.html' => true,
    'TEM_Prep_Ion_Beam_Mill.html' => true,
);
```

*`$scanDirs` is the folders that are scanned. `$excludeFiles` is keyed by the bare filename, and that key is checked in every scanned folder, which is why the three `/About_Equipment/TEM_Prep_*.html` pages stay off the list.*

**Real Output (`php sitemap.php` from the site root at commit `09323b0`, first entries)**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://nano.nau.edu/index.html</loc>
  </url>
  <url>
    <loc>https://nano.nau.edu/About_Equipment/Amatrol_87_MS1.html</loc>
  </url>
  <url>
    <loc>https://nano.nau.edu/About_Equipment/Amatrol_87_MS2.html</loc>
  </url>
…
</urlset>
```

*The homepage is first, then case-insensitive A-Z. There is no `lastmod`. This run printed 113 `<url>` entries; the middle is omitted above. Every `<loc>` uses `https://nano.nau.edu`, even when you run the script on your own machine.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/sitemap.php` | Builds the XML on each request: which folders are scanned, which bare filenames are left out, the knowledge-base walk, and the sort. |
| `/.htaccess` | Rewrites `/sitemap.xml` and `/sitemap_mpact.xml` to `/sitemap.php`. |
| `/robots.txt` | Advertises the sitemap. The `Sitemap:` line stays pointed at `https://nano.nau.edu/sitemap.php`. |
| `/[Page].html` | A root page you unlist. Add the robots meta here, and point canonical at the live page only if this file is a duplicate. |
| `/About_Equipment/[InstrumentPage].html` | An instrument page you unlist. Same robots meta. Leave canonical on this page unless it duplicates another URL. |
| `/equipment.json` | `"listed": false` keeps the instrument out of the Reserve Equipment and Contact Us equipment dropdowns. |
| `/Equipment.html` | Comment out the catalog card, and remove the instrument from the JSON-LD `ItemList`. |
| `/Rates.html` | Comment out the public rate row. |
| `/JS/booking.js` | Reserve Equipment dropdown. Confirm it still skips `listed: false`. |
| `/JS/script.js` | Contact Us equipment dropdown. Confirm the same skip in `populateEquipmentData()`. |

> [!WARNING]
> **Any `.html` file uploaded to the web root or `/About_Equipment/` is published to crawlers at once.**
> The next request to `/sitemap.php` lists it. Drafts, backups, and `OLD_` copies must be added to `$excludeFiles` or kept off the server. Nothing warns you when a file is picked up.

> [!NOTE]
> **The sitemap is live.**
> `/sitemap.php` builds the list on every request. Every `.html` file in the web root and in `/About_Equipment/` is listed automatically. Knowledge-base folders that hold an `index.html` are listed as trailing-slash URLs, and a path containing `/404` is skipped. The homepage comes first, then case-insensitive A-Z (`strcasecmp`). There is no `lastmod`, on purpose (lines 18–21). The response may be cached for up to an hour (`Cache-Control: public, max-age=3600`).

> [!IMPORTANT]
> **Verify against your local codebase:** `$scanDirs`, `$excludeFiles`, and the five filenames in that array (`OLD_CHIPS_Scholars_Program.html`, `SCEAcademicPathways.html`, `TEM_Prep_Disk_Grinder.html`, `TEM_Prep_Dimple_Grinder.html`, `TEM_Prep_Ion_Beam_Mill.html`); the `RewriteRule` for `sitemap.xml` in `/.htaccess`; and the `Sitemap:` line in `/robots.txt`. Your tree may already contain pages this commit did not.

> [!TIP]
> **A new root page is listed for you.**
> [Add a New Page](../General/add-a-new-page.md) does not need a sitemap edit. To hide a page, use [Part 2](#part-2-unlist-a-page). For an instrument, continue with [Part 3](#part-3-unlist-an-instrument-everywhere). A new folder of pages is [Part 4](#part-4-list-a-new-folder).

---

## Step-by-Step Instructions

A `.html` file you add in the web root or in `/About_Equipment/` joins the sitemap by itself. You edit `/sitemap.php` when you need to hide a file, or when the new pages live in a folder the script does not scan yet.

---

### Part 1: Understand How the List Is Built

1. To get started, please open `/sitemap.php` in your code editor.
2. Read the closing lines of the file comment and the two response headers.

   **Real Code Snippet (`/sitemap.php` — why `lastmod` is omitted, and the cache header, lines 18–25)**

   ```php
    * GEO plan item #4: robots.txt already advertises a sitemap; this is the
    * file that makes that URL resolve. lastmod is omitted on purpose — file
    * mtimes on a static deploy are not content-change dates, and a wrong date
    * is worse for search than none.
    */

   header('Content-Type: application/xml; charset=UTF-8');
   header('Cache-Control: public, max-age=3600');
   ```

   There is no `<lastmod>` on any URL. File dates on a deploy are not content-change dates, and a wrong date is worse for search than none. Please leave that as it is. `Cache-Control: public, max-age=3600` tells browsers and caches they may keep the XML for an hour. The script still rebuilds the list on every request that reaches PHP. A client that cached the old XML can show it for up to an hour, and the page does not tell you the copy is stale.

3. The folder scan is not recursive. It opens each name in `$scanDirs`, keeps files whose name ends in `.html`, and skips any bare filename that is a key in `$excludeFiles`.

   **Real Code Snippet (`/sitemap.php` — a missing folder is skipped, lines 49–53)**

   ```php
   foreach ($scanDirs as $relDir) {
       $dir = $relDir === '' ? $root : $root . DIRECTORY_SEPARATOR . $relDir;
       if (!is_dir($dir)) {
           continue;
       }
   ```

   **Real Code Snippet (`/sitemap.php` — `.html` files and the exclusion check, lines 64–76)**

   ```php
           if (!preg_match('/\.html$/i', $name)) {
               continue;
           }
           if (isset($excludeFiles[$name])) {
               continue;
           }

           $full = $dir . DIRECTORY_SEPARATOR . $name;
           if (!is_file($full)) {
               continue;
           }

           $pages[] = ($relDir === '' ? $name : $relDir . '/' . $name);
   ```

   The empty string in `$scanDirs` is the web root, so a root page such as `/Visit.html` is listed only because that file is on disk and is not a key in `$excludeFiles`. Nothing in the script names it. The check on line 67 uses the filename alone, not the folder. The comment above the array talks about duplicates in the web root. The code is wider than that comment: the same bare name is dropped in every scanned folder. That is how `TEM_Prep_Disk_Grinder.html` is left out of `/About_Equipment/`. A key written as `About_Equipment/TEM_Prep_Disk_Grinder.html` would never match, the page would stay in the sitemap, and PHP would not report an error.

4. The knowledge base is a separate walk. It is not a `$scanDirs` entry, and it does not read `$excludeFiles`.

   **Real Code Snippet (`/sitemap.php` — knowledge-base trailing-slash URLs, lines 82–103)**

   ```php
   // Knowledge-base pages are MkDocs directory URLs (index.html behind a trailing slash).
   // Walk only index.html files so assets, 404, and search JSON stay out of the sitemap.
   $kbRoot = $root . DIRECTORY_SEPARATOR . 'knowledge-base';
   if (is_dir($kbRoot)) {
       $kbIter = new RecursiveIteratorIterator(
           new RecursiveDirectoryIterator($kbRoot, FilesystemIterator::SKIP_DOTS)
       );
       foreach ($kbIter as $file) {
           if (!$file->isFile()) {
               continue;
           }
           if (strtolower($file->getFilename()) !== 'index.html') {
               continue;
           }
           $rel = str_replace('\\', '/', substr($file->getPathname(), strlen($root) + 1));
           if (strpos($rel, '/404') !== false) {
               continue;
           }
           $dir = dirname($rel);
           $pages[] = ($dir === 'knowledge-base') ? 'knowledge-base/' : $dir . '/';
       }
   }
   ```

   Only files named `index.html` are kept. The URL is the folder with a trailing slash (`knowledge-base/` for the top index, otherwise `knowledge-base/techniques/transmission-electron-microscopy/` and so on). A path that contains `/404` is skipped. Assets and `search_index.json` stay out because they are not `index.html`. Putting a knowledge-base filename into `$excludeFiles` does nothing. The sitemap still returns HTTP 200, and the URL is still there. To keep a knowledge-base page off the list, do not deploy its `index.html`. If `/llms.txt` names that page, update [Update llms.txt](05-update-llms-txt.md) in the same change.

5. The list is sorted after both walks finish.

   **Real Code Snippet (`/sitemap.php` — homepage first, then case-insensitive A-Z, lines 105–114)**

   ```php
   // Homepage first, then A–Z by path so the document is stable across requests.
   usort($pages, function ($a, $b) {
       if ($a === 'index.html') {
           return -1;
       }
       if ($b === 'index.html') {
           return 1;
       }
       return strcasecmp($a, $b);
   });
   ```

   `index.html` is always first. Every other path is ordered with `strcasecmp`, so `About_Equipment/` sorts before `About_Us.html`. The host on every `<loc>` is the hardcoded `$baseUrl`, `https://nano.nau.edu` (line 27). Please do not change that to a local host. A committed localhost value would publish localhost URLs on the production sitemap, and the script would still return HTTP 200.

---

### Part 2: Unlist a Page

Use this part for a draft, a backup, an `OLD_` copy, or any other `.html` file in the web root or `/About_Equipment/` that must not be advertised. An instrument also needs [Part 3](#part-3-unlist-an-instrument-everywhere).

1. In `/sitemap.php`, add the bare filename to `$excludeFiles`. Copy the existing `'name.html' => true` shape.

   | # | What to Change | Description | Options / Example |
   |---|---|---|---|
   | ① | `$excludeFiles` key | Bare filename only. No folder, no leading slash. The same key is applied in the root and in every `$scanDirs` folder. A path key never matches, and the file stays listed with no error. | `'[Page].html' => true` |
   | ② | Array value | `isset` decides membership. `true` matches the file today. The boolean `false` also excludes, because `isset` is still true. `null` does not: `isset` is false, so the page stays in the sitemap and nothing warns you. | `true` |
   | ③ | Robots meta | Tells a crawler that already knows the URL not to index it. Sitemap exclusion does not do this. | `<meta name="robots" content="noindex, nofollow">` |
   | ④ | Canonical | Point this at the live page only when the file is a duplicate. A hidden page that is not a duplicate keeps its own canonical. Pointing a real page at a different URL asks search engines to treat it as that other page. | `https://nano.nau.edu/[LivePage].html` |

   **Example Snippet (one new exclusion)**

   ```php
   $excludeFiles = array(
       'OLD_CHIPS_Scholars_Program.html' => true,
       'SCEAcademicPathways.html' => true,
       'TEM_Prep_Disk_Grinder.html' => true,
       'TEM_Prep_Dimple_Grinder.html' => true,
       'TEM_Prep_Ion_Beam_Mill.html' => true,
       '[Page].html' => true, // bare filename; do not set this value to null
   );
   ```

2. Open the page you are hiding and add the robots meta in `<head>`, directly under the viewport meta. The September 17 instrument pages did it this way. The same tag is on line 7 of `/About_Equipment/TEM_Prep_Dimple_Grinder.html` and `/About_Equipment/TEM_Prep_Ion_Beam_Mill.html`.

   **Real Code Snippet (`/About_Equipment/TEM_Prep_Disk_Grinder.html` — `noindex` with the page's own canonical, lines 4–10)**

   ```html
       <script src='../JS/gtm.js'></script>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <meta name="robots" content="noindex, nofollow">
       <title>Fischione Model 160 | TEM Prep Disk Grinder | MPaCT Lab</title>
       <meta name="description" content="Pre-thin TEM specimens up to 18 mm to uniform parallel thickness with the Fischione Model 160 Disk Grinder at MPaCT Lab for controlled mechanical pre-thinning prior to dimpling or ion milling.">
       <link rel="canonical" href="https://nano.nau.edu/About_Equipment/TEM_Prep_Disk_Grinder.html">
   ```

   That canonical still points at the TEM prep page itself. These files are hidden, not duplicates of another URL. Leave a hidden page's canonical on itself.

3. If the file is a duplicate of a live page, point `<link rel="canonical">` at the live page instead. The two older root exclusions already do that. They do **not** have a robots meta. Do not copy them as a finished unlist. Exclusion without `noindex` leaves the file open to indexing when something still links to it, and the sitemap will not tell you.

   **Real Code Snippet (`/SCEAcademicPathways.html` — duplicate canonical for the degree-programs page, lines 11–12)**

   ```html
       <title>Degree Programs | NAU Nano</title>
       <link rel="canonical" href="https://nano.nau.edu/degree-programs.html">
   ```

   **Real Code Snippet (`/OLD_CHIPS_Scholars_Program.html` — duplicate canonical for the live CHIPS page, lines 10–11)**

   ```html
       <title>CHIPS Scholars Program | NAU & University of Arizona | NAU Nano</title>
       <link rel="canonical" href="https://nano.nau.edu/CHIPS_Scholars_Program.html">
   ```

   **Example Snippet (robots meta, and a canonical only when the file is a duplicate)**

   ```html
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <!-- Add this line. Without it, a crawler that already has the URL can still index the page. -->
   <meta name="robots" content="noindex, nofollow">
   <link rel="canonical" href="https://nano.nau.edu/[LivePage].html"> <!-- Change the href only if this file duplicates [LivePage].html -->
   ```

4. Do not add a `Disallow` line in `/robots.txt` for a normal public HTML page. Listing a path there advertises it. The `Sitemap:` line is covered in [Part 5](#part-5-keep-old-sitemap-urls-working) and in [Update robots.txt and AI Crawler Access](01-update-robots-and-ai-crawlers.md). Private files that must not be fetched belong in [Update Security Headers and Blocked Paths](../Security/06-update-security-headers.md), not in the sitemap array.

---

### Part 3: Unlist an Instrument Everywhere

Hiding the HTML file in [Part 2](#part-2-unlist-a-page) is only the sitemap half. Commit `57788ce` changed nine files so the three TEM prep instruments stayed off the catalog, the rate sheet, and the booking lists: the three `/About_Equipment/TEM_Prep_*.html` pages, `/sitemap.php`, `/equipment.json`, `/Equipment.html`, `/Rates.html`, `/JS/booking.js`, and `/JS/script.js`. Do these steps for `[InstrumentPage].html`. The extra time in the header is this part.

> [!WARNING]
> **`"listed": false` does not update the catalog card, the `ItemList`, or the sitemap.**
> Those are separate copies. Skip one and the instrument stays public in that place. Nothing checks that the copies match.

1. Complete [Part 2](#part-2-unlist-a-page) for the instrument's HTML file: bare filename in `$excludeFiles`, plus `<meta name="robots" content="noindex, nofollow">`. Leave the canonical on that instrument page unless it is a duplicate.
2. In `/equipment.json`, set `"listed": false` on that instrument's object. `EQ-034`, `EQ-035`, and `EQ-036` are the three TEM prep rows (the flag is on lines 352, 363, and 374).

   **Real Code Snippet (`/equipment.json` — `listed` false on the dimple grinder, lines 345–355)**

   ```json
           {
               "id": "EQ-034",
               "name": "TEM Prep - Dimple Grinder",
               "category": "Sample Prep",
               "status": "EXPECTED",
               "expectedDate": "09/01/26",
               "warningMessage": null,
               "listed": false,
               "accessories": [],
               "aboutPage": "About_Equipment/TEM_Prep_Dimple_Grinder.html"
           },
   ```

   **Example Snippet (flag to add on the instrument you are hiding)**

   ```text
   "id": "EQ-[###]",
   "listed": false  # boolean false; delete this note before saving
   ```

   Paste the `"listed": false` line only, without the `#` note. `equipment.json` is strict JSON, and a comment makes `response.json()` throw, so every dropdown that loads it comes up empty. The value must be the boolean `false`, not the string `"false"` and not a missing key. Both `/JS/booking.js` and `/JS/script.js` test `listed !== false`. A string or a missing key still passes that test, so the instrument stays in the dropdowns and the form does not show an error.

3. On `/Equipment.html`, comment out the instrument's catalog card. Do not delete it. The TEM prep cards were wrapped in one HTML comment so they can be put back.

   **Real Code Snippet (`/Equipment.html` — catalog cards commented out, not deleted, lines 1356–1434)**

```html
                    <!-- TEM Prep - Disk Grinder, Dimple Grinder, and Ion Beam Mill
                    <div class="tech-card" data-category="Sample Prep">
…
                    </div>
                    -->
```

   The comment is the only thing hiding the cards. `equipment.json` does not build this grid. Leaving the card in place keeps the instrument on the public catalog, with no error on the page.

4. In the same file, remove the instrument's `ListItem` from the catalog `ItemList`, renumber `position` from 1 with no gaps, and set `numberOfItems` to how many `ListItem` objects remain. Today `numberOfItems` is 39, and the three TEM prep pages are not in the list. The same commit also inserted the DualBeam FIB-SEM `ListItem` (that is why the count went from 41 to 39: three removed, one added). When you are only unlisting, do not add another instrument in the same edit. The hand-copied block is easy to break: a stray comma voids the JSON-LD, and the page still looks fine. Follow [Update Equipment Structured Data (JSON-LD)](03-update-equipment-structured-data.md), Part 4, for the catalog list.

   **Real Code Snippet (`/Equipment.html` — catalog `ItemList` count, lines 135–138)**

   ```json
             "@type": "ItemList",
             "@id": "https://nano.nau.edu/Equipment.html#itemlist",
             "numberOfItems": 39,
             "itemListElement": [
   ```

   **Example Snippet (the `ListItem` to delete, then renumber what remains)**

   ```json
   {
     "@type": "ListItem",
     "position": 1,
     "url": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html", // delete this object, then renumber position
     "item": {
       "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#product"
     }
   }
   ```

   Delete that whole object for `[InstrumentPage].html`. JSON-LD has no comments, so do not leave a note inside the `<script>` block. A leftover comma or comment voids the block, and the catalog page still looks normal.

5. On `/Rates.html`, comment out the instrument's table row the same way. Do not delete `/rates.json` rows as part of this task. Commit `57788ce` left `EQ-034`, `EQ-035`, and `EQ-036` in `/rates.json`. Reserve Equipment only looks up a rate after the visitor picks an instrument, and the dropdown no longer offers a `listed: false` instrument, so those JSON rows sit unused. The public rate sheet is this HTML table, not `rates.json`.

   **Real Code Snippet (`/Rates.html` — rate rows kept in a comment, lines 397–416)**

   ```html
                                       <!-- TEM Prep rows kept for reuse; not listed
                                       <tr>
                                           <td>TEM Prep - Dimple Grinder</td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                       </tr>
                                       <tr>
                                           <td>TEM Prep - Disk Grinder</td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                       </tr>
                                       <tr>
                                           <td>TEM Prep - Ion Beam Mill</td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                           <td><span class="tbd-chip">TBD</span></td>
                                       </tr>
                                       -->
   ```

   **Example Snippet (comment out the row; do not delete it)**

   ```html
   <!-- [Instrument name] kept for reuse; not listed
   <tr>
       <td>[Instrument name]</td>
       <td>[Billable unit]</td>
       <td>[Internal rate]</td>
       <td>[External rate]</td>
   </tr>
   -->
   ```

6. Confirm the two dropdown filters are still in the code. You do not add a new line per instrument. Both filters were added once in commit `57788ce`. Removing either one puts every `listed: false` instrument back into that dropdown, and the instrument page does not warn you.

   `populateEquipmentData()` in `/JS/script.js` fills the Contact Us equipment inquiry (`#equipmentCategory` and `#equipmentName`). Please do not edit `applyCategoryFromUrl()` (around line 1357). That function only opens a catalog category from `?category=`.

   **Real Code Snippet (`/JS/script.js` — Contact Us list drops `listed: false`, around line 700)**

   ```javascript
           const equipmentList = (data.equipment || []).filter(item => item.listed !== false);
   ```

   **Real Code Snippet (`/JS/script.js` — category filter uses the same test, lines 721–723)**

   ```javascript
               const filtered = selectedCat
                   ? equipmentList.filter(item => item.category === selectedCat && item.listed !== false)
                   : equipmentList.filter(item => item.listed !== false);
   ```

   **Real Code Snippet (`/JS/booking.js` — Reserve Equipment loads only listed instruments, lines 446–451)**

   ```javascript
               equipmentData = (data.equipment || []).filter(item => item.listed !== false).map(item => {
                   if (EDUCATIONAL_IDS.includes(item.id)) {
                       return Object.assign({}, item, { category: 'Educational' });
                   }
                   return item;
               });
   ```

   **Real Code Snippet (`/JS/booking.js` — equipment dropdown filter, lines 488–490)**

   ```javascript
           const filtered = category
               ? equipmentData.filter(i => i.category === category && i.listed !== false)
               : equipmentData.filter(i => i.listed !== false);
   ```

7. Search the repository for `[InstrumentPage].html`. At commit `09323b0` the TEM prep filenames appear in `$excludeFiles`, in `aboutPage` inside `/equipment.json`, in the commented cards on `/Equipment.html`, and in the three instrument pages themselves. They do not appear in other instruments' related-instrument cards. If your page still does, update those cards with [Update Equipment Breadcrumbs and Related Instruments](04-update-equipment-breadcrumbs.md). A related-card link keeps working after the sitemap drops the URL, and `/sitemap.php` does not report it.

   Commit `57788ce` also did not change `/llms.txt`. The knowledge-base page `https://nano.nau.edu/knowledge-base/techniques/tem-sample-preparation/` is still listed, because its `index.html` is still on disk. Unlisting the `/About_Equipment/` files does not take that technique page down. Remove the `index.html` only if that page should come down too, and then update [Update llms.txt](05-update-llms-txt.md).

---

### Part 4: List a New Folder

You do not need this part for a new root page or a new file in `/About_Equipment/`. Those are picked up on the next request. You need it when the new HTML files live in a folder the script does not already open.

1. Add the folder name to `$scanDirs`. Use the name only, with no leading slash and no trailing slash. The scan is one directory deep. Files in a subfolder of that directory are not listed, and nothing warns you. Do not add `knowledge-base` here. That tree has its own walk. Scanning it as a normal folder would advertise `knowledge-base/index.html` and `knowledge-base/404.html` as file URLs, beside the trailing-slash URLs the walk already emits.

   | # | What to Change | Description | Options / Example |
   |---|---|---|---|
   | ① | `$scanDirs` entry | Folder under the web root. The empty string is the root and is already there. Keep `About_Equipment`. | `'[NewFolder]'` |

   **Example Snippet (scan one new folder)**

   ```php
   $scanDirs = array(
       '',
       'About_Equipment',
       '[NewFolder]', // name only; a typo is skipped with no error
   );
   ```

2. Please look again at the `is_dir` check from Part 1 (lines 49–53). If the name is wrong, or the folder is not on the server yet, the loop `continue`s. The sitemap still returns HTTP 200 and simply omits that folder.

> [!WARNING]
> **A `$scanDirs` name that is not a real directory is skipped silently.**
> Pages in that folder never appear in the XML. The script does not log the skip and does not change the HTTP status.

---

### Part 5: Keep Old Sitemap URLs Working

`/robots.txt` used to advertise `sitemap_mpact.xml`. Crawlers that saved that address, or the usual `sitemap.xml`, still need a sitemap. The rewrite sends both names to `/sitemap.php`. You normally do not change this rule. It is here so a sitemap edit does not "fix" it by accident.

1. In `/.htaccess`, the sitemap rule is the second `RewriteRule` inside `mod_rewrite`.

   **Real Code Snippet (`/.htaccess` — `sitemap.xml` and `sitemap_mpact.xml` serve `sitemap.php`, lines 9–16)**

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

   The pattern `^sitemap(_mpact)?\.xml$` matches only those two names at the root of the site. It does not match `/sitemap.php`, which is the real script. One syntax error anywhere in `/.htaccess` makes Apache return HTTP 500 for the whole site. PHP's built-in server never reads this file, so it will not show that failure. Read [Update Security Headers and Blocked Paths](../Security/06-update-security-headers.md) before you change anything else in `/.htaccess`.

2. Leave the last line of `/robots.txt` pointed at the PHP script, not at `sitemap.xml`. Changing which crawlers are allowed is a different task: [Update robots.txt and AI Crawler Access](01-update-robots-and-ai-crawlers.md).

   **Real Code Snippet (`/robots.txt` — advertised sitemap URL, around line 69)**

   ```text
   Sitemap: https://nano.nau.edu/sitemap.php
   ```

---

## Technical best practices and validation

* **Do not add `lastmod`.** The comment on lines 18–21 is the reason. A file timestamp from deploy is not the day the page's content changed, and a wrong date is worse for search than none.
* **Keep `$baseUrl` as `https://nano.nau.edu`.** Local runs are supposed to print production URLs. A localhost value in the committed file would advertise localhost to crawlers, and the script would still return HTTP 200.
* **Exclusion keys are bare filenames.** `isset` is the whole test. A path key never matches. A `null` value does not exclude. Either mistake leaves the page in the sitemap with no error.
* **The same bare name is excluded in every scanned folder.** Two files that share a filename, one in the root and one in `/About_Equipment/`, would both drop. Name the key after the file you actually mean to hide, and search for that filename afterward.
* **`$excludeFiles` does not cover `/knowledge-base/`.** Only the `index.html` walk does, and its only skip is a path containing `/404`. A draft `index.html` under that tree is published on the next request.
* **Do not add `knowledge-base` to `$scanDirs`.** The dedicated walk already lists those pages as trailing-slash URLs. A second, flat scan would also list `404.html` and `index.html` as files.
* **`"listed": false` is a boolean, and it is only the dropdown flag.** The catalog card, the `ItemList`, the rate row, and `$excludeFiles` are separate edits. A string `"false"` stays in the dropdowns because the test is `!== false`.
* **Comment cards and rate rows out; do not delete them.** That is what commit `57788ce` did, so the markup can be restored. Deleting `/rates.json` rows was not part of that commit.
* **The `ItemList` is hand-copied.** `numberOfItems` must equal the number of `ListItem` objects, and `position` must run from 1 with no gaps. One stray comma silently voids the block. [Update Equipment Structured Data (JSON-LD)](03-update-equipment-structured-data.md) is the check for that block.
* **Treat `/.htaccess` as site-wide.** A bad edit is HTTP 500 on Apache for every URL, not a sitemap-only fault. `php -S` hides that because it ignores `.htaccess`.
* **Allow up to an hour after deploy.** `max-age=3600` lets a cache keep the previous XML. A fresh `php sitemap.php` on the server shows the new list immediately; a crawler that cached the URL may not.

---

## Let's Verify Your Changes

1. From the site root, run `php sitemap.php`. You can also start PHP's built-in server and open `/sitemap.php` in a browser. The `<loc>` values should use `https://nano.nau.edu`, not your local host.
2. Search that XML for the page you added or hid. A new root or `/About_Equipment/` file is present. A filename you put in `$excludeFiles` is absent. A knowledge-base page appears as a trailing-slash URL, not as `index.html`. The five filenames already in `$excludeFiles` stay absent unless you removed them.
3. Check the cache header on `/sitemap.php`. On Windows PowerShell, `curl` is an alias, so run `curl.exe`.

   **Example Snippet (headers from the built-in server or from Apache)**

   ```bash
   curl.exe -I http://127.0.0.1:[port]/sitemap.php  # port you passed to php -S
   ```

   You want `Content-Type: application/xml; charset=UTF-8` and `Cache-Control: public, max-age=3600`. The built-in server sends these because they come from PHP, not from `.htaccess`.

4. Compare the three public URLs on Apache (production, staging, or a local Apache such as XAMPP). `/sitemap.xml` and `/sitemap_mpact.xml` must return the same XML as `/sitemap.php`. PHP's built-in server does **not** read `.htaccess`, so those two names 404 there. A 404 on `php -S` does not mean the rewrite is broken. An HTTP 500 on Apache means `/.htaccess` itself failed; stop and check [Update Security Headers and Blocked Paths](../Security/06-update-security-headers.md).
5. For an instrument, confirm each public copy, not only the sitemap. The card is gone on `/Equipment.html`. The row is gone on `/Rates.html`. The name is absent from the Reserve Equipment dropdown and from the Contact Us equipment inquiry. Those dropdowns load `/equipment.json` with `fetch`, so open them through a local web server, not as a `file://` page. In the catalog page source, `numberOfItems` matches the number of `ListItem` entries, with no gap in `position`. For the JSON-LD check, follow [Update Equipment Structured Data (JSON-LD)](03-update-equipment-structured-data.md).
6. After deploy, you may resubmit `https://nano.nau.edu/sitemap.php` in [Google Search Console](https://search.google.com/search-console). Caches may keep the previous XML for up to an hour, so a mismatch in the first hour is not by itself a failed edit. Confirm with a fresh `php sitemap.php` on the server, or with a request that bypasses the cache.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
