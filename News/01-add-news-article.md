# Add a News Article

**One-line description:** Add a new article to the news grid and the interactive reader.
**When you'd do this:** The lab has a new announcement, equipment installation, or milestone to share.
**Difficulty:** Complex
**Estimated time:** 20-30 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `News.html` | The compact "teaser" cards shown in the grid. |
| `JS/news.js` | The full article payload (text, stats, gallery) shown when a user clicks a card. |

> [!WARNING]
> The news page uses a hybrid approach. The grid cards are hardcoded in `News.html` so they load instantly. When clicked, `news.js` intercepts the click and loads the full article text from the `ARTICLES` array. **You must update both files** and ensure the `data-article-id` and `id` match exactly.

## Steps

### Part 1: Update the Data Payload (`JS/news.js`)

1. Open `JS/news.js`.
2. Locate the `const ARTICLES = [` array.
3. Add a new object to the top of the array (so it appears as the newest). 
4. Fill out the properties:

```javascript
{
    id: 'my-new-article-id', // MUST be unique. No spaces.
    title: 'My New Article Title',
    tagLabel: 'Facility', // E.g., Facility, Installation, Partnership
    date: '2026-06-01', // YYYY-MM-DD format
    readTime: '3 min read',
    heroImage: 'Images/my_image.jpg',
    heroAlt: 'Description of image',
    statusBadge: 'New Update',
    
    // 3 quick stats shown at the top
    stats: [
        { value: '1', label: 'Stat 1' },
        { value: '2', label: 'Stat 2' },
        { value: '3', label: 'Stat 3' }
    ],
    
    // The main body text, split by sections
    sections: [
        {
            heading: 'Section 1 Heading',
            body: 'Paragraph text goes here...'
        }
    ],
    
    // Optional Call to Action at the bottom
    cta: { text: 'Want to know more?', label: 'Click Here', href: 'Contact_Us.html' },
    
    // Optional images
    gallery: [] 
}
```

### Part 2: Update the Grid Card (`News.html`)

1. Open `News.html`.
2. Locate the `<div class="news-grid" id="newsGrid">`.
3. Copy an existing `<article class="news-card">` block and paste it at the top of the grid.
4. Update the card to match your JavaScript data:

| # | What to Change | Example |
|---|---|---|
| ① | Data ID | `data-article-id="my-new-article-id"` (MUST match the JS id) |
| ② | Tags | `data-tags="facility,announcement"` (Used for the filter buttons) |
| ③ | Banner Image | `<img src="Images/my_image.jpg"...>` |
| ④ | Badge | `<span class="news-card__badge...">New Update</span>` |
| ⑤ | Date | `<span class="news-card__timestamp" data-date="2026-06-01">Jun 1, 2026</span>` |
| ⑥ | Title | `<h3 class="news-card__title">My New Article Title</h3>` |
| ⑦ | Excerpt | `<p class="news-card__excerpt">Short summary text...</p>` |

## Design Considerations
*   **Excerpt Length:** Keep the `news-card__excerpt` in the HTML roughly the same length as other cards (about 2-3 lines). If it is too long, the card grid will become misaligned.
*   **Image Sizing:** Use a 16:9 aspect ratio image for the hero image to ensure it looks good in the grid banner.

## Verify It Worked
*   Load `News.html` in your browser.
*   Check that your new card appears at the top of the grid.
*   Click the card. Verify that the screen transitions to the full article view, and that your text, stats, and images load correctly.
*   Click the "Back" arrow or reset the filters to ensure you can return to the grid.

---
*After making this change and deploying, update this how-to page if anything about the process changed.*
*Last reviewed: 2026-05-15*
