
# Update Global Navigation Menu

**Summary:** Add, remove, or modify links, dropdown grids, and mega-menu items in the site-wide top navigation header bar.

**When to change:** When a new prominent page is introduced, or the site hierarchy is reorganized to improve user navigation.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 15 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/Global_nav.png" alt="Global navigation bar with brand, menu items, and Contact button" /><br/>
  <em>Figure 1: Site-wide top navigation — Home icon, MPaCT Lab, Degree Programs, Workforce, News, About, and Contact CTA</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/includes/header.html` | Navigation menu markup loaded into every page. |
| `/JS/layout.js` | Fetches the header, injects it into `#site-header`, highlights active links from `data-path`, and wires mobile menu + dropdown behavior. |
| `/CSS/style.css` | Nav bar, mega-menu, and mobile drawer styles (edit only if you are changing appearance, not link targets). |

> [!NOTE]
> **Client-side include architecture:** Each page includes `<div id="site-header"></div>` and loads `JS/layout.js` (root pages use `JS/layout.js`; pages in subfolders such as `/About_Equipment/` use `../JS/layout.js`). On load, `layout.js` runs `loadPart('site-header', '/includes/header.html', …)` and then `highlightActiveLink`, `initMobileMenu`, and `initAllDropdowns`. Editing `/includes/header.html` once updates navigation on every page that uses this layout. See [add-a-new-page.md](add-a-new-page.md) when adding a new page to the site.

> [!IMPORTANT]
> **Verify against your local codebase:** Class names, dropdown IDs, `href` targets, and hash anchors can change. Before editing, confirm selectors and filenames in your active copy of `/includes/header.html`, and confirm any `href="#…"` fragment matches a real `id` on the destination page.

---

## Step-by-Step Instructions

### Part 1: Locate the Navigation Menu Structure

1. Open `/includes/header.html` in your code editor.
2. Search for the navigation container: `<nav class="nav-menu">`. All top-level items live inside this element.

   **Real Code Snippet (navigation container)**
   ```html
   <nav class="nav-menu">
   ```

3. Inside `<nav class="nav-menu">`, each entry is a `<div class="nav-item">`. There are three patterns in the live file:
   * **Icon-only Home** — `nav-link nav-link--icon` (no text label).
   * **Mega-menu dropdowns** — `nav-item nav-item--has-dropdown` with a `<button class="nav-dropdown-btn">` and a `.nav-mega-panel` grid (MPaCT Lab, Degree Programs, Workforce).
   * **Plain links & CTA** — direct `<a class="nav-link">` or the gold Contact button (`btn btn-sm btn-gold`).

---

### Part 2: Add or Edit Standard Top-Level Links

Use this pattern for simple bar links such as **News** and **About** (not dropdown parents).

1. Copy an existing `nav-item` block and paste it inside `<nav class="nav-menu">` in the order you want.
2. Update `href`, visible link text, and `data-path`.

   **Real Code Snippet (News link in header.html)**
   ```html
   <div class="nav-item">
       <a href="/News.html" class="nav-link"
           data-path="News.html">News</a>
   </div>
   ```

   **Example Snippet (new top-level link blueprint)**
   ```html
   <div class="nav-item">
       <a href="/[YourPage].html" class="nav-link"
           data-path="[YourPage].html">[Link Label]</a>
   </div>
   ```

3. **Active highlighting:** `data-path` must match the current page’s HTML filename exactly (e.g. `News.html`). In `layout.js`, `highlightActiveLink()` compares `link.dataset.path` to `window.location.pathname.split('/').pop()` (query strings and hash fragments are not part of `data-path`). The gold **Contact** button has no `data-path` and is not auto-highlighted.

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Target URL (`href`) | Destination path. Use a leading slash for site-root pages. | `href="/News.html"` |
| ② | Active path (`data-path`) | Filename used for active-state matching. | `data-path="News.html"` |
| ③ | Link class | Standard text links use `nav-link`. | `class="nav-link"` |
| ④ | Visible label | Text shown in the bar. | `News` |

---

### Part 3: Add or Edit Mega-Menu Dropdown Links

Dropdown items live inside `.nav-dropdown-grid` (MPaCT Lab uses the full grid; Degree Programs and Workforce use `.nav-dropdown-grid--single`).

