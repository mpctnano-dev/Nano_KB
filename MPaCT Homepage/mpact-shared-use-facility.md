# MPaCT Lab Page · Open Access / Shared-Use Facility Section
**File:** `MPaCT.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

This section has two distinct editable areas:

1. A **stacked image pair** — two overlapping photos positioned via CSS. The front photo sits on top; the back photo is offset behind and below it.
2. Three **highlight items** — each with a colored icon circle, a heading, and a short description.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Front (top) photo | `class="about-shared-image-frame primary-frame"` | Primary lab photo |
| Back (behind) photo | `class="about-shared-image-frame secondary-frame"` | Secondary lab photo |
| Highlight items | `class="shared-highlight-item"` | Icon, heading, description for each bullet |

---

## SECTION 1 — Swapping the Stacked Photos

The two images overlap visually — one is styled as the primary (foreground) and one as the secondary (background/offset). They are controlled by `primary-frame` and `secondary-frame` CSS classes, not by z-index values you set yourself.

### Step 1 — Find the image stack
Search for:
```
class="about-shared-image-stack"
```

You will see this structure:
```html
<div class="about-shared-image-stack">
    <div class="about-shared-image-frame primary-frame">
        <img src="Images/microscopy_lab.jpg" alt="Students working in the MPaCT Lab"
            class="about-shared-img">
    </div>
    <div class="about-shared-image-frame secondary-frame">
        <img src="Images/students_lab.jpg" alt="Equipment training session"
            class="about-shared-img">
    </div>
</div>
```

### Step 2 — Replace the image
- To replace the **front photo**: change `src` and `alt` on the `<img>` inside `primary-frame`.
- To replace the **back photo**: change `src` and `alt` on the `<img>` inside `secondary-frame`.

Place the new image file in `Images/` first.

### Step 3 — Verify the overlap in the browser
Open `MPaCT.html` and scroll to this section. Both photos should be visible — one overlapping the other. If only one photo appears, the other image file is likely missing or the path is wrong.

> **Tip:** The front and back photos should be related but different — e.g., one showing equipment, one showing people. Using two nearly identical photos defeats the purpose of the stacked layout.

> **Note:** Do not change `class="about-shared-img"` on the `<img>` tags — this class applies the border radius and object-fit behavior that makes the stacked look work. Removing it will make the images render as plain rectangles.

### Image specs

| Property | Recommended |
|----------|------------|
| Format | `.jpg` for photos |
| Width | 600–900 px |
| Aspect ratio | 4:3 (landscape) — both photos should match for a clean stack |
| File size | Under 300 KB |

---

## SECTION 2 — Adding or Editing Highlight Items

The three highlight items ("Open to All Researchers", "Staff-Supported Operations", "Maintained & Calibrated") are below the section text.

### Finding a highlight item
Search for:
```
class="shared-highlight-item"
```

Each item follows this structure:
```html
<div class="shared-highlight-item">
    <div class="shared-highlight-icon blue">   <!-- ① icon color -->
        <i class="fas fa-users"></i>           <!-- ② Font Awesome icon -->
    </div>
    <div>
        <h4>Open to All Researchers</h4>       <!-- ③ heading -->
        <p>No departmental restrictions...</p> <!-- ④ description -->
    </div>
</div>
```

### Changing the icon color

The color modifier on `shared-highlight-icon` controls the circle color:

| Class | Color |
|-------|-------|
| `shared-highlight-icon blue` | NAU Blue circle |
| `shared-highlight-icon gold` | NAU Gold circle |
| `shared-highlight-icon green` | Green circle |

Current assignment: item 1 = blue, item 2 = gold, item 3 = green.

### Changing the icon symbol

The `<i class="fas fa-???">` uses Font Awesome 6. Replace the icon class with any solid icon from the Font Awesome catalog. Common options used on this site:

| Icon class | Symbol |
|-----------|--------|
| `fa-users` | People group |
| `fa-hands-helping` | Hands |
| `fa-tools` | Wrench/tools |
| `fa-microscope` | Microscope |
| `fa-flask` | Flask |
| `fa-shield-alt` | Shield |

### Adding a new highlight item

Paste this template inside `class="shared-highlights"`, after the last `</div>` of the third item:

```html
<div class="shared-highlight-item">
    <div class="shared-highlight-icon blue">
        <i class="fas fa-flask"></i>
    </div>
    <div>
        <h4>Your Heading Here</h4>
        <p>One or two sentences describing this highlight. Keep it concise.</p>
    </div>
</div>
```

> **Note:** Three highlight items is the current design. Adding a fourth is fine, but adding more than four may make the section feel dense on desktop. Test in the browser after adding.

> **Please double-check:** After adding a highlight item, verify the icon renders. If you see a blank square instead of an icon, the Font Awesome class name is wrong — check the exact class on [fontawesome.com/icons](https://fontawesome.com/icons) (free/solid icons only).

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Swap front photo | `class="about-shared-image-frame primary-frame"` |
| Swap back photo | `class="about-shared-image-frame secondary-frame"` |
| Edit a highlight item | `class="shared-highlight-item"` |
| Change icon color | `class="shared-highlight-icon` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| `src=""` of either photo | `alt=""` on the same `<img>` tag |
| Icon color class | Verify it matches the surrounding items' visual rhythm (blue → gold → green pattern) |
