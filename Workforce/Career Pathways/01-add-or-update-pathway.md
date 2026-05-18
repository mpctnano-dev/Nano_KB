# Add or Update Career Pathways

**One-line description:** Add a new career pathway (or edit an existing one) in the interactive tree-view menu on the Career Pathways page.
**When you'd do this:** A new degree program aligns with the lab, or the lab begins supporting a new professional track (e.g., "Quality Control Engineering").
**Difficulty:** Moderate
**Estimated time:** 20 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `CareerPathways.html` | The navigation tree and the content panels are both hardcoded here. |

## How the Page Works
The Career Pathways page uses a "Tree View" layout. On the left is the navigation sidebar (`<div class="cp-tree">`). On the right is the content area containing multiple hidden panels (`<div class="cp-panel">`). 

When a user clicks a button in the sidebar (e.g., `data-target="new-path"`), the JavaScript hides all panels and reveals the panel with the matching ID (e.g., `id="new-path"`).

## Steps: Adding a New Pathway

### Part 1: Add the Sidebar Link

1. Open `CareerPathways.html`.
2. Locate the navigation tree (`<div class="cp-tree">`).
3. Decide which category it belongs under (e.g., Degree Pathways or Professional Development).
4. Inside that category's `<div class="cp-tree-leaves">`, copy an existing button:
   ```html
   <button class="cp-tree-leaf" data-target="ee-path">
       <div class="cp-leaf-dot"></div>
       Electrical Engineering
   </button>
   ```
5. Paste it where you want it in the list.
6. Change the text ("Electrical Engineering") to your new path name.
7. Change the `data-target` to a unique, lowercase ID with no spaces (e.g., `data-target="quality-control"`).

### Part 2: Create the Content Panel

1. Still in `CareerPathways.html`, scroll down past the sidebar to the `<div class="cp-content-body">` section.
2. You will see several large blocks starting with `<div class="cp-panel" id="...">`.
3. To ensure your new page looks correct, **copy an entire existing panel block** (from `<div class="cp-panel">` down to its closing `</div>`).
4. Paste it at the bottom of the content container (but inside `<div class="cp-content-body">`).
5. Change the `id` of your new block to perfectly match the `data-target` you created in Part 1:
   ```html
   <div class="cp-panel" id="quality-control">
   ```
6. Update the content inside your new panel:
   *   Update the top title and description paragraph.
   *   Update the 3 statistics cards at the top (`<div class="cp-stat-card">`).
   *   Update the table rows (`<table class="cp-career-table">`).

## Verifying It Worked
*   Load `CareerPathways.html` locally.
*   Click your new link in the left sidebar.
*   Verify that your new content panel appears on the right without the page reloading.
*   Click another link to ensure the active state properly switches away from your new panel.
