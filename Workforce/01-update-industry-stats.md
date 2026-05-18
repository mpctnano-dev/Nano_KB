# Update Industry Statistics

**One-line description:** Update the statistical numbers shown in the Hero section and the Arizona Semiconductor Boom cards.
**When you'd do this:** Annual reporting reveals new numbers for students trained, capital investments, or job projections.
**Difficulty:** Trivial
**Estimated time:** 5 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The stats are hardcoded in the HTML. |

## Steps

### Hero Stats
1. Open `WorkForceDevelopment.html`.
2. Locate the hero panel stats by searching for `<div class="wfd-hero__panel-stats">`.
3. You will see blocks like this:
   ```html
   <div>
       <div class="wfd-pstat__num">40<em>K+</em></div>
       <div class="wfd-pstat__label">AZ Tech Jobs</div>
   </div>
   ```
4. Update the number (inside the text or the `<em>` tag) and the label as needed.

### Arizona Boom Stats
1. Scroll down to the `<div class="wfd-stats__grid">` section.
2. You will see stat cards like this:
   ```html
   <div class="wfd-stat">
       <div class="wfd-stat__num">$210<span>B+</span></div>
       <div class="wfd-stat__desc">Capital Investment<br>Since 2020</div>
   </div>
   ```
3. Update the number, suffix (inside the `<span>`), and description. 
4. If you update these numbers, be sure to update the citation directly below them in the `<div class="wfd-stats__source">` block!

## Verify It Worked
*   Load `WorkForceDevelopment.html` in a browser.
*   Verify the numbers align correctly and the suffixes (e.g., "+", "B") still make grammatical sense.
