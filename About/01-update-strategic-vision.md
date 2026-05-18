# Update Strategic Vision Pipeline

**One-line description:** Update the numbered stages in the Strategic Vision section (Research Infrastructure, Workforce Development, Industry Services).

##**When you'd do this:** The lab's primary mission pillars evolve or new tags need to be added.

##**Difficulty:** Routine

##**Estimated time:** 10 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `About_Us.html` | The pipeline stages are hardcoded in the HTML. |

## Steps

1. Open `About_Us.html`.
2. Locate the vision pipeline by searching for `<div class="vision-pipeline">`.
3. Inside, you will see a list of `<div class="pipeline-stage">` blocks.
4. To add or modify a stage, fill in these values within the block:

| # | What to Change | Options / Example |
|---|---|---|
| ① | Icon Color | `<div class="stage-icon gold">` (Options: default (blue), `gold`, `green`) |
| ② | FontAwesome Icon | `<i class="fas fa-graduation-cap"></i>` |
| ③ | Stage Number | `<div class="stage-number">02</div>` |
| ④ | Title | `<h3>Workforce Development</h3>` |
| ⑤ | Description | `<p>In direct partnership with TSMC...</p>` |
| ⑥ | Tag Links | `<a href="degree-programs.html" class="stage-tag-link"><span>Industry Curriculum</span></a>` |

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

## Verify It Worked
*   Load `About_Us.html` in a browser.
*   Scroll to "Strategic Vision".
*   Verify the updated text, icons, and link tags are functioning.

---
*After making this change and deploying, update this how-to page if anything about the process changed.*
*Last reviewed: 2026-05-15*
