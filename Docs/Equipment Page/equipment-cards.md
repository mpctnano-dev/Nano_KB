# Equipment Catalog · Equipment Cards
**File:** `Equipment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="/md_file_images/equipment-cards.png" alt="Equipment Catalog instrument cards showing category badges, equipment photos, location, description, and the About and Reserve buttons on each card" />

*The instrument card grid — each card shows the category badge (e.g. FABRICATION), availability status, instrument name, photo, location, and action buttons. This is what you are editing when adding or changing a card.*

Every instrument on the catalog page is a `<div class="tech-card">` block inside `id="equipmentContainer"`. Cards display in a category-sorted order determined by an inline JavaScript array — not by their position in the HTML. You can paste a new card anywhere inside the container and the sort script will place it in the correct visual position automatically.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Card grid | `id="equipmentContainer"` | Add, remove, or comment out cards |
| Individual card | Search the instrument name (e.g., `LPKF ProtoLaser`) | Any field on that card |
| Badge color | `class="badge badge-` | Category badge color and label |
| Status dot | `class="status-dot` | HTML fallback status (see JSON note below) |
| Reserve button | `class="btn-tech btn-tech-fill"` | Enable / disable reservation link |

> **Note:** The HTML `status-dot` class is only a **fallback**. At page load, `JS/Equipment_Status.js` fetches `equipment.json` and overwrites the status dot and Reserve button for every matched card. To reliably change an instrument's live status, update `equipment.json` — see [equipment-json-status.md](equipment-json-status.md).

---

## SECTION 1 — Adding a New Equipment Card

### Step 1 — Find the container
Search for:
```
id="equipmentContainer"
```

### Step 2 — Paste the card template
Place it anywhere inside `#equipmentContainer` — before or after any existing card. The sort script handles visual positioning.

```html
<div class="tech-card" data-category="Metrology" data-location="Flagstaff">
    <div class="tech-card-header">
        <div class="d-flex justify-between align-center mb-2">
            <span class="badge badge-blue">Metrology</span>   <!-- ① badge -->
            <span class="status-dot available"></span>         <!-- ② status (HTML fallback) -->
        </div>
        <h3 class="tech-title">Your Equipment Name</h3>       <!-- ③ name -->
    </div>
    <div class="tech-img-box">
        <img src="Images/YourImage.png"                        <!-- ④ image -->
             alt="Your Equipment Name"
             onerror="this.src='https://placehold.co/400x300?text=Equipment'">
    </div>
    <div class="tech-body">
        <div class="tech-info">
            <span><i class="fas fa-map-marker-alt"></i> Building 98E</span>  <!-- ⑤ location -->
            <span><i class="fas fa-tag"></i> Model: Your Model</span>        <!-- ⑥ model/capability -->
        </div>
        <div class="tech-actions">
            <a href="About_Equipment/YourPage.html"
               class="btn-tech btn-tech-outline">About</a>                   <!-- ⑦ about link -->
            <a href="Reserve_Equipment.html?equipment=EQ-XXX"
               class="btn-tech btn-tech-fill">Reserve</a>                    <!-- ⑧ reserve -->
        </div>
    </div>
</div>
```

### Step 3 — Fill in the eight values

| # | What | Where | Notes |
|---|------|-------|-------|
| ① | Badge color + label | `class="badge badge-???"` | Must match badge table in Section 2 |
| ② | HTML status | `class="status-dot ???"` | Fallback only — update `equipment.json` too |
| ③ | Instrument name | `<h3 class="tech-title">` | Must exactly match the `name` field you'll add to `equipment.json` |
| ④ | Image | `src=""` and `alt=""` | Place file in `Images/` first — see image specs in Section 5 |
| ⑤ | Location text | First `<span>` in `.tech-info` | Room/building; for multi-location see Section 4 |
| ⑥ | Model or capability | Second `<span>` in `.tech-info` | Use `fa-tag` for model numbers, `fa-bolt`/`fa-tools` etc. for capability |
| ⑦ | About link | `href=""` on `.btn-tech-outline` | `About_Equipment/YourPage.html` — page must exist |
| ⑧ | Reserve link | `.btn-tech-fill` `<a>` or `<button>` | Use `<a>` if bookable (with EQ-XXX ID); disabled `<button>` if not |

### Step 4 — Add the instrument to `equipment.json`
A card without a matching JSON entry will retain whatever status you put in the HTML — it won't be managed by the status system. See [equipment-json-status.md](equipment-json-status.md) for how to add the entry.

### Step 5 — Add the category to the sort order if it's new
If the card's `data-category` is a new value not already in `const order = [...]`, the card will sort to the end of the grid. See [equipment-filter-bar.md](equipment-filter-bar.md).

> **Please double-check:** After adding the card, reload `Equipment.html` in a browser. Confirm the card appears in the correct category group, the badge color matches, the image loads, and both buttons work.

---

## SECTION 2 — Badge Classes by Category

The `badge` class and the text label inside it are independent — changing one without the other shows the wrong color for the label displayed.

| `data-category` value | Badge class | Label text | Color |
|----------------------|------------|------------|-------|
| Metrology | `badge-blue` | Metrology | Blue |
| Fabrication | `badge-gold` | Fabrication | Gold |
| Electrical | `badge-purple` | Electrical | Purple |
| Sample Prep | `badge-green` | Sample Prep | Green |
| Mechatronics | `badge-blue` | Mechatronics | Blue |
| Educational | `badge-blue` | Educational | Blue |
| Support Systems | `badge-purple` | Support Systems | Purple |

