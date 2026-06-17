# Degree Programs (`degree-programs.html`) — Documentation Index
**File:** `degree-programs.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This Page Is

<img src="/md_file_images/degree-programs-hero.png" alt="Degree Programs page hero showing the page title, subtitle, and stats panel with degree count, ABET accreditation, average salary, and PTAP hours" />

*The Degree Programs hero — the cinematic banner with the stats panel overlay. The stats panel is the most commonly edited element on this section.*

`degree-programs.html` lists the six undergraduate engineering and computing degree programs offered at NAU that connect to the MPaCT Lab's semiconductor and microelectronics focus. It has a cinematic full-viewport hero, a scrollable row-per-program layout, a metrology training callout, a campus locations card grid, and a CTA section.

The page has two architectural quirks worth understanding before editing anything:

1. **The hero CSS is defined inline.** All `.ml-hero` styles (the cinematic hero layout, stats panel, typography, animations) live in a `<style>` block inside the page's own `<head>`, not in `CSS/style.css`. Searching the global stylesheet for `.ml-hero` will return nothing for this page.

2. **Scroll animations are driven by an inline script.** Elements with `class="reveal-on-scroll"` fade in on scroll via a self-contained `<script>` block at the very bottom of the page body. This is separate from the `mpact.js` pattern used on `MPaCT.html`. Optional stagger is controlled by `data-delay="200"` (milliseconds) on individual elements.

---

## Sections on This Page

| # | Section | Editable? | Doc |
|---|---------|-----------|-----|
| 1 | **Cinematic Hero** — background photo, headline, stats panel | Stats panel only — see shared doc | [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md) |
| 2 | **Bachelor's Programs** — one `program-row` per degree | Yes — non-obvious template | [degree-programs-program-rows.md](degree-programs-program-rows.md) |
| 3 | **Metrology Training** — two-column callout with skill checklist | Basic edits only — see note below | — |
| 4 | **Campus Locations** — two campus cards | Basic edits only — see note below | — |
| 5 | **CTA Section** — "Ready to Start Your Journey?" | Basic edits only — no doc needed | — |

---

## Section 1 — Cinematic Hero

**Doc:** [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md)

The hero shares its HTML structure and CSS class names (`ml-` prefix) with `MPaCT.html`, but its CSS is defined in an **inline `<style>` block** inside `degree-programs.html`'s own `<head>` — not in the global `CSS/style.css`. If you need to change the hero's visual appearance (height, colors, font sizes), search for `DEGREE PROGRAMS — CINEMATIC PAGE DESIGN SYSTEM` inside `degree-programs.html` itself.

The stats panel (numbers, labels, tags) is documented in the shared doc linked above.

> **Note:** The background image path is `src="Images/lab_services.jpg"`. To swap it, replace the filename only — do not change any surrounding attributes. The image should fill a 16:9 or taller frame at high resolution.

---

## Section 2 — Bachelor's Programs

**Doc:** [degree-programs-program-rows.md](degree-programs-program-rows.md)

Each degree is a `<article class="program-row">` inside `<div class="program-rows">`. Adding a new program, removing one, or updating feature tags all require following the template in the detail doc. The key non-obvious dependency: feature icons are **inline SVG**, not Font Awesome — you must paste SVG markup directly.

Go here when: adding a new degree, removing one, changing features/icons, or updating the external "View Degree Details" link.

---

## Section 3 — Metrology Training

**Search for:** `class="metrology-grid"`

This section is a two-column layout: an image on the left, text content on the right. The content (heading, paragraphs, and checklist items) is plain HTML — safe to edit directly.

Each checklist item is a `<div class="metrology-skill">` containing an inline SVG checkmark and a `<span>` with text:

```html
<div class="metrology-skill">
    <svg viewBox="0 0 24 24" width="20" height="20" fill="var(--nau-green)">
        <path d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z" />
    </svg>
    <span>Your skill text here</span>
</div>
```

To add a skill item, copy the block above and change the `<span>` text. The SVG path is a standard checkmark — do not modify it. The green color comes from `var(--nau-green)` defined in `CSS/style.css`.

> **Tip:** Keep skill labels concise — one line of text. Long labels may wrap awkwardly inside the checklist grid.

---

## Section 4 — Campus Locations

**Search for:** `class="campus-grid"`

The section contains two `<div class="campus-card">` blocks. Each card has an image, a badge label, a heading, a description, and a list of `campus-feature` tags.

There are **two badge variants** — use the correct one for each campus type:

```html
<!-- Default badge (white text on dark) -->
<div class="campus-card-badge">Main Campus</div>

<!-- Gold badge (gold text/border) -->
<div class="campus-card-badge campus-card-badge-gold">Phoenix Area</div>
```

The second card uses `data-delay="200"` on its opening div to stagger its scroll-reveal animation slightly after the first card. Keep this attribute in place when editing.

To update campus information: change the heading text, description paragraph, image `src`, and `campus-feature` span labels. No JavaScript changes are required.

> **Note:** Adding a third campus card will reflow the grid. The `.campus-grid` CSS uses `auto-fit` columns — a third card will appear on a new row on narrow screens. Test at mobile widths before deploying.

---

## Related Files

| File | Role |
|------|------|
| `degree-programs.html` | Entire page — including inline `<style>` for hero CSS |
| `CSS/style.css` | Styles for program rows, metrology section, campus cards, CTA |
| `JS/layout.js` | Injects site header and footer — do not modify |
| [`_shared-hero-stats-panel.md`](../_shared-hero-stats-panel.md) | Shared doc for the stats panel in the hero |
