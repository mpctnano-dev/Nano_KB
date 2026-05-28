
# Update Site-Wide Metadata or SEO

**Summary:** Modify page descriptions, document titles, canonical URLs, or social Open Graph / Twitter tags to improve search snippets and link-preview cards.

**When to change:** When marketing requests refreshed social preview cards, or an SEO audit recommends stronger titles and descriptions.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 30 minutes (single page) to several hours (shared image or wording across many files)

---

## Visual Reference

<p align="center">
  <em>Figure 1: Primary homepage metadata in <code>/index.html</code> — full Open Graph and Twitter stack</em>
</p>

**Real Code Snippet (`/index.html` — social-ready homepage head)**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description"
        content="Explore NAU Nano, Northern Arizona University's shared-use hub for microelectronics research, nanotechnology innovation, semiconductor training, and industry collaboration across Arizona.">
    <meta name="theme-color" content="#002454">
    <title>Microelectronics & Nanotechnology | NAU Nano</title>
    <link rel="canonical" href="https://nano.nau.edu/index.html">
    <link rel="icon" type="image/png" href="Images/NAU_Nano_favicon.png">

    <!-- Open Graph / Social Sharing -->
    <meta property="og:type" content="website">
    <meta property="og:site_name" content="NAU Nano">
    <meta property="og:title" content="Microelectronics & Nanotechnology | NAU Nano">
    <meta property="og:description" content="Northern Arizona University's shared-use hub for microelectronics research, nanotechnology innovation, semiconductor training, and industry collaboration across Arizona.">
    <meta property="og:url" content="https://nano.nau.edu/index.html">
    <meta property="og:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">
    <meta property="og:image:alt" content="NAU sign on the Flagstaff campus in winter">
    <meta property="og:locale" content="en_US">
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="Microelectronics & Nanotechnology | NAU Nano">
    <meta name="twitter:description" content="NAU's shared-use hub for microelectronics research, semiconductor training, and industry collaboration.">
    <meta name="twitter:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">
