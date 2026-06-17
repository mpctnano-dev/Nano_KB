
# Add / Modify News Category Filters

**Summary:** Add, remove, or modify the news category filter buttons situated in the filter bar at the top of the News page.

**When to change:** When the lab begins publishing a new category of articles (such as "Grants", "Publications", or "Student Spotlights") and requires a dedicated filter button.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 5 minutes

---

> [!NOTE]
> **Understanding Code Snippet Labels:**
> *   **Real code snippet:** Indicates the exact code block currently present in the codebase. Use these to search for and locate the correct files/lines.
> *   **Example snippet:** Represents a template or reference code structure. You will need to customize, adapt, or copy-paste these when implementing your changes.

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/NewsFilter_%231.png" alt="News category filter bar" /><br/>
  <em>Figure 1: The news category filter bar situated at the top of the News & Updates page, containing the active category buttons.</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/News.html` | The main News catalog document where the filter button sections are structured in the HTML. |

---

## Step-by-Step Instructions

### Part 1: Insert the Filter Button in News.html

1. To get started, please open `News.html` in your code editor.
2. Scroll to the top of the page below the header and search for the division tag with the class name `"filter-group"`:
   
   **Real code snippet:**
   `<div class="filter-group">`

   <p align="center">
     <img src="/md_file_images/NewsFilter_%232.png" alt="Locating the filter-group in News.html" /><br/>
     <em>Figure 2: Locating the filter-group division container inside News.html where filter button elements are declared.</em>
   </p>

   This container division is where all active filter buttons are structured. In your editor, you will see a list of button tags representing each category currently shown on the live site (such as "Facility", "Equipment", and "Workforce").
3. Inside this container division tag, you will see a list of category filtering buttons.
4. Copy an existing button block and paste it inside the container in your desired order:
   
   **Example snippet:**
   ```html
   <button class="news-filter-btn" data-tag="facility">Facility</button>
   ```
5. Customize your new category button:
   * **The Data Tag Hook:** Change the `data-tag` attribute to a single, lowercase identifier (no spaces):
     
     **Real code snippet:**
     `data-tag="grants"`
   * **The Button Label:** Modify the visible text to clearly describe the category for readers:
     
     **Real code snippet:**
     `<button class="news-filter-btn" data-tag="grants">Grants &amp; Funding</button>`

> [!TIP]
> **Removing a Filter Button:**
> To remove a category filter from the site, simply locate and delete (or comment out) the corresponding `<button class="news-filter-btn" ...>` line from the `"filter-group"` container in `News.html`.

---

### Part 2: Tag Your Preview Cards

The client-side filtering script operates by matching the button's `data-tag` value to the comma-separated `data-tags` attribute defined on the instrument/article cards.

1. Still inside `News.html`, scroll down to the news grid container division:
   
   **Real code snippet:**
   `<div class="news-grid" id="newsGrid">`

   <p align="center">
     <img src="/md_file_images/News_%233.png" alt="Locating the newsGrid container in News.html" /><br/>
     <em>Figure 3: Locating the main newsGrid container element inside News.html where individual article cards are declared.</em>
   </p>

   This container represents the main news catalog grid holding all of MPaCT's news card items. The custom client-side script targets this specific wrapper element by its ID (`newsGrid`) to execute filtering.

2. Locate the individual article tags (`<article class="news-card">`) representing the news entries you want to associate with your new filter.
3. Edit their `data-tags` attribute to include your new tag value (separated by commas with no spaces):
   
   **Real code snippet:**
   ```html
   <!-- Before edit -->
   <article class="news-card" data-tags="announcement,partnership" ...>

   <!-- After edit -->
   <article class="news-card" data-tags="announcement,partnership,grants" ...>
   ```

   <p align="center">
     <img src="/md_file_images/NewsFilter_%233.png" alt="Modifying data-tags attribute in News.html" /><br/>
     <em>Figure 4: Modifying the data-tags attribute of a news card inside News.html to associate it with the new category filter.</em>
   </p>

   The `data-tags` attribute contains a comma-separated list of category keys associated with that specific news entry. Appending your new tag key enables the matching filter button to find and display this card when clicked.
4. **Update the Card Badge:** To display a visual category badge on the card, modify the text inside the metadata span tag within the card:
   
   **Example snippet:**
   `<span class="news-card__badge">Grants</span>`

   <p align="center">
     <img src="/md_file_images/NewsFilter_%234.png" alt="Updating the news card badge markup" /><br/>
     <em>Figure 5: Updating the text content within the news-card__badge span inside the article card body to display the new category.</em>
   </p>

   The card badge is a visual tag styling (like the blue background and white text for scholarships, or green for equipment) located inside the card's header area. Modifying this span element prints the friendly category name on the card's thumbnail.

> [!TIP]
> **Removing Tags from Card Previews:**
> To dissociate an article card from a filter, delete the target identifier from that card's comma-separated `data-tags="..."` attribute list, and update or clear the `<span class="news-card__badge">` text label.

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

*Last reviewed: May 26, 2026*
