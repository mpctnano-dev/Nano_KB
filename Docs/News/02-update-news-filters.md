# Update News Catalog Filters

**Summary:** Add, remove, or modify the news category filter buttons situated in the filter bar at the top of the News page.

**Trigger:** When the lab begins publishing a new category of articles (such as "Grants", "Publications", or "Student Spotlights") and requires a dedicated filter button.

**Difficulty:** Routine

**Estimated Time:** 5 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `News.html` | The main News catalog document where the filter button sections are structured in the HTML. |

---

## Step-by-Step Instructions

### Part 1: Insert the Filter Button in News.html

1. To get started, please open `News.html` in your code editor.
2. Scroll to the top of the page below the header and search for the division tag with the class name `"filter-group"`:
   `<div class="filter-group">`
3. Inside this container division tag, you will see a list of category filtering buttons.
4. Copy an existing button block and paste it inside the container in your desired order:
   ```html
   <button class="news-filter-btn" data-tag="facility">Facility</button>
   ```
5. Customize your new category button:
   * **The Data Tag Hook:** Change the `data-tag` attribute to a single, lowercase identifier (no spaces):
     `data-tag="grants"`
   * **The Button Label:** Modify the visible text to clearly describe the category for readers:
     `<button class="news-filter-btn" data-tag="grants">Grants &amp; Funding</button>`

---

### Part 2: Tag Your Preview Cards

The client-side filtering script operates by matching the button's `data-tag` value to the comma-separated `data-tags` attribute defined on the instrument/article cards.

1. Still inside `News.html`, scroll down to the news grid container division:
   `<div class="news-grid" id="newsGrid">`
2. Locate the individual article tags (`<article class="news-card">`) representing the news entries you want to associate with your new filter.
3. Edit their `data-tags` attribute to include your new tag value (separated by commas with no spaces):
   ```html
   <!-- Before edit -->
   <article class="news-card" data-tags="announcement,partnership" ...>

   <!-- After edit -->
   <article class="news-card" data-tags="announcement,partnership,grants" ...>
   ```
4. **Update the Card Badge:** To display a visual category badge on the card, modify the text inside the metadata span tag within the card:
   `<span class="news-card__badge">Grants</span>`

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

1. Open `News.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Confirm that your new category filter button appears in the filter row.
3. Click the new button and verify that only the associated articles display below.
4. Click the "All" category button to confirm that the entire grid resets cleanly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
