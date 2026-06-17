
# Clone a Program Page

**Summary:** Duplicate the existing PTAP or Intel CHIPS Scholars program template to bootstrap new workforce training or scholarship program pages with the same hero panel, partnership strip, Bento Grid, and milestone layouts.

**When to change:** When the lab introduces a new corporate partnership training program, workforce certification, or specialized scholarship track that requires a dedicated, premium subpage.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 35 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/wf_PTAP.png" alt="PTAP program page" /><br/>
  <em>Figure 1: PTAP program page — master template for apprenticeship-style layouts</em>
</p>

<p align="center">
  <img src="/md_file_images/Intel_chips.png" alt="Intel CHIPS Scholars program page" /><br/>
  <em>Figure 2: Intel CHIPS Scholars program page — cloned from the PTAP structure with a unique `intel-*` class namespace</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/PTAP.html` | Master template for apprenticeship-style pages; carries page-specific styles in an inline `<style>` block and `ptap-*` class prefixes. |
| `/CHIPS_Scholars_Program.html` | Reference clone of PTAP for scholarship cohorts; uses `intel-*` classes defined in `/CSS/style.css` and includes the `#scholarshipForm` registration block. |
| `/CSS/style.css` | Shared stylesheet; houses `intel-*` program styles (and any new namespace you add when cloning from CHIPS). |
| `/includes/header.html` | Global navigation markup — register the new page in the appropriate mega-menu dropdown. |
| `/IntelScholarshipSubmission.php` | Server-side mail handler for scholarship form submissions (only when cloning a page with a registration form). |
| `/JS/layout.js` | Injects `/includes/header.html` and `/includes/footer.html` into every page at load time. |

> [!NOTE]
> **Two styling patterns in the live codebase:** `/PTAP.html` keeps its layout CSS in a large inline `<style>` block inside `<head>`. `/CHIPS_Scholars_Program.html` reuses the same visual patterns but stores styles under `intel-*` selectors in `/CSS/style.css`. When cloning, pick the source file that best matches your target page type, then stay consistent with that pattern.

> [!IMPORTANT]
> **Local preview requires a web server:** `layout.js` uses `fetch()`, which does not work on the `file://` protocol. Preview with Live Server (VS Code), another local HTTP server, or the deployed host.

---

## Step-by-Step Instructions

### Part 1: Duplicate and Rename the Template File

1. In the repository root, choose a source template:
   * **`/PTAP.html`** — apprenticeship or certification pages that do not need a registration form.
   * **`/CHIPS_Scholars_Program.html`** — scholarship or cohort-application pages that include the `#scholarshipForm` block and validation scripts.
2. Duplicate the file and rename the copy. Use a URL-safe filename consistent with nearby pages (the live site uses mixed conventions such as `About_Us.html` and `CHIPS_Scholars_Program.html`).

   **Example filenames:**
   * `semiconductor-fundamentals.html`
   * `CHIPS_Scholars_Program.html` (existing Intel CHIPS Scholars page)

3. Do not use spaces in the filename; many web servers treat paths case-sensitively, so match the exact casing you will use in navigation `href` attributes.

---

### Part 2: Update SEO Metadata and Page Details

Each program page owns its own `<head>` metadata. For field-by-field guidance and social-tag conventions, see [update-site-wide-metadata.md](update-site-wide-metadata.md).

1. Open your newly created program page in your code editor.
2. Inside `<head>`, update the document title, meta description, and canonical URL to match the new program.

   **Real Code Snippet (`/CHIPS_Scholars_Program.html` — basic SEO head)**
   ```html
   <meta name="description" content="The NAU Intel-SRC CHIPS Scholars Program — a funded 12-student research cohort at NAU and the University of Arizona training engineers in advanced packaging, manufacturing process metrology, and co-packaged optics for Arizona's semiconductor industry.">
   <title>CHIPS Scholars Program | NAU & University of Arizona | NAU Nano</title>
   <link rel="canonical" href="https://nano.nau.edu/CHIPS_Scholars_Program.html">
   ```

   **Example Snippet (new program page blueprint)**
   ```html
   <meta name="description" content="[One-sentence summary of the program for search snippets.]">
   <title>[Program Name] | MPaCT Lab | NAU Nano</title>
   <link rel="canonical" href="https://nano.nau.edu/[YourPage].html">
   ```

3. Confirm `<link rel="icon">` still points to `Images/NAU_Nano_favicon.png` and that stylesheet paths remain correct (`CSS/style.css` for root-level pages).

---

### Part 3: Namespace Styles and Theme Colors

Cloning copies hundreds of class names tied to the source prefix (`ptap-*` or `intel-*`). Reusing the same prefix on a second page risks CSS collisions.

#### Option A: Cloned from `/PTAP.html` (inline styles)

1. Scroll to the `<style>` block inside `<head>`.
2. Search-and-replace the prefix across the file — for example, `ptap-` → `myprog-` — in both the CSS rules and the matching HTML `class` attributes.
3. Adjust accent colors inside the renamed rules (`.myprog-bento__accent--gold`, icon themes, HSL background values) to match the new partner branding.

   **Example Snippet (accent theme override inside inline `<style>`)**
   ```css
   .myprog-bento__accent--blue {
       background: linear-gradient(90deg, var(--nau-blue), #0066cc);
   }
   ```

#### Option B: Cloned from `/CHIPS_Scholars_Program.html` (shared stylesheet)

1. Copy the `intel-*` rule blocks in `/CSS/style.css` and rename the prefix (e.g. `intel-` → `acme-`) in both the CSS and your new HTML file.
2. Keep new rules grouped and commented so future editors can find them alongside the existing `intel-*` section.

