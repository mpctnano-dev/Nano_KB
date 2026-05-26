# Clone a Program Page

**Summary:** Duplicate the existing PTAP program template to bootstrap new workforce training or scholarship program pages (such as the Intel CHIPS scholarship page).

**Trigger:** When the lab introduces a new corporate partnership training program, workforce certification, or specialized scholarship track that requires a dedicated, premium subpage.

**Difficulty:** Careful

**Estimated Time:** 35 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `PTAP.html` | The master template source file that serves as the basis for duplication. |
| `includes/header.html` | To register your newly created page inside the site-wide navigation menu. |
| `JS/script.js` | To register any special custom event listeners or scroll observers on the new page. |

---

## Duplication & Customization Steps

Duplicating an existing, high-end program layout ensures that you retain the precise spacing, advanced HSL color tokens, and built-in scroll observers that make the website feel premium. Follow these steps sequentially:

### Step 1: Duplicate and Rename the Template File

1. Locate the file `PTAP.html` in the root folder of the project.
2. Duplicate the file and rename the new copy. Use all lowercase letters and hyphens (kebab-case) for the name:
   * **Example:** `IntelCHIPS.html` or `semiconductor-fundamentals.html`.
   * **Tip:** Do not use spaces or capital letters in the filename, as this can cause link breaks on some web servers.

---

### Step 2: Update SEO Metadata and Page Details

To ensure the new page ranks well and displays the correct info in browser tabs, we must update the header tags.

1. Open your newly created program page (e.g. `IntelCHIPS.html`) in your code editor.
2. Look at the top of the file inside the `<head>` tag.
3. Update the document title tag (`<title>`) to reflect the new program name:
   ```html
   <title>Intel CHIPS Program | MPaCT Lab | NAU Nano</title>
   ```
4. Modify the meta description tag to accurately summarize what the program offers:
   ```html
   <meta name="description" content="Learn about the Intel CHIPS Scholarship Program at Northern Arizona University, providing advanced semiconductor training and career pathways.">
   ```
5. Update the canonical link tag to point directly to the new URL:
   ```html
   <link rel="canonical" href="https://nano.nau.edu/IntelCHIPS.html">
   ```

---

### Step 3: Tweak Styles and Theme Colors

Each program page can have a unique visual theme. The template controls colors through CSS custom variables declared in the `<style>` block inside the header.

1. Scroll to the `<style>` tag located inside the `<head>` section.
2. Look for the color token declarations defined under the `:root` selector or individual grid card overrides:
   ```css
   .pt-grid__card--gold {
       --card-accent: var(--nau-gold);
       background: hsla(45, 100%, 96%, 1); /* Customize HSL values here */
   }
   ```
3. You can modify these values to give the new page a unique signature look (for instance, matching Intel's branding by shifting highlight accents to a custom deep blue).

---

### Step 4: Swap Out Section Content

Now, you can update the visual content blocks sequentially by following the steps detailed in [04-update-ptap-and-intel-chips-pages.md](../Workforce/04-update-ptap-and-intel-chips-pages.md):

1. **Hero Details Card:** Update duration, qualification titles, and CTA link destinations inside the division tag with the class name `"ml-hero__panel"`: `<div class="ml-hero__panel">`.
2. **Partnership Strip:** Replace old partner brand logos with your new corporate sponsor logos in the division track: `<div class="partners-track">`.
3. **Bento Grid Highlights:** Rewrite stats, watermark numbers, and icon markers inside the Bento grid containers: `<div class="pt-grid">`.
4. **Coursework List:** Customize the curricula list item details (`<li class="curr-item">`) and replace the sidebar note card image (`Images/your-new-image.jpg`) to align with the new program.

---

### Step 5: Register the Page in Global Navigation

To make the new page accessible to users browsing the site, we need to add it to the website's main menu.

1. Open `includes/header.html`.
2. Find the navigation area corresponding to your category (such as "Workforce" or "Academics").
3. Add a new list item (`<li>`) with a hyperlink tag (`<a>`) linking to your new page:
   ```html
   <li><a href="IntelCHIPS.html">Intel CHIPS Program</a></li>
   ```
4. Save the file. Because the website dynamically includes the header on all pages via `JS/layout.js`, your new navigation link will instantly appear site-wide!

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Cloning a template and inserting mismatched text, unbalanced course lists, or heavy assets breaks standard layouts.
*   **Visual Impact:** Stretched bento blocks, overlapping titles, and laggy scroll reveals.
*   **Recommended Solutions:**
    *   **Bento Tile Bounds:** Keep coursework lists in tiles perfectly balanced (e.g. matching sizes of adjacent course rows).
    *   **Scope Protection:** Assign a unique kebab-case class to your body tag, and namespace all specific layout adjustments in `CSS/style.css` to prevent layout leaks.

---

## Let's Verify Your Changes

1. Open your new page to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Navigate the page and ensure all customized titles, HSL color highlights, and logo strips display properly.
3. Test all Call-to-Action buttons to verify they link to the correct pages.
4. Click the "Workforce" drop-down menu in the top navigation bar to confirm your new page link is listed and works perfectly from other sections of the site.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
