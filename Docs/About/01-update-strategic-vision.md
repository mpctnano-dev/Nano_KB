
# Update Strategic Vision Section

**Summary:** Update the numbered vision milestone stages (such as Research Infrastructure, Workforce Development, or Industry Services) displayed in the Strategic Vision section of the About Us page.

**When to change:** When the lab's primary mission pillars are revised or new highlights are added to the vision pipeline.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 10 minutes

---

## Visual Reference

<p align="center">
  <img src="/md_file_images/Strategic_Vision_%231.png" alt="Strategic Vision Pipeline" /><br/>
  <em>Strategic Vision Pipeline</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/About_Us.html` | The main About Us document containing the hardcoded strategic vision pipeline markup. |

---

## Step-by-Step Instructions

### Part 1: Please Locate the Pipeline in About_Us.html

1. To get started, please open `About_Us.html` in your code editor.
2. Please search for the container division tag with the class name `"vision-pipeline"`. Inside, you will find:
   **Real code snippet:** `<div class="vision-pipeline">`

   <p align="center">
     <img src="/md_file_images/Strategic_Vision_%232.png" alt="Locating the vision-pipeline Container" /><br/>
     <em>Locating the main vision-pipeline container class in About_Us.html</em>
   </p>
3. Inside this container division, you will see nested division blocks representing individual pipeline stages.

   <p align="center">
     <img src="/md_file_images/Strategic_Vision_%233.png" alt="Nested Pipeline Stages" /><br/>
     <em>Identifying individual nested pipeline-stage elements</em>
   </p>

---

### Part 2: Let's Add or Edit a Pipeline Stage

1. To add a new stage or customize an existing milestone, duplicate or locate the target stage division block.
2. Each stage is represented by a division tag with the class names `"pipeline-stage"`, `"about-reveal"`, and a sequence delay class:
   **Real code snippet:** `<div class="pipeline-stage about-reveal about-delay-2">`

   <p align="center">
     <img src="/md_file_images/Strategic_Vision_%234.png" alt="Individual Stage Markup" /><br/>
     <em>Structure of a pipeline-stage container with animation delay classes</em>
   </p>
3. You can customize the elements inside this block using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Icon Color | Controls the background color theme of the circular icon container. | `<div class="stage-icon gold">` (Options: default/blue, `gold`, `green`) |
| ② | FontAwesome Icon | The visual glyph inside the icon circle, representing the category/theme. | `<i class="fas fa-graduation-cap"></i>` |
| ③ | Stage Number | The sequential numbering shown at the top-left of the milestone card. | `<div class="stage-number">02</div>` |
| ④ | Title | The primary heading for the specific pipeline milestone. | `<h3>Workforce Development</h3>` |
| ⑤ | Description | The main narrative text detailing key accomplishments and partnerships. | `<p>In direct partnership with TSMC...</p>` |
| ⑥ | Tag Links | Hyperlinks representing related programs, labs, or curricula. | `<a href="degree-programs.html" class="stage-tag-link"><span>Industry Curriculum</span></a>` |

> [!NOTE]
> **Understanding the Font Awesome `<i>` Class & Sourcing Icons:**
> *   **What the class means:** The tag `<i class="fas fa-graduation-cap"></i>` utilizes Font Awesome classes. Here, `fas` stands for **Font Awesome Solid** (the icon style), and `fa-graduation-cap` specifies the **particular glyph** (a graduation cap).
> *   **How we got that icon:** 
>     1. Visit the official [Font Awesome Icon Library](https://fontawesome.com/icons).
>     2. Use the search bar to find keywords related to your pipeline stage (e.g., `flask` for research, `handshake` for services, `graduation-cap` for workforce, or `cubes` / `microchip` for R&D/packaging).
>     3. Ensure you filter by **Free** icons.
>     4. Click on your desired icon, copy the HTML `<i>` tag code snippet, and paste it into your `About_Us.html` stage markup.

---

### Example Markup

Below is a complete example of a single pipeline stage to guide your edits:

**Example snippet:**
```html
<!-- Example of a single pipeline stage (Stage 04: Advanced Packaging R&D) -->
<div class="pipeline-stage about-reveal about-delay-4">
    <div class="stage-icon green"><i class="fas fa-cubes"></i></div>
    <div class="stage-card">
        <div class="stage-number">04</div>
        <h3>Advanced Packaging R&D</h3>
        <p>Collaborating with national laboratories and global OSAT partners to pioneer heterogeneous integration and advanced thermal management technologies for next-generation packaging systems.</p>
        <div class="stage-tags">
            <a href="services.html#packaging" class="stage-tag-link"><span>Heterogeneous Integration</span></a>
            <a href="Contact_Us.html?category=packaging" class="stage-tag-link"><span>Packaging Consortium</span></a>
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

*Last Updated: May 22, 2026*
