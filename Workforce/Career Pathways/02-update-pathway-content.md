# Update Pathway Content

**Summary:** Edit the specific statistics, details, career outcomes tables, and interactive accordions inside an existing Career Pathway details panel.

**Trigger:** When median industry wages are adjusted, new job titles are added to a career track, or professional development curriculum resources are revised.

**Difficulty:** Routine

**Estimated Time:** 15 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `CareerPathways.html` | The main Career Pathways document containing the hardcoded details panels. |

---

## Step-by-Step Instructions

### Part 1: Please Locate your Target Panel in CareerPathways.html

1. To get started, please open `CareerPathways.html` in your code editor.
2. Scroll below the sidebar menu to the main content container division tag:
   `<div class="cp-content-body">`
3. Please locate the specific panel division tag with the ID attribute matching the pathway you want to update (for example, the electrical engineering track: `<div class="cp-panel" id="ee-path">`).

---

### Part 2: Edit Specific Visual Components

Within your target panel block, customize individual sections using the guide table below:

| Component Type | HTML Signature to Look For | What to Change / Rules |
|---|---|---|
| **Top Statistic Card** | `<div class="cp-stat-card ...">` | Update the number inside `<div class="cp-stat-num">` (e.g. `$95K`) and the label inside `<div class="cp-stat-label">`. Keep to exactly 3 cards for layout stability. |
| **Overview Paragraph** | `<p class="cp-overview">` | Plain text description of the pathway. |
| **Focus Area Pill** | `<span class="cp-focus-pill">` | Update the text and the FontAwesome icon (`<i class="fas ...">`). |
| **Career Table Row** | `<tr>` inside `<table class="cp-career-table">` | To add a new job role, copy an existing `<tr>` to `</tr>` block. Update the Title (`.cp-role-title`), Salary (`.cp-salary`), and the skill tags (`.cp-skill-tag`). |
| **Accordion Block** | `<div class="cp-accordion">` | Used for Professional Development sections. Update the title inside `.cp-accordion__title`, the subtitle inside `.cp-accordion__sub`, and the inner HTML content inside `.cp-accordion__content`. |

---

### Example Markup

Below is a complete example of a Career Table Row to guide your edits:

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

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Overcrowding stat cards squeezes metric columns. Adding dozens of skill tags wraps pills onto multiple uneven lines.
*   **Visual Impact:** Misaligned text boxes and uneven skill tags break table rows.
*   **Recommended Solutions:**
    *   **Stat Ceiling:** Keep exactly 3 statistic cards (`.cp-stat-card`) in the pathway header panel for layout stability.
    *   **Skill Tag Limits:** Restrict the career outcomes table skill list to 3 or 4 high-value tags per job title to prevent column stretching.
    *   **Accordion Heights:** Set standard paddings on accordion body panels to avoid jagged page jumping during expand/collapse transitions.

---

## Let's Verify Your Changes

1. Open `CareerPathways.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Click the corresponding sidebar menu link to open your modified career panel.
3. Verify that your numbers, table rows, and skill pills display perfectly.
4. Click the accordion headers to ensure they expand and collapse smoothly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
