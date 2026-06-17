# MPaCT Lab Page · Areas of Scientific Focus
**File:** `MPaCT.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="../md_file_images/mpact-research-areas.png" alt="Areas of Scientific Focus section showing the section heading and four numbered research entries (01 Thin Film Deposition, 02 Micro and Nanofabrication, 03 Materials Characterization, 04 Semiconductor Device Testing) each with a title, description, and instrument tags" />

*The Areas of Scientific Focus section — four numbered research area entries. Each entry has a number, title, description, and instrument tags. This is what you edit when adding or updating a research area.*

This section displays four numbered research area entries. Each entry has a two-digit number (`01`–`04`), a title, a description paragraph, and a row of instrument/method tags. The entries animate in from the left in a staggered sequence when the user scrolls to this section — this stagger is controlled by CSS delay classes on each card.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Research area grid | `class="ml-research__grid"` | Add, remove, or reorder entries |
| Individual entry | `class="ml-ra ml-reveal"` | Number, title, description, tags |
| Instrument tags | `class="ml-ra__tags"` | Add or remove tag labels |

> **Note:** Each entry carries `class="ml-reveal"` — this is required for the scroll fade-in animation driven by `JS/mpact.js`. Do not remove it when adding new entries.

---

## SECTION 1 — How the Stagger Animation Works

The first entry animates immediately on scroll. Entries 2, 3, and 4 each have an additional delay class that makes them appear slightly after the one before, creating a stagger effect.

| Entry | Classes on the `<div>` | Delay |
|-------|------------------------|-------|
| 01 (first) | `class="ml-ra ml-reveal"` | None — animates first |
| 02 | `class="ml-ra ml-reveal ml-d1"` | Short delay |
| 03 | `class="ml-ra ml-reveal ml-d2"` | Medium delay |
| 04 | `class="ml-ra ml-reveal ml-d3"` | Longest delay |

When adding a new entry, assign it the next delay class in sequence. If you add a fifth entry, use `ml-d4` — but first verify `ml-d4` is defined in `CSS/style.css`. Search for `ml-d3` in `style.css` to see if a `ml-d4` rule already exists next to it. If not, you will need to add it.

---

## SECTION 2 — Editing an Existing Entry

### Finding an entry
Search for:
```
class="ml-research__grid"
```

Each entry inside the grid follows this structure:
```html
<div class="ml-ra ml-reveal">        <!-- or ml-d1 / ml-d2 / ml-d3 for entries 2–4 -->
    <div class="ml-ra__num">01</div>  <!-- ① number label -->
    <div>
        <div class="ml-ra__title">Thin Film Deposition &amp; Analysis</div>  <!-- ② title -->
        <p class="ml-ra__desc">Investigation of PVD/CVD thin film properties...</p>  <!-- ③ description -->
        <div class="ml-ra__tags">
            <span class="ml-ra__tag">XRD</span>         <!-- ④ tags -->
            <span class="ml-ra__tag">Ellipsometry</span>
            <span class="ml-ra__tag">Sputtering</span>
        </div>
    </div>
</div>
```

| # | What | Where | Notes |
|---|------|-------|-------|
| ① | Entry number | `class="ml-ra__num"` | Sequential two-digit labels: `01`, `02`, `03`, `04` |
| ② | Title | `class="ml-ra__title"` | Short capability name |
| ③ | Description | `class="ml-ra__desc"` | 2–3 sentences |
| ④ | Tags | `<span class="ml-ra__tag">` inside `.ml-ra__tags` | Instrument or method names; 2–4 tags per entry |

---

## SECTION 3 — Adding or Removing Tags

Tags are plain `<span>` elements inside `class="ml-ra__tags"`.

**To add a tag:**
```html
<span class="ml-ra__tag">New Instrument</span>
```
Paste it inside `.ml-ra__tags` after the last existing `<span>`.

**To remove a tag:** delete the `<span>` element.

> **Tip:** Keep tags to instrument names or method abbreviations (e.g., `AFM`, `XRD`, `I-V / C-V`). Full sentences don't work in tag pills — they overflow the pill shape.

---

## SECTION 4 — Adding a New Research Area

### Step 1 — Find the grid
Search for `class="ml-research__grid"`.

### Step 2 — Paste the template
Add a new entry after the last `</div>` of the fourth entry:

```html
<div class="ml-ra ml-reveal ml-d4">
    <div class="ml-ra__num">05</div>
    <div>
        <div class="ml-ra__title">Your Research Area Title</div>
        <p class="ml-ra__desc">2–3 sentence description of the research area and the kinds of questions it addresses.</p>
        <div class="ml-ra__tags">
            <span class="ml-ra__tag">Instrument One</span>
            <span class="ml-ra__tag">Instrument Two</span>
            <span class="ml-ra__tag">Method</span>
        </div>
    </div>
</div>
```

### Step 3 — Verify `ml-d4` exists in CSS
Open `CSS/style.css` and search for:
```
ml-d3
```
If `ml-d4` is not defined immediately after it, add this rule to `style.css` in the same block:
```css
.ml-d4 { animation-delay: 0.6s; }
```
Adjust the delay value to maintain the stagger rhythm (existing delays are typically 0.15s apart).

### Step 4 — Verify the animation in the browser
Scroll down to this section and confirm the new entry animates in after the fourth entry. If it appears instantly with no animation, either `ml-reveal` is missing or `mpact.js` did not find the element.

> **Please double-check:** After adding an entry, also update the number sequence. If the new entry is 05, confirm `01` through `04` are still numbered correctly in order.

---

## SECTION 5 — Removing a Research Area

Delete the entire `<div class="ml-ra ml-reveal ...">` block. Then re-check that the remaining entries' number labels (`ml-ra__num`) are still sequential.

> **Note:** After removing an entry, renumber the remaining entries if needed to keep the `01`, `02`, `03` sequence clean. The numbers are plain text — just edit the content inside `class="ml-ra__num"`.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Find all research entries | `class="ml-research__grid"` |
| Edit a specific entry | Search its title (e.g., `Thin Film Deposition`) |
| Add or edit a tag | `class="ml-ra__tags"` |
| Check delay classes in CSS | `ml-d3` in `CSS/style.css` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| Add a new entry with `ml-d4` | Verify `ml-d4` is defined in `CSS/style.css` |
| Remove an entry | Renumber remaining entries (`ml-ra__num`) to stay sequential |
| Add/remove an instrument from tags | No other dependency — tags are independent labels |
