
# Update Cross Campus Impact Section

**Summary:** Add, remove, or modify the colleges and departments listed in the "Cross Campus Impact" section of the About Us page.

**When to change:** When the lab establishes a formal collaboration or research workflow with a new NAU department, or expands active multi-disciplinary programs.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★</span>

**Estimated Time:** 5 minutes

---

## Visual Reference

<p align="center">
  <img src="../../md_file_images/Cross_Campus_Impact_%231.png" alt="Cross Campus Impact Section" /><br/>
  <em>Visual overview of the Cross Campus Impact section on the About Us page</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/About_Us.html` | The main About Us document containing the hardcoded campus affiliations grid. |

---

## Step-by-Step Instructions

### Part 1: Locate the Grid in About_Us.html

1. To get started, please open `About_Us.html` in your code editor.
2. Please search for the container division tag with the class name `"affiliations-grid"`. Inside, you will find:
   **Real code snippet:** `<div class="affiliations-grid">`

   <p align="center">
     <img src="../../md_file_images/Cross_Campus_Impact_%232.png" alt="Locating the affiliations-grid Container" /><br/>
     <em>Locating the affiliations-grid container division in the HTML source code</em>
   </p>
3. Inside this container division tag, you will see a collection of nested division blocks representing individual affiliation cards.

   <p align="center">
     <img src="../../md_file_images/Cross_Campus_Impact_%233.png" alt="Nested Affiliation Cards" /><br/>
     <em>Identifying the individual nested affiliation-card blocks</em>
   </p>

---

### Part 2: Add or Edit an Affiliation Card

1. To add a new college card or modify an existing one, duplicate or locate the target card division block.
2. Each college card is represented by a division tag with the class name `"affiliation-card"`, along with transition reveal and delay classes:
   **Real code snippet:** `<div class="affiliation-card about-reveal about-delay-2">`

   <p align="center">
     <img src="../../md_file_images/Cross_Campus_Impact_%234.png" alt="Individual Card Markup" /><br/>
     <em>Structure of an affiliation-card container with animation delay classes</em>
   </p>
3. You can customize the elements inside this block using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Icon Color | Controls the background color theme of the circular icon container. | `<div class="affiliation-icon gold">` (Options: default/blue, `gold`, `green`, `purple`) |
| ② | FontAwesome Icon | The visual glyph inside the icon circle, representing the specific department or area. | `<i class="fas fa-atom"></i>` |
| ③ | College/Department Name | The main heading element for the affiliated college or department card. | `<h3>College of the Environment...</h3>` |
| ④ | Bullet Points | The list items displaying specific research or collaboration areas. | `<li>Nanomaterials characterization</li>` |

> [!NOTE]
> **Understanding the Font Awesome `<i>` Class & Sourcing Icons:**
> *   **What the class means:** The tag `<i class="fas fa-atom"></i>` utilizes Font Awesome classes. Here, `fas` stands for **Font Awesome Solid** (the icon style), and `fa-atom` specifies the **particular glyph** (an atom).
> *   **How we got that icon:** 
>     1. Visit the official [Font Awesome Icon Library](https://fontawesome.com/icons).
>     2. Use the search bar to find keywords related to your affiliated college/department (e.g., `atom` for science, `tree` / `seedling` for forestry/environment, `laptop-code` / `microchip` for engineering, or `heartbeat` / `dna` for health/biology/genetics).
>     3. Ensure you filter by **Free** icons.
>     4. Click on your desired icon, copy the HTML `<i>` tag code snippet, and paste it into your `About_Us.html` affiliation card markup.

---

### Example Markup

Below is a complete example of a single affiliation card to guide your edits:

**Example snippet:**
```html
<!-- Example of a single affiliation card (Stage 05: Biological Sciences) -->
<div class="affiliation-card about-reveal about-delay-5">
    <div class="affiliation-icon purple">
        <i class="fas fa-dna"></i>
    </div>
    <h3>Department of Biological Sciences</h3>
    <ul class="affiliation-areas">
        <li>Bio-nanotechnology & biosensors</li>
        <li>Biocompatible materials testing</li>
        <li>Cellular imaging & microfluidics</li>
    </ul>
</div>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** An odd number of college affiliation cards wraps unevenly, leaving a single card centered or left-aligned on the second row. Mismatched brand logo aspect ratios create jagged top/bottom card borders.
*   **Visual Impact:** Jagged grid shapes look clumsy and unprofessional, drawing negative attention to institutional logos.
*   **Recommended Solutions:**
    *   **Multiples-of-Four Alignment:** Keep college card lists in groups of 4.
    *   **Standardized Logo Assets:** Pre-crop all college logo images to matching dimensions with built-in white margins so they possess identical visual weight inside the white card bounds.
    *   **Horizontal Swipe Strip:** If the college network grows beyond 8, refactor the `.affiliation-grid` container to use a touch-enabled horizontal swipe strip on mobile, rather than wrapping cards vertically.
    *   **Responsive Slider/Carousel with Arrow Navigation:** Instead of wrapping cards vertically into uneven rows when adding an odd number of affiliations (e.g. 5), refactor the grid into a single-row slider container accompanied by navigation arrow buttons (`<button class="carousel-arrow prev-btn"><i class="fas fa-chevron-left"></i></button>` and `<button class="carousel-arrow next-btn"><i class="fas fa-chevron-right"></i></button>`). This allows horizontal scrolling or swiping, keeping the layout perfectly symmetrical and premium-looking across all screen sizes.
    *   **Visual Balance Flexbox Fallback:** In your CSS, configure `.affiliations-grid` to fallback to a centering flex layout when cards wrap. By applying `display: flex; flex-wrap: wrap; justify-content: center; gap: var(--grid-gap);`, any trailing or odd-numbered cards (like a 5th card) will automatically center on the next row rather than looking misaligned on the left, keeping the overall section visually balanced.
    *   **Featured Hero Card Span:** Designate the primary college card to span two columns (e.g. `grid-column: span 2` or `grid-column: 1 / -1` on desktop) so that a grid of 5 cards completes a perfect 2x3 aesthetic block, eliminating any awkward trailing visual space.

---

## Let's Verify Your Changes

1. Open `About_Us.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Scroll down to the bottom to view the "Cross Campus Impact" section.
3. Verify that your new affiliation card loads correctly and that the bulleted research areas display neatly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last Updated: May 22, 2026*

