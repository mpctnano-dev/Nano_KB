# Add or Update Career Pathways

**Summary:** Add a new career pathway or edit an existing option within the interactive tree-view menu on the Career Pathways page.

**Trigger:** When a new academic degree program aligns with the laboratory's scope, or the lab begins supporting a new professional training track (such as "Quality Control Engineering").

**Difficulty:** Moderate

**Estimated Time:** 20 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `CareerPathways.html` | The main Career Pathways document where the navigation sidebar tree and the detail content panels are hardcoded. |

---

## Architectural Concept

The Career Pathways page uses an interactive "split-pane" layout.
* **On the Left:** A tree navigation sidebar representing different educational or professional branches: `<div class="cp-tree">`.
* **On the Right:** A dynamic content panel area where detailed cards reside: `<div class="cp-panel">`.

When a visitor clicks a leaf button in the sidebar (tagged with a specific target attribute like `data-target="quality-control"`), the page's client-side JavaScript automatically hides all other detail panels and reveals the matching panel block containing the identical ID value (`id="quality-control"`).

---

## Step-by-Step Instructions

### Part 1: Add the Sidebar Navigation Leaf

1. To get started, please open `CareerPathways.html` in your code editor.
2. Please search for the sidebar container division tag with the class name `"cp-tree"`. Inside, you will find:
   `<div class="cp-tree">`
3. Identify the logical category under which the new pathway should be placed (for instance, the "Degree Pathways" or "Professional Development" lists).
4. Inside that category's leaves container division tag (`<div class="cp-tree-leaves">`), copy an existing leaf button block and paste it inside:
   ```html
   <button class="cp-tree-leaf" data-target="ee-path">
       <div class="cp-leaf-dot"></div>
       Electrical Engineering
   </button>
   ```
5. Customize your new leaf:
   * **The Label Text:** Rewrite "Electrical Engineering" to reflect your new career path name.
   * **The Data Target Hook:** Change the `data-target` attribute value to a unique, lowercase, hyphenated identifier:
     `data-target="quality-control"`

---

### Part 2: Create the Matching Detail Panel

Now, we must create the detail panel that displays when your new sidebar leaf is clicked.

1. Scroll down the document past the sidebar tree to locate the content body container division tag:
   `<div class="cp-content-body">`
2. You will see several large nested division blocks that represent individual details panels. They are set as hidden by default.
3. Copy an entire existing panel block (starting from `<div class="cp-panel">` down to its closing `</div>`) and paste it at the bottom of the container, inside the `"cp-content-body"` wrapper.
4. Update the container tag of your new panel so its ID attribute *exactly matches* the `data-target` value you defined on your leaf button in Part 1:
   `<div class="cp-panel" id="quality-control">`
5. Customize the content elements inside this panel block:
   * **The Header Title & Summary:** Modify the main heading tag (`<h2>`) and description paragraph tag (`<p>`).
   * **The Statistics Cards:** Update the statistics value highlights inside the stats card division tags (`<div class="cp-stat-card">`).
   * **The Career Outcomes Table:** Update the career rows inside the outcomes table tag (`<table class="cp-career-table">`) to list matching job titles, median starting salaries, and typical hiring partners.

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Adding more than 8 pathways makes the sidebar navigation extremely long, overflowing off the bottom of the viewport on smaller screens.
*   **Visual Impact:** Menu items cut off visually, preventing users from accessing lower tracks.
*   **Recommended Solutions:**
    *   **Scrollable Sidebar Panel:** Set the sidebar menu styling rules in CSS to support vertical scrolling with a hidden scrollbar (`max-height: 100vh; overflow-y: auto; scrollbar-width: none;`).
    *   **Category Grouping:** If paths grow further, group career tracks into subcategories or use collapsible parent tabs to keep the sidebar compact.

---

## Let's Verify Your Changes

1. Open `CareerPathways.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Click your new link in the left sidebar menu.
3. Verify that the matching detail panel is instantly revealed on the right side of the screen without reloading the page.
4. Click neighboring links to confirm that the active states toggle cleanly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
