# Update Salary Outcomes Data

**Summary:** Update the median starting salaries listed inside the Bureau of Labor Statistics (BLS) outcomes table at the bottom of the Workforce Development page.

**Trigger:** When annual data releases from the Bureau of Labor Statistics (BLS) are published, requiring an update to local technology career wage expectations.

**Difficulty:** Trivial

**Estimated Time:** 5 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The main Workforce Development document where the salary table rows are hardcoded. |
| `CareerPathways.html` | Sibling career details document containing identical starting salary metrics that must be kept in sync. |

> [!WARNING]
> **Keep both pages synchronized!**
> The salary estimates displayed inside the BLS table on this page represent the same figures listed on the interactive **Career Pathways** page. If you modify a starting salary here, you must also update the corresponding role highlight cards in `CareerPathways.html` to ensure site visitors see consistent financial statistics across the entire website!

---

## Step-by-Step Instructions

### Part 1: Please Locate the Salary Table in WorkForceDevelopment.html

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Scroll to the bottom of the page and search for the division tag with the class name `"wfd-salary-table"`:
   `<div class="wfd-salary-table">`
3. Inside this container division tag, you will see a series of nested division blocks representing individual rows in the salary table.

---

### Part 2: Let's Add or Edit a Salary Row

1. To update the starting salary for an engineering field, locate the target row division block.
2. Each line is represented by a division row tag with the class name `"wfd-salary-row"`:
   `<div class="wfd-salary-row">`
3. You can customize the elements inside this block using the following guidelines:
   * **The Career Field Title:** Modify the text inside the division tag with the class name `"wfd-salary-row__field"`:
     `<div class="wfd-salary-row__field">Electrical Engineering</div>`
   * **The Salary Figure:** Update the wage value inside the division tag with the class name `"wfd-salary-row__amount"`. Suffixes (like `"/yr"`) should be placed inside a span tag (`<span>`):
     `<div class="wfd-salary-row__amount">$79,000<span>/yr</span></div>`
4. **Update the Citation:** Scroll to the bottom of the table and locate the source division tag:
   `<div class="wfd-salary-source">`
   Modify the text to cite the correct BLS reporting year (e.g. "Data source: Bureau of Labor Statistics 2026").

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** High-density statistical tables clip on mobile viewports. Mismatched color coding causes badges to collide.
*   **Visual Impact:** Text squeezing and overlapping table grids make salary comparisons hard to read.
*   **Recommended Solutions:**
    *   **Responsive Table Wrappers:** Set tables inside an `overflow-x: auto` division tag.
    *   **Column Flexing:** Ensure wage columns are styled with strict min-widths so numbers never wrap.
    *   **Alternate Row Shading:** Maintain zebra-striping styling classes (`.table-striped`) to guide the eye across wide metrics grids.

---

## Let's Verify Your Changes

1. Open `WorkForceDevelopment.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Verify that the updated career paths and dollar amounts align correctly on the right side of the table, and check that all currencies and commas are formatted consistently.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
