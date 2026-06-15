# MPaCT Lab Page · What the Lab Does — Capability Cards
**File:** `MPaCT.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="../../md_file_images/mpact-capability-cards.png" alt="What the Lab Does section showing the section heading, descriptive text on the right, and the 2x2 CORE CAPABILITIES grid with Metrology, Lithography, Deposition, and Characterization cards — each with a colored icon and instrument summary" />

*The "What the Lab Does" section — the 2×2 capability card grid is on the right. These are the four cards you edit when updating what the lab's core technical areas are.*

The "What the Lab Does" section includes a 2×2 grid of capability cards. Each card has a colored icon circle, a capability name, and a short instrument/tool summary. These cards communicate the lab's four core technical areas at a glance.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Cards grid | `class="capabilities-grid"` | Add or remove capability cards |
| Individual card | `class="capability-card"` | Icon, color, name, instruments listed |

---

## SECTION 1 — Editing a Capability Card

### Finding a card
Search for:
```
class="capabilities-grid"
```

Each card inside the grid looks like this:
```html
<div class="capability-card">
    <div class="capability-icon blue">     <!-- ① icon circle color -->
        <i class="fas fa-microscope"></i>  <!-- ② Font Awesome icon -->
    </div>
    <div class="capability-info">
        <h4>Metrology</h4>                 <!-- ③ capability name -->
        <p>AFM, SEM, Optical Profiling</p> <!-- ④ instruments list -->
    </div>
</div>
```

| # | What | Where | Notes |
|---|------|-------|-------|
| ① | Icon circle color | `class="capability-icon ???"` | See color table below |
| ② | Icon symbol | `class="fas fa-???"` | Font Awesome solid icon — see icon list below |
| ③ | Capability name | `<h4>` | Short label, 1–3 words |
| ④ | Instruments | `<p>` inside `.capability-info` | Comma-separated instrument names — keep brief |

---

## SECTION 2 — Icon Circle Color Classes

| Class | Color | Current use |
|-------|-------|------------|
| `capability-icon blue` | NAU Blue | Metrology |
| `capability-icon gold` | NAU Gold | Lithography |
| `capability-icon green` | Green | Deposition |
| `capability-icon purple` | Purple | Characterization |

> **Note:** Each card currently uses a distinct color. If you add a fifth card, reuse one of the four colors — there is no fifth color class defined. Using an undefined class (e.g., `capability-icon red`) will render with no background color.

---

## SECTION 3 — Adding a New Capability Card

### Step 1 — Find the grid
Search for `class="capabilities-grid"`.

### Step 2 — Paste the template
Add a new card inside the grid, after the last `</div>` of the last existing card:

```html
<div class="capability-card">
    <div class="capability-icon blue">
        <i class="fas fa-flask"></i>
    </div>
    <div class="capability-info">
        <h4>Your Capability</h4>
        <p>Tool One, Tool Two, Tool Three</p>
    </div>
</div>
```

### Step 3 — Choose icon and color
Pick a color from the table in Section 2. Pick an icon from Font Awesome:

| Icon class | Symbol |
|-----------|--------|
| `fa-microscope` | Microscope (currently Metrology) |
| `fa-layer-group` | Layers (currently Lithography) |
| `fa-atom` | Atom (currently Deposition) |
| `fa-bolt` | Lightning (currently Characterization) |
| `fa-flask` | Flask |
| `fa-wave-square` | Waveform |
| `fa-radiation` | Radiation symbol |

### Step 4 — Verify in the browser
Scroll to "What the Lab Does" and confirm the new card appears. Check that the icon renders (solid square = wrong class name) and the color circle shows correctly.

> **Please double-check:** The grid is CSS `auto-fill` — it reflows cards based on available width. Five cards on desktop may show 3 + 2 or 2 + 2 + 1 depending on screen width. Test at 1280px and 768px after adding a card.

---

## SECTION 4 — Removing a Capability Card

Delete the entire `<div class="capability-card">` block. The remaining cards reflow automatically.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Find cards grid | `class="capabilities-grid"` |
| Edit a specific card | Search the capability name (e.g., `Metrology`) |
| Change icon color | `class="capability-icon` |
