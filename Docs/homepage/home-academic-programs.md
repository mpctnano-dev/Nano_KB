# Home Page · Academic Programs Carousel
**File:** `index.html` + `JS/script.js`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="/md_file_images/homepage-academic-programs.png" alt="Integrated Education Paths academic programs carousel on the homepage" />

*The Integrated Education Paths carousel — cards are paginated with left/right arrow buttons and dot indicators. Note that the total card count is hardcoded in script.js, so you must update that number whenever you add or remove a card.*

The Integrated Education Paths section is a carousel of academic program cards. Cards are paginated — visitors click the left/right arrow buttons to move between pages of cards, and dots at the bottom track their position.

The carousel has a **hidden dependency in JavaScript**: the total card count is hardcoded as the number `7` in `script.js`. If you add or remove a card without updating that number, the arrow buttons and pagination dots will not work correctly.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Cards container | `id="eduCardsGrid"` | Add, remove, or reorder program cards |
| Individual card | `class="edu-card-wrapper"` | Edit any single card |
| Section heading | `class="edu-title"` | Change the heading text |
| View All button | `href="degree-programs.html"` | Change the button destination |

> **Note:** The carousel arrows and pagination dots are powered by `JS/script.js`. Any structural change to the cards (adding, removing) requires a matching update in that file. See Section 2.

---

## SECTION 1 — Adding a New Program Card

### Step 1 — Find the card container
In `index.html`, press **Ctrl+F** and search for:
```
id="eduCardsGrid"
```

### Step 2 — Paste the card template
Copy the block below and paste it **inside** `#eduCardsGrid`, after the last `</div>` of the last existing card wrapper and before the container's own closing `</div>`.

```html
<div class="edu-card-wrapper">
    <div class="edu-card">
        <div class="edu-card-header blue">              <!-- ① header color: blue or gold -->
            <span class="edu-semester">Fall 2026</span> <!-- ② auto-updated by JS — see Note -->
            <span class="edu-duration">4 Years</span>   <!-- ③ program length -->
        </div>
        <div class="edu-card-body">
            <span class="edu-card-tag blue">Bachelor of Science</span>  <!-- ④ degree tag -->
            <h3 class="edu-card-title">Program Name</h3>               <!-- ⑤ name -->
            <p class="edu-card-desc">Short description connecting this degree to the semiconductor or tech industry. Keep to 2–3 sentences.</p>  <!-- ⑥ -->
            <ul class="edu-card-topics">
                <li>Topic One</li>    <!-- ⑦ course topics — 3 to 5 items -->
                <li>Topic Two</li>
                <li>Topic Three</li>
                <li>Topic Four</li>
            </ul>
            <a href="degree-programs.html#anchor" class="edu-card-link">Explore Program  <!-- ⑧ link -->
                <svg width="16" height="16" viewBox="0 0 24 24" fill="none"
                    stroke="currentColor" stroke-width="2" stroke-linecap="round"
                    stroke-linejoin="round">
                    <line x1="5" y1="12" x2="19" y2="12"></line>
                    <polyline points="12 5 19 12 12 19"></polyline>
                </svg>
            </a>
        </div>
    </div>
</div>
```

### Step 3 — Fill in the eight values

| # | What | Where | Notes |
|---|------|-------|-------|
| ① | Header accent | `class="edu-card-header ???"` | Use `blue` or `gold` — alternate with the previous card |
| ② | Semester | `<span class="edu-semester">` | Leave as-is; JS overwrites it — see Section 3 |
| ③ | Duration | `<span class="edu-duration">` | E.g., `4 Years`, `2 Years`, `16 Weeks` |
| ④ | Degree tag | `<span class="edu-card-tag ???">` | Text + color (`blue` or `gold`) — match the header color |
| ⑤ | Program name | `<h3 class="edu-card-title">` | Official program name |
| ⑥ | Description | `<p class="edu-card-desc">` | 2–3 sentences; connect the degree to industry relevance |
| ⑦ | Topics | `<ul class="edu-card-topics">` | 3–5 bullet topics; keep each to 2–4 words |
| ⑧ | Link | `href=""` on `.edu-card-link` | Anchor on `degree-programs.html` or a standalone page |

### Step 4 — Update the card count in `script.js`  ← CRITICAL

> **This step is mandatory.** If you skip it, the carousel arrows and dots will malfunction.

