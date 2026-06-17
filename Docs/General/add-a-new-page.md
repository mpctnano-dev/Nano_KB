
# Add a New Page

**Summary:** Create a brand-new standalone HTML page in the site root and wire it into the shared header/footer layout system (and optionally the global navigation).

**When to change:** When the lab introduces a major new initiative, dedicated service track, or standalone section of the website.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 30 minutes

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/[new-page-name].html` (created) | The new HTML document and page-specific content. |
| `/includes/header.html` | Global navigation markup (only if the page should appear in the top menu). |
| `/includes/footer.html` | Global footer markup (loaded automatically; edit only if footer content must change site-wide). |
| `/JS/layout.js` | Injects `/includes/header.html` and `/includes/footer.html` into every page. |
| `/CSS/style.css` | Shared styles; add page-scoped rules here only when styles belong in the global stylesheet. |

---

## Page Shell Architecture

Every standard site page follows the same runtime pattern:

* **Header and footer** are empty placeholders (`<div id="site-header">` and `<div id="site-footer">`). `/JS/layout.js` fetches `/includes/header.html` and `/includes/footer.html` and injects them at load time.
* **Main content** lives inside `<main>...</main>`.
* **Scripts** at the bottom of `<body>` load layout first; many pages also load `/JS/script.js` for shared UI behavior.

> [!IMPORTANT]
> **Local preview requires a web server:** `layout.js` uses `fetch()`, which does not work on the `file://` protocol. Preview with Live Server (VS Code), another local HTTP server, or the deployed host.

---

## Step-by-Step Instructions

### Part 1: Duplicate a Root-Level Template

Do not create a blank HTML file from scratch. Duplicating an existing page preserves fonts, favicon, `CSS/style.css`, and the layout hooks.

1. In the repository root, choose a lean template such as `/services.html` or `/About_Us.html`.
   * Prefer **`/services.html`** for a simple content page (hero + sections, minimal page-specific JavaScript).
   * Use **`/About_Us.html`** when you need full SEO/Open Graph tags and the `<noscript>` navigation fallback.
   * Avoid **`/PTAP.html`** or **`/CHIPS_Scholars_Program.html`** unless you are building a program-style page; those files carry large inline `<style>` blocks and program-specific scripts. For that workflow, see [clone-program-page.md](clone-program-page.md) and [../Workforce/04-update-ptap-and-intel-chips-pages.md](../Workforce/04-update-ptap-and-intel-chips-pages.md).
2. Duplicate the file in the root folder.
3. Rename the copy. Use a URL-safe filename (lowercase and hyphens when possible, e.g. `industry-partners.html`). The live site also uses mixed conventions (`About_Us.html`, `Contact_Us.html`); stay consistent with nearby pages you link to.

---

### Part 2: Update `<head>` Metadata

Update the `<head>` section so the browser tab, search engines, and social previews match the new page. For a full field-by-field checklist, see [update-site-wide-metadata.md](update-site-wide-metadata.md).

At minimum, change:

| Tag | What to set |
|---|---|
| `<title>` | Page title shown in the browser tab. |
| `<meta name="description">` | Short summary for search results. |
| `<link rel="canonical" href="https://nano.nau.edu/[your-file].html">` | Canonical URL for the new page. |

If you duplicated `/About_Us.html`, also update Open Graph and Twitter meta tags (`og:title`, `og:url`, etc.) to match the new page.

Remove template-only assets from `<head>` when they no longer apply—for example, delete `<script src="JS/about.js"></script>` if you copied `/About_Us.html` but are not building another About-style page.

---

### Part 3: Rebuild Content Inside `<main>`

1. Open your new file (e.g. `/industry-partners.html`).
2. Locate `<main>` and delete the template’s inner `<section>` / `<div>` blocks, keeping only the `<main>` wrapper.
3. Add your content using semantic `<section>` elements and `.container` for horizontal alignment.

**Spacing conventions used across the site:**

