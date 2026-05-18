# Update News Filters

**One-line description:** Add, remove, or rename the filter buttons at the top of the News page.
**When you'd do this:** The lab starts publishing a new category of news (e.g., "Grants" or "Publications") and needs a dedicated button for it.
**Difficulty:** Easy
**Estimated time:** 5 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `News.html` | The visual filter buttons are hardcoded in the HTML. |

## Steps

### Part 1: Add the Filter Button

1. Open `News.html`.
2. Locate the filter bar by searching for `<div class="filter-group">`.
3. You will see a list of buttons that look like this:
   ```html
   <button class="news-filter-btn" data-tag="facility">Facility</button>
   ```
4. Copy an existing button and paste it where you want the new filter to appear.
5. Update the `data-tag` attribute to a single, lowercase word with no spaces (e.g., `data-tag="grants"`).
6. Update the visible text between the tags to what the user should see (e.g., `Grants & Funding`).

### Part 2: Tag Your Articles

The filter works by matching the button's `data-tag` to the `data-tags` attribute on individual news cards. 

1. Still in `News.html`, scroll down to the `<div class="news-grid" id="newsGrid">`.
2. Find the `<article class="news-card">` that should appear when your new filter is clicked.
3. Update the `data-tags` attribute to include your new tag (comma-separated, no spaces):
   ```html
   <!-- Before -->
   <article class="news-card" data-tags="announcement,partnership" ...>

   <!-- After -->
   <article class="news-card" data-tags="announcement,partnership,grants" ...>
   ```
4. (Optional) If you want the visual badge on the card to display this new category, update the HTML inside the card's `<div class="news-card__meta">` block:
   ```html
   <span class="news-card__tag news-card__tag--announcement">Grants</span>
   ```

## Verify It Worked
*   Load `News.html` locally.
*   Click your new filter button. Ensure it turns blue and the correct subset of articles appears below.
*   Click the "All" button to ensure the grid resets correctly.
