
# Update Equipment Structured Data (JSON-LD)

**Summary:** Add, update, or remove JSON-LD in the hand-copied structured-data blocks on the equipment pages and the catalog, using `/JS/organization.json` and `/JS/equipment-schema.json` as the copy sources.

**When to change:** When an instrument page is added, renamed, or unlisted, its specs or status change, or the lab's name, address, or hours change.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 30 minutes

---

## Visual Reference

**Real Code Snippet (`/About_Equipment/DualBeam_FIBSEM.html` — the Product node, lines 124–186)**

```json
        {
          "@type": "Product",
          "@id": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html#product",
          "name": "FEI Quanta 3D FEG DualBeam (FIB-SEM)",
          "sku": "EQ-024",
          "category": "Metrology",
          "url": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html",
          "provider": {
            "@id": "https://nano.nau.edu/#organization"
          },
          "description": "Perform site-specific FIB milling, cross-sectioning, and high-resolution SEM imaging with the FEI Quanta 3D FEG DualBeam FIB-SEM at MPaCT Lab for 2D/3D materials characterization and TEM sample prep.",
          "image": "https://nano.nau.edu/Images/DualBeam.jpg",
          "brand": {
            "@type": "Brand",
            "name": "FEI"
          },
          "model": "Quanta 3D FEG",
          "additionalProperty": [
            {
              "@type": "PropertyValue",
              "name": "Electron Source",
              "value": "Field-emission gun, 200 V – 30 kV accelerating voltage"
            },
…
            {
              "@type": "PropertyValue",
              "name": "Operational status",
              "value": "Expected — installation scheduled for 2026-09-01"
            }
          ],
          "isSimilarTo": [
            {
              "@id": "https://nano.nau.edu/About_Equipment/SEM.html#product"
            },
            {
              "@id": "https://nano.nau.edu/About_Equipment/TEM.html#product"
            }
          ]
        }
```

