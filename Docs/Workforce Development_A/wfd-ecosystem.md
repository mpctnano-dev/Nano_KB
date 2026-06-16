# Workforce Development · Arizona Ecosystem Cards (§6)
**File:** `WorkForceDevelopment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Section Overview

The Ecosystem section displays four company cards in a grid. Each card shows a company logo, an investment/descriptor tag, a company name, and a short description paragraph.

The non-obvious parts of this section:

1. The company logo sits inside a **wrapper div** (`wfd-eco__card-logo-wrap`) that controls its display size. Placing the logo `<img>` directly in the card head without this wrapper breaks the logo alignment.
2. The investment text is a **plain `<span>`** with class `wfd-eco__card-invest` — not a badge class. Using a `badge-*` class from the equipment page here will break the card head's flex layout.
3. Logo images require a **transparent background** to display correctly against the dark card background.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| All company cards | `class="wfd-eco__grid"` | Add or remove cards |
| A specific card | Company name (e.g., `TSMC Arizona`) | Any field on that card |
| Company logo | `class="wfd-eco__card-logo"` | Logo image src and alt |
| Investment text | `class="wfd-eco__card-invest"` | Dollar figure or descriptor text |
| "More companies" bar | `class="wfd-eco__more"` | The text listing additional companies |

---

## SECTION 1 — Card Template

```html
<div class="wfd-eco__card">
    <div class="wfd-eco__card-head">
        <div class="wfd-eco__card-logo-wrap">
            <img src="Images/YourLogo.png" alt="Company Name" class="wfd-eco__card-logo">
        </div>
        <span class="wfd-eco__card-invest">$165B Invested</span>
    </div>
    <h3>Company Name</h3>
    <p>Two to three sentence description of the company's Arizona presence and relevance to NAU graduates.</p>
</div>
```

---

## SECTION 2 — Adding a New Company Card

### Step 1 — Prepare the logo image

Place the logo file in `Images/`. Requirements:

| Property | Required |
|----------|----------|
| Format | `.png` or `.webp` — must have a **transparent background** |
| Size | 200 × 80 px minimum; aspect ratio should be roughly landscape |
| File size | Under 80 KB |
| Naming | No spaces: `company_logo.png` ✓ — `company logo.png` ✗ |

> **Note:** If the logo has a white background, it will appear as a white rectangle on the dark card background. Use a version with a transparent background, or ask for an SVG/PNG export from the company's press kit. Most major companies provide these at their brand resource pages.

### Step 2 — Add the card

Search for `class="wfd-eco__grid"` and paste the card template inside that div. Cards can be placed in any order.

### Step 3 — Fill in the four values

| Field | Notes |
|-------|-------|
| `src` on `<img>` | Path to logo in `Images/` |
| `alt` on `<img>` | Company name only — no extra description needed |
| `wfd-eco__card-invest` text | Dollar investment, "AZ Headquartered", "Standards Group", etc. Keep under 25 characters |
| `<h3>` and `<p>` | Company name and 2–3 sentence description |

> **Please double-check:** After adding the card, reload the page at both desktop and mobile widths. Confirm the logo is visible against the dark background and the card head alignment looks even. If the logo appears as a white box, the image does not have a transparent background.

---

## SECTION 3 — Removing a Company Card

Search for the company name (e.g., `TSMC Arizona`) and delete the entire `<div class="wfd-eco__card">` block from its opening tag through its closing `</div>`.

> **Note:** The grid is styled for four cards. After removing one, three cards will display — the grid may reflow to show all three on one row on wide screens, leaving a visible gap. Test at desktop widths and add a replacement card if the layout looks unbalanced.

---

## SECTION 4 — Updating the "More Companies" Bar

Below the four cards, a text bar lists additional companies operating in Arizona.

**Search for:** `class="wfd-eco__more"`

```html
<div class="wfd-eco__more wfd-reveal wfd-d2">
    <div class="wfd-eco__more-text">
        <strong>280+</strong> semiconductor companies operating in Arizona, including onsemi,
        Amkor, ASM, ASML, Boeing, Raytheon, and Honeywell.
    </div>
</div>
```

Edit the `<strong>` count number and the company list inside `wfd-eco__more-text` directly. Keep the list to a single line of text — it wraps awkwardly at narrow screen widths if it runs too long.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a company card | `class="wfd-eco__grid"` |
| Find a specific card | Company name (e.g., `Intel Arizona`) |
| Update logo image | `class="wfd-eco__card-logo"` |
| Update investment text | `class="wfd-eco__card-invest"` |
| Update "more companies" bar | `class="wfd-eco__more"` |

### Changes That Must Always Be Made Together

| If you change… | You must also… |
|---|---|
| Add a new company card | Place logo in `Images/` with transparent background first |
| Logo image filename | Update `src` path in the `<img>` tag |
| Remove a card (leaving 3) | Verify 3-card layout looks intentional at desktop widths |
