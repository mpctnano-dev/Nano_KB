# Update Affiliated Colleges

**Summary:** Add, remove, or modify the colleges and departments listed in the "Cross Campus Impact" section of the About Us page.

**Trigger:** When the lab establishes a formal collaboration or research workflow with a new NAU department, or expands active multi-disciplinary programs.

**Difficulty:** Trivial

**Estimated Time:** 5 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `About_Us.html` | The main About Us document containing the hardcoded campus affiliations grid. |

---

## Step-by-Step Instructions

### Part 1: Please Locate the Grid in About_Us.html

1. To get started, please open `About_Us.html` in your code editor.
2. Please search for the container division tag with the class name `"affiliations-grid"`. Inside, you will find:
   `<div class="affiliations-grid">`
3. Inside this container division tag, you will see a collection of nested division blocks representing individual affiliation cards.

---

### Part 2: Let's Add or Edit an Affiliation Card

1. To add a new college card or modify an existing one, duplicate or locate the target card division block.
2. Each college card is represented by a division tag with the class name `"affiliation-card"`, along with transition reveal and delay classes:
   `<div class="affiliation-card about-reveal about-delay-2">`
3. You can customize the elements inside this block using the guide table below:

| # | What to Change | Options / Example |
|---|---|---|
| ① | Icon Color | `<div class="affiliation-icon gold">` (Options: default (blue), `gold`, `green`, `purple`) |
| ② | FontAwesome Icon | `<i class="fas fa-atom"></i>` |
| ③ | College/Department Name | `<h3>College of the Environment...</h3>` |
| ④ | Bullet Points | `<li>Nanomaterials characterization</li>` |

---

### Example Markup

Below is a complete example of a single affiliation card to guide your edits:

```html
<!-- Example of a single affiliation card -->
<div class="affiliation-card about-reveal about-delay-2">
    <div class="affiliation-icon gold">
        <i class="fas fa-atom"></i>
    </div>
    <h3>College of the Environment, Forestry & Natural Sciences</h3>
    <ul class="affiliation-areas">
        <li>Nanomaterials characterization</li>
        <li>Surface & interface analysis</li>
        <li>Environmental materials research</li>
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

---

## Let's Verify Your Changes

1. Open `About_Us.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Scroll down to the bottom to view the "Cross Campus Impact" section.
3. Verify that your new affiliation card loads correctly and that the bulleted research areas display neatly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
