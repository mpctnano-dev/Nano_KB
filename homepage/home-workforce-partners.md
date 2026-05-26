# Home Page · Workforce Development — Industry Partners
**File:** `index.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

The Workforce Development section includes a white partner card grid on the right side. Each card shows a company logo and a partner-type label. The grid is **hardcoded in HTML** — no CMS or data file feeds it.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Partner grid | `class="partners-grid"` | Add, remove, or reorder partner cards |
| Individual partner card | `class="partner-card"` | Logo image and partner label |

> **Note:** The grid is styled as a strict **2-column layout** (`grid-template-columns: repeat(2, 1fr)` in `CSS/style.css`). This means an odd number of logos (3 or 5) will leave the last row with one card and one empty cell, which looks broken. See Section 3 for the safe approach.

---

## SECTION 1 — Adding a New Partner Logo

### Step 1 — Prepare the logo image
See **Section 4** for image specifications before you start. Logo files must be in the `Images/` folder before you reference them.

### Step 2 — Find the partner grid
In `index.html`, press **Ctrl+F** and search for:
```
class="partners-grid"
```

### Step 3 — Paste the card template
Add a new `<div class="partner-card">` block **inside** `class="partners-grid"`, after the last existing partner card's closing `</div>`:

```html
<div class="partner-card">
    <img src="Images/YourPartnerLogo.png" class="partner-logo" alt="Partner Name Logo">
    <span class="partner-label">Industry Partner</span>
</div>
```

### Step 4 — Fill in the two values

| What | Where | Notes |
|------|-------|-------|
| Logo file | `src="Images/???"` | Filename must match exactly — case sensitive on the server |
| Alt text | `alt="???"` | Describe the logo: `"Intel Logo"`, `"TSMC Logo"`, etc. |
| Partner label | `<span class="partner-label">` | Short label shown below the logo — see common options below |

**Common label options:** `Premier Partner` · `Industry Partner` · `Co-Op Program` · `Research Partner` · `Founding Sponsor`

### Step 5 — Verify in the browser
Open `index.html`, scroll to Workforce Development, and confirm:
- The logo appears and is not broken (no missing image icon)
- The logo is not too large or too small relative to the others
- The grid still looks balanced

> **Please double-check:** The logos render grayscale by default and colorize on hover — this is intentional CSS behavior. Do not attempt to override it unless the partner specifically requests full-color display at all times.

---

## SECTION 2 — Replacing an Existing Partner Logo

### Step 1 — Find the card to replace
Search Ctrl+F for the company name in the `alt` attribute, for example:
```
alt="Intel Logo"
```

### Step 2 — Swap the image file
Replace the `src=""` value with the new logo filename. Place the new file in `Images/` first.

```html
<!-- Before -->
<img src="Images/intel.png" class="partner-logo" alt="Intel Logo">

<!-- After — updated to a new logo file -->
<img src="Images/intel_2025.png" class="partner-logo" alt="Intel Logo">
```

### Step 3 — Update the alt text if needed
If you replaced a partner entirely (not just the logo file), update `alt=""` to reflect the new company name.

> **Tip:** Keep the old logo file in `Images/` until the change is confirmed live. Deleting it before verification leaves a broken image if something goes wrong.

---

## SECTION 3 — Grid Layout and Logo Count

The partner grid uses CSS `grid-template-columns: repeat(2, 1fr)` — a fixed two-column layout. Here is how different counts look:

| Number of logos | Grid result | Looks OK? |
|----------------|------------|-----------|
| 2 | 1 row, 2 columns | ✓ Fine |
| 4 | 2 rows, 2 columns | ✓ Fine (current state) |
| 6 | 3 rows, 2 columns | ✓ Fine |
| 3 | 1 full row + 1 orphaned card | ✗ Unbalanced |
| 5 | 2 full rows + 1 orphaned card | ✗ Unbalanced |

**If you need exactly 5 logos:** Change the grid from 2-column to a layout where 5 fills evenly. Open `CSS/style.css`, search for `.partners-grid`, and change the rule:

```css
/* Before — 2-column grid */
.partners-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
}

/* After — auto-fill so 5 logos sit evenly (adjust min-width as needed) */
.partners-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
    gap: 15px;
}
```

> **Note:** Changing the CSS grid affects how the grid renders at **all screen sizes**, not just desktop. After making the change, resize the browser window to check how the grid looks on mobile too. On small screens the cards stack — `auto-fill` generally handles this gracefully, but always verify.

> **Please double-check:** After any grid count change, load the page at 375px wide (mobile) and 1280px wide (desktop) and confirm the partner box still sits inside its white container without overflow.

---

## SECTION 4 — Logo Image Guidelines

### Where to place logo files
Drop the image into the `Images/` folder at the project root.

### Recommended specs

| Property | Recommended value | Why |
|----------|------------------|-----|
| File format | `.png` with transparent background | Blends cleanly on the white card |
| Display height | 50 px rendered | CSS forces `height: 50px` — logos taller than this are scaled down |
| Actual file height | 100–200 px | 2× the rendered size for crisp display on retina screens |
| File size | Under 50 KB | Logo files should be tiny; use TinyPNG to compress |
| Background | Transparent | Opaque white backgrounds look OK on white cards, but logos with visible rectangles look off |

### Naming convention
Use company name, no spaces:
- `intel.png` ✓
- `TSMC.png` ✓
- `new partner logo.png` ✗ (spaces break file paths on some servers)

### How logos appear
By default all logos are displayed **grayscale** and at 80% opacity. On hover they snap to full color. This behavior is controlled by these CSS rules in `style.css`:

```css
.partner-logo {
    filter: grayscale(100%);
    opacity: 0.8;
}
.partner-card:hover .partner-logo {
    filter: grayscale(0%);
    opacity: 1;
}
```

Do not change these rules unless all partners have approved full-color always-on display.

---

## SECTION 5 — Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a new partner card | `class="partners-grid"` |
| Replace a specific logo | `alt="[Company] Logo"` |
| Change partner label text | `class="partner-label"` |
| Fix odd-number orphan row | `.partners-grid` in `CSS/style.css` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| `src=""` of a logo | `alt=""` on the same image if the company changed |
| Number of partners (to an odd number) | CSS grid columns — see Section 3 |