Open `JS/script.js` and press **Ctrl+F** to search for:
```
Math.ceil(7 /
```

You will find this line:
```javascript
totalPages = Math.ceil(7 / cardsPerView); // 7 cards total
```

Change `7` to the new total number of cards. If you added one card to the existing seven:
```javascript
totalPages = Math.ceil(8 / cardsPerView); // 8 cards total
```

> **Note:** The comment `// 7 cards total` is there as a reminder — update it too so the next editor knows what number to change.

### Step 5 — Verify the carousel in the browser
Open `index.html`, scroll to Academic Programs, and confirm:
- The new card appears
- The left/right arrows advance and retreat correctly
- The correct number of dots appears at the bottom
- On a narrowed browser window (simulate mobile), the card still displays properly

> **Please double-check:** Test the right-arrow button on the last page — it should become disabled (grayed out) and stop responding. If it keeps working past the last card, the card count in `script.js` is still set to the old number.

---

## SECTION 2 — How the Carousel Works (Dependencies)

The carousel is driven by a JavaScript block in `JS/script.js`. It uses four HTML elements by their `id`:

| ID | Element | Role |
|----|---------|------|
| `eduCardsGrid` | The grid of cards | Slides left/right via CSS `transform` |
| `eduNavLeft` | Left arrow button | Goes to previous page |
| `eduNavRight` | Right arrow button | Goes to next page |
| `eduDots` | Dot row below carousel | Shows current page position |

**Do not rename or delete these four IDs.** If any of them is missing from the HTML, the entire carousel silently stops functioning — no error dialog, no warning, just non-working arrows.

### How pages are calculated

```javascript
function getCardsPerView() {
    if (window.innerWidth <= 768) return 1;   // mobile: 1 card per page
    if (window.innerWidth <= 1024) return 2;  // tablet: 2 cards per page
    return 3;                                 // desktop: 3 cards per page
}

totalPages = Math.ceil(7 / cardsPerView);     // ← change 7 to your card count
```

On desktop, 7 cards → 3 pages (3 + 3 + 1). On tablet, 7 cards → 4 pages. On mobile, 7 cards → 7 pages (one per page).

> **Note:** When you add a card, the dots regenerate automatically from the new `totalPages` value. You do not need to touch the dots HTML — but you **must** update the hardcoded `7` in the JS for the calculation to be correct.

---

## SECTION 3 — Semester Labels (Auto-Updated by JS)

The `<span class="edu-semester">` on each card is **overwritten by JavaScript every time the page loads**. The logic in `script.js` sets it to:

- **Feb 15 – Aug 31:** `Fall [current year]`
- **Sep 1 – Feb 14:** `Spring [current year]`

You do not need to update semester labels manually. The value in the HTML source is just a fallback — it only shows for a fraction of a second before JS updates it.

**Exception — PTAP card:** The Process Technician Apprenticeship card has `"Enrolling Now"` instead of a semester. The JS skips this card intentionally. Do not change it to a semester year.

---

## SECTION 4 — Removing a Program Card

Delete the entire `<div class="edu-card-wrapper">` block — from the opening tag to its matching closing `</div>`. Then update the card count in `script.js` (Section 1, Step 4).

> **Please double-check:** After removing a card, verify the right-arrow button disables correctly on the new last page.

---

## SECTION 5 — Header Color Options

Cards alternate between `blue` and `gold` header accents. Match the `edu-card-tag` color to the header color on the same card.

| Class on header | Class on tag | Appearance |
|----------------|-------------|-----------|
| `edu-card-header blue` | `edu-card-tag blue` | NAU Blue header + blue badge |
| `edu-card-header gold` | `edu-card-tag gold` | NAU Gold header + gold badge |

The current alternating pattern: Blue → Gold → Blue → Gold → Blue → Gold → Blue.  
Maintain this alternation when inserting a new card.

---

## SECTION 6 — Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add or remove a card | `id="eduCardsGrid"` (in `index.html`) |
| Update card count | `Math.ceil(7 /` (in `JS/script.js`) |
| Find a specific card | Search the program name (e.g., `Computer Engineering`) |
| Change header color | `class="edu-card-header` |
| Change degree tag | `class="edu-card-tag` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| Add or remove a card in `index.html` | Card count number in `JS/script.js` |
| `edu-card-header blue/gold` | `edu-card-tag blue/gold` on the same card |
