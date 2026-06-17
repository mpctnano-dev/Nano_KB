
# Add a News Article

**Summary:** Add a new article to the news catalog grid on `News.html` and register its full body content payload inside the dynamic article registry (`JS/news.js`).

**When to change:** When the lab has a major research milestone, new equipment installation announcement, or corporate partnership update to share.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★★</span>

**Estimated Time:** 20-30 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/News_%231.png" alt="News & Updates Page Overview" /><br/>
  <em>MPaCT News & Updates page visual layout and news card grid</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/News.html` | The public HTML document holding the instant-load compact preview cards. |
| `/JS/news.js` | The JavaScript dynamic registry holding full article payloads (text, stats lists, and photo galleries) rendered inside the overlay reader. |

> [!WARNING]
> **Keep IDs perfectly aligned!**
> The news page uses a performance-optimized hybrid design. The grid preview cards are hardcoded in the HTML for instant rendering and SEO benefits, but the full article is loaded dynamically from the `ARTICLES` array inside `news.js` upon clicking a card. You must update *both* files and ensure the HTML `data-article-id` matches the JS `id` property exactly!

---

## Step-by-Step Instructions

### Part 1: Update the JavaScript Article Registry

1. To get started, please open `JS/news.js` in your code editor.
2. Locate the main array definition near the top of the file:
   
   **Real code snippet:** `const ARTICLES = [`

   <p align="center">
     <img src="/md_file_images/News_%232.png" alt="Locating the ARTICLES array inside JS/news.js" /><br/>
     <em>Locating the main ARTICLES registry array definition inside JS/news.js</em>
   </p>

3. Insert a new article object block at the very top of the array inside the `ARTICLES` registry (defined in `JS/news.js`). This ensures it displays as the newest entry on the website.
4. Customize the article properties within the new object inside `JS/news.js` using the following structure:
   
   **Example snippet (inside `JS/news.js`):**
   ```javascript
   {
       id: 'tem-microscope-installation', // Unique lowercase ID (no spaces or special characters)
       title: 'JEOL TEM System Installed at MPaCT Lab',
       tagLabel: 'Equipment',         // Eyebrow tag category
       date: '2026-06-01',           // Publication date in YYYY-MM-DD format
       readTime: '4 min read',       // Estimated read time
       heroImage: 'Images/TEM.jpg',  // Path to the main hero graphic
       heroAlt: 'JEOL Transmission Electron Microscope in MPaCT Lab',
       statusBadge: 'New Equipment',
       
       // Three key stats displayed in the reader header
       stats: [
           { value: '200 kV', label: 'Acceleration' },
           { value: '0.14 nm', label: 'Resolution' },
           { value: 'LaB6', label: 'Emitter Type' }
       ],
       
       // Full article body segments
       sections: [
           {
               heading: 'Advanced Atomic-Scale Imaging',
               body: 'The MPaCT Lab has officially completed the installation and calibration of our new JEOL Transmission Electron Microscope (TEM) system. This state-of-the-art instrument enables sub-nanometer imaging...'
           }
       ],
       
       // Optional Call-to-Action block
       cta: { text: 'Interested in TEM training or imaging services?', label: 'Reserve Equipment', href: 'ReserveEquipment.html#tem' },
       
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
    
    **Example snippet:**
    ```javascript
    sections: [
        {
            heading: 'Advanced Atomic-Scale Imaging',
            body: 'We are absolutely thrilled to install this new high-resolution microscope. You can explore our full range of analysis tools on the <a href="Equipment.html">Equipment Catalog</a> page, or contact a qualified operator for custom imaging projects.'
        }
    ]
    ```

#### 2. Adding an Image Gallery
To show a clean grid of images at the bottom of the article:
*   **Gallery Objects:** Populate the `gallery` array with object blocks defining the image source, alt text, and caption:
    
    **Example snippet:**
    ```javascript
    gallery: [
        { 
            src: 'Images/tem_detail1.jpg', 
            alt: 'TEM sample holder detail', 
            caption: 'Close-up of the ultra-high vacuum TEM sample holder.' 
        },
        { 
            src: 'Images/tem_detail2.jpg', 
            alt: 'Gold nanoparticle lattice grid', 
            caption: 'High-resolution imaging showing crystalline lattice planes of gold nanoparticles.' 
        }
    ]
    ```
*   **Three-Up Layout:** If you add 3 or more images, you can set the optional `galleryLayout` property to `'three-up'` to display a gorgeous, balanced three-column photo grid:
    
    **Example snippet:**
    ```javascript
    galleryLayout: 'three-up',
    ```

#### 3. Inserting Rich Content Blocks (The `featured` Key)
The overlay reader is capable of rendering specialized content blocks to break up long-form text. You can add one of these types inside the optional `featured` key:

*   **Technical Specifications Table:** Perfect for listing machine parameters:
    
    **Example snippet:**
    ```javascript
    featured: {
        type: 'spec-table',
        heading: 'Technical Specifications',
        rows: [
            { label: 'Acceleration Voltage', val: '80 - 200 kV' },
            { label: 'Point Resolution', val: '0.14 nm' }
        ]
    }
    ```
*   **Bullet Features List:** Excellent for checking outreach or program details:
    
    **Example snippet:**
    ```javascript
    featured: {
        type: 'feature-list',
        heading: 'Key System Features',
        items: [
            'High-brightness LaB6 electron source',
            'Fully integrated digital camera system',
            'Cryo-imaging compatible sample holder'
        ]
    }
    ```
*   **Related Equipment Cards:** Excellent for linking directly to bookable assets in the lab:
    
    **Example snippet:**
    ```javascript
    featured: {
        type: 'equipment-cards',
        heading: 'Associated Analysis Systems',
        items: [
            { name: 'JEOL TEM', img: 'Images/TEM.jpg', desc: 'Transmission Electron Microscope' }
        ]
    }
    ```

---

### Part 3: Insert the HTML Preview Card

1. To get started, please open `News.html` in your code editor.
2. Please search for the news grid container division tag with the ID `"newsGrid"`. Inside, you will find:
   
   **Real code snippet:** `<div class="news-grid" id="newsGrid">`

   <p align="center">
     <img src="/md_file_images/News_%233.png" alt="Locating the newsGrid container in News.html" /><br/>
     <em>Locating the newsGrid container inside News.html</em>
   </p>

3. Duplicate an existing article block with the class name `"news-card"` and paste it at the very top of the grid list inside `News.html` (under the `#newsGrid` element).
4. Customize your new preview card using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | ID & Tag Hook | Unique lower-case ID to fetch full article data and dynamic tag filters. | `<article class="news-card" data-article-id="tem-microscope-installation" data-tags="equipment,announcement">` *(Ensure the ID matches the JS file exactly!)* |
| ② | Image Source | Relative path to the pre-cropped 16:9 thumbnail graphic for the preview card. | `<img src="Images/TEM.jpg" class="news-card__img" alt="JEOL TEM System">` |
| ③ | Timestamp | Publication date metadata and user-visible timestamp formatting. | `<span class="news-card__timestamp" data-date="2026-06-01">Jun 1, 2026</span>` *(Keep synchronized with JS date!)* |
| ④ | Card Title | The primary bold title shown on the news grid preview card. | `<h3 class="news-card__title">JEOL TEM Microscope Installed</h3>` |
| ⑤ | Excerpt / Teaser | A short 150-character introductory blurb shown below the card title. | `<p class="news-card__excerpt">The MPaCT Lab has officially completed the installation and calibration of our new JEOL TEM...</p>` |

---

### Example Markup

Below is a complete example of a single news preview card to guide your edits:

**Example snippet (inside `News.html`):**
```html
<!-- Example of a single news preview card -->
<article class="news-card" data-article-id="tem-microscope-installation" data-tags="equipment,announcement">
    <div class="news-card__img-wrap">
        <img src="Images/TEM.jpg" class="news-card__img" alt="JEOL TEM Microscope">
    </div>
    <div class="news-card__body">
        <span class="news-card__timestamp" data-date="2026-06-01">Jun 1, 2026</span>
        <h3 class="news-card__title">JEOL TEM Microscope Installed</h3>
        <p class="news-card__excerpt">The MPaCT Lab has officially completed the installation and calibration of our new JEOL Transmission Electron Microscope (TEM) system, enabling sub-nanometer imaging...</p>
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
    *   **Image Asset Location:** Save all new article hero graphics, gallery photos, and details in the `/Images/` directory at the project root before referencing their relative paths (e.g., `Images/TEM.jpg`) in the HTML and JS files.
    *   **Reader Overlay Safety:** For dynamic content loading, check that your text segments utilize standard line-breaks to keep reader windows beautifully aligned on mobile screens.

---

## Let's Verify Your Changes

1. Open `News.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Confirm that your new article card displays at the very top of the news grid.
3. Click the card. Verify that the page slides transition smoothly and that the dynamic overlay reader populates your full article text, stats list, and call-to-action buttons correctly.
4. Click the back arrow button inside the reader to ensure you can return to the main news grid.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: May 26, 2026*
