# Update Pathway Content

**One-line description:** Edit the specific statistics, tables, and accordions inside a Career Pathway panel.
**When you'd do this:** Median salaries change, new job roles are added to a track, or professional development resources are updated.
**Difficulty:** Routine
**Estimated time:** 15 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `CareerPathways.html` | The pathway content panels are hardcoded in the HTML. |

## Updating a Panel's Content

1. Open `CareerPathways.html`.
2. Scroll down past the sidebar to the `<div class="cp-content-body">` section.
3. Find the `<div class="cp-panel" id="...">` block that corresponds to the pathway you want to update.
4. Inside that block, locate the specific component you want to change using the reference table below:

### Component Breakdown

| Component Type | HTML Signature to Look For | What to Change / Rules |
| :--- | :--- | :--- |
| **Top Statistic Card** | `<div class="cp-stat-card ...">` | Update the number inside `<div class="cp-stat-num">` (e.g. `$95K`) and the label inside `<div class="cp-stat-label">`. Keep to exactly 3 cards for layout stability. |
| **Overview Paragraph** | `<p class="cp-overview">` | Plain text description of the pathway. |
| **Focus Area Pill** | `<span class="cp-focus-pill">` | Update the text and the FontAwesome icon (`<i class="fas ...">`). |
| **Career Table Row** | `<tr>` inside `<table class="cp-career-table">` | To add a new job role, copy an existing `<tr>` to `</tr>` block. Update the Title (`.cp-role-title`), Salary (`.cp-salary`), and the skill tags (`.cp-skill-tag`). |
| **Accordion Block** | `<div class="cp-accordion">` | Used for Professional Development sections. Update the title inside `.cp-accordion__title`, the subtitle inside `.cp-accordion__sub`, and the inner HTML content inside `.cp-accordion__content`. |

```html
<!-- Example of updating a Career Table Row -->
<tr>
    <td>
        <div class="cp-role-title">Process Engineer</div>
    </td>
    <td><span class="cp-salary">$85K - $120K</span></td>
    <td>
        <div class="cp-skills-cell">
            <span class="cp-skill-tag">Lithography</span>
            <span class="cp-skill-tag">Etch</span>
            <span class="cp-skill-tag">SPC</span>
        </div>
    </td>
</tr>
```

## Verifying It Worked
*   Load `CareerPathways.html` locally.
*   Click the relevant link in the left sidebar to open your updated panel.
*   Verify your text updates look correct, table columns align properly, and any accordion blocks open and close smoothly when clicked.
