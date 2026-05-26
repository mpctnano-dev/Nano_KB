# Shared Component — Cinematic Hero Stats Panel
**Used on:** `MPaCT.html`, `degree-programs.html`, `WorkForceDevelopment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This Component Is

Three pages on the site share an identical hero layout: a full-viewport banner split into two columns — headline/text on the left, a floating dark stats panel on the right. The panel contains numbered statistics, a label beneath each, and a row of keyword tags at the bottom.

Each page uses different CSS class prefixes but the HTML structure is identical. When you learn to edit one, you know all three.

| Page | Panel class prefix | Stats num class | Stats label class | Tags class |
|------|--------------------|-----------------|-------------------|------------|
| `MPaCT.html` | `ml-hero__panel` | `ml-pstat__num` | `ml-pstat__label` | `ml-hero__panel-tag` |
| `degree-programs.html` | `ml-hero__panel` | `ml-pstat__num` | `ml-pstat__label` | `ml-hero__panel-tag` |
| `WorkForceDevelopment.html` | `wfd-hero__panel` | `wfd-pstat__num` | `wfd-pstat__label` | `wfd-hero__panel-tag` |

> **Note:** `MPaCT.html` and `degree-programs.html` share the same `ml-` prefix, but `degree-programs.html` defines its `ml-hero` styles in an **inline `<style>` block** inside its own `<head>` — not in `CSS/style.css`. If you need to change the visual appearance of the degree-programs hero, look in the `<head>` of that file, not in the global stylesheet.

---

## SECTION 1 — Updating a Stat Number

**Search for (in the relevant page):**

| Page | Search snippet |
|------|---------------|
| `MPaCT.html` | `class="ml-pstat__num"` |
| `degree-programs.html` | `class="ml-pstat__num"` |
| `WorkForceDevelopment.html` | `class="wfd-pstat__num"` |

Each stat is a two-line block:

```html
<!-- MPaCT.html example -->
<div>
    <div class="ml-pstat__num">$13<em>M</em></div>
    <div class="ml-pstat__label">Facility Investment</div>
</div>
```

- The main number goes before the `<em>` tag: `$13`, `35`, `600`, etc.
- The unit or suffix goes inside `<em>`: `M`, `+`, `hrs`, `sq&nbsp;ft`
- The label beneath it goes in `ml-pstat__label` / `wfd-pstat__label`

To update a number, change only the text before `<em>` and the text inside `<em>`. Do not change the surrounding `<div>` structure.

**Example — updating instrument count from 35 to 40 on MPaCT.html:**
```html
<!-- Before -->
<div class="ml-pstat__num">35<em>+</em></div>
<div class="ml-pstat__label">Advanced Instruments</div>

<!-- After -->
<div class="ml-pstat__num">40<em>+</em></div>
<div class="ml-pstat__label">Advanced Instruments</div>
```

> **Please double-check:** Stat numbers on the hero panel often repeat elsewhere on the same page (in body copy, in the Facility Buildout badge, etc.). Search the page for the old number before saving to find all occurrences.

---

## SECTION 2 — Adding or Removing a Stat

Each page currently has exactly four stats in the panel. Adding or removing one will change the panel's grid layout.

**Search for:** `class="ml-hero__panel-stats"` (or `wfd-hero__panel-stats`)

```html
<div class="ml-hero__panel-stats">
    <div>  <!-- ← one stat block -->
        <div class="ml-pstat__num">$13<em>M</em></div>
        <div class="ml-pstat__label">Facility Investment</div>
    </div>
    <div>...</div>
    <div>...</div>
    <div>...</div>
</div>
```

To add a stat: paste a new `<div>` block with `__num` and `__label` inside `__panel-stats`.  
To remove a stat: delete the entire `<div>` block (opening through closing `</div>`).

> **Note:** The panel stats grid is set to `2 × 2` in CSS. Adding a fifth stat may overflow the panel or break the layout depending on screen size. Stick to 4 stats, or test thoroughly at multiple screen widths if you go beyond that.

---

## SECTION 3 — Updating the Panel Label

The small uppercase text at the top of the panel ("Lab at a Glance", "Program Impact") is separate from the stats.

**Search for:** `class="ml-hero__panel-label"` (or `wfd-hero__panel-label`)

```html
<div class="ml-hero__panel-label">Lab at a Glance</div>
```

Edit the text directly. Keep it short — this is a label, not a sentence.

---

## SECTION 4 — Updating Keyword Tags

The row of tags at the bottom of the panel are plain `<span>` elements.

**Search for:** `class="ml-hero__panel-tag"` (or `wfd-hero__panel-tag`)

```html
<div class="ml-hero__panel-tags">
    <span class="ml-hero__panel-tag">Open-Access</span>
    <span class="ml-hero__panel-tag">Lithography</span>
    <span class="ml-hero__panel-tag">Thin Films</span>
    <span class="ml-hero__panel-tag">Characterization</span>
    <span class="ml-hero__panel-tag">Metrology</span>
</div>
```

- To **change** a tag label: edit the text inside the `<span>`.
- To **add** a tag: paste `<span class="ml-hero__panel-tag">New Tag</span>` inside the tags `<div>`.
- To **remove** a tag: delete the `<span>` element.

> **Tip:** Tags wrap onto a second line if there are too many. Four to five tags is the comfortable range before the panel starts to feel crowded on smaller desktop widths.

---

## Quick Reference

| Task | Search For (in each page) |
|------|--------------------------|
| Change a stat number | `class="ml-pstat__num"` / `class="wfd-pstat__num"` |
| Change a stat label | `class="ml-pstat__label"` / `class="wfd-pstat__label"` |
| Change panel title | `class="ml-hero__panel-label"` / `class="wfd-hero__panel-label"` |
| Add or edit a tag | `class="ml-hero__panel-tags"` / `class="wfd-hero__panel-tags"` |
