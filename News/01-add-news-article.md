# Add a News Article

**Summary:** Add a new article to the news catalog grid on `News.html` and register its full body content payload inside the dynamic article registry (`JS/news.js`).

**Trigger:** When the lab has a major research milestone, new equipment installation announcement, or corporate partnership update to share.

**Difficulty:** Coordinate-first

**Estimated Time:** 20-30 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `News.html` | The public HTML document holding the instant-load compact preview cards. |
| `JS/news.js` | The JavaScript dynamic registry holding full article payloads (text, stats lists, and photo galleries) rendered inside the overlay reader. |

> [!WARNING]
> **Keep IDs perfectly aligned!**
> The news page uses a performance-optimized hybrid design. The grid preview cards are hardcoded in the HTML for instant rendering and SEO benefits, but the full article is loaded dynamically from the `ARTICLES` database array inside `news.js` upon clicking a card. You must update *both* files and ensure the HTML `data-article-id` matches the JS `id` property exactly!

---

## Step-by-Step Instructions

### Part 1: Update the JavaScript Article Registry

1. To get started, please open `JS/news.js` in your code editor.
2. Locate the main array definition near the top of the file:
   `const ARTICLES = [`
3. Insert a new article object block at the very top of the array (this ensures it displays as the newest entry on the website).
4. Customize the article properties using the following structure:
   ```javascript
   {
       id: 'lpkf-protolaser-arrival', // Unique lowercase ID (no spaces or special characters)
       title: 'LPKF ProtoLaser R4 Arrives at MPaCT Lab',
       tagLabel: 'Facility',         // Eyebrow tag category
       date: '2026-06-01',           // Publication date in YYYY-MM-DD format
       readTime: '3 min read',       // Estimated read time
       heroImage: 'Images/lpkf_hero.jpg', // Path to the main hero graphic
       heroAlt: 'LPKF ProtoLaser rapid prototyping system',
       statusBadge: 'New Installation',
       
       // Three key stats displayed in the reader header
       stats: [
           { value: '10μm', label: 'Feature Size' },
           { value: 'Sub-ps', label: 'Pulse Width' },
           { value: 'Cold', label: 'Ablation Mode' }
       ],
       
       // Full article body segments
       sections: [
           {
               heading: 'Precision Prototyping Capabilities',
               body: 'The MPaCT Lab has officially taken delivery of the LPKF ProtoLaser system...'
           }
       ],
       
       // Optional Call-to-Action block
       cta: { text: 'Interested in prototyping services?', label: 'Submit Service Request', href: 'ServiceRequest.html#laser' },
       
       gallery: [] // Leave empty if there is no gallery, or populate with image path strings
   }
   ```

---

### Part 2: Formatting and Structuring Article Content

To write a highly engaging article that renders perfectly inside the dynamic overlay reader, you can customize the body text, add an image gallery, or embed high-fidelity feature modules. Here is how to format each component inside the `ARTICLES` registry object:

#### 1. Formatting Paragraphs and HTML Links
The reader dynamically loops through your `sections` array, wrapping each `body` text value inside a standard paragraph tag (`<p>`). 
*   **Multiple Paragraphs:** Enter each paragraph as a separate object with its own `heading` and `body` fields inside the `sections` array.
*   **Embedding HTML Formatting:** You can write standard HTML elements (such as bold markers or hyperlinks) directly inside the `body` string. Remember to use single quotes for HTML attributes to prevent breaking the JavaScript string parameters:
    ```javascript
    sections: [
        {
            heading: 'Precision Prototyping Capabilities',
            body: 'We are absolutely thrilled to install this system. You can explore our full capabilities on the <a href="Equipment.html">Equipment Catalog</a> page, or contact an operator for custom work.'
        }
    ]
    ```

#### 2. Adding an Image Gallery
To show a clean grid of images at the bottom of the article:
*   **Gallery Objects:** Populate the `gallery` array with object blocks defining the image source, alt text, and caption:
    ```javascript
    gallery: [
        { 
            src: 'Images/lpkf_detail1.jpg', 
            alt: 'Laser lens detailing', 
            caption: 'Close-up of the LPKF laser positioning lens.' 
        },
        { 
            src: 'Images/lpkf_detail2.jpg', 
            alt: 'Finished copper board', 
            caption: 'High-precision structured PCB ready for assembly.' 
        }
    ]
    ```
