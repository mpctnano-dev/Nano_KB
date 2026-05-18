# Update Ecosystem Partners

**One-line description:** Add or update the industry partner cards (e.g., TSMC, Intel) in the Ecosystem section.
**When you'd do this:** NAU signs a new partnership agreement or an existing partner announces a major new investment.
**Difficulty:** Routine
**Estimated time:** 15 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The ecosystem cards are hardcoded in the HTML. |
| `Images/` | You will need to upload a new logo image for a new partner. |

## Steps

1. **Prepare the Logo:** Obtain a high-quality, transparent `.png` logo for the new partner and save it to the `Images/` folder (e.g., `Images/new_partner.png`).
2. Open `WorkForceDevelopment.html`.
3. Locate the ecosystem grid by searching for `<div class="wfd-eco__grid">`.
4. Copy an existing `<div class="wfd-eco__card">` block and paste it into the grid.
5. Update the block with the new details:

| Component | HTML Element | Example |
|---|---|---|
| **Logo** | `<img src="..." class="wfd-eco__card-logo">` | `<img src="Images/new_partner.png" alt="Company Name">` |
| **Investment Badge** | `<span class="wfd-eco__card-invest">` | `$10B Invested` or `AZ Headquartered` |
| **Company Name** | `<h3>` | `<h3>NXP Semiconductors</h3>` |
| **Description** | `<p>` | A 1-2 sentence summary of their Arizona footprint. |

```html
<!-- Example Card -->
<div class="wfd-eco__card">
    <div class="wfd-eco__card-head">
        <div class="wfd-eco__card-logo-wrap">
            <img src="Images/intel.png" alt="Intel" class="wfd-eco__card-logo">
        </div>
        <span class="wfd-eco__card-invest">$34.5B Invested</span>
    </div>
    <h3>Intel Arizona</h3>
    <p>Arizona hosts Intel's largest manufacturing site...</p>
</div>
```

## Design Considerations: Expanding the Grid

Currently, the `.wfd-eco__grid` is designed as a rigid 4-column layout (`grid-template-columns: repeat(4, minmax(0, 1fr))`). 

> [!WARNING]
> If you add a 5th partner, the grid will wrap to a new row, leaving a single card sitting awkwardly below the original four.

**If the number of partners increases beyond 4, we strongly suggest updating the layout to a horizontal scroll:**
1. **Manual Scroll (CSS Only):** Change the grid to `display: flex; overflow-x: auto; scroll-snap-type: x mandatory; gap: 20px;` and give the cards a `min-width` (e.g. `min-width: 300px;`) so users can swipe or use a scrollbar to view more partners without breaking the page height.
2. **Carousel Arrows (JS + CSS):** Add left/right navigation arrows. You can reuse the existing `scrollGrid()` logic found in `JS/script.js` (used on the homepage equipment carousel) to smoothly pan the ecosystem cards left and right when arrows are clicked.
3. **Infinite Marquee (CSS Animation):** If there are many partners (8+), you could convert the row into a continuously scrolling CSS animation.

## Verify It Worked
*   Load `WorkForceDevelopment.html` locally.
*   Verify the new logo image loads and is centered correctly within the white bounding box.
*   Check that the grid layout wraps nicely on smaller screens.