1. Open the parent section you need (`id="mpactDropdownItem"`, `id="degreeDropdownItem"`, or `id="wfdDropdownItem"`).
2. Inside the matching `.nav-dropdown-grid`, insert or edit a `.nav-dropdown-link` anchor.

   **Real Code Snippet (Equipment Catalog card in MPaCT dropdown)**
   ```html
   <a href="/Equipment.html" class="nav-dropdown-link"
       data-path="Equipment.html" role="menuitem">
       <div class="nav-dropdown-icon">
           <svg width="22" height="22" viewBox="0 0 24 24" fill="none"
               stroke="currentColor" stroke-width="2" stroke-linecap="round"
               stroke-linejoin="round">
               <path d="M14.7 6.3a1 1 0 0 0 0 1.4l1.6 1.6a1 1 0 0 0 1.4 0l3.77-3.77a6 6 0 0 1-7.94 7.94l-6.91 6.91a2.12 2.12 0 0 1-3-3l6.91-6.91a6 6 0 0 1 7.94-7.94l-3.76 3.76z"></path>
           </svg>
       </div>
       <div class="nav-dropdown-text">
           <strong>Equipment Catalog</strong>
           <small>35+ instruments</small>
       </div>
   </a>
   ```

   **Example Snippet (new mega-menu card blueprint)**
   ```html
   <a href="/[TargetPage].html" class="nav-dropdown-link"
       data-path="[TargetPage].html" role="menuitem">
       <div class="nav-dropdown-icon">
           <!-- Copy an existing <svg> from a sibling card, or add a new icon -->
       </div>
       <div class="nav-dropdown-text">
           <strong>[Primary Label]</strong>
           <small>[Short subtitle]</small>
       </div>
   </a>
   ```

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Target URL (`href`) | Page path or hash link. Use a leading slash for root pages. | `href="/services.html"` or `href="/CareerPathways.html#degree-programs"` |
| ② | Active path (`data-path`) | HTML filename only (no `#` fragment, no `?query`). | `data-path="CareerPathways.html"` |
| ③ | ARIA role (`role`) | Marks the control as a menu item for assistive tech. | `role="menuitem"` |
| ④ | Primary label | Bold card title. | `<strong>Services</strong>` |
| ⑤ | Subtitle | Supporting line in smaller type. | `<small>3D Printing &amp; Support</small>` |

> [!TIP]
> **Anchor links in dropdowns:** Degree Programs and Workforce menus use hash links (e.g. `/CareerPathways.html#degree-programs`, `/WorkForceDevelopment.html#training`). Put only the filename in `data-path`; put the fragment in `href`. On `/CareerPathways.html`, hash routing is handled by that page’s own script (`#degree-programs`, `#professional-development`, and aliases documented there).

> [!WARNING]
> **Hash targets must exist on the destination page:** Before adding or changing a `#fragment`, open the target HTML and confirm a matching `id` (or page-specific hash handler). For example, the live **Labs** item uses `href="/degree-programs.html#labs"` but `/degree-programs.html` has no `id="labs"` at review time—update the page `id` or change the `href` so the link scrolls correctly.

---

### Part 4: Enable Disabled “Soon” Placeholder Links

Some mega-menu cells are non-clickable placeholders (`nav-dropdown-link--disabled`). The live MPaCT menu includes **Safety & Training** and **Billing Rates** in this state.

1. Locate the disabled block inside the relevant `.nav-dropdown-grid`.

   **Real Code Snippet (disabled Safety & Training placeholder)**
   ```html
   <div class="nav-dropdown-link nav-dropdown-link--disabled"
       role="menuitem" aria-disabled="true">
       <div class="nav-dropdown-icon">
           <!-- svg omitted for brevity -->
       </div>
       <div class="nav-dropdown-text">
           <strong>Safety &amp; Training <span class="nav-soon-badge">Soon</span></strong>
           <small>Certifications &amp; SOPs</small>
       </div>
   </div>
   ```

2. To publish the link, convert the outer `<div>` to an `<a>`, remove the disabled modifier and `aria-disabled`, add `href` and `data-path`, and remove the `nav-soon-badge` span.

   **Example Snippet (enabled link after conversion)** — `/Safety_Training.html` exists in the repo; the nav item is still disabled in `/includes/header.html` until you apply this change.
   ```html
   <a href="/Safety_Training.html" class="nav-dropdown-link"
       data-path="Safety_Training.html" role="menuitem">
       <div class="nav-dropdown-icon">
           <!-- reuse the existing svg from the disabled block -->
       </div>
       <div class="nav-dropdown-text">
           <strong>Safety &amp; Training</strong>
           <small>Certifications &amp; SOPs</small>
       </div>
   </a>
   ```

---

## Design Impact & Layout Solutions

* **The design discrepancy risk:** Too many top-level items wrap the menu onto a second line, breaking header alignment or overlapping page content.
* **Visual impact:** Double-row headers look cluttered and unprofessional.
* **Recommended solutions:**
    * **Avoid crowding the bar:** The live header already has seven top-level slots (Home icon, MPaCT Lab, Degree Programs, Workforce, News, About, Contact). Prefer nesting new destinations inside an existing mega-menu instead of adding another bar item.
    * **Use dropdowns:** Group subtopics under MPaCT Lab, Degree Programs, or Workforce.
    * **Active state consistency:** Keep `data-path` aligned with real filenames. `layout.js` also forces the Degree Programs trigger active on `degree-programs.html` (and `degree-programs-grid.html` if present) and the Workforce trigger active on `WorkForceDevelopment.html`, even when the matching dropdown card is not the exact link clicked.

---

## Let's Verify Your Changes

1. Preview the site over HTTP (Live Server or your deploy host). `fetch()` in `layout.js` does not work on `file://`.
2. Confirm new or edited links appear in the top bar and navigate to the correct page.
3. Load the target page and confirm the correct item shows the active state.
4. **Mobile check:** Narrow the viewport, open the hamburger menu, and confirm new items appear and are tappable in the mobile drawer.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: May 27, 2026*