</head>
```

> [!NOTE]
> In the repository, long `content` attributes are sometimes wrapped onto multiple lines (as with `<meta name="description">` above). That is valid HTML; keep the full string intact when editing.

---

## Target Files

Metadata lives in each page’s own `<head>`. It is **not** injected by `/JS/layout.js` (that script only loads `/includes/header.html` and `/includes/footer.html`).

### Pages with full Open Graph + Twitter tags

Use these when updating the **default social preview image** or other shared `og:*` / `twitter:*` values:

| Path | Purpose |
|---|---|
| `/index.html` | Homepage; primary site branding and default OG image URL. |
| `/About_Us.html` | About overview; page-specific `og:title` / `og:url`. |
| `/Contact_Us.html` | Contact form; uses `twitter:card` value `summary` (not `summary_large_image`). |
| `/degree-programs.html` | Degree programs overview. |
| `/Equipment.html` | Equipment catalog landing page. |
| `/MPaCT.html` | MPaCT Lab facility overview. |
| `/News.html` | News and announcements index. |
| `/WorkForceDevelopment.html` | Workforce development hub. |

### Root pages with basic SEO only (`<title>`, `<meta name="description">`, `<link rel="canonical">`)

These pages do **not** currently include Open Graph tags unless you add them. Global find-and-replace on `og:image` will **not** match here.

| Path | Notes |
|---|---|
| `/Academics.html` | Placeholder page — currently has `<title>` and favicon only (no description or canonical yet). |
| `/CareerPathways.html` | Career pathways map; title, description, and canonical. |
| `/CHIPS_Scholars_Program.html` | Intel CHIPS Scholars program. |
| `/EquipmentRequest.html` | Instrument lease / request form. |
| `/Lab_Calendar.html` | Lab calendar. |
| `/PTAP.html` | Process Technician Apprenticeship Program. |
| `/Rates.html` | Billing and recharge rates. |
| `/Research_Publications.html` | Publications library. |
| `/Reserve_Equipment.html` | Equipment reservation form. |
| `/Safety_Training.html` | Safety and training overview. |
| `/ServiceRequest.html` | Fabrication / service request portal. |
| `/services.html` | External services catalog. |

### Equipment detail pages

| Path | Notes |
|---|---|
| `/About_Equipment/*.html` | **41** instrument pages (e.g. `SEM.html`, `TEM.html`, `XRD.html`, `MPI_TS200.html`). Each has its own `<title>`, `<meta name="description">`, and `<link rel="canonical">`. Paths use `../Images/` and `../CSS/` because files live in a subfolder. **No Open Graph tags** on these pages today. |

> [!WARNING]
> **Head metadata is page-specific.** Crawlers and social scrapers read each HTML file’s `<head>` directly. A change to the default preview image must be applied on every file that contains `og:image` (the eight social-ready pages listed above), not only on `index.html`. Changing `<title>` or `<meta name="description">` for the whole site still requires editing **each** page that should rank or preview differently — there is no single shared SEO include.

> [!IMPORTANT]
> **Verify against your local codebase:** Filenames, canonical URLs, and which pages include OG tags can change. Before a global replace, search the repo for the exact string you plan to change (e.g. `og:image` or `NAU-nano-og-image.jpg`).

---

## Step-by-Step Instructions

### Part 1: Locate and review SEO meta tags

1. Open the target page in your editor.
2. Scroll to the `<head>` block at the top of the file (above `<body>`).
3. Identify whether the page uses the **full social stack** (Open Graph + Twitter) or **basic SEO only** (see tables above).

**Real Code Snippet (equipment detail page — `/About_Equipment/SEM.html`)**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JEOL FE-SEM | Scanning Electron Microscope | MPaCT Lab</title>
    <meta name="description" content="Examine surface morphology, microstructure, and elemental composition with the JEOL JSM-IT710HR FE-SEM at MPaCT Lab for semiconductor, materials, and device characterization.">
    <link rel="canonical" href="https://nano.nau.edu/About_Equipment/SEM.html">
    <link rel="icon" type="image/png" href="../Images/NAU_Nano_favicon.png">
    ...
</head>
```

**Example Snippet (blueprint for a new or updated page head)**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="[One- or two-sentence summary under ~160 characters.]">
    <meta name="theme-color" content="#002454">
    <title>[Page Topic] | [Section or NAU Nano]</title>
    <link rel="canonical" href="https://nano.nau.edu/[YourPage].html">
    <link rel="icon" type="image/png" href="Images/NAU_Nano_favicon.png">

    <!-- Optional: copy from /index.html or /About_Us.html if this page needs social cards -->
    <meta property="og:type" content="website">
    <meta property="og:site_name" content="NAU Nano">
    <meta property="og:title" content="[Social headline]">
    <meta property="og:description" content="[Social summary.]">
    <meta property="og:url" content="https://nano.nau.edu/[YourPage].html">
    <meta property="og:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">
    <meta property="og:image:alt" content="[Accessible description of preview image]">
    <meta property="og:locale" content="en_US">
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="[Social headline]">
    <meta name="twitter:description" content="[Shorter social summary.]">
    <meta name="twitter:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">
</head>
```

For equipment pages under `/About_Equipment/`, use `href="../Images/NAU_Nano_favicon.png"` and canonical URLs that include the `/About_Equipment/` path segment.

#### Fields to update

| # | What to change | Description | Example (placeholder) |
|---|---|---|---|
| ① | `<title>` | Browser tab label and primary search link text. | `<title>Services \| MPaCT Lab \| NAU Nano</title>` |
| ② | `<meta name="description">` | Search result snippet; keep under ~160 characters. | `content="Submit designs or samples to the MPaCT Lab..."` |
| ③ | `<link rel="canonical">` | Preferred URL for indexing; must match the deployed file path. | `href="https://nano.nau.edu/services.html"` |
| ④ | `og:title` | Headline on LinkedIn and other OG consumers (social-ready pages only). | `content="Contact NAU Nano \| Equipment Access & Collaboration"` |
| ⑤ | `og:description` | Supporting text on social preview cards. | `content="Reach NAU Nano for equipment access..."` |
| ⑥ | `og:url` | Canonical share URL; should match this page’s live URL. | `content="https://nano.nau.edu/Contact_Us.html"` |
| ⑦ | `og:image` / `twitter:image` | Preview image; must be an absolute `https://` URL. | `content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg"` |
| ⑧ | `twitter:card` | Card layout (`summary_large_image` or `summary`). | `content="summary_large_image"` |

The site does **not** use `<meta name="keywords">`; rely on title and description instead.

---

### Part 2: Site-wide find-and-replace (shared OG image or repeated strings)

Use this when the **same literal tag line** appears on multiple files (typical for updating the default `og:image` on the eight social-ready pages).

1. Open your editor’s global Search/Replace panel (e.g. `Ctrl + Shift + F` or `Cmd + Shift + F` in VS Code).
2. Limit the search scope to `*.html` in the repository root (and run a second pass on `/About_Equipment/*.html` only if you have added matching tags there).
3. Paste the **exact** existing line into **Find** (copy from one source file to avoid typos).
4. Paste the updated line into **Replace**.
5. Review the match list before confirming — do not replace inside `/how-to/manual-export.html` or other non-production HTML unless intended.

**Example Snippet (VS Code search/replace layout)**

```text
Find:
<meta property="og:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">

Replace:
<meta property="og:image" content="https://nano.nau.edu/Images/new-preview-card.jpg">

Files to include:
*.html
```

**Real Code Snippet (current default OG image line on social-ready pages)**

```html
<meta property="og:image" content="https://nano.nau.edu/Images/NAU-nano-og-image.jpg">
```

**Example Snippet (after you upload a new preview asset)**

```html
<meta property="og:image" content="https://nano.nau.edu/Images/new-preview-card.jpg">
```

Also update matching `twitter:image` lines on the same pages when you change the shared artwork.

---

## Technical best practices and validation

* **Absolute URLs for social images:** Platforms cannot resolve relative paths such as `Images/NAU-nano-og-image.jpg` in `og:image` or `twitter:image`. Use full `https://nano.nau.edu/...` URLs.
* **Preview image size:** Target roughly **1200 × 630** pixels (about 1.91:1) so cards are not over-cropped.
* **Length limits:** Aim for **≤ 60 characters** in `<title>` and **≤ 160 characters** in meta descriptions to reduce truncation in search results.
* **Canonical consistency:** The canonical `href` should match the real filename (e.g. `Reserve_Equipment.html` vs legacy names). Fix mismatches when you touch a page.
* **Per-page copy:** Titles and descriptions should describe **that** page’s content. Do not paste the homepage description onto every file.

---

## Let's verify your changes

1. **View page source:** Open the page in a browser, right-click, and choose **View Page Source**. Confirm `<title>`, description, canonical, and any `og:*` / `twitter:*` tags match your intent.
2. **HTML sanity:** Ensure `<head>` is closed with `</head>` and that every edited tag still has matching quotes and closing `>`.
3. **Social debuggers (after deploy):** Paste the **live** URL into:
   * [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)
   * [Twitter/X Card Validator](https://cards-dev.twitter.com/validator)
4. **Search spot-check:** After deployment, search for the site or page title and confirm the updated snippet appears (caches can take time to refresh).

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: May 27, 2026*
