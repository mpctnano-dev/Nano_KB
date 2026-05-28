
# Update Arizona's Semiconductor Ecosystem Section

**Summary:** Add, remove, or modify the industry partner card blocks (such as TSMC, Intel, or Microchip) displayed in the regional ecosystem section of the Workforce Development page.

**When to change:** When a new corporate semiconductor or technology partnership is established, or a partner's investment statistics or local description is updated.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 15 minutes

---

## Visual Reference

<p align="center">
  <img src="../md_file_images/Wf_Ecosystem_%231.png" alt="Arizona's Semiconductor Ecosystem Section" /><br/>
  <em>Figure 1: Arizona's Semiconductor Ecosystem Section visual layout in WorkForceDevelopment.html</em>
</p>


---

## Target Files

| Path | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The main Workforce Development page containing the hardcoded ecosystem grid markup. |
| `Images/` | The folder where the partner's transparent logo asset should be saved. |

---

## Step-by-Step Instructions

### Part 1: Locate the Grid in WorkForceDevelopment.html

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Please search for the container division tag with the class name `"wfd-eco__grid"`.

### Real Code Snippet

```html
<div class="wfd-eco__grid wfd-reveal wfd-d1">
```

```html
<section class="wfd-ecosystem">
    <div class="container">
        ...
        <div class="wfd-eco__grid wfd-reveal wfd-d1">
            ...
        </div>
    </div>
</section>
```


3. Inside this container division tag, you will see nested division blocks representing individual ecosystem cards.

---

### Part 2: Add or Edit a Partner Card

1. To add a new partner card or customize an existing one, duplicate or locate the target card division block.
2. Each partner is represented by a division card tag with the class name `"wfd-eco__card"`:

### Real Code Snippet

```html
<div class="wfd-eco__card">
```

```html
<div class="wfd-eco__card">
    <div class="wfd-eco__card-head">
        <div class="wfd-eco__card-logo-wrap">
            <img src="Images/TSMC.png" alt="TSMC" class="wfd-eco__card-logo">
        </div>
        <span class="wfd-eco__card-invest">$165B Invested</span>
    </div>
    <h3>TSMC Arizona</h3>
    <p>Six fabs and two advanced packaging facilities planned for the North Phoenix...</p>
</div>
```


3. You can customize the elements inside this card block using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Partner Logo Image | The visual brand mark of the corporate partner. Transparent PNG or WEBP logos are recommended to blend clean with the white bounding box container background. | `<img src="Images/TSMC.png" alt="TSMC" class="wfd-eco__card-logo">` (Options: lowercase PNG/WEBP inside `Images/`) |
| ② | Investment / HQ Badge | The highlight badge showing the partner's local presence metrics or headquarters location to demonstrate the regional footprint depth. | `<span class="wfd-eco__card-invest">$165B Invested</span>` or `<span class="wfd-eco__card-invest">AZ Headquartered</span>` |
| ③ | Company Name | The primary header displaying the name of the company or regional semiconductor facility. | `<h3>TSMC Arizona</h3>` |
| ④ | Footprint Description | A 1-2 sentence description summarizing the partner's manufacturing operations, employee footprint, or university integration within Arizona's technology corridor. | `<p>Six fabs and two advanced packaging facilities planned...</p>` |

---

### Example Snippet

```html
<div class="wfd-eco__card">
    <div class="wfd-eco__card-head">
        <div class="wfd-eco__card-logo-wrap">
            <img src="Images/intel.png" alt="Intel" class="wfd-eco__card-logo">
        </div>
        <span class="wfd-eco__card-invest">$34.5B Invested</span>
    </div>
    <h3>Intel Arizona</h3>
    <p>Arizona hosts Intel's largest manufacturing site with 10,000+ employees, producing leading-edge processors and advanced packaging technologies.</p>
</div>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Adding an odd or prime number of partner cards (such as 5) wraps them unevenly on desktop, leaving a single card sitting awkwardly aligned to the left of the final row with vast empty white space to its right.
*   **Visual Impact:** Visual empty slots in the grid look unfinished and ruin the grid's balance.
*   **Recommended Desktop & Website Solutions:**
    *   **Centered Flexbox Rows:** Convert the desktop grid from a strict four-column grid to a centered flex wrap container. This automatically centers any wrapped cards on the bottom row, maintaining clean, symmetric, and premium visual layouts:
        ```css
        .wfd-eco__grid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center; /* Centers any remaining odd cards on wrapped rows */
            gap: 30px;
        }
        .wfd-eco__card {
            flex: 0 1 calc(25% - 22.5px); /* Lays out 4 cards per row on desktop */
            min-width: 280px;
        }
        ```
    *   **Featured Spanning Card:** Style the primary partner card (e.g., TSMC) to span across two grid columns on desktop, balancing the odd card count:
        ```css
        .wfd-eco__card:first-child {
            grid-column: span 2; /* TSMC spans 2 columns, perfectly balancing a 5-card layout */
        }
        ```
    *   **Multiple-of-Four Rule:** Keep ecosystem partner card lists strictly aligned in groups of 4 to prevent wrapping anomalies entirely.
*   **Recommended Mobile Solutions:**
    *   **Horizontal Swipe Strip:** On screens below 768px, replace the vertical wrapping grid with a touch-enabled horizontal scroll strip (`white-space: nowrap; overflow-x: auto;`) for modern, native-feeling mobile swiping.

---

## Let's Verify Your Changes

1. Open `WorkForceDevelopment.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Scroll to the "Ecosystem" section.
3. Verify that your new company card loads cleanly, the logo is centered inside the white bounding box, and the text aligns correctly next to adjacent cards.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: May 27, 2026*
