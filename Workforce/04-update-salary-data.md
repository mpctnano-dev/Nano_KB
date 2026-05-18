# Update Salary Data

**One-line description:** Update the median starting salaries in the BLS outcomes table at the bottom of the page.
**When you'd do this:** Annual Bureau of Labor Statistics (BLS) data is released, requiring an update to the expected starting salaries.
**Difficulty:** Trivial
**Estimated time:** 5 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The salary table rows are hardcoded in the HTML. |
| `CareerPathways.html` | Contains identical salary estimates that must be kept in sync. |

> [!WARNING]
> **Data Synchronization Required**
> The salary estimates listed here must match the data on the **Career Pathways** page. If you update a salary number on this page, you MUST also open `CareerPathways.html` and update the corresponding role card to maintain consistency across the site.

## Steps

1. Open `WorkForceDevelopment.html`.
2. Locate the salary table by searching for `<div class="wfd-salary-table">`.
3. You will see several rows that look like this:
   ```html
   <div class="wfd-salary-row">
       <div class="wfd-salary-row__field">Electrical Engineering</div>
       <div class="wfd-salary-row__amount">$79,000<span>/yr</span></div>
   </div>
   ```
4. Update the dollar amount for the relevant engineering fields.
5. If you update these figures, be sure to update the citation text at the bottom of the table (`<div class="wfd-salary-source">`) to reflect the new data year.

## Verify It Worked
*   Load `WorkForceDevelopment.html` locally.
*   Ensure the amounts align correctly on the right side of the table and the formatting (commas, dollar signs) is consistent.
