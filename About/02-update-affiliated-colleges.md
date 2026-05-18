# Update Affiliated Colleges

**One-line description:** Add or modify the colleges and departments listed in the "Cross Campus Impact" section.
**When you'd do this:** The lab establishes a formal partnership with a new NAU department or expands its research areas.
**Difficulty:** Trivial
**Estimated time:** 5 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `About_Us.html` | The affiliation cards are hardcoded. |

## Steps

1. Open `About_Us.html`.
2. Locate the affiliations grid by searching for `<div class="affiliations-grid">`.
3. Inside, you will see a list of `<div class="affiliation-card">` blocks.
4. To add or modify a card, fill in these values within the block:

| # | What to Change | Options / Example |
|---|---|---|
| ① | Icon Color | `<div class="affiliation-icon gold">` (Options: default (blue), `gold`, `green`, `purple`) |
| ② | FontAwesome Icon | `<i class="fas fa-atom"></i>` |
| ③ | College/Department Name | `<h3>College of the Environment...</h3>` |
| ④ | Bullet Points | `<li>Nanomaterials characterization</li>` |

```html
<!-- Example of a single affiliation card -->
<div class="affiliation-card about-reveal about-delay-2">
    <div class="affiliation-icon gold">
        <i class="fas fa-atom"></i>
    </div>
    <h3>College of the Environment, Forestry & Natural Sciences</h3>
    <ul class="affiliation-areas">
        <li>Nanomaterials characterization</li>
        <li>Surface & interface analysis</li>
        <li>Environmental materials research</li>
    </ul>
</div>
```

## Verify It Worked
*   Load `About_Us.html` in a browser.
*   Scroll to the bottom to view the "Cross Campus Impact" section.
*   Verify the updated cards display correctly.

---
*After making this change and deploying, update this how-to page if anything about the process changed.*
*Last reviewed: 2026-05-15*