* Apply **`section-padding`** on a `<section>` when the whole band needs vertical spacing (see `/degree-programs.html`).
* Or combine **`container section-padding`** on an inner `<div>` (common on equipment about pages under `/About_Equipment/`).

**Example Snippet (minimal content block)**

```html
<main>
    <section class="section-padding">
        <div class="container">
            <h1>Industry Partners</h1>
            <p>Your page introduction goes here.</p>
        </div>
    </section>
</main>
```

---

### Part 4: Verify Layout Placeholders and Scripts

Confirm these elements exist and match a working root-level page such as `/services.html`:

**Real Code Snippet (top of `<body>`)**

```html
<div id="site-header"></div>
```

**Real Code Snippet (end of `<body>`, before `</body>`)**

```html
<div id="site-footer"></div>
<script src="JS/layout.js"></script>
```

Many pages also include shared site behavior:

```html
<script src="JS/script.js"></script>
```

Include `/JS/script.js` when your page uses patterns that depend on global handlers (dropdowns, shared utilities). Omit page-specific scripts from the template (e.g. `about.js`) unless you still need them.

`layout.js` requests includes with root-absolute paths (`/includes/header.html`, `/includes/footer.html`). Root-level pages use `src="JS/layout.js"`; pages under `/About_Equipment/` use `src="../JS/layout.js"` instead.

---

### Part 5: Scope Custom Styles (When Needed)

A unique `<body>` class is **optional**. Many pages use a plain `<body>` tag; others scope styles with prefixed class names (e.g. `about-hero` on `/About_Us.html`) or a dedicated body class such as `mpact-page` on `/MPaCT.html`.

When adding custom CSS:

1. Prefer **prefixed class names** on your markup (e.g. `industry-hero`, `industry-card`) to avoid collisions.
2. Optionally add `<body class="industry-partners-page">` and namespace rules (e.g. `.industry-partners-page .industry-card { ... }`).
3. Put shared tokens in `/CSS/style.css`; use a `<style>` block in `<head>` only for substantial page-specific layout (as on `/PTAP.html`).

---

### Part 6: Register in Navigation (Optional)

If the page should appear in the top navigation, follow [update-global-navigation.md](update-global-navigation.md) and add the link in `/includes/header.html`. Use the exact filename (including case) in `href` and `data-path` attributes so active-link highlighting in `layout.js` works correctly.

---

## Layout Configuration Reference

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Header placeholder | Injection target for the global header. | `<div id="site-header"></div>` |
| ② | Footer placeholder | Injection target for the global footer. | `<div id="site-footer"></div>` |
| ③ | Layout script | Loads includes and initializes nav/dropdowns. | `<script src="JS/layout.js"></script>` |
| ④ | Shared script (optional) | Site-wide UI helpers. | `<script src="JS/script.js"></script>` |
| ⑤ | Body scope (optional) | Extra isolation for custom CSS. | `<body class="industry-partners-page">` |

---

## Design Impact & Layout Solutions

* **Broken header/footer:** Missing `site-header` / `site-footer` divs, wrong script path (`JS/` vs `../JS/`), or opening the file via `file://` instead of a local server.
* **Stale template content or scripts:** Leftover sections, forms, or `<script src="JS/about.js">` from the duplicated file can cause errors or wrong behavior.
* **Navigation link 404:** `href` in `/includes/header.html` does not match the actual filename (case-sensitive on many servers).
* **Recommended solutions:**
  * Compare your file side-by-side with `/services.html` before publishing.
  * Remove unused template scripts and inline handlers.
  * Test the final URL path on a local server before deployment.

---

## Let's Verify Your Changes

1. Open the new page through a local web server (for example, Live Server in VS Code).
2. Confirm the global header and footer render and that navigation dropdowns work.
3. Confirm your content sits inside `.container` (or equivalent layout wrappers) and spacing looks consistent with peer pages.
4. If you added nav links, click them from another page and confirm the new page loads and shows the correct active state.
5. If you updated SEO tags, spot-check the `<title>` and description in the browser tab and, if applicable, a social preview debugger.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 27, 2026
