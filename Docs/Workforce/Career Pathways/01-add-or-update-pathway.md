
# Add/Modify Career Pathways Program Navigator

**Summary:** Add a new career pathway or edit an existing option within the interactive Program Navigator tree-view menu on the Career Pathways page.

**When to change:** When a new academic degree program aligns with the laboratory's scope, or the lab begins supporting a new professional training track (such as "Quality Control Engineering").

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★</span>

**Estimated Time:** 20 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/Career_Pathways_%231.png" alt="Career Pathways Program Navigator sidebar" /><br/>
  <em>Figure 1: Program Navigator sidebar with Degree Programs and Professional Development categories</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/CareerPathways.html` | Main Career Pathways page containing the sidebar Program Navigator tree and the detailed content panels. |

---

## Architectural Concept

The Career Pathways page uses an interactive split-pane layout.

* **On the left:** A tree navigation sidebar representing different educational or professional branches (`<div class="cp-tree">`).
* **On the right:** A dynamic content panel area where detailed pathway cards reside (`<div class="cp-content-area">`).

When a visitor clicks a leaf button in the sidebar (tagged with `data-panel="[panelKey]"`), the page's client-side JavaScript hides all other detail panels and reveals the matching block whose `id` is `panel-[panelKey]` (for example, `data-panel="ce"` maps to `id="panel-ce"`).

<p align="center">
  <img src="/md_file_images/Career_Pathways_%232.png" alt="Career Pathways Program Navigator with Computer Engineering panel selected" /><br/>
  <em>Figure 2: Selected pathway panel (Computer Engineering) with stats, overview, and career table</em>
</p>

---

## Step-by-Step Instructions

### Part 1: Add the Sidebar Navigation Leaf

1. Open `/CareerPathways.html` in your code editor.
2. Locate the sidebar container `<div class="cp-tree">`.
3. Identify the category where the new pathway belongs (either **Degree Programs** or **Professional Development**).
4. Inside that category's `<div class="cp-tree-leaves">`, duplicate an existing leaf `<button>` and update its label and `data-panel` value.

**Example Snippet (New Leaf Button Blueprint)**

```html
<button class="cp-tree-leaf" data-panel="quality-control">
    <span class="cp-leaf-dot"></span> Quality Control Engineering
</button>
```

**Real Code Snippet (Degree Programs Leaf List)**

```html
<div class="cp-tree-leaves">
    <button class="cp-tree-leaf" data-panel="ce">
        <span class="cp-leaf-dot"></span> Computer Engineering
    </button>
    <button class="cp-tree-leaf" data-panel="cs">
        <span class="cp-leaf-dot"></span> Computer Science
    </button>
    <button class="cp-tree-leaf" data-panel="ee">
        <span class="cp-leaf-dot"></span> Electrical Engineering
    </button>
</div>
```

> [!WARNING]
> **Use the Correct Attribute:**
> * **Do NOT use `data-target`:** The client-side JavaScript queries `data-panel`. If you use `data-target` instead, the click handler will not read the value and the panel will not toggle.

---

### Part 2: Create the Matching Detail Panel

Create the detail panel that displays when your new sidebar leaf is clicked.

1. Scroll to the content area container `<div class="cp-content-area">`.
2. Duplicate an entire existing `<div class="cp-panel">` block and paste it inside the content area.
3. Set the new panel's `id` to `panel-[panelKey]`, where `[panelKey]` exactly matches the leaf's `data-panel` value from Part 1.

**Example Snippet (New Pathway Panel Blueprint)**

```html
<div class="cp-panel" id="panel-quality-control">
    <div class="cp-content-header">
        <i class="fas fa-microchip"></i>
        <div>
            <h2>Quality Control Engineering</h2>
            <div class="cp-content-sub">Professional Training Track</div>
        </div>
    </div>
    <div class="cp-content-body">
        <div class="cp-stats-row">
            <div class="cp-stat-card grad-blue">
                <div class="cp-stat-num">80</div>
                <div class="cp-stat-label">Hours</div>
            </div>
            <div class="cp-stat-card grad-gold">
                <div class="cp-stat-num">$65K</div>
                <div class="cp-stat-label">Median Starting Salary</div>
            </div>
            <div class="cp-stat-card grad-green">
                <div class="cp-stat-num">8 wks</div>
                <div class="cp-stat-label">Program Length</div>
            </div>
        </div>
        <p class="cp-overview">
            Provides comprehensive training on microelectronics and semiconductor manufacturing quality assurance, cleanroom protocols, statistical process control, metrology techniques, and safety standards.
        </p>
    </div>
</div>
```

**Real Code Snippet (Computer Engineering Panel Header)**

```html
<div class="cp-panel" id="panel-ce">
    <div class="cp-content-header">
        <i class="fas fa-microchip"></i>
        <div>
            <h2>Computer Engineering</h2>
            <div class="cp-content-sub">Bachelor of Science</div>
        </div>
    </div>
    <div class="cp-content-body">
        ...
    </div>
</div>
```

> [!IMPORTANT]
> **Understanding the ID Prefixing:**
> * **Panel ID mapping:** The panel `id` must use the prefix `panel-` plus your `data-panel` value (for example, `panel-quality-control`). If you omit the prefix (for example, `id="quality-control"`), the JavaScript cannot find the target element and clicking the sidebar link has no effect.

---

### Configuration Reference Table

To configure a new career pathway, modify the properties in the table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| 1 | `data-panel` Attribute | Unique handle on the leaf button that the click handler uses to select which content card to show. | `data-panel="quality-control"` (lowercase, alphanumeric, hyphenated string) |
| 2 | Leaf Button Label | User-visible text for the pathway in the sidebar tree. | `Quality Control Engineering` |
| 3 | Panel Container `id` | Identifier on the content panel. Must follow `panel-[panelKey]`, matching the leaf's `data-panel` exactly. | `id="panel-quality-control"` |
| 4 | Panel Header Title | Primary heading (`<h2>`) in the content card header when the pathway is selected. | `<h2>Quality Control Engineering</h2>` |

---

## Design Impact & Layout Solutions

* **The Design Discrepancy Risk:** Adding more than eight pathways makes the sidebar navigation very long, overflowing the viewport on smaller screens.
* **Visual Impact:** Menu items are cut off visually, preventing users from accessing lower tracks.
* **Recommended Solutions:**
    * **Scrollable sidebar panel:** Set sidebar menu CSS to support vertical scrolling with a hidden scrollbar (`max-height: 100vh; overflow-y: auto; scrollbar-width: none;`).
    * **Category grouping:** If paths grow further, group career tracks into subcategories or use collapsible parent tabs to keep the sidebar compact.

---

## Let's Verify Your Changes

1. Open `/CareerPathways.html` in your web browser or use a code editor preview extension.
2. Click your new link in the left sidebar menu.
3. Verify that the matching detail panel is revealed on the right without reloading the page.
4. Click neighboring links to confirm that active states toggle cleanly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 27, 2026
