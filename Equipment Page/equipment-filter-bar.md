# Equipment Catalog · Filter Bar & Sort Order
**File:** `Equipment.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

The filter bar sits between the hero and the card grid. It has three functional areas:

1. **Search box** — live text search, powered automatically by `JS/script.js`. No manual changes needed.
2. **Category filter buttons** — show/hide cards by `data-category`. Adding a new category is a **three-part change**.
3. **Location filter buttons** — show/hide cards by `data-location`. Currently: All, Flagstaff, NVC (Phoenix).

The **sort order** for the card grid is controlled by an inline `<script>` at the very bottom of `Equipment.html` — it is listed here because adding a new category requires updating both the filter button and the sort array.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Category filter buttons | `class="filter-group"` (first one) | Add or remove category buttons |
| Location filter buttons | `class="filter-group location-filter"` | Add or remove location buttons |
| Sort order array | `const order = [` | Which category appears first in the grid |

---

## SECTION 1 — Adding a New Category

This is a three-part change. All three must be done or the feature will be broken in some way.

### Part 1 — Add the filter button

Search for:
```
class="filter-group"
```
The first `filter-group` is the category row. Add your new button **after the last existing category button**, before the closing `</div>`:

```html
<!-- Add after the last category button inside the first class="filter-group" -->
<button class="filter-btn category-btn" data-filter="Deposition">Deposition</button>
```

The `data-filter` value must exactly match the `data-category` you will put on the cards (case-sensitive).

### Part 2 — Add `data-category` to each card in that category

On every card belonging to the new category, set:

```html
<div class="tech-card" data-category="Deposition">
```

And update the badge to match:
```html
<span class="badge badge-purple">Deposition</span>
```

> **Note:** There is no badge class predefined for brand-new category names. Reuse one of the existing color classes (`badge-blue`, `badge-gold`, `badge-purple`, `badge-green`) that best represents the new category visually.

### Part 3 — Add the category to the sort order array

Search for:
```
const order = [
```

You will find the inline script at the bottom of `Equipment.html`:

```javascript
const order = ['Fabrication', 'Metrology', 'Mechatronics',
               'Electrical', 'Sample Prep', 'Support Systems', 'Educational'];
```

Insert your new category at the position where it should appear in the grid. Categories listed earlier appear higher on the page.

```javascript
// Example: adding Deposition after Fabrication
const order = ['Fabrication', 'Deposition', 'Metrology', 'Mechatronics',
               'Electrical', 'Sample Prep', 'Support Systems', 'Educational'];
```

> **Please double-check:** After making all three changes, reload the page, click the new filter button, and confirm only the correct cards appear. Then click "All" to confirm all cards return. A card that doesn't respond to the filter has a `data-category` that doesn't match `data-filter` exactly.

### What happens if you miss a part

| Part missed | Visible symptom |
|-------------|----------------|
| Filter button only | Cards exist but can't be filtered to in isolation |
| `data-category` on cards only | Filter button appears but clicking it shows zero cards |
| Sort order entry only | New cards appear, filter works, but new category sorts to the very end of the grid regardless of button position |

---

## SECTION 2 — Removing a Category

### Step 1 — Delete the filter button
Search for the button by its `data-filter` value and delete the `<button>` element.

### Step 2 — Remove from sort order
Search for `const order = [` and remove the category name from the array string.

> **Note:** The cards themselves do not need to be changed. Cards whose category has no filter button will still appear under "All" and still sort according to their position in `const order = [...]`. To remove the cards entirely, delete each `<div class="tech-card">` block for that category individually.

---

## SECTION 3 — Adding a New Location

The location filter currently supports `"Flagstaff"` and `"NVC"`. To add a third location (e.g., a Tucson facility):

### Step 1 — Add the location button

Search for:
```
class="filter-group location-filter"
```

Add a new button inside this group after the last location button:

```html
<button class="filter-btn location-btn" data-location="Tucson">Tucson</button>
```

### Step 2 — Add `data-location` to relevant cards

```html
<div class="tech-card" data-category="Metrology" data-location="Tucson">
```

For equipment at multiple locations including Tucson:
```html
<div class="tech-card" data-category="Metrology" data-location="Flagstaff,Tucson">
```

> **Note:** Cards with no `data-location` attribute will be hidden by the Tucson filter — they only appear under "All". This applies to any new specific location filter, not just Tucson.

---

## SECTION 4 — Changing Category Sort Order

The card grid always renders in the order defined by the `const order = [...]` array, regardless of where cards appear in the HTML source.

**Search for:** `const order = [`

```javascript
const order = ['Fabrication', 'Metrology', 'Mechatronics',
               'Electrical', 'Sample Prep', 'Support Systems', 'Educational'];
```

To move a category earlier in the grid, move its string earlier in the array:

```javascript
// Before: Metrology is second
const order = ['Fabrication', 'Metrology', ...];

// After: Metrology is first
const order = ['Metrology', 'Fabrication', ...];
```

Categories not listed in the array sort to the very end of the grid.

> **Tip:** The sort array position is completely independent of the filter button order in the HTML. Moving a button in the filter bar does not change visual sort order in the grid. You must update the array separately.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a category filter button | `class="filter-group"` (first group) |
| Add a location filter button | `class="filter-group location-filter"` |
| Update sort order | `const order = [` |

### Changes That Must Always Be Made Together

| If you add a new category… | You must also… |
|---|---|
| Add filter button | Add `data-category` to all cards in that category |
| Add `data-category` to cards | Add category string to `const order = [...]` |
| Add to `const order = [...]` | Add the filter button (otherwise sorted but not filterable in isolation) |
