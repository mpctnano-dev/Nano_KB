# Update Ecosystem Partners

**Summary:** Add, remove, or modify the industry partner card blocks (such as TSMC or Intel) displayed in the regional ecosystem section of the Workforce Development page.

**Trigger:** When a new corporate semiconductor or technology partnership is established, or a partner's investment statistics or local description is updated.

**Difficulty:** Routine

**Estimated Time:** 15 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `WorkForceDevelopment.html` | The main Workforce Development page containing the hardcoded ecosystem grid markup. |
| `Images/` | The folder where the partner's transparent logo asset should be saved. |

---

## Step-by-Step Instructions

### Part 1: Prepare the Partner Logo Asset

1. Obtain a high-quality, transparent logo image for the new corporate partner (saved as a transparent PNG or WEBP).
2. Save the graphic inside the `Images/` folder using a clean, lowercase name:
   * **Example:** `Images/new_partner.png`.

---

### Part 2: Locate the Grid in WorkForceDevelopment.html

1. To get started, please open `WorkForceDevelopment.html` in your code editor.
2. Please search for the division tag with the class name `"wfd-eco__grid"`. Inside, you will find:
   `<div class="wfd-eco__grid">`
3. Inside this container division tag, you will see nested division blocks representing individual ecosystem cards.

---

### Part 3: Add or Edit a Partner Card

1. Let's add the new partner card to the grid! To do this, simply copy an existing ecosystem card division block and paste it inside the container in your desired order.
2. Each partner is represented by a division card tag with the class name `"wfd-eco__card"`:
   `<div class="wfd-eco__card">`
3. You can customize the elements inside this card block using the guide table below:

| Component | HTML Element | Example |
|---|---|---|
| **Logo** | `<img src="..." class="wfd-eco__card-logo">` | `<img src="Images/new_partner.png" alt="Company Name">` |
| **Investment Badge** | `<span class="wfd-eco__card-invest">` | `$10B Invested` or `AZ Headquartered` |
| **Company Name** | `<h3>` | `<h3>NXP Semiconductors</h3>` |
| **Description** | `<p>` | A 1-2 sentence summary of their Arizona footprint. |

---

### Example Markup

Below is a complete example of a single ecosystem card to guide your edits:

```html
<!-- Example Card -->
<div class="wfd-eco__card">
    <div class="wfd-eco__card-head">
        <div class="wfd-eco__card-logo-wrap">
            <img src="Images/intel.png" alt="Intel" class="wfd-eco__card-logo">
        </div>
        <span class="wfd-eco__card-invest">$34.5B Invested</span>
    </div>
    <h3>Intel Arizona</h3>
    <p>Arizona hosts Intel's largest manufacturing site...</p>
</div>
```

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Adding a 5th partner card wraps it down to a second row, leaving a single card centered awkwardly by itself.
*   **Visual Impact:** Visual empty slots in the grid look unfinished and unpolished.
*   **Recommended Solutions:**
    *   **Multiple-of-Four Rule:** Keep ecosystem partner card lists aligned in groups of 4.
    *   **Horizontal Swipe Strip:** If the regional ecosystem network grows continually, replace the static column wrapping grid with a touch-enabled horizontal scroll strip (`white-space: nowrap; overflow-x: scroll;`) for slick mobile swiping.

---

## Let's Verify Your Changes

1. Open `WorkForceDevelopment.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Scroll to the "Ecosystem" section.
3. Verify that your new company card loads cleanly, the logo is centered inside the white bounding box, and the text aligns correctly next to adjacent cards.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
