# Workforce Development · Pathways & PTAP (§4)
**File:** `WorkForceDevelopment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Section Overview

<img src="../md_file_images/wfd-pathways-ptap.png" alt="Semiconductor Workforce & Apprenticeship Pathways section showing the eyebrow label, section heading, the 2x2 pathway card grid, and the PTAP feature block below with the program description and CTA buttons" />

*The Pathways & PTAP section — the 2×2 pathway card grid at the top and the highlighted PTAP callout block below it. Edit the pathway cards when updating training track descriptions; edit the PTAP block when updating the apprenticeship program details.*

The Workforce Pathways section (`id="workforce"`) contains two parts in the same `<section>`:

1. **Pathway cards** — a 2×2 grid of `.wfd-pathway-card` blocks describing training tracks
2. **PTAP feature block** — a highlighted two-column callout for the Process Technician Apprenticeship Program

Both parts use `wfd-reveal` + stagger classes for their scroll animation. Both are driven by `JS/workforce-development.js`, not an inline script.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Pathway cards grid | `class="wfd-pathways__grid"` | Add, remove, or edit pathway cards |
| Individual card | Card title text (e.g., `On-the-Job Training`) | Any field on that card |
| Card tags | `class="wfd-pathway-card__tags"` | Tags inside a specific card |
| PTAP block | `class="wfd-ptap"` | All PTAP content |
| PTAP partner logo | `class="wfd-ptap__partner"` | Partner name and logo image |
| PTAP features | `class="wfd-ptap__features"` | Checkbox-style feature list |

---

## SECTION 1 — Pathway Cards

### Template

```html
<div class="wfd-pathway-card wfd-reveal">
    <div class="wfd-pathway-card__icon"><i class="fas fa-tools"></i></div>
    <div class="wfd-pathway-card__title">Your Pathway Title</div>
    <p class="wfd-pathway-card__desc">
        Two to three sentences describing this training pathway and who it's for.
    </p>
    <div class="wfd-pathway-card__tags">
        <span class="wfd-pathway-card__tag">Tag One</span>
        <span class="wfd-pathway-card__tag">Tag Two</span>
        <span class="wfd-pathway-card__tag">Tag Three</span>
    </div>
</div>
```

### Adding a New Pathway Card

**Step 1** — Search for `class="wfd-pathways__grid"` and place the new card anywhere inside that `<div>`.

**Step 2** — Choose a Font Awesome solid icon. Replace `fa-tools` in `<i class="fas fa-tools"></i>` with any `fas fa-*` class. Find icons at [fontawesome.com/icons](https://fontawesome.com/icons) — filter by "Solid" style.

**Step 3** — Add the scroll animation stagger class. The current cards use `wfd-d1`, `wfd-d2`, `wfd-d3` in sequence after the first card (which has no delay class). Add `wfd-d4` to a fifth card, but **first verify `wfd-d4` is defined in `CSS/style.css`**:

```
Search style.css for: wfd-d4
```

If not found, either add it to `style.css` or reuse an existing delay class.

The current delay class assignments:

| Card | Classes |
|------|---------|
| Card 1 (On-the-Job Training) | `wfd-reveal` — no delay |
| Card 2 (Integrated Degree Programs) | `wfd-reveal wfd-d1` |
| Card 3 (Safety & Compliance) | `wfd-reveal wfd-d2` |
| Card 4 (Semiconductor Fundamentals) | `wfd-reveal wfd-d3` |

**Step 4** — Add two to three tags inside `wfd-pathway-card__tags`. Tags are styled for short labels (one to three words each).

> **Please double-check:** After adding the card, reload the page and scroll to it. If the card does not appear at all, the `wfd-reveal` class is missing or `workforce-development.js` is not loading. Open DevTools (F12 → Console) and check for script errors.

### Removing a Pathway Card

Search for the card title text and delete the entire `<div class="wfd-pathway-card ...">` block (from its opening tag through its closing `</div>`).

> **Note:** After removing a card, renumber the remaining stagger delay classes so they run consecutively (`wfd-d1`, `wfd-d2`, ...). Gaps in the sequence don't break anything visually, but the stagger timing will feel uneven.

---

## SECTION 2 — PTAP Feature Block

The PTAP block is a two-column highlight below the pathway cards: a photo on the left, detailed content on the right.

**Search for:** `class="wfd-ptap"`

### 2a — Changing the Photo

```html
<div class="wfd-ptap__image">
    <img src="Images/semiconductor_lab_technician.jpg" alt="...">
    <div class="wfd-ptap__image-overlay"></div>