*`sku` is the EQ number from `/JS/equipment-schema.json`. `isSimilarTo` names the same pages as the related-instrument cards, which holds on all 42 pages today.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/About_Equipment/*.html` | The 42 instrument pages. Each has one hand-copied `<!-- BEGIN structured-data -->` block in `<head>`. |
| `/About_Equipment/DualBeam_FIBSEM.html` | Model block for a new or rebuilt instrument page. |
| `/Equipment.html` | Catalog block: the same three shared nodes, then `CollectionPage`, `BreadcrumbList`, and an `ItemList` of 39 instruments. |
| `/JS/organization.json` | Copy source for `ResearchOrganization`, `Place`, and `WebSite`. Not loaded by any page or script. |
| `/JS/equipment-schema.json` | Copy source for each instrument's `sku`, `brand`, and `model`. Not loaded by any page or script. |

> [!WARNING]
> **These blocks are hand-copied, not generated.**
> One stray comma, or a `//` comment left in the script, makes the JSON invalid. Crawlers then drop the whole `@graph`, not just the bad node, while the page still looks fine. Lab facts drift the same way: editing `/JS/organization.json` alone changes nothing a visitor or a crawler sees, and editing one HTML file leaves the other 42 copies on the old fact. There is no error either way.

> [!IMPORTANT]
> **Verify against your local codebase:** Before you edit, search for `<!-- BEGIN structured-data -->` and confirm you still have 42 files in `/About_Equipment/` plus `/Equipment.html`. Re-check `numberOfItems` (39 today), the `data-filter` values on the catalog buttons, and that no HTML or JS file references `organization.json` or `equipment-schema.json`. On this tree those two files are copy sources only.

> [!NOTE]
> **Shared nodes and page-specific nodes**
>
> `ResearchOrganization`, `Place`, and `WebSite` are copies of the `organization`, `place`, and `website` objects in `/JS/organization.json`. `WebPage`, `BreadcrumbList`, and `Product` are written for that instrument. The catalog uses `CollectionPage` and `ItemList` instead of `WebPage` and `Product`. `dateModified` sits on `WebPage`, or on `CollectionPage` in `/Equipment.html`. It is not on `Product`.
>
> | Node | Scope | Holds `dateModified`? |
> |---|---|---|
> | `ResearchOrganization` | Shared. First object in every `@graph`. | No |
> | `Place` | Shared. Second object. | No |
> | `WebSite` | Shared. Third object. | No |
> | `WebPage` | Instrument pages only. | Yes |
> | `BreadcrumbList` | Instrument pages and the catalog. | No |
> | `Product` | Instrument pages only. | No |
> | `CollectionPage` | `/Equipment.html` only, in the `WebPage` slot. Its `@id` still ends in `#webpage`. | Yes |
> | `ItemList` | `/Equipment.html` only. | No |

> [!TIP]
> **Jump to the change you need**
> Lab name, address, or hours: [Part 3](#part-3-update-lab-facts-on-every-copy). Adding or removing a catalog entry: [Part 4](#part-4-update-the-catalog-itemlist).

---

## Step-by-Step Instructions

The JSON-LD is a static `<script type="application/ld+json">` in the page. No script reads the two JSON files at runtime, so a crawler only sees what you pasted between the HTML comments.

---

### Part 1: Locate the block and tell shared nodes from page-specific ones

1. To get started, please open `/About_Equipment/DualBeam_FIBSEM.html`.
2. Search for `<!-- BEGIN structured-data -->`. The block ends at `<!-- END structured-data -->`. There is one script, and the whole `@graph` is inside it.

   **Real Code Snippet (`/About_Equipment/DualBeam_FIBSEM.html` — start of the structured-data block, lines 17–21)**

   ```html
       <!-- BEGIN structured-data -->
       <script type="application/ld+json">
       {
         "@context": "https://schema.org",
         "@graph": [
   ```

3. Leave the first three objects alone unless you are doing Part 3. They are the shared lab nodes. `dateModified` is on the `WebPage` object that follows them, not on `Product`.

   **Real Code Snippet (`/About_Equipment/DualBeam_FIBSEM.html` — WebPage node, including dateModified, lines 78–94)**

   ```json
           {
             "@type": "WebPage",
             "@id": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html#webpage",
             "url": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html",
             "name": "FEI Quanta 3D FEG DualBeam | FIB-SEM | MPaCT Lab",
             "isPartOf": {
               "@id": "https://nano.nau.edu/#website"
             },
             "about": {
               "@id": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html#product"
             },
             "breadcrumb": {
               "@id": "https://nano.nau.edu/About_Equipment/DualBeam_FIBSEM.html#breadcrumb"
             },
             "description": "Perform site-specific FIB milling, cross-sectioning, and high-resolution SEM imaging with the FEI Quanta 3D FEG DualBeam FIB-SEM at MPaCT Lab for 2D/3D materials characterization and TEM sample prep.",
             "dateModified": "2026-09-17T08:49:00-07:00"
           },
   ```

   On this page, `WebPage` `name` matches `<title>`, and `WebPage` `description` matches both `<meta name="description">` and `Product` `description`. `Product` `name` matches the `<h1>`, which is not the same string as `<title>`. That split holds for the description and the title on all 42 instrument pages.

4. Open `/JS/organization.json`. The same three facts live here under the wrapper keys `organization`, `place`, and `website`. Those keys are not pasted into the HTML. The HTML pastes the inner objects into `@graph`.

   **Real Code Snippet (`/JS/organization.json` — wrapper key around the shared organization object, lines 1–4)**

   ```json
   {
     "organization": {
       "@type": "ResearchOrganization",
       "@id": "https://nano.nau.edu/#organization",
   ```

5. Open `/Equipment.html` and search for the same begin comment. The fourth object is a `CollectionPage`, not a `WebPage`. Its `@id` still ends in `#webpage`. Nothing else in that block points at the fragment. Leave it. Renaming it does not change what visitors see, and it throws away the id this page already publishes.

   **Real Code Snippet (`/Equipment.html` — CollectionPage node, lines 97–116)**

   ```json
           {
             "@type": "CollectionPage",
             "@id": "https://nano.nau.edu/Equipment.html#webpage",
             "url": "https://nano.nau.edu/Equipment.html",
             "name": "MPaCT Lab Equipment Catalog | NAU Nano",
             "isPartOf": {
               "@id": "https://nano.nau.edu/#website"
             },
             "about": {
               "@id": "https://nano.nau.edu/#organization"
             },
             "breadcrumb": {
               "@id": "https://nano.nau.edu/Equipment.html#breadcrumb"
             },
             "mainEntity": {
               "@id": "https://nano.nau.edu/Equipment.html#itemlist"
             },
             "description": "Browse the MPaCT Lab equipment catalog at NAU Nano, featuring capabilities in fabrication, metrology, electrical characterization, mechatronics, testing, and more for research and training.",
             "dateModified": "2026-08-14T15:47:12-07:00"
           },
   ```

   That description matches `<meta name="description">` on `/Equipment.html`. It does not match `og:description`, which is a different sentence. When you edit the catalog, copy the meta description, not the Open Graph description.

---

### Part 2: Add structured data to a new equipment page

1. Duplicate `/About_Equipment/DualBeam_FIBSEM.html` to `/About_Equipment/[InstrumentPage].html` (pick a sibling in the same category if that is closer). Update the visible breadcrumb and the related-instrument cards with [Update Equipment Breadcrumbs and Related Instruments](04-update-equipment-breadcrumbs.md). The JSON-LD category and `isSimilarTo` list have to name those same pages.
2. Set `<title>`, `<meta name="description">`, and the canonical URL. Description length and canonical rules are in [Update Site-Wide Metadata or SEO](../General/update-site-wide-metadata.md). Then use that same meta description string on both the `WebPage` node and the `Product` node. Set `WebPage` `name` to the `<title>` text, and set `Product` `name` (and the last breadcrumb name) to the `<h1>` text.
3. Search for `<!-- BEGIN structured-data -->`. Do not add a second JSON-LD script. Replace the `WebPage`, `BreadcrumbList`, and `Product` objects. Leave the first three objects unless Part 3 applies. If this page already exists and you are only changing specs or status, skip the duplicate and edit that page's `Product` node, then `dateModified`.
4. Update the page-specific fields with the table below.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | `@id` | Stable id other nodes point at. Change the page URL and the fragment (`#webpage`, `#breadcrumb`, `#product`) together. Do not change `#organization`, `#place`, or `#website`. A new `@id` that leaves an old reference splits the graph. The page still renders. | `https://nano.nau.edu/About_Equipment/[InstrumentPage].html#product` |
| ② | `sku` | EQ number. The same key must exist in `/JS/equipment-schema.json`. Keys in that file today are `EQ-001` through `EQ-042`, one per instrument page, with no gaps. | `EQ-[###]` |
| ③ | `category` | Must be a catalog `data-filter` value, not `all`. A value that is not in the list makes the category crumb open an unfiltered catalog. A case typo still filters, so the click will not show the mistake. Use the button spelling anyway. | `Educational`, `Electrical`, `Fabrication`, `Mechatronics`, `Metrology`, `Sample Prep`, `Support Systems` |
| ④ | `brand` / `model` | Copy both from that EQ entry. `brand` is an object. Forgetting the JSON file does not change the live page; the next editor then copies the stale brand. | `"name": "[Brand]"`, `"model": "[Model]"` |
| ⑤ | `image` | Absolute URL of the photo this page shows. The `<img>` may use `../Images/…`. JSON-LD must not. A relative image still shows on the page and gives crawlers nothing. | `https://nano.nau.edu/Images/[ImageFile]` |
| ⑥ | `additionalProperty` | Specs the page shows, as `PropertyValue` pairs. End with a property named `Operational status`. The hero badge is not read from this block. `/JS/Equipment_Status.js` draws it from `/equipment.json` (`Available`, or a short date when status is `EXPECTED`). This property is a separate sentence. Changing `equipment.json` does not update it, and the page still looks fine. | `Available`, or `Expected — installation scheduled for 2026-09-01` (em dash) |
| ⑦ | `isSimilarTo` | One `@id` per related-instrument card, same pages and same order. DualBeam has two cards; the other 41 pages have three. A mismatch does not break the page. Crawlers and the cards then disagree. | `https://nano.nau.edu/About_Equipment/[RelatedPage].html#product` |
| ⑧ | `dateModified` | On the `WebPage` node only. Do not put it on `Product`. Use the offset already on these pages. An old stamp still parses; the date is just wrong. | `2026-09-17T08:49:00-07:00` |

5. The category buttons are the allowed spellings, including `Mechatronics`, which no instrument uses today. `all` is not a product category. `Support Systems` is one `data-filter` value even though the label wraps onto two lines.

   **Real Code Snippet (`/Equipment.html` — catalog category buttons, lines 527–535)**

   ```html
                               <button class="filter-btn category-btn active" data-filter="all">All</button>
                               <button class="filter-btn category-btn" data-filter="Metrology">Metrology</button>
                               <button class="filter-btn category-btn" data-filter="Fabrication">Fabrication</button>
                               <button class="filter-btn category-btn" data-filter="Electrical">Electrical</button>
                               <button class="filter-btn category-btn" data-filter="Sample Prep">Sample Prep</button>
                               <button class="filter-btn category-btn" data-filter="Mechatronics">Mechatronics</button>
                               <button class="filter-btn category-btn" data-filter="Educational">Educational</button>
                               <button class="filter-btn category-btn" data-filter="Support Systems">Support
                                   Systems</button>
   ```

   You do not edit the filter function for a new instrument. It is here so you can see the case-insensitive match. An unknown `category` returns without selecting a button, so the catalog opens unfiltered and nothing tells the visitor the link was wrong.

   **Real Code Snippet (`/JS/script.js` — category query matching, lines 1357–1364)**

   ```javascript
   const applyCategoryFromUrl = () => {
       if (categoryBtns.length === 0) return false;
       const raw = new URLSearchParams(window.location.search).get('category');
       if (!raw) return false;
       const match = Array.from(categoryBtns).find(
           (btn) => (btn.getAttribute('data-filter') || '').toLowerCase() === raw.toLowerCase()
       );
       if (!match) return false;
   ```

6. Paste the three page-specific objects over the old ones, still inside `@graph`, with a comma between objects. Delete every `//` comment before you save. A comment left in the file is the same silent failure as a stray comma.

   **Example Snippet (page-specific nodes for a new instrument)**

   ```json
   {
     "@type": "WebPage",
     "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#webpage", // page URL plus #webpage
     "url": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html",
     "name": "[Title tag text]", // same text as <title>
     "isPartOf": { "@id": "https://nano.nau.edu/#website" },
     "about": { "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#product" },
     "breadcrumb": { "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#breadcrumb" },
     "description": "[Meta description]", // same string as <meta name="description"> and Product description
     "dateModified": "[YYYY-MM-DDTHH:MM:SS-07:00]" // WebPage only
   },
   {
     "@type": "BreadcrumbList",
     "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#breadcrumb",
     "itemListElement": [
       { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://nano.nau.edu/index.html" },
       { "@type": "ListItem", "position": 2, "name": "Equipment", "item": "https://nano.nau.edu/Equipment.html" },
       { "@type": "ListItem", "position": 3, "name": "[Category]", "item": "https://nano.nau.edu/Equipment.html?category=[Category]" }, // exact data-filter spelling
       { "@type": "ListItem", "position": 4, "name": "[Instrument name]" } // no item URL; same text as the h1
     ]
   },
   {
     "@type": "Product",
     "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#product",
     "name": "[Instrument name]", // h1, not the title
     "sku": "EQ-[###]", // same key you add to equipment-schema.json
     "category": "[Category]",
     "url": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html",
     "provider": { "@id": "https://nano.nau.edu/#organization" },
     "description": "[Meta description]",
     "image": "https://nano.nau.edu/Images/[ImageFile]", // absolute https, not ../Images/
     "brand": { "@type": "Brand", "name": "[Brand]" },
     "model": "[Model]",
     "additionalProperty": [
       { "@type": "PropertyValue", "name": "[Spec name]", "value": "[Spec the page shows]" },
       { "@type": "PropertyValue", "name": "Operational status", "value": "Available" } // or the Expected sentence, with an em dash
     ],
     "isSimilarTo": [
       { "@id": "https://nano.nau.edu/About_Equipment/[RelatedPage].html#product" } // one object per related card, same order
     ]
   }
   ```

7. Add the EQ entry to `/JS/equipment-schema.json` under `instruments`. Keep the three-digit form. The `notes` object at the top of that file is editor commentary (for example, when the brand is not in the `<h1>`). It is not copied into the HTML. Skipping this file does not break the new page. It only makes the copy source lie about the instrument.

   **Real Code Snippet (`/JS/equipment-schema.json` — EQ entries around the DualBeam row, lines 30–32)**

   ```json
       "EQ-023": { "brand": "NIL Technology", "model": "CNI v3.0" },
       "EQ-024": { "brand": "FEI", "model": "Quanta 3D FEG" },
       "EQ-025": { "brand": "PELCO", "model": "FlexScribe 300" },
   ```

   **Example Snippet (new EQ entry)**

   ```json
   "EQ-[###]": { "brand": "[Brand]", "model": "[Model]" }, // same brand and model as the Product node; keep the comma if another key follows
   ```

8. If you renamed a file, search `/About_Equipment/*.html` and `/Equipment.html` for the old filename inside the JSON-LD (`#product`, breadcrumb URLs, and ItemList `url`). A card you already retargeted can still leave an `@id` on the old address. Nothing on the page reports that. Then add or move the catalog row in Part 4. A new file under `/About_Equipment/` is picked up by the sitemap unless its filename is excluded; the ItemList is a separate list. Both steps are in [Add or Unlist a Page in the Sitemap](02-update-sitemap.md) and in Part 4 below. Missing the ItemList does not drop the page from the sitemap, and it does not show an error.

---

### Part 3: Update lab facts on every copy

Use this when the lab's name, address, or hours change. Do not use it to rewrite one instrument.

1. Edit `/JS/organization.json` first. Name, alternate names, description, and logo are on `organization`. Street address and hours are on `place`. Site name and language are on `website`. Do not change the `@id` values. Other nodes point at `https://nano.nau.edu/#organization`, `#place`, and `#website`. A new `@id` orphans those references, and every page still looks normal.

   **Real Code Snippet (`/JS/organization.json` — place object, the copy source, lines 23–47)**

   ```json
     "place": {
       "@type": "Place",
       "@id": "https://nano.nau.edu/#place",
       "name": "Building 98E, South Engineering Lab",
       "address": {
         "@type": "PostalAddress",
         "streetAddress": "561 E Pine Knoll Dr",
         "addressLocality": "Flagstaff",
         "addressRegion": "AZ",
         "postalCode": "86001",
         "addressCountry": "US"
       },
       "openingHoursSpecification": {
         "@type": "OpeningHoursSpecification",
         "dayOfWeek": [
           "Monday",
           "Tuesday",
           "Wednesday",
           "Thursday",
           "Friday"
         ],
         "opens": "08:00",
         "closes": "17:00"
       }
     },
   ```

2. Find and replace the same fact in `/Equipment.html` and `/About_Equipment/*.html`. Use the same method as Part 2 of [Update Site-Wide Metadata or SEO](../General/update-site-wide-metadata.md): copy the exact line from one HTML file into Find, put the new line in Replace, and review the match list before you confirm.

   The HTML copy is the inner object, indented further than the JSON file. Compare the source above with the block below. A Find string copied from `/JS/organization.json`, spaces included, matches nothing. The 43 pages then keep the old address or hours, and nothing warns you.

   **Real Code Snippet (`/About_Equipment/DualBeam_FIBSEM.html` — the same Place node inside the page, lines 43–67)**

   ```json
           {
             "@type": "Place",
             "@id": "https://nano.nau.edu/#place",
             "name": "Building 98E, South Engineering Lab",
             "address": {
               "@type": "PostalAddress",
               "streetAddress": "561 E Pine Knoll Dr",
               "addressLocality": "Flagstaff",
               "addressRegion": "AZ",
               "postalCode": "86001",
               "addressCountry": "US"
             },
             "openingHoursSpecification": {
               "@type": "OpeningHoursSpecification",
               "dayOfWeek": [
                 "Monday",
                 "Tuesday",
                 "Wednesday",
                 "Thursday",
                 "Friday"
               ],
               "opens": "08:00",
               "closes": "17:00"
             }
           },
   ```

   > [!WARNING]
   > **Copy the Find string from an HTML file, not from `/JS/organization.json`.**
   > The extra indentation means a search for the JSON file's line matches zero HTML lines. The pages stay on the old fact and still look fine. Also search only `/Equipment.html` and `/About_Equipment/*.html`. Do not replace a short word such as `NAU Nano` on its own. `"name": "NAU Nano"` appears twice in each instrument block (the organization and the website). That pair should change together. A shorter string also hits page titles and product names that merely contain those words.

3. Limit the replace to those 43 files. `/knowledge-base/` pages have their own `application/ld+json` graphs and are not copies of `/JS/organization.json`. A replace that wanders into that folder rewrites a different description of MPaCT Lab. This procedure does not cover those pages.
4. After the replace, parse one instrument page and `/Equipment.html` with the check in Let's Verify. A replace that drops a comma voids that page's whole graph and leaves the layout untouched.

   **Example Snippet (hours change, JSON file first, then the HTML line)**

   ```json
   "opens": "08:00", // edit this in organization.json first
   "closes": "[HH:MM]" // 24-hour local time; keep the comma
   ```

   **Example Snippet (find-and-replace scoped to the HTML copies)**

   ```text
   Find:
             "closes": "17:00"

   Replace:
             "closes": "[HH:MM]"

   Files to include:
   Equipment.html
   About_Equipment/*.html
   ```

   The spaces in front of `"closes"` are part of the Find string. Count them from the HTML file you copied.

---

### Part 4: Update the catalog ItemList

The `ItemList` in `/Equipment.html` is the machine-readable catalog. `numberOfItems` is 39. Positions run from 1 through 39 with no gaps, in the same order as the visible cards. The three TEM prep pages (`TEM_Prep_Disk_Grinder.html`, `TEM_Prep_Dimple_Grinder.html`, `TEM_Prep_Ion_Beam_Mill.html`) are left out on purpose. Their cards sit in an HTML comment around lines 1356–1434, so they are not on the catalog, and their URLs are not in this list. Those three pages still have their own Product blocks. Do not add them back unless you are listing the instruments again. The rest of an unlisting (sitemap, `noindex`, cards, booking lists) is [Add or Unlist a Page in the Sitemap](02-update-sitemap.md).

**Real Code Snippet (`/Equipment.html` — ItemList opening and the first entry, lines 134–146)**

```json
        {
          "@type": "ItemList",
          "@id": "https://nano.nau.edu/Equipment.html#itemlist",
          "numberOfItems": 39,
          "itemListElement": [
            {
              "@type": "ListItem",
              "position": 1,
              "url": "https://nano.nau.edu/About_Equipment/B2_AFM.html",
              "item": {
                "@id": "https://nano.nau.edu/About_Equipment/B2_AFM.html#product"
              }
            },
```

**Real Code Snippet (`/Equipment.html` — last ListItem, position 39, lines 443–452)**

```json
            {
              "@type": "ListItem",
              "position": 39,
              "url": "https://nano.nau.edu/About_Equipment/LN2_Storage_Dewars.html",
              "item": {
                "@id": "https://nano.nau.edu/About_Equipment/LN2_Storage_Dewars.html#product"
              }
            }
          ]
        }
```

The last object has no comma after it. A comma after that object, or a missing comma between two objects, voids the whole catalog graph. `/Equipment.html` still looks the same.

#### Case A: Add a ListItem

1. Open `/Equipment.html` and search for `"@type": "ItemList"`.
2. Copy one `ListItem` and place it in the same order as the new visible card.
3. Set `url` to the page and `item` `@id` to that page's `#product` id from Part 2.
4. Renumber every `position` so the values are 1, 2, 3, and so on, with no gaps and no duplicates.
5. Set `numberOfItems` to the new count. If you skip this, `JSON.parse` still succeeds and the page looks the same. The declared count is simply wrong.

   **Example Snippet (new catalog ListItem)**

   ```json
   {
     "@type": "ListItem",
     "position": 40, // renumber the whole list after you insert; do not leave two items on the same position
     "url": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html",
     "item": {
       "@id": "https://nano.nau.edu/About_Equipment/[InstrumentPage].html#product"
     }
   }
   ```

   **Example Snippet (count line)**

   ```json
   "numberOfItems": 40, // must equal how many ListItem objects are in the array
   ```

#### Case B: Remove a ListItem

1. Delete that instrument's `ListItem` from `itemListElement`.
2. Renumber `position` from 1 with no gaps.
3. Set `numberOfItems` to the new length. A skipped renumber still parses. Two items can share a position, or the count can disagree with the array, and the catalog page will not show it.
4. Removing the row does not remove the instrument page's own structured-data block. That page still describes the instrument until you unlist it with [Add or Unlist a Page in the Sitemap](02-update-sitemap.md). Leaving the Product block up is what the three TEM prep pages do today.

---

## Technical best practices and validation

* **Valid JSON, or the whole block is ignored:** No trailing commas, no comments, no smart quotes. The failure is silent on the page. `JSON.parse` in Let's Verify is what tells you.
* **Absolute `https://` URLs:** Every `url`, `@id`, `image`, and breadcrumb `item` should be an absolute `https://nano.nau.edu/...` URL. Relative paths work in the HTML and fail for crawlers.
* **Stable `@id` values:** Keep `#organization`, `#place`, `#website`, and the catalog's `#webpage` fragment. Change an instrument's `@id` only when the file name changes, and then update every reference, including other pages' `isSimilarTo` lists and the ItemList.
* **No markup for facts the page does not show:** Do not add a spec, price, award, or review that is not on that instrument page. Do not strip the shared `Place` node just because the instrument page does not print the street address. That node is the lab's address, copied on purpose. Specs belong in `additionalProperty` only when the page states them.
* **One script per page:** These pages have a single `application/ld+json` script. A second script is easy to miss, because the check below reads only the first one.

---

## Let's Verify Your Changes

PHP's built-in server does not read `.htaccess`. That does not matter here. The JSON-LD is ordinary HTML, so `php -S` is enough. You would need Apache only if you were also checking rewrites or response headers.

1. From the website root, start a local server and open the page you edited, for example `php -S 127.0.0.1:8080` and then `http://127.0.0.1:8080/About_Equipment/DualBeam_FIBSEM.html`. You can also open the file directly for this check. The script is in the HTML either way.
2. On that page, open the browser DevTools console and run:

   ```javascript
   JSON.parse(document.querySelector('script[type="application/ld+json"]').textContent)
   ```

   The selector matches the double-quoted `type="application/ld+json"` on these pages. There is one such script on each instrument page and on `/Equipment.html`, so `querySelector` is the right call. A `SyntaxError` means the block is invalid and crawlers will ignore the whole graph. The page will still look normal. `null` means you are not on a page that has the block. Expand the result and confirm `@graph` has six nodes. On an instrument page those are the three shared nodes plus `WebPage`, `BreadcrumbList`, and `Product`. On `/Equipment.html` the last three are `CollectionPage`, `BreadcrumbList`, and `ItemList`.
3. In the parsed `Product` (or the `ItemList` on the catalog), confirm `sku`, `category`, `image`, and `isSimilarTo`. Confirm `dateModified` is on `WebPage` or `CollectionPage`, not on `Product`.
4. Read the visible breadcrumb (`nav.eq-crumb`) and compare it with `BreadcrumbList`. The names should be Home, Equipment, the category, and the instrument. Home is an icon plus visually hidden text, not a visible word. The last crumb and the last `ListItem` should use the `<h1>`, and that last `ListItem` has no `item` URL.
5. Compare each related-instrument card's `href` with `isSimilarTo`. The card file name plus `#product` should equal the `@id`, in the same order. DualBeam has two cards. Every other instrument page has three.
6. On `/Equipment.html`, confirm `numberOfItems` equals the number of `ListItem` objects, the positions are 1 through that count, and the three `TEM_Prep_` URLs are absent. The order should follow the visible cards from top to bottom.
7. After the page is deployed, paste the live URL into [Google's Rich Results Test](https://search.google.com/test/rich-results) and the [Schema.org validator](https://validator.schema.org/). Google cannot fetch a `php -S` URL on your machine, so a URL test needs the public page. Before deploy, paste the script's JSON (not the HTML comments) into the Schema.org validator's code field. Treat a parse error or a schema error as a failed edit. A notice that the page is not eligible for a product rich-result card is not, by itself, a broken block. This graph is context for the instrument, not a star rating.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
