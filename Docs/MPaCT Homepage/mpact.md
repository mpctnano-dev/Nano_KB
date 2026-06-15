# MPaCT Lab Page (`MPaCT.html`) — Documentation Index
**File:** `MPaCT.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This Page Is

<img src="../../md_file_images/mpact-hero.png" alt="MPaCT Lab page hero showing the dark lab background image, page title, subtitle, CTA buttons, and the stats panel with facility investment, instrument count, and facility size" />

*The MPaCT.html hero — the full-viewport dark banner with the lab stats overlay. This is the first section visitors see.*

`MPaCT.html` is the dedicated page for the MPaCT (Microelectronics Processing and Characterization Testing Lab) facility. It gives researchers, students, and industry partners a detailed overview of the lab — what it is, what it does, how it was built, and how to get access.

The page loads three JavaScript files: `layout.js` (header/footer), `script.js` (shared site logic), and `mpact.js` (page-specific scroll animations). The `mpact.js` file drives the `ml-reveal` fade-in animations on every section. Do not remove `class="ml-reveal"` from any section wrapper — doing so will prevent that section from animating in when the user scrolls to it.

---

## Sections on This Page

| # | Section | Editable? | Doc |
|---|---------|-----------|-----|
| 1 | **Hero** — background photo, headline, eyebrow, CTA buttons | Basic edits only — no doc needed | — |
| 2 | **Lab at a Glance** (stats panel inside hero) — numbers, tags | Yes — shared pattern across 3 pages | [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md) |
| 3 | **Hero Divider** — animated PCB circuit graphic | Do not edit — purely decorative JS animation | — |
| 4 | **Intro** — one-paragraph mission statement | Basic edits only — no doc needed | — |
| 5 | **Open Access / Shared-Use Facility** — text, stacked image pair, highlight items | Yes — image stack and icon colors non-obvious | [mpact-shared-use-facility.md](mpact-shared-use-facility.md) |
| 6 | **What the Lab Does** — capability cards with colored icons | Yes — icon color class system | [mpact-capability-cards.md](mpact-capability-cards.md) |
| 7 | **Facility Buildout** — floor plan image with overlaid stat badge, spec rows | Yes — badge overlay and spec rows non-obvious | [mpact-facility-buildout.md](mpact-facility-buildout.md) |
| 8 | **How to Access the Lab** — four numbered steps, two CTA buttons | Basic edits only — no doc needed | — |
| 9 | **Areas of Scientific Focus** — research area cards with tags and animation delays | Yes — delay class pattern, tag structure | [mpact-research-areas.md](mpact-research-areas.md) |
| 10 | **Final CTA** — heading, paragraph, two buttons | Basic edits only — no doc needed | — |

---

## Section 2 — Lab at a Glance (Hero Stats Panel)

**Doc:** [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md)

This is a floating dark panel on the right side of the hero containing four statistics and a row of keyword tags. The same panel structure appears on `degree-programs.html` and `WorkForceDevelopment.html`. One shared doc covers all three — learn the pattern once.

Go here when: changing the `$13M`, `35+`, `3K+ sq ft` figures or updating the keyword tags.

---

## Section 5 — Open Access / Shared-Use Facility

**Doc:** [mpact-shared-use-facility.md](mpact-shared-use-facility.md)

This section has a stacked two-image layout (two overlapping photos, one offset behind the other) and three highlight items with colored icon circles. The stacking is purely CSS — but changing which photo is "in front" requires knowing which frame class is which.

Go here when: swapping the lab photos or adding/editing the highlight bullet items.

---

## Section 6 — What the Lab Does

**Doc:** [mpact-capability-cards.md](mpact-capability-cards.md)

Four capability cards showing the lab's core areas (Metrology, Lithography, Deposition, Characterization). Each card has a colored icon circle controlled by a CSS modifier class (`blue`, `gold`, `green`, `purple`).

Go here when: adding a new capability area, changing a card's icon, or changing the color of an icon circle.

---

## Section 7 — Facility Buildout

**Doc:** [mpact-facility-buildout.md](mpact-facility-buildout.md)

This section shows the facility floor plan image with a stat badge overlaid in the bottom-right corner, plus a list of key specification rows below. The overlay badge is positioned absolutely over the image — its values must be updated manually when the facts change.

Go here when: updating the `3,000+` sq ft figure, the `35+` instruments count, the key spec rows (ISO environment, partners, departments), or swapping the floor plan image.

---

## Section 9 — Areas of Scientific Focus

**Doc:** [mpact-research-areas.md](mpact-research-areas.md)

Four research area entries, each numbered (01–04) with a title, description paragraph, and a row of instrument tags. Each card after the first has a CSS animation delay class (`ml-d1`, `ml-d2`, `ml-d3`) that staggers the scroll animation. Adding a fifth area requires following the delay class pattern.

Go here when: adding a new research area, updating instrument tags, or editing a research description.

---

## Related Files

| File | Role |
|------|------|
| `CSS/style.css` | Global styles including `ml-hero`, `ml-buildout`, `ml-access` etc. |
| `JS/mpact.js` | Scroll reveal animations for `.ml-reveal` elements — page-specific |
| `JS/script.js` | Shared site logic (sticky header, etc.) |
| `JS/layout.js` | Injects header and footer — do not modify |
| `includes/header.html` | Site navigation |
| `includes/footer.html` | Site footer |