</div>
```

Replace only the `src` filename and `alt` text. Do not remove the `wfd-ptap__image-overlay` div — it applies a dark gradient over the photo for contrast. Removing it will make the photo look washed-out against the section background.

### 2b — Updating the Partner Logo

The partner name and logo are in a two-part wrapper:

```html
<div class="wfd-ptap__partner">
    <span>In Partnership with</span>
    <img src="Images/TSMC.png" alt="TSMC Logo">
</div>
```

To change the partner:
1. Replace `src="Images/TSMC.png"` with the new logo's path in `Images/`.
2. Update the `alt` text to match the new company name.
3. The `<span>In Partnership with</span>` text can also be changed if the relationship type differs (e.g., "Sponsored by").

> **Note:** The partner logo displays at a constrained max-width set by `wfd-ptap__partner img` in `style.css`. Place logos at minimum 200 × 80 px to avoid appearing blurry. Use `.png` with a transparent background so the logo reads cleanly on the dark section background.

### 2c — Updating the Badge

The small label above the title reads "State-Registered Apprenticeship":

```html
<div class="wfd-ptap__badge">
    <i class="fas fa-star"></i>
    State-Registered Apprenticeship
</div>
```

Change the text directly. The `fas fa-star` icon can be swapped with any Font Awesome solid icon class.

### 2d — Updating PTAP Feature Checkboxes

The four bulleted features inside the PTAP block use Font Awesome check circles:

```html
<div class="wfd-ptap__features">
    <div class="wfd-ptap__feature"><i class="fas fa-check-circle"></i>OSHA 30 Safety</div>
    <div class="wfd-ptap__feature"><i class="fas fa-check-circle"></i>Vacuum Technology</div>
    ...
</div>
```

- To **change** a feature label: edit the text after `</i>` on the relevant line.
- To **add** a feature: paste `<div class="wfd-ptap__feature"><i class="fas fa-check-circle"></i>Your Label</div>` inside `.wfd-ptap__features`.
- To **remove** a feature: delete the entire `<div class="wfd-ptap__feature">` line.

> **Tip:** Keep feature labels short — one line of text. The features display in a two-column grid on desktop and the labels must not wrap awkwardly.

### 2e — Updating the "View PTAP Details" Link

```html
<a href="PTAP.html" class="wfd-ptap__link">
    View PTAP Details <i class="fas fa-arrow-right"></i>
</a>
```

If the PTAP page is renamed or moved, update `href="PTAP.html"` to match. The page at that path must exist — a broken link here is not caught by any JS and will give users a 404.

---

## SECTION 3 — Scroll Animation Reference

Every element with `class="wfd-reveal"` is invisible on load and fades in when it enters the viewport. This is controlled by `JS/workforce-development.js` via `IntersectionObserver`.

| Class | Effect |
|-------|--------|
| `wfd-reveal` | Required — element starts invisible; `is-visible` is added by JS on scroll |
| `wfd-d1` | Adds a short CSS `transition-delay` — element appears slightly after `wfd-reveal` elements at the same scroll position |
| `wfd-d2` | Medium delay |
| `wfd-d3` | Longest defined delay — used on the fourth pathway card |

> **Note:** The `is-visible` class (not `revealed`) is what triggers the CSS transition. If elements on this page stay invisible, check that `JS/workforce-development.js` is loading: open DevTools (F12 → Sources) and look for the file. If missing, verify the `<script src="JS/workforce-development.js">` tag at the bottom of the page body.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a pathway card | `class="wfd-pathways__grid"` |
| Edit a specific card | Card title text |
| Edit card tags | `class="wfd-pathway-card__tags"` |
| Edit PTAP block | `class="wfd-ptap"` |
| Change partner logo | `class="wfd-ptap__partner"` |
| Edit PTAP features | `class="wfd-ptap__features"` |
| Update PTAP link | `class="wfd-ptap__link"` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| Add a 5th pathway card | Verify `wfd-d4` exists in `style.css` before using it |
| Partner logo image file | Update `src` path AND `alt` text |
| PTAP page filename | Update `href` on `.wfd-ptap__link` |
| Remove a pathway card | Renumber remaining stagger delay classes in sequence |
