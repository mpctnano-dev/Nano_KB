
# Program & Training Pages (PTAP / Intel CHIPS)

**Summary:** Maintain hero metrics, partnership logo strips, interactive Bento Grid showcases, and the cohort application form on the PTAP (`/PTAP.html`) and Intel CHIPS Scholars (`/CHIPS_Scholars_Program.html`) program pages.

**When to change:** When scholarship structures, curricula, or fab-partner details are updated, or when adjusting the registration form's validation behavior.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★</span>

**Estimated Time:** 20 minutes

---

## Visual Reference

<p align="center">
  <img src="../md_file_images/wf_PTAP.png" alt="PTAP program page" /><br/>
  <em>Figure 1: PTAP Program page layout</em>
</p>

<p align="center">
  <img src="../md_file_images/Intel_chips.png" alt="Intel chips program page" /><br/>
  <em>Figure 2: Intel CHIPS Scholars program page layout</em>
</p>


---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/PTAP.html` | Main HTML document housing the Process Technician Apprenticeship Program structures, inline styles, and reveal animations. |
| `/CHIPS_Scholars_Program.html` | Sister HTML page containing the funded Master's scholarship cohort details and dynamic application form. |

---

## Step-by-Step Instructions

### Part 1: Update the Hero Quick-Info Panel

To update hero stipends, cohort sizes, or timeline stats, locate the quick-info panel container `<div class="ml-hero__panel">` at the top of either `/PTAP.html` or `/CHIPS_Scholars_Program.html` and modify the text within the `.ml-highlight__title` and `.ml-highlight__desc` elements.

---

### Part 2: Update the Partnership Logos

To modify corporate or institutional sponsors, locate the partnership logos container (`.ptap-partners__logos` or `.intel-partners__logos`) and add or swap `<img>` tags. Always apply the class `ptap-partners__logo` or `intel-partners__logo` to ensure consistent height matching and hover opacity.

---

### Part 3: Edit Bento Grid Highlights and Modifiers

The center of both program pages features an interactive 12-column Bento Grid (`.ptap-bento__grid` or `.intel-bento__grid`) utilizing two distinct, reusable card styles. 

*   **Option A: Narrative Details Card** (Text-heavy card with an icon circle and top border accent):

    **Example Snippet (Option A: Detail Narrative Card Blueprint)**
    ```html
    <div class="ptap-bento__card ptap-bento__card--wide ptap-reveal">
        <div class="ptap-bento__accent ptap-bento__accent--[color]"></div>
        <div class="ptap-bento__icon ptap-bento__icon--[color]">
            <i class="[Font_Awesome_Class]"></i>
        </div>
        <h3>[Card_Heading_Text]</h3>
        <p>[Card_Description_Narrative]</p>
    </div>
    ```

*   **Option B: Feature Stat Card** (Dynamic stat card with an oversized background watermark number):

    **Example Snippet (Option B: Feature Stat Card Blueprint)**
    ```html
    <div class="ptap-bento__card ptap-bento__card--narrow ptap-bento__card--featured ptap-reveal [Reveal_Delay]">
        <div class="ptap-bento__watermark">[Watermark_Text]</div>
        <div class="ptap-bento__stat">[Stat_Number]<em>[Stat_Unit]</em></div>
        <div class="ptap-bento__stat-label">[Stat_Label]</div>
    </div>
    ```

#### Configure Bento Layout Modifiers
Customize the grid behavior and themes by modifying helper classes on the card containers:

| Modifying Target | CSS Utility / HTML Tag | Purpose |
|---|---|---|
| **Column Span (Desktop)** | `ptap-bento__card--wide`<br>`ptap-bento__card--narrow`<br>`ptap-bento__card--third` | Spans 8, 4, or 4 columns respectively within the 12-column desktop layout. |
| **Accent & Icon Theme** | `ptap-bento__accent--[color]` | Sets the color scheme (glows/borders) for icons and top bars. Options: `gold`, `blue`, `green`, `cyan`. |
| **Watermark Layer** | `<div class="ptap-bento__watermark">` | Renders a large background watermark. Keep it brief (e.g. `600`, `$15K`, `9+3`). |
| **Staggered Animations** | `ptap-d1` to `ptap-d5` | Controls slide-in entrance delays on scroll. Apply to stagger row cards. |

---

### Part 4: Modify the Coursework Curriculum List

Curriculum course lists (`.ptap-coursework__list`) and research pillars (`.intel-focus__list`) utilize simple structural divisions inside their respective grid wrappers. To add, remove, or modify items, edit the `.ptap-coursework__item` or `.intel-focus__item` list children.

---

### Part 5: Update the 'How It Works' Milestones

Cohort milestone stages use a balanced grid track (`.ptap-pathway__steps`) connected via a horizontal progress line on desktop screen resolutions. When updating steps, maintain uniform paragraph word lengths within the `.ptap-pathway__step` elements to prevent uneven vertical expansion.

---

### Part 6: Manage the Scholars Registration Form & Validation

The registration booking form (`#scholarshipForm`) at the bottom of `/CHIPS_Scholars_Program.html` gathers prospective student records and posts submissions to `IntelScholarshipSubmission.php`.