> [!TIP]
> **Keep watermarks compact:** When theming stat cards, keep bento watermark text short (e.g. `$15K`, `12`, `9+3`) to prevent overflow on tablet breakpoints. See [04-update-ptap-and-intel-chips-pages.md](../Workforce/04-update-ptap-and-intel-chips-pages.md) for bento modifier reference.

---

### Part 4: Swap Out Section Content

After namespacing, replace placeholder copy, logos, and stats section by section. Full editing guidance for each block lives in [04-update-ptap-and-intel-chips-pages.md](../Workforce/04-update-ptap-and-intel-chips-pages.md).

| Section | Container to locate | What to change |
|---|---|---|
| **Hero quick-info panel** | `<div class="ml-hero__panel">` | Stipend, cohort size, and timeline stats inside `.ml-highlight__title` and `.ml-highlight__desc`. |
| **Partnership strip** | `.ptap-partners__logos` or `.intel-partners__logos` | Sponsor `<img>` tags with class `ptap-partners__logo` or `intel-partners__logo`. |
| **Bento Grid highlights** | `.ptap-bento__grid` or `.intel-bento__grid` | Narrative cards (Option A) and feature stat cards (Option B); apply layout modifiers (`--wide`, `--narrow`, `--third`) and reveal delays (`ptap-d1`–`ptap-d5` or equivalent). |
| **Coursework / focus lists** | `.ptap-coursework__list` or `.intel-focus__list` | Individual `.ptap-coursework__item` or `.intel-focus__item` entries. |
| **How It Works milestones** | `.ptap-pathway__steps` | Step copy inside `.ptap-pathway__step` elements; keep paragraph lengths balanced. |

---

### Part 5: Register the Page in Global Navigation

Follow [update-global-navigation.md](update-global-navigation.md) to add the page to the site-wide menu. Program pages typically belong in the **Workforce** dropdown (`id="wfdDropdownItem"`) or **Degree Programs** dropdown (`id="degreeDropdownItem"`), depending on audience.

1. Open `/includes/header.html`.
2. Locate the appropriate `.nav-dropdown-grid` inside the target mega-menu.
3. Insert a new `.nav-dropdown-link` card matching the live pattern.

   **Real Code Snippet (PTAP entry in Workforce dropdown)**
   ```html
   <a href="/PTAP.html" class="nav-dropdown-link"
       data-path="PTAP.html" role="menuitem">
       <div class="nav-dropdown-icon">
           <svg width="22" height="22" viewBox="0 0 24 24" fill="none"
               stroke="currentColor" stroke-width="2" stroke-linecap="round"
               stroke-linejoin="round">
               <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"></path>
               <polyline points="9 12 11 14 15 10"></polyline>
           </svg>
       </div>
       <div class="nav-dropdown-text">
           <strong>PTAP Apprenticeship</strong>
           <small>2-year program with TSMC</small>
       </div>
   </a>
   ```

   **Example Snippet (new program mega-menu card blueprint)**
   ```html
   <a href="/[YourPage].html" class="nav-dropdown-link"
       data-path="[YourPage].html" role="menuitem">
       <div class="nav-dropdown-icon">
           <!-- Copy an existing <svg> from a sibling card -->
       </div>
       <div class="nav-dropdown-text">
           <strong>[Program Name]</strong>
           <small>[Short subtitle]</small>
       </div>
   </a>
   ```

4. Set `data-path` to the HTML filename only (no leading slash, no `#` fragment) so `layout.js` active-link highlighting works correctly.

---

### Part 6: Wire Up a Registration Form (Scholarship Pages Only)

Skip this part when cloning an apprenticeship-style page from `/PTAP.html`. When cloning from `/CHIPS_Scholars_Program.html`:

1. Keep the form element id `#scholarshipForm` and ensure every `<input>` / `<textarea>` `name` attribute matches the `$detailFields` array in `/IntelScholarshipSubmission.php`.
2. Confirm the form `action` points to `/IntelScholarshipSubmission.php` and that `/JS/validation.js` is loaded before the page-scoped validation script at the bottom of `<body>`.
3. For validation behavior, field limits, and mailer configuration details, see **Part 6** in [04-update-ptap-and-intel-chips-pages.md](../Workforce/04-update-ptap-and-intel-chips-pages.md) and the PHP mailer configuration guide (not yet documented in this knowledge base).

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Reusing an existing class prefix (`ptap-*` or `intel-*`) on a second page causes CSS rule collisions; mismatched form `name` attributes between HTML and PHP block scholarship submissions.
*   **Visual Impact:** Stretched bento tiles, overlapping hero titles, broken scroll-reveal timing, or red error outlines that persist after the user corrects inputs.
*   **Recommended Solutions:**
    *   **Namespace every clone:** Assign a unique prefix to all program-specific classes and keep styles scoped to that prefix (inline `<style>` or a dedicated block in `/CSS/style.css`).
    *   **Validate names and endpoints:** Align form field `name` values byte-for-byte with `/IntelScholarshipSubmission.php` before deploying a scholarship page.
    *   **Cross-check navigation:** Confirm `href` and `data-path` in `/includes/header.html` match the new filename exactly (case-sensitive on many servers).

---

## Let's Verify Your Changes

1. Open your new page through a local web server (for example, Live Server in VS Code) — not via `file://`.
2. Confirm the global header and footer render and that your new link appears in the correct mega-menu dropdown.
3. Scroll the full page and verify hero stats, partnership logos, bento cards, and milestone steps display with the intended theme colors.
4. Test all Call-to-Action buttons and in-memory anchors (e.g. `#register`) resolve to the correct sections.
5. If the page includes `#scholarshipForm`, trigger an empty submit to confirm validation outlines appear, then submit dummy data and verify the success state resets cleanly.
6. Spot-check `<title>` and meta description in the browser tab; confirm the canonical URL matches the deployed path.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 28, 2026
