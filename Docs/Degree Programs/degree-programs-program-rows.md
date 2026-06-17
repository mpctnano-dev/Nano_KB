# Degree Programs · Bachelor's Program Rows
**File:** `degree-programs.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="/md_file_images/degree-programs-rows.png" alt="Bachelor's Degree Programs section showing the section heading and three horizontal program row cards — Computer Engineering, Computer Science, Electrical Engineering — each with a photo, degree type, description, feature tags, and a View Degree Details link" />

*The Bachelor's Degree Programs section — each degree is a horizontal row with a photo on the left and content on the right. This is the section you edit when adding, removing, or updating a program row.*

The Bachelor's Programs section displays each degree as a horizontal `<article class="program-row">` inside `<div class="program-rows">`. Each row has a photo on the left, content on the right (title, degree type, description, four feature tags, and an external link).

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| All program rows | `class="program-rows"` | Add or remove rows |
| A specific row | The degree name (e.g., `Computer Engineering`) | Any field on that row |
| Feature tags | `class="program-row-features"` | Feature text and inline SVG icon |
| External link | `class="program-row-link"` | Destination URL and link text |

> **Note:** Feature icons are **inline SVG**, not Font Awesome. There is no icon class to reference — the SVG path markup is pasted directly into the HTML. See Section 3 for how to handle this.

---

## SECTION 1 — Adding a New Program Row

### Step 1 — Find the container

Search for:
```
class="program-rows"
```

### Step 2 — Paste the template

Add the new `<article>` block anywhere inside `<div class="program-rows">`. Visual order on the page matches the HTML source order for this section.

```html
<article class="program-row reveal-on-scroll" id="your-program-id">
    <div class="program-row-image">
        <img src="Images/YourImage.jpg" alt="Descriptive alt text">
    </div>
    <div class="program-row-content">
        <div class="program-row-header">
            <h3 class="program-row-title">Your Degree Name</h3>
            <span class="program-row-degree">Bachelor of Science</span>
        </div>
        <p class="program-row-desc">
            Two to three sentences describing the program and its connection to the semiconductor/engineering industry.
        </p>
        <div class="program-row-features">
            <div class="program-row-feature">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <!-- paste SVG path here — see Section 3 -->
                </svg>
                Feature Label One
            </div>
            <div class="program-row-feature">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <!-- paste SVG path here -->
                </svg>
                Feature Label Two
            </div>
            <div class="program-row-feature">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <!-- paste SVG path here -->
                </svg>
                Feature Label Three
            </div>
            <div class="program-row-feature">
                <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <!-- paste SVG path here -->
                </svg>
                Feature Label Four
            </div>
        </div>
        <a href="https://degree-search.nau.edu/degree/your-degree-slug" target="_blank" class="program-row-link">
            View Degree Details
            <svg viewBox="0 0 24 24" width="16" height="16" fill="currentColor">
                <path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z" />
            </svg>
        </a>
    </div>
</article>
```

### Step 3 — Fill in the six values

| Field | Where | Notes |
|-------|-------|-------|
| `id` attribute | `<article ... id="your-program-id">` | Used for anchor links (e.g., `degree-programs.html#computer-engineering`). Use lowercase-with-hyphens. Required. |
| Image | `<img src="Images/YourImage.jpg">` | Place file in `Images/` — see image specs in Section 4 |
| Degree name | `<h3 class="program-row-title">` | Short official degree name |
| Degree type | `<span class="program-row-degree">` | E.g., `Bachelor of Science` or `Bachelor of Science (BSET)` |
| Description | `<p class="program-row-desc">` | 2–3 sentences only — longer text overflows on mobile |
| Feature labels | Text after each `</svg>` in `.program-row-feature` | Exactly 4 features — see note below |
| External link | `href` on `.program-row-link` | Must link to an official NAU page. Use `target="_blank"` |

> **Note:** The `.program-row-features` grid is styled for exactly four items. A fifth feature will overflow the grid boundary on narrow screens — stick to four, or verify thoroughly at 320px–768px widths if you must add a fifth.

> **Please double-check:** After adding the row, reload the page and scroll to it. Confirm the row fades in on scroll (the `reveal-on-scroll` animation). If it does not fade in, the inline scroll script may have a syntax error — open DevTools (F12 → Console) and check for errors.

---

## SECTION 2 — Removing a Program Row

Search for the degree name (e.g., `Computer Engineering`) to find its `<article>` block. Delete the entire `<article>` from its opening tag to its closing `</article>`.

The remaining rows reflow automatically — no gap is left.