#### 1. Form Validation Integration (JavaScript)
The form relies on the global validator `MPCT.Validation` (loaded from `JS/validation.js`) to enforce validation. A page-scoped script wireup runs when users interact with the page:

*   **Invalid Outlining:** Every invalid input element is outlined using a consistent red outline via `MPCT.Validation.markInvalid(el)`.
*   **Name Validation:** Triggers live input listening on fields marked with `data-name-field` to block numerical entries and emojis.
*   **Email & Phone Verification:** Validates email strings with `MPCT.Validation.isEmail(v)` and enforces a masked telephone input format `(XXX) XXX-XXXX` requiring at least 7 digits.
*   **Radio Group Selection:** Validates radio button groups (`degree_interest`) by highlighting the visible wrapper `.intel-register__radio-row` if no option is picked:

    **Real Code Snippet (Radio Group Selection Validation)**
    ```javascript
    var radios = form.querySelectorAll('input[name="degree_interest"]');
    var picked = Array.prototype.some.call(radios, function (r) { return r.checked; });
    if (!picked) {
        var row = radios[0].closest('.intel-register__radio-row');
        row.style.outline = '2px solid var(--nau-red, #c0392b)';
    }
    ```
*   **Textarea Word Limits:** Integrates `data-max-words="500"` on notes textareas, dynamically updating a remaining word counter.
*   **Synthetic Dispatching:** Dispatches a synthetic `'input'` event to input fields on form resets to clear remaining character warning tags.

#### 2. Server-Side Processing (PHP)
Submissions are handled by `IntelScholarshipSubmission.php`, which performs several validation checks:
1. Re-validates all fields server-side using `includes/validation.php` to align byte-for-byte with the client-side JavaScript rules.
2. Formats a clear field-by-field email table.
3. Utilizes PHPMailer to transmit two distinct messages:
   * **Lab Notification:** Transmitted to the primary lab administrators and CC recipients.
   * **Applicant Confirmation:** Transmitted back to the student acknowledging registration.

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Mismatched form input tags or target attributes between the HTML frontend and PHP mailer validation schemas will block scholarship submissions or cause email data losses.
*   **Visual Impact:** Broken input styling, overlapping red error outlines, or misaligned bento layout blocks.
*   **Recommended Solutions:**
    *   **Validate Names and Endpoints:** Always keep name attributes inside `<input>` fields completely aligned with the `$detailFields` array in `IntelScholarshipSubmission.php`.
    *   **Keep Watermarks Compact:** Ensure background bento watermarks remain short (e.g. `$15K` or `12`) to prevent visual overflows on narrower tablet resolutions.
    *   **Outline Reset Verification:** Verify that any dynamically set style outlines on custom radio rows are cleanly cleared when users change inputs.

---

## Let's Verify Your Changes

1. Open `/PTAP.html` and `/CHIPS_Scholars_Program.html` in your web browser.
2. Scroll to the booking form at the bottom of the Scholars page, deliberately trigger an empty submit, and verify that the red outlines and error description box display as expected.
3. Successfully complete and submit the form using dummy data, then verify that the success message clears and input states reset cleanly.
4. If a local test mail server is running, check your inbox and verify that both the administrator notification and student confirmation emails are formatted and delivered.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

Last reviewed: May 28, 2026
