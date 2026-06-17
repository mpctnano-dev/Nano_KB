
# Update Industry Stats

**Summary:** Update the key statistical metrics and impact figures displayed inside the hero panel and the Arizona Semiconductor Boom section on the Workforce Development page.

**When to change:** When annual workforce reporting yields new figures for student enrollment, technology industry job expansion, or capital investment projections.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★</span>

**Estimated Time:** 5 minutes

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/WorkForceDevelopment.html` | The main Workforce Development page document containing the hardcoded statistical panel structures. |

> [!IMPORTANT]
> **Codebase Integrity & Local Verification:**
> Before carrying out any modifications described in this guide, **always verify these instructions and code snippets against the active, local files in your codebase**.
> * **Verify Exact Class & Tag Matches:** HTML markup and classes can adapt over time. Make sure class structures like `wfd-hero__panel-stats` and `wfd-stats__grid` match exactly.
> * **Never Assume Snapshot Ground-Truth:** Always double-check dynamic key strings (like `4<em>+</em>` vs `40K<span>+</span>`) across your actual, active files to prevent style or color mismatches.

---

## Step-by-Step Instructions

### Part 1: Update the Hero Overview Stats

<p align="center">
  <img src="/md_file_images/Industry_Stats_%234.png" alt="Hero Overview Stats" /><br/>
  <em>Figure 1: Hero Overview Stats visual layout in WorkForceDevelopment.html</em>
</p>

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Please search for the stats container division tag with the class name `"wfd-hero__panel-stats"`. Inside, you will find:

   **Real Code Snippet**
   ```html
   <div class="wfd-hero__panel-stats">
   ```

```html
<div class="wfd-hero__panel">
    <div class="wfd-hero__panel-label">Program Impact</div>
    <div class="wfd-hero__panel-stats">
        ...
    </div>
</div>
```


3. Inside this container division tag, locate individual statistical subdivisions. You can customize the elements inside this block using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Metric Value | The primary number indicating program achievements. | `40` (inside `<div class="wfd-pstat__num">`) |
| ② | Metric Suffix | Unit indicators or symbols. Must be enclosed inside an emphasis tag (`<em>`) to colorize them gold. | `<em>K+</em>` (inside `<div class="wfd-pstat__num">40<em>K+</em></div>`) |
| ③ | Metric Label | The descriptive text displayed beneath the number explaining the metric. | `<div class="wfd-pstat__label">AZ Tech Jobs</div>` |

> [!CAUTION]
> **Strict Tag Matching:**
> The hero panel stats must strictly use emphasis tags (`<em>`) inside `.wfd-pstat__num` for gold-accented suffixes. Using `span` tags here will break the styling since CSS variables are explicitly scoped to `em` elements in this section!

**Example Snippet**

Here is an example of the updated hero stats inside `WorkForceDevelopment.html`:
```html
<div class="wfd-hero__panel-stats">
    <div>
        <div class="wfd-pstat__num">8<em>+</em></div>
        <div class="wfd-pstat__label">Key Fab Partners</div>
    </div>
    <div>
        <div class="wfd-pstat__num">720<em>hrs</em></div>
        <div class="wfd-pstat__label">Specialized Coursework</div>
    </div>
    <div>
        <div class="wfd-pstat__num">50<em>K+</em></div>
        <div class="wfd-pstat__label">Arizona Tech Openings</div>
    </div>
    <div>
        <div class="wfd-pstat__num">3<em>yr</em></div>
        <div class="wfd-pstat__label">Accelerated Pipeline</div>
    </div>
</div>
```

---

### Part 2: Update the Arizona Semiconductor Boom Cards

<p align="center">
  <img src="/md_file_images/Industry_Stats_%231.png" alt="Arizona Semiconductor Boom Cards" /><br/>
  <em>Figure 2: Arizona Semiconductor Boom Cards visual layout in WorkForceDevelopment.html</em>
</p>

1. Scroll down the document to find the stats grid container division tag with the class name `"wfd-stats__grid"`:

   **Real Code Snippet**
   ```html
   <div class="wfd-stats__grid wfd-reveal wfd-d1">
   ```

```html
<section class="wfd-stats">
    <div class="container">
        ...
        <div class="wfd-stats__grid wfd-reveal wfd-d1">
            ...
        </div>
    </div>
</section>
```


2. Each statistics highlight card is defined by a division tag with the class name `"wfd-stat"`. You can customize the elements inside these blocks using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Card Value | The primary statistic number representing regional growth. | `$210` (inside `<div class="wfd-stat__num">`) |
| ② | Card Suffix | Suffixes or unit symbols (like plus signs or abbreviations). Must be wrapped inside a `span` tag (`<span>`) to align neatly. | `<span>B+</span>` (inside `<div class="wfd-stat__num">$210<span>B+</span></div>`) |
| ③ | Card Description | The label narrative. Use line-break tags (`<br>`) strategically to preserve vertical alignment. | `<div class="wfd-stat__desc">Capital Investment<br>Since 2020</div>` |
| ④ | Data Citation | Source attribution at the bottom of the grid. Update this when modifying the stats to maintain authority and credibility. | `<div class="wfd-stats__source">Source: Arizona Commerce Authority, 2025</div>` |

> [!WARNING]
> **Tag Style Inversion:**
> Unlike the hero stats in Part 1, the grid cards must strictly use `span` tags (`<span>`) inside `.wfd-stat__num` for suffixes (e.g. `<span>+</span>`). Placing an `em` tag inside this block will break color styling and alignment!

**Example Snippet**

Here is an example of the updated semiconductor boom cards inside `WorkForceDevelopment.html`:
```html
<div class="wfd-stats__grid wfd-reveal wfd-d1">
    <div class="wfd-stat">
        <div class="wfd-stat__num">$350<span>B+</span></div>
        <div class="wfd-stat__desc">Projected Investment<br>Through 2030</div>
    </div>
    <div class="wfd-stat">
        <div class="wfd-stat__num">85<span>+</span></div>
        <div class="wfd-stat__desc">New Semiconductor<br>Corporate Expansions</div>
    </div>
    <div class="wfd-stat">
        <div class="wfd-stat__num">55K<span>+</span></div>
        <div class="wfd-stat__desc">Direct &amp; Supply Chain<br>Jobs Created</div>
    </div>
    <div class="wfd-stat">
        <div class="wfd-stat__num">#1</div>
        <div class="wfd-stat__desc">Leading Growth Hub<br>for Advanced Fabs</div>
    </div>
</div>
<div class="wfd-stats__source wfd-reveal wfd-d2">
    Source: Arizona Commerce Authority &amp; Business Facilities Magazine, 2025
</div>
```

---

## Design Impact & Layout Solutions

* **The Design Discrepancy Risk:** Adding extra cards, removing cards, or introducing excessively long text descriptions can wrap columns unevenly or break symmetry, compromising the high-end premium aesthetic.
* **Visual Impact:** Jagged card alignments, misaligned headers, or empty spaces that make the page look unpolished.
* **Recommended Solutions:**
  * **Strict Multiples Alignment:** Maintain exactly 4 cards in the lower grid (`.wfd-stats__grid`) to match the 4-column layout on desktop views.
  * **Line Break Optimization:** Keep description texts compact. Always use `<br>` to break long text labels into two neat lines of similar lengths.
  * **Mobile Scaling Verification:** Always preview modifications locally on mobile screens to ensure the grid items stack elegantly without text overlap.

---

## Let's Verify Your Changes

1. Open `WorkForceDevelopment.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Verify that the updated numerical figures and label descriptions render correctly, and check that all gold suffixes and plus indicators sit aligned.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 27, 2026
