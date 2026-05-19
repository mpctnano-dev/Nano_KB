# Update Strategic Vision Pipeline

**Summary:** Update the numbered vision milestone stages (such as Research Infrastructure, Workforce Development, or Industry Services) displayed in the Strategic Vision section of the About Us page.

**Trigger:** When the lab's primary mission pillars are revised or new highlights are added to the vision pipeline.

**Difficulty:** Routine

**Estimated Time:** 10 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `About_Us.html` | The main About Us document containing the hardcoded strategic vision pipeline markup. |

---

## Step-by-Step Instructions

### Part 1: Please Locate the Pipeline in About_Us.html

1. To get started, please open `About_Us.html` in your code editor.
2. Please search for the container division tag with the class name `"vision-pipeline"`. Inside, you will find:
   `<div class="vision-pipeline">`
3. Inside this container division, you will see nested division blocks representing individual pipeline stages.

---

### Part 2: Let's Add or Edit a Pipeline Stage

1. To add a new stage or customize an existing milestone, duplicate or locate the target stage division block.
2. Each stage is represented by a division tag with the class names `"pipeline-stage"`, `"about-reveal"`, and a sequence delay class:
   `<div class="pipeline-stage about-reveal about-delay-2">`
3. You can customize the elements inside this block using the guide table below:

| # | What to Change | Options / Example |
|---|---|---|
| ① | Icon Color | `<div class="stage-icon gold">` (Options: default (blue), `gold`, `green`) |
| ② | FontAwesome Icon | `<i class="fas fa-graduation-cap"></i>` |
| ③ | Stage Number | `<div class="stage-number">02</div>` |
| ④ | Title | `<h3>Workforce Development</h3>` |
| ⑤ | Description | `<p>In direct partnership with TSMC...</p>` |
| ⑥ | Tag Links | `<a href="degree-programs.html" class="stage-tag-link"><span>Industry Curriculum</span></a>` |

---

### Example Markup

Below is a complete example of a single pipeline stage to guide your edits:

```html
<!-- Example of a single pipeline stage -->
<div class="pipeline-stage about-reveal about-delay-2">
    <div class="stage-icon gold"><i class="fas fa-graduation-cap"></i></div>
    <div class="stage-card">
        <div class="stage-number">02</div>
        <h3>Workforce Development</h3>
        <p>In direct partnership with TSMC, Intel, and Microchip Technology...</p>
        <div class="stage-tags">
            <a href="degree-programs.html#ptap" class="stage-tag-link"><span>PTAP Apprenticeship</span></a>
            <a href="WorkForceDevelopment.html" class="stage-tag-link"><span>Semiconductor Lab Training</span></a>
        </div>
    </div>
</div>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Adding more stages horizontally squeezes the circular milestone blocks, causing them to crowd each other or wrap awkwardly into multiple uneven rows.
*   **Visual Impact:** This ruins the cohesive "horizontal pipeline" motif, leading to cramped text alignments and hard-to-read numbers.
*   **Recommended Solutions:**
    *   **Strict Count Boundaries:** Keep the strategic timeline at exactly 6 milestones.
    *   **CSS Stepper Timeline:** If you must add a 7th stage, modify `CSS/style.css` to add a media query that transitions the horizontal line to a vertical stepper layout on screens below 1024px, preventing column compression.
    *   **Text Wrapping Protection:** Limit titles inside `.pipeline-stage__title` to 2 words max to avoid line-break collisions.

---

## Let's Verify Your Changes

1. Open `About_Us.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Scroll down to the "Strategic Vision" section.
3. Verify that the milestones render correctly and that all customized titles, descriptions, and tag hyperlinks link to their proper targets.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