> **Tip:** If you want to temporarily hide a program rather than delete it, wrap the article in an HTML comment:
> ```html
> <!-- Temporarily hidden
> <article class="program-row reveal-on-scroll" id="...">
>     ...
> </article>
> -->
> ```

---

## SECTION 3 — Working with Inline SVG Feature Icons

Unlike most site icons (which use Font Awesome), program row feature icons are **inline SVG elements**. There is no icon class — you paste SVG `<path>`, `<rect>`, `<circle>`, or `<line>` elements directly inside an `<svg>` wrapper.

### Reusing an existing icon from another row

The simplest approach: copy the entire `<svg>...</svg>` block from an existing `.program-row-feature` on the page and paste it into your new feature. The icon will look the same as the one you copied.

### Using a simple generic icon

If you need a generic "bullet" icon and don't want to copy a complex path, use this minimal diamond shape:

```html
<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
    <polygon points="12 2 22 12 12 22 2 12" />
</svg>
```

### Getting a specific icon

Icons on this site were sourced from [Feather Icons](https://feathericons.com). To find a matching icon:
1. Go to feathericons.com, find your icon
2. Click it — a modal appears with the SVG source
3. Copy just the inner path/shape elements (not the outer `<svg>` tag itself)
4. Paste them inside the `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">` wrapper in the template

> **Note:** The `stroke="currentColor"` attribute means the icon color inherits from the surrounding CSS text color. Do not replace it with a hardcoded hex value.

---

## SECTION 4 — Image Specs for Program Rows

| Property | Recommended |
|----------|------------|
| Format | `.jpg` for photos |
| Dimensions | At least 600 × 400 px — the image box is square on desktop and fills full-width on mobile |
| File size | Under 250 KB |
| Naming | No spaces: `electrical_engineering.jpg` ✓ — `electrical engineering.jpg` ✗ |
| Location | `Images/` directory only |

The `<img>` tag has no `onerror` fallback in this section. A missing image leaves an empty gray box — check the browser console for a 404 error if the box appears blank.

---

## SECTION 5 — Updating External Links

Each program row ends with a "View Degree Details" link that opens the official NAU degree page in a new tab.

**Search for:** `class="program-row-link"`

```html
<a href="https://degree-search.nau.edu/degree/computer-engineering-bs" target="_blank" class="program-row-link">
    View Degree Details
    ...
</a>
```

To update the destination, change only the `href` value. The link text ("View Degree Details") and the arrow SVG after it should remain as-is for visual consistency.

> **Note:** Always use `target="_blank"` on external NAU links so the user does not leave the nano.nau.edu site. Without `target="_blank"`, clicking the link navigates away from the page entirely.

> **Please double-check:** After updating a link, open the page and click the button to verify it opens the correct NAU page in a new tab.

---

## SECTION 6 — Scroll Animation (`reveal-on-scroll`)

Every `<article class="program-row">` also has `class="reveal-on-scroll"`. This triggers the fade-in animation defined by an inline `<script>` at the very bottom of `degree-programs.html`.

The script works as follows:
- On page load and on scroll, it checks if each `.reveal-on-scroll` element has entered the viewport
- When it does, it adds the class `revealed` to that element
- CSS in `style.css` defines the `revealed` state as fully visible (opacity + transform)

**Optional stagger delay:** Add `data-delay="200"` to an element to delay its reveal by 200 ms after entering the viewport. Useful when two elements enter at the same time and you want them to appear slightly offset.

```html
<!-- No delay (default) -->
<article class="program-row reveal-on-scroll" id="...">

<!-- 200ms delay after viewport entry -->
<div class="campus-card reveal-on-scroll" data-delay="200">
```

> **Note:** This scroll script is **not** the same as `mpact.js`. Do not assume behavior from `MPaCT.html` applies here. The animation relies on the inline `<script>` block — if that block is accidentally deleted, all `reveal-on-scroll` elements will stay invisible on the page.

---

## Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Add a new program row | `class="program-rows"` |
| Find a specific row | Degree name (e.g., `Mechanical Engineering`) |
| Change feature labels/icons | `class="program-row-features"` |
| Update external degree link | `class="program-row-link"` |
| Change degree type label | `class="program-row-degree"` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| Add a new `<article class="program-row">` | Add a unique `id` attribute — required for anchor navigation |
| Feature count beyond 4 | Test at mobile widths — grid is sized for 4 items |
| An external `href` on `.program-row-link` | Verify the target URL is live before deploying |
| A program row image filename | Update the `src` path in the `<img>` tag — no auto-resolution |
