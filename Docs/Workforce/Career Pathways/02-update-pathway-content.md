
# Update Career Pathway Content

**Summary:** Edit the specific statistics, career outcome tables, and key metric cards inside an existing Career Pathway details panel.

**When to change:** When median industry wages are adjusted, new job titles are added to a career track, or key skill requirements are updated.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 15 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/Career_Pathways_%233.png" alt="Career Pathways panel with stats row and career outcomes table" /><br/>
  <em>Figure 1: Pathway detail panel showing stat cards, overview text, and the career pathways salary table</em>
</p>


---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/CareerPathways.html` | Main Career Pathways page containing the hardcoded detail panels for each pathway. |

---

## Step-by-Step Instructions

### Part 1: Locate Your Target Panel

1. Open `/CareerPathways.html` in your code editor.
2. Scroll to the main content container represented by the class `cp-content-area`.

**Real Code Snippet (Content Area Container)**

```html
<div class="cp-content-area">
    <div class="cp-panel" id="panel-welcome">...</div>
    <div class="cp-panel" id="panel-ce">...</div>
    <div class="cp-panel" id="panel-cs">...</div>
    <div class="cp-panel" id="panel-ee">...</div>
</div>
```

3. Locate the specific panel whose `id` matches the pathway you want to update (for example, the Electrical Engineering panel uses `id="panel-ee"`).

**Real Code Snippet (Panel Opening Tag)**

```html
<div class="cp-panel" id="panel-ee">
```

---

### Part 2: Update Statistic Cards

Within your target panel, the header metrics live inside `<div class="cp-stats-row">`. Each metric is a `.cp-stat-card` with a theme class and two inner elements: `.cp-stat-num` (the value) and `.cp-stat-label` (the caption).

**Real Code Snippet (Statistic Cards Row — Computer Engineering)**

```html
<div class="cp-stats-row">
    <div class="cp-stat-card grad-blue">
        <div class="cp-stat-num">120</div>
        <div class="cp-stat-label">Credit Hours</div>
    </div>
    <div class="cp-stat-card grad-gold">
        <div class="cp-stat-num">$93K</div>
        <div class="cp-stat-label">Median Starting Salary</div>
    </div>
    <div class="cp-stat-card grad-green">
        <div class="cp-stat-num">4 yrs</div>
        <div class="cp-stat-label">Program Length</div>
    </div>
</div>
```

#### Configure Stat Card Themes

| Modifying Target | CSS Utility | Purpose |
|---|---|---|
| **Credit Hours metric** | `grad-blue` | Blue-themed card for program hour counts. |
| **Salary metric** | `grad-gold` | Gold-themed card for wage or salary figures. |
| **Program duration** | `grad-green` | Green-themed card for length-of-program values. |

---

### Part 3: Update Career Outcomes Table Rows

Below the overview paragraph, each pathway includes a career table (`.cp-career-table`) with role titles, salary ranges, and skill tags. Edit existing `<tr>` rows or duplicate a row to add a new job title.

**Real Code Snippet (Single Career Table Row — Computer Engineering)**

```html
<tr>
    <td class="cp-role-title">Embedded Systems Engineer</td>
    <td class="cp-salary">$70K – $85K</td>
    <td class="cp-salary">$100K – $130K</td>
    <td class="cp-skills-cell">
        <span class="cp-skill-tag">C/C++</span>
        <span class="cp-skill-tag">RTOS</span>
        <span class="cp-skill-tag">FPGA</span>
    </td>
</tr>
```

**Example Snippet (New Career Row Blueprint)**

```html
<tr>
    <td class="cp-role-title">Process Engineer</td>
    <td class="cp-salary">$70K – $88K</td>
    <td class="cp-salary">$100K – $130K</td>
    <td class="cp-skills-cell">
        <span class="cp-skill-tag">SPC</span>
        <span class="cp-skill-tag">Metrology</span>
        <span class="cp-skill-tag">Cleanroom Protocols</span>
    </td>
</tr>
```

#### Configuration Reference

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | **Statistic Card Theme & Number** | Sets the numeric value and visual gradient of a key metric card in the pathway panel header. | **Class options:** `.grad-blue` (Credit Hours), `.grad-gold` (Salary), `.grad-green` (Program Duration). <br>**Example:** `<div class="cp-stat-card grad-blue"><div class="cp-stat-num">120</div><div class="cp-stat-label">Credit Hours</div></div>` |
| ② | **Career Table Role Title** | Defines the job title in the first column of the career outcomes table. | `<td class="cp-role-title">Process Engineer</td>` |
| ③ | **Salary Benchmarks** | Displays salary range splits (Entry-Level in column 2, Mid-Level in column 3). | **Entry-level:** `<td class="cp-salary">$70K – $88K</td>`<br>**Mid-level:** `<td class="cp-salary">$100K – $130K</td>` |
| ④ | **Core Skill Tags** | Keyword pills inside the fourth column (`.cp-skills-cell`) showing required competencies. | `<span class="cp-skill-tag">RTOS</span>` (limit to 3–4 tags per row to prevent layout breakage) |

---

## Design Impact & Layout Solutions

* **The Design Discrepancy Risk:** Overcrowding stat cards squeezes metric columns. Adding dozens of skill tags wraps pills onto multiple uneven lines.
* **Visual Impact:** Misaligned text boxes and uneven skill tags break table rows.
* **Recommended Solutions:**
    * **Stat ceiling:** Keep exactly 3 statistic cards (`.cp-stat-card`) in the pathway header panel for layout stability.
    * **Skill tag limits:** Restrict the career outcomes table skill list to 3 or 4 high-value tags per job title to prevent column stretching.

---

## Let's Verify Your Changes

1. Open `/CareerPathways.html` in your web browser, or use a code editor preview extension (such as Live Server) to render the page locally.
2. Click the corresponding sidebar menu link to open your modified career panel.
3. Verify that your numbers, table rows, and skill pills display correctly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 27, 2026
