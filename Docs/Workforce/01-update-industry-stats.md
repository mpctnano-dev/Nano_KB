# Update Workforce Industry Statistics

**Summary:** Update the key statistical metrics and impact figures displayed inside the hero panel and the Arizona Semiconductor Boom cards on the Workforce Development page.

**Trigger:** When annual workforce reporting yields new figures for student enrollment, technology industry job expansion, or capital investment projections.

**Difficulty:** Trivial

**Estimated Time:** 5 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The main Workforce Development page document containing the hardcoded statistical panel structures. |

---

## Step-by-Step Instructions

### Part 1: Update the Hero Overview Stats

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Please search for the stats container division tag with the class name `"wfd-hero__panel-stats"`. Inside, you will find:
   `<div class="wfd-hero__panel-stats">`
3. Inside this block, locate individual statistical subdivisions:
   * **The Metric Value:** Update the number inside the division tag with the class name `"wfd-pstat__num"`. You can place symbols or suffixes inside an emphasis tag (`<em>`) to colorize them gold:
     `<div class="wfd-pstat__num">40<em>K+</em></div>`
   * **The Metric Label:** Modify the accompanying label inside the division tag with the class name `"wfd-pstat__label"`:
     `<div class="wfd-pstat__label">AZ Tech Jobs</div>`

---

### Part 2: Update the Arizona Semiconductor Boom Cards

1. Scroll down the document to find the stats grid container division tag:
   `<div class="wfd-stats__grid">`
2. Each stat highlight card is represented by a division tag with the class name `"wfd-stat"`:
   `<div class="wfd-stat">`
3. You can customize the elements inside this block using the following guidelines:
   * **The Numerical Value:** Update the number inside the division tag with the class name `"wfd-stat__num"`. Suffixes (like a plus sign or units) should be wrapped inside a span tag (`<span>`):
     `<div class="wfd-stat__num">$210<span>B+</span></div>`
   * **The Paragraph Description:** Modify the description text inside the division tag with the class name `"wfd-stat__desc"`:
     `<div class="wfd-stat__desc">Capital Investment<br>Since 2020</div>`
4. **Update the Citation:** If you change these numbers, scroll to the bottom of the grid and locate the citation source division tag:
   `<div class="wfd-stats__source">`
   Update the citation text or links to accurately reflect the data reporting source.

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Direct styling shifts, unmatched padding heights, or adding excessive list items can cause grids or column layouts to wrap unevenly.
*   **Visual Impact:** Uneven spacing, jagged line-breaks, or overlapping panels that look unpolished.
*   **Recommended Solutions:**
    *   **Strict Alignment:** Maintain matching card lists in even multipliers (such as multiples of 2, 3, or 4).
    *   **Height Uniformity:** Standardize description lengths and use robust layout parameters (like `min-height`) in global CSS files.
    *   **Preview Tools:** Always verify changes locally across desktop and mobile views before merging into the main repository.

---

## Let's Verify Your Changes

1. Open `WorkForceDevelopment.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Verify that the updated numerical figures and label descriptions render correctly, and check that all gold suffixes and plus indicators sit aligned.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
