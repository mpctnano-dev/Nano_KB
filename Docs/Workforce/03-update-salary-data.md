
# Update Career Outcomes & Salary Table

**Summary:** Update the median starting salaries listed inside the Bureau of Labor Statistics (BLS) outcomes table at the bottom of the Workforce Development page and synchronize them with their respective career spotlight cards.

**When to change:** When annual data releases from the Bureau of Labor Statistics (BLS) are published, requiring an update to local technology career wage expectations.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★</span>

**Estimated Time:** 5 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/Wf_Salary_%231.png" alt="Career Outcomes & Salary Table" /><br/>
  <em>Figure 1: Career Outcomes & Salary Table visual layout in WorkForceDevelopment.html</em>
</p>


---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/WorkForceDevelopment.html` | Main Workforce Development page containing the hardcoded median starting salary table. |
| `/CareerPathways.html` | Interactive Program Navigator page containing matching starting salary spotlight stats. |

---

## Step-by-Step Instructions

### Part 1: Locate and Update the Salary Table in WorkForceDevelopment.html

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Scroll to the bottom of the page and locate the main container for the salary table using the class name `"wfd-salary-table"`:

   **Real Code Snippet**
   ```html
   <div class="wfd-salary-table wfd-reveal">
   ```

```html
<div class="wfd-salary-table wfd-reveal">
    <div class="wfd-salary-table__head">
        ...
    </div>
    <div class="wfd-salary-row">
        ...
    </div>
</div>
```

3. Locate the row division block representing the specific career path you wish to edit. Each row is represented by a `wfd-salary-row` class:

   **Real Code Snippet**
   ```html
   <div class="wfd-salary-row">
   ```

4. Edit the career details inside the row using the configuration schema table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Career Field Title | The visible text label for the engineering or technology discipline. Modified when academic programs or target professions are renamed. | `<div class="wfd-salary-row__field">Electrical Engineering</div>` |
| ② | Salary Figure | The median starting annual wage from the latest BLS reporting. Must include a dollar sign and appropriate comma styling. | `<div class="wfd-salary-row__amount">$79,000<span>/yr</span></div>` |
| ③ | Salary Suffix | Suffix indicating the wage frequency (typically annually `/yr`), enclosed in a `<span>` tag. | `<span>/yr</span>` |

5. **Update the Citation:** Scroll to the bottom of the table to find the source tag (`<div class="wfd-salary-source">`) and update the BLS or O*NET reporting years to ensure reference accuracy.

---

### Part 2: Synchronize Spotlight Cards in CareerPathways.html

> [!IMPORTANT]
> **Maintain Sibling Alignment!**
> To prevent confusing site visitors with conflicting financial statistics, any starting salary updated in `WorkForceDevelopment.html` must be kept in perfect synchronization with the matching career track card on the interactive `CareerPathways.html` page.

1. Open `CareerPathways.html` in your code editor.
2. Locate the corresponding spotlight card for the program you updated. Search for the unique panel identifier matching the degree discipline:

   **Real Code Snippet**
   ```html
   <div class="cp-panel" id="panel-ce">
   ```

```html
<div class="cp-panel" id="panel-ce">
    <div class="cp-stats-row">
        ...
        <div class="cp-stat-card grad-gold">
            <div class="cp-stat-num">$93K</div>
            <div class="cp-stat-label">Median Starting Salary</div>
        </div>
        ...
    </div>
</div>
```

3. Locate the gold spotlight metrics card (`cp-stat-card grad-gold`) inside that panel and update the numerical wage:

   **Real Code Snippet**
   ```html
   <div class="cp-stat-card grad-gold">
   ```

4. Edit the spotlight values using the configuration schema table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Spotlight Panel ID | The unique identifier corresponding to each degree/career panel in the sidebar navigator. Used to select the correct panel. | `id="panel-ce"` (Options: `panel-ce`, `panel-cs`, `panel-ee`, `panel-et`, `panel-me`, `panel-mre`) |
| ② | Spotlight Salary Figure | The primary highlighted starting salary value inside the gold badge metrics row. Must match the value on the main Workforce page, abbreviated with 'K'. | `<div class="cp-stat-num">$93K</div>` (Matches `$93,000` starting salary) |

---

**Example Snippet**

Here is an example of a single row in `WorkForceDevelopment.html`:
```html
<div class="wfd-salary-row">
    <div class="wfd-salary-row__field">Electrical Engineering</div>
    <div class="wfd-salary-row__amount">$79,000<span>/yr</span></div>
</div>
```

**Example Snippet**

Here is an example of the matching metrics row in `CareerPathways.html`:
```html
<div class="cp-stat-card grad-gold">
    <div class="cp-stat-num">$79K</div>
    <div class="cp-stat-label">Median Starting Salary</div>
</div>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Modifying salary figures in one file while forgetting the other leads to conflicting site data. Additionally, expanding the row count in the Workforce page's salary table can break responsive layout balances on mobile viewports.
*   **Visual Impact:** Text crowding, clipping table boundaries, or mismatched colors when custom rows are added.
*   **Recommended Solutions:**
    *   **Strict Sibling Sync:** Always double-check both `WorkForceDevelopment.html` and `CareerPathways.html` after making salary updates.
    *   **Text Wrap Protection:** Keep career titles concise to prevent vertical line-wrap collisions inside the narrow `.wfd-salary-row__field` container.
    *   **Ensure Proper Suffix Markup:** Always wrap the `/yr` text in a `<span>` tag to prevent style collisions or large-sized suffix text.

---

## Let's Verify Your Changes

1. Launch a local development server or open both `WorkForceDevelopment.html` and `CareerPathways.html` directly in your web browser.
2. Scroll to the bottom of the Workforce Development page and confirm that the modified starting salaries are formatted correctly with dollar signs and commas.
3. Open the interactive Career Pathways Program Navigator, select the corresponding discipline in the sidebar, and confirm that the gold starting salary metric card matches the value on the main page (e.g. `$79K` matches `$79,000`).
4. Resize the browser window to a mobile viewport width (e.g., 375px) to ensure the table does not clip and text fits perfectly without overlapping.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 27, 2026