> **Note:** `data-category` on the `<div class="tech-card">` must exactly match `data-filter` on the category filter button (case-sensitive). A mismatch means the filter button won't show or hide that card — no error will appear.

---

## SECTION 3 — Status States (HTML Fallback)

The HTML status is what displays before `equipment.json` loads. Replace the **entire element** when switching between states.

**Search for:** `class="status-dot`

```html
<!-- STATE 1: Available — green dot -->
<span class="status-dot available"></span>

<!-- STATE 2: Unavailable / in use — orange dot -->
<span class="status-dot busy"></span>

<!-- STATE 3: Coming soon — dot hidden + date label -->
<div class="d-flex align-center">
    <span class="status-dot expected"></span>
    <span class="status-text expected">July 1st</span>
</div>
```

For the coming-soon date, edit only the text inside `<span class="status-text expected">`.

> **Note:** `Equipment_Status.js` hides the dot element (`dot.style.display = 'none'`) when status is EXPECTED and renders the date label instead. This is why `status-dot expected` in HTML looks the same as the `status-dot available` + text wrapper pattern — the JS reconciles both patterns at runtime.

### Reserve Button States

**Search for:** `class="btn-tech btn-tech-fill"`

```html
<!-- ENABLED — links to reservation form -->
<a href="Reserve_Equipment.html?equipment=EQ-026"
   class="btn-tech btn-tech-fill">Reserve</a>

<!-- DISABLED — instrument not available -->
<button class="btn-tech btn-tech-fill disabled"
        style="opacity: 0.6; cursor: not-allowed;">Reserve</button>
```

The `EQ-XXX` number must match the `id` field in `equipment.json` for the reservation system to pre-select the correct instrument.

### Paired Changes — Always Do Both

| If status is… | Reserve button must be… |
|---|---|
| `available` | `<a href="Reserve_Equipment.html?equipment=EQ-XXX" ...>Reserve</a>` |
| `busy` or `expected` | `<button ... class="btn-tech btn-tech-fill disabled" ...>Reserve</button>` |

---

## SECTION 4 — Multi-Location Cards

Cards available at both Flagstaff and NVC use a comma-separated `data-location` attribute and show two location lines in `.tech-info`.

```html
<div class="tech-card" data-category="Educational" data-location="Flagstaff,NVC">
    ...
    <div class="tech-info">
        <span><i class="fas fa-map-marker-alt"></i> Flagstaff - Building 98E</span>
        <span><i class="fas fa-map-marker-alt"></i> NVC Room 144 (Unavailable)</span>
        <span><i class="fas fa-robot"></i> Automation Training</span>
    </div>
```

**`data-location` values:**

| Value | Shown when filter is… |
|-------|----------------------|
| `"Flagstaff"` | "All" or "Flagstaff" |
| `"NVC"` | "All" or "NVC (Phoenix)" |
| `"Flagstaff,NVC"` | "All", "Flagstaff", or "NVC (Phoenix)" |
| *(no attribute)* | "All" only |

> **Note:** Cards with no `data-location` attribute are hidden by both the Flagstaff and NVC filters — they only appear when "All" is selected. This is intentional for instruments with an unspecified or temporary location.

---

## SECTION 5 — About-Only Cards (No Reserve Button)

Some cards have only an About button and no Reserve button at all. These use a modified `.tech-actions` layout:

```html
<div class="tech-actions" style="grid-template-columns: 1fr;">
    <a href="About_Equipment/YourPage.html"
       class="btn-tech btn-tech-outline"
       style="width: 100%; text-align: center;">About</a>
</div>
```

The inline `style="grid-template-columns: 1fr;"` makes the About button span the full card width instead of the default two-column split. Use this pattern for equipment that cannot be independently reserved (e.g., educational modules managed by staff only).

---

## SECTION 6 — Temporarily Hiding a Card

Wrap the entire card in an HTML comment:

```html
<!-- Temporarily hidden — remove comment tags to restore
<div class="tech-card" data-category="Fabrication">
    ...
</div>
-->
```

> **Tip:** Search for the instrument name to find its card quickly. Several cards are already commented out in the file (Stylus Profilometer, Spin Coater, High-Vacuum Sputter, etc.). Search for `Temporarily hidden` to see all of them.

The grid reflows automatically — no gap is left where the card was.

---

## SECTION 7 — Image Specs

| Property | Recommended |
|----------|------------|
| Format | `.png` for instruments on white/transparent background; `.jpg` for photos |
| Size | 400×300 px (matches the `.tech-img-box` aspect ratio) |
| File size | Under 200 KB |
| Naming | No spaces: `SEM.jpg` ✓, `SEM image.jpg` ✗ |

The `onerror` attribute displays a gray placeholder if the image file is missing. If you see placeholder text on the live page, the `src` path doesn't match — check spelling and case.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a new card | `id="equipmentContainer"` |
| Find a specific card | Instrument name (e.g., `LPKF ProtoLaser`) |
| Change badge | `class="badge badge-` |
| Change status dot | `class="status-dot` |
| Change reserve button | `class="btn-tech btn-tech-fill"` |
| Find commented-out cards | `Temporarily hidden` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| `data-category` on a card | Badge class + badge label text on the same card |
| Badge class | Badge label text on the same card |
| Status to available | Reserve button → enabled `<a>` with EQ-XXX + update `equipment.json` |
| Status to unavailable/expected | Reserve button → disabled `<button>` + update `equipment.json` |
| Instrument name in `<h3 class="tech-title">` | `name` field in `equipment.json` (must match exactly for status override to work) |
| Add new `data-category` not in sort array | Add it to `const order = [...]` — see [equipment-filter-bar.md](equipment-filter-bar.md) |
