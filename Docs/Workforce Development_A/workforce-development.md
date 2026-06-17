# Workforce Development (`WorkForceDevelopment.html`) — Documentation Index
**File:** `WorkForceDevelopment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This Page Is

<img src="/md_file_images/wfd-hero.png" alt="Workforce Development page hero showing the full-viewport banner with the page title, subtitle, CTA buttons, and the stats panel with semiconductor industry figures" />

*The WorkForceDevelopment.html hero — the full-width banner section visitors see first. The stats panel on the right shows the key industry figures that appear across the rest of the page.*

`WorkForceDevelopment.html` covers NAU's semiconductor workforce pipeline — industry partnerships, apprenticeship pathways, the PTAP program, AZ market context, employer ecosystem, and career outcome data. It has seven distinct sections.

One architectural pattern applies to every non-hero section on this page:

**Scroll animations are driven by `JS/workforce-development.js`.** Any element with `class="wfd-reveal"` starts invisible and fades in when it enters the viewport. The JS file adds `is-visible` to trigger the CSS transition. Stagger delay is applied via `wfd-d1`, `wfd-d2`, `wfd-d3` classes on individual elements.

> **Note:** This animation system is different from both `mpact.js` (used on `MPaCT.html`) and the inline scroll script on `degree-programs.html`. Do not mix class names across pages — `wfd-reveal`/`is-visible` only work here because `workforce-development.js` is loaded on this page's `<script>` tag. If that script tag is removed, every `wfd-reveal` element stays permanently invisible.

> **Note:** The hero CSS for this page lives in `CSS/style.css` (under the `wfd-` prefix), unlike `degree-programs.html` where equivalent `ml-` styles are inline in the `<head>`. You can search `style.css` for `.wfd-hero` to find and edit hero visual styles.

---

## Sections on This Page

| # | Section | Editable? | Doc |
|---|---------|-----------|-----|
| 1 | **Hero** — background photo, headline, stats panel, CTA buttons | Stats panel only — see shared doc | [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md) |
| 2 | **Mission / Intro Pillars** — statement, body text, three pillar cards | Basic edits — see note below | — |
| 3 | **Arizona Stats** — four large numbered stats | Basic edits — see note below | — |
| 4 | **Workforce Pathways & PTAP** — four pathway cards + PTAP feature block | Yes — non-obvious template | [wfd-pathways-ptap.md](wfd-pathways-ptap.md) |
| 5 | **NAU's Investment** — image + overlay badge + highlights + quote | Yes — overlay badge has non-obvious structure | [wfd-investment.md](wfd-investment.md) |
| 6 | **Ecosystem** — four company cards with logos | Yes — logo image handling, card template | [wfd-ecosystem.md](wfd-ecosystem.md) |
| 7 | **Career Outcomes** — salary table + growth card + credentials | Yes — **cross-page dependency with `CareerPathways.html`** | [wfd-career-outcomes.md](wfd-career-outcomes.md) |

---

## Section 1 — Cinematic Hero

**Doc:** [_shared-hero-stats-panel.md](../_shared-hero-stats-panel.md)

The stats panel (numbers, labels, tags) uses the `wfd-` prefix class names documented in the shared doc. The CTA buttons on the left side link internally — update `href` if the target page filename changes:

```html
<a href="degree-programs.html" class="btn btn-primary">View Degree Programs</a>
<a href="Contact_Us.html?category=research" class="btn btn-outline">Partner With Us</a>
```

> **Note:** The `?category=research` query string on the second button pre-selects a category on the Contact form. If the Contact page form changes its category field, this pre-selection may stop working silently.

---

## Section 2 — Mission / Intro Pillars

**Search for:** `class="wfd-pillars"`

Three `.wfd-pillar` cards, each with a Font Awesome icon, a title, and a description paragraph. All text is safe to edit directly.

Each pillar uses a Font Awesome icon in a wrapper div:

```html
<div class="wfd-pillar__icon"><i class="fas fa-handshake"></i></div>
```

To change the icon, replace `fa-handshake` with any Font Awesome solid icon class. The full icon list is at [fontawesome.com/icons](https://fontawesome.com/icons) — filter by "Solid" style, which corresponds to the `fas` prefix.

The pillar cards use `wfd-reveal`, `wfd-d1`, `wfd-d2` for stagger. Keep these classes intact when editing.

---

## Section 3 — Arizona Semiconductor Stats

**Search for:** `class="wfd-stats__grid"`

Four `.wfd-stat` blocks arranged in a row. Each stat has a number with a suffix and a two-line description:

```html
<div class="wfd-stat">
    <div class="wfd-stat__num">$210<span>B+</span></div>
    <div class="wfd-stat__desc">Capital Investment<br>Since 2020</div>
</div>
```

The suffix (`B+`, `+`, `K+`) goes inside a `<span>` inside `wfd-stat__num`. This is different from the hero stats panel, which uses `<em>`. Use `<span>` here, not `<em>`.

A source line below the grid attributes the data to Arizona Commerce Authority. Update this if the data year changes.

> **Tip:** Keep stat descriptions to two short lines using `<br>` between them. Longer descriptions overflow the stat block on mobile.

---

## Section 4 — Workforce Pathways & PTAP

**Doc:** [wfd-pathways-ptap.md](wfd-pathways-ptap.md)

The most complex section — four pathway cards plus the PTAP highlighted feature block. Go here when: adding or removing a pathway, editing the PTAP partner logo, or updating PTAP feature checkboxes.

---

## Section 5 — NAU's Investment

**Doc:** [wfd-investment.md](wfd-investment.md)

The `$13M` overlay badge pinned to the photo is the key non-obvious element — it uses absolute CSS positioning with two specific class names that must stay intact. The investment highlights follow a four-item FA-icon template. Go here when: updating the badge figure, swapping the photo, editing highlights, or updating the pull quote.

> **Note:** The `$13M` figure appears in at least three places in this section (badge, heading, body paragraph) and also in the hero stats panel. Search the file for `13` before saving to find all occurrences.

---

## Section 6 — Arizona Ecosystem (Company Cards)

**Doc:** [wfd-ecosystem.md](wfd-ecosystem.md)

Company logos require a transparent background and must be placed inside `wfd-eco__card-logo-wrap` — not placed directly in the card. The investment text is a plain `<span>` class, not a badge class. Go here when: adding a new company, swapping a logo, or updating the "more companies" bar.

---

## Section 7 — Career Outcomes

**Doc:** [wfd-career-outcomes.md](wfd-career-outcomes.md)

**Cross-page dependency:** Salary figures here also appear in `CareerPathways.html`. Updating one without the other leaves contradictory data on the site. Go here when: refreshing salary data, updating source year references, editing the national projection number, or adding credentials.

---

## Related Files

| File | Role |
|------|------|
| `WorkForceDevelopment.html` | Entire page |
| `CSS/style.css` | All `wfd-` component styles, including hero (`wfd-hero`), scroll animation states (`wfd-reveal`, `is-visible`, `wfd-d1`–`wfd-d3`) |
| `JS/workforce-development.js` | Drives `wfd-reveal` scroll animations via IntersectionObserver — required for any element with that class to be visible |
| `JS/layout.js` | Injects site header and footer — do not modify |
| `PTAP.html` | Linked from the PTAP feature block — must exist for the "View PTAP Details" button to work |
| [`_shared-hero-stats-panel.md`](../_shared-hero-stats-panel.md) | Shared doc for the hero stats panel (`wfd-` prefix) |
