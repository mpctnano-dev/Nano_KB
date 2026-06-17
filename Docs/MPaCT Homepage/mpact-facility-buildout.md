# MPaCT Lab Page · Facility Buildout Section
**File:** `MPaCT.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="/md_file_images/mpact-facility-buildout.png" alt="Building the Future section showing the floor plan blueprint image with the sq ft and instrument count badge overlaid in the corner, the section heading, and the spec rows for Facility Environment, Primary Users, and Industry Partners below" />

*The Facility Buildout section — the floor plan image is on the left with the stat badge pinned to it. The key spec rows (Environment, Users, Partners) are below the heading on the right.*

The Facility Buildout section has two distinct editable areas:

1. A **floor plan image** with a stat badge overlaid in the corner — the badge floats on top of the image and contains two statistics.
2. A list of **key specification rows** (Facility Environment, Primary Users, Industry Partners) displayed below the image.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Floor plan image | `class="ml-buildout__facility-image"` | The floor plan photo |
| Image overlay badge | `class="ml-buildout__image-badge"` | Overlaid stat numbers on the image |
| Key spec rows | `class="ml-buildout__spec"` | Spec label + value pairs |
| Section heading | `class="ml-buildout__title"` | "Building the Future" heading |

> **Note:** The decorative corner brackets (`ml-buildout__image-corner`), shimmer, and vignette overlays on the image frame are purely CSS animations — do not remove those elements when editing the image.

---

## SECTION 1 — Swapping the Floor Plan Image

### Step 1 — Find the image
Search for:
```
class="ml-buildout__facility-image"
```

```html
<img src="Images/blueprint_1.jpeg"
     alt="MPaCT Lab first floor life safety plan: facility layout and room designations"
     class="ml-buildout__facility-image">
```

### Step 2 — Replace the file
1. Place the new image in `Images/`.
2. Change `src="Images/blueprint_1.jpeg"` to the new filename.
3. Update the `alt=""` to accurately describe the new image.

> **Tip:** The image renders at a fixed height inside the frame. A landscape-oriented floor plan or facility photo (wider than tall) works best. Portrait images will be cropped at the top and bottom.

### Image specs

| Property | Recommended |
|----------|------------|
| Format | `.jpeg` or `.png` |
| Orientation | Landscape (wider than tall) |
| Minimum width | 800 px |
| File size | Under 400 KB |

---

## SECTION 2 — Updating the Overlay Badge Stats

The badge floats over the bottom-right corner of the image and shows two statistics. It is positioned absolutely via CSS — you only need to edit the text values.

### Finding the badge
Search for:
```
class="ml-buildout__image-badge"
```

```html
<div class="ml-buildout__image-badge">
    <div class="ml-buildout__badge-stat">
        <span class="ml-buildout__badge-value">3,000+</span>  <!-- ① number -->
        <span class="ml-buildout__badge-label">sq ft</span>   <!-- ② unit/label -->
    </div>
    <div class="ml-buildout__badge-stat">
        <span class="ml-buildout__badge-value">35+</span>     <!-- ③ number -->
        <span class="ml-buildout__badge-label">Instruments</span>  <!-- ④ unit/label -->
    </div>
</div>
```

### Changing a badge stat
- Edit the number inside `ml-buildout__badge-value`.
- Edit the unit or label inside `ml-buildout__badge-label`.

**Example — updating from 35+ to 40+ instruments:**
```html
<!-- Before -->
<span class="ml-buildout__badge-value">35+</span>
<span class="ml-buildout__badge-label">Instruments</span>

<!-- After -->
<span class="ml-buildout__badge-value">40+</span>
<span class="ml-buildout__badge-label">Instruments</span>
```

> **Please double-check:** These numbers also appear in the hero panel ("Lab at a Glance") and in body copy elsewhere on the page. Search for `35` and `3,000` in `MPaCT.html` to find all occurrences and update them consistently. See [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md) for the hero panel update steps.

---

## SECTION 3 — Updating Key Specification Rows

Below the image, a list of spec rows shows concise facts about the facility.

### Finding the spec rows
Search for:
```
class="ml-buildout__spec"
```

Each row is:
```html
<div class="ml-buildout__spec">
    <span class="ml-buildout__spec-label">Facility Environment</span>  <!-- ① left label -->
    <span class="ml-buildout__spec-value">ISO 6 • Controlled Environment</span>  <!-- ② right value -->
</div>
```

- Edit the **label** (left side) inside `ml-buildout__spec-label`.
- Edit the **value** (right side) inside `ml-buildout__spec-value`.
- The `&bull;` or `•` character in the value is a separator bullet — keep it if listing multiple items.

### Adding a new spec row

Paste a new block inside `class="ml-buildout__specs"`, after the last existing `</div>`:

```html
<div class="ml-buildout__spec">
    <span class="ml-buildout__spec-label">Your Label</span>
    <span class="ml-buildout__spec-value">Your Value</span>
</div>
```

### Current spec rows

| Label | Value |
|-------|-------|
| Facility Environment | ISO 6 • Controlled Environment |
| Primary Users | 5+ NAU Departments |
| Industry Partners | TSMC • Intel |

> **Note:** Keep spec values short — one line each. Long values will wrap and break the aligned two-column appearance of the spec list.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Swap floor plan image | `class="ml-buildout__facility-image"` |
| Update overlay badge stats | `class="ml-buildout__image-badge"` |
| Update a spec row | `class="ml-buildout__spec"` |
| Add a new spec row | `class="ml-buildout__specs"` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| `35+` instruments in badge | Same figure in hero panel + body copy (search `35` in file) |
| `3,000+` sq ft in badge | Same figure in hero panel + body copy |
| Floor plan `src=""` | `alt=""` on the same `<img>` tag |