*   **Three-Up Layout:** If you add 3 or more images, you can set the optional `galleryLayout` property to `'three-up'` to display a gorgeous, balanced three-column photo grid:
    ```javascript
    galleryLayout: 'three-up',
    ```

#### 3. Inserting Rich Content Blocks (The `featured` Key)
The overlay reader is capable of rendering specialized content blocks to break up long-form text. You can add one of these types inside the optional `featured` key:

*   **Technical Specifications Table:** Perfect for listing machine parameters:
    ```javascript
    featured: {
        type: 'spec-table',
        heading: 'Technical Specifications',
        rows: [
            { label: 'Laser Power', val: '8 W (Max)' },
            { label: 'Repeatability', val: '+/- 0.23 µm' }
        ]
    }
    ```
*   **Bullet Features List:** Excellent for checking outreach or program details:
    ```javascript
    featured: {
        type: 'feature-list',
        heading: 'Key Program Elements',
        items: [
            'Full MPaCT/NAU branded exterior wrap',
            'Portable digital microscopy tools'
        ]
    }
    ```
*   **Related Equipment Cards:** Excellent for linking directly to bookable assets in the lab:
    ```javascript
    featured: {
        type: 'equipment-cards',
        heading: 'Associated Lab Systems',
        items: [
            { name: 'LPKF ProtoLaser R4', img: 'Images/LPKF.png', desc: 'Rapid PCB prototyping' }
        ]
    }
    ```

---

### Part 3: Insert the HTML Preview Card

1. To get started, please open `News.html` in your code editor.
2. Please search for the news grid container division tag with the ID `"newsGrid"`. Inside, you will find:
   `<div class="news-grid" id="newsGrid">`
3. Duplicate an existing article block with the class name `"news-card"` and paste it at the very top of the grid list.
4. Customize your new preview card using the guide table below:

| # | What to Change | Options / Example |
|---|---|---|
| ① | ID & Tag Hook | `<article class="news-card" data-article-id="lpkf-protolaser-arrival" data-tags="facility,announcement">` (Ensure the ID matches Part 1!) |
| ② | Image Source | `<img src="Images/lpkf_hero.jpg" class="news-card__img" alt="...">` |
| ③ | Timestamp | `<span class="news-card__timestamp" data-date="2026-06-01">Jun 1, 2026</span>` (Keep date synchronized with Part 1) |
| ④ | Card Title | `<h3 class="news-card__title">LPKF ProtoLaser R4 Arrives</h3>` |
| ⑤ | Excerpt / Teaser | `<p class="news-card__excerpt">The MPaCT Lab has officially taken delivery...</p>` |

---

### Example Markup

Below is a complete example of a single news preview card to guide your edits:

```html
<!-- Example of a single news preview card -->
<article class="news-card" data-article-id="lpkf-protolaser-arrival" data-tags="facility,announcement">
    <div class="news-card__img-wrap">
        <img src="Images/lpkf_hero.jpg" class="news-card__img" alt="LPKF Laser System">
    </div>
    <div class="news-card__body">
        <span class="news-card__timestamp" data-date="2026-06-01">Jun 1, 2026</span>
        <h3 class="news-card__title">LPKF ProtoLaser R4 Arrives</h3>
        <p class="news-card__excerpt">The MPaCT Lab has officially taken delivery of the new LPKF ProtoLaser system, enabling high-precision rapid prototyping...</p>
    </div>
</article>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Massive teaser text summaries or varying aspect-ratio hero images stretch preview cards unevenly, breaking news grid columns.
*   **Visual Impact:** Uneven grid rows make the page hard to read and look disorganized.
*   **Recommended Solutions:**
    *   **Excerpt Consistency:** Keep card excerpts (`.news-card__excerpt`) under 150 characters (2 to 3 lines max) to guarantee matching card shapes.
    *   **16:9 Crop Standard:** Pre-crop all article graphics to a landscape 16:9 ratio before uploading to the server.
    *   **Reader Overlay Safety:** For dynamic content loading, check that your text segments utilize standard line-breaks to keep reader windows beautifully aligned on mobile screens.

---

## Let's Verify Your Changes

1. Open `News.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Confirm that your new article card displays at the very top of the news grid.
3. Click the card. Verify that the page slides transition smoothly and that the dynamic overlay reader populates your full article text, stats list, and call-to-action buttons correctly.
4. Click the back arrow button inside the reader to ensure you can return to the main news grid.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
