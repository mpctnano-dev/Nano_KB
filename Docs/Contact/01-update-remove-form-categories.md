
# Update/Remove Form Categories

**Summary:** Add, remove, or modify the interactive inquiry categories displayed as selectable cards on the Contact Us page, and register their fields in both front-end scripts and back-end processors.

**When to change:** When the laboratory introduces a new operational division (such as "Student Internships") or re-evaluates the key inquiry topics available to site visitors.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★★</span>

**Estimated Time:** 30 minutes

---

## Visual Reference

<p align="center">
  <img src="../md_file_images/Contact_Form_Categories_%231.png" alt="Contact Us Category Portal Overview" /><br/>
  <em>Overview of the interactive category selection cards on the Contact Us portal</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/Contact_Us.html` | The contact gateway document hosting the visual category selection cards. |
| `/JS/script.js` | The client-side script containing the `fieldData` map that defines and injects custom inputs when a category card is clicked. |
| `/FormSubmission.php` | The backend PHP mail processor that registers the valid categories and maps input names to formatted email labels. |

> [!WARNING]
> **Keep all three layers synchronized!**
> The contact portal operates as a unified three-part system. If you add a card in the HTML but forget to define its input fields in JavaScript, clicking the card will cause an injection error and fail to load. If you update the front-end fields but forget to register them in the PHP script, the server will block the form submission as invalid. You must complete all parts sequentially!

> [!IMPORTANT]
> **Codebase Integrity & Local Verification:**
> Before carrying out any modifications described in this guide, **always verify these instructions and code snippets against the active, local files in your codebase**.
> * **Verify Exact Class & Tag Matches:** Stylesheets (`CSS/style.css`) and grid architectures can be updated over time. Make sure structure classes like `grid-4`, `gatewayGrid`, and card wrappers are exact.
> * **Front-end Script Dependencies:** The form relies on shared assets in `JS/layout.js` (header/footer rendering), `JS/validation.js` (field-level input validation helpers), and `JS/script.js` (category selection logic). Ensure all three are imported correctly at the footer of `Contact_Us.html`.
> * **Icon Sourcing:** Visual icons rely on **Font Awesome v6.4.0** loaded via CDN. Verify that your class names (`fas fa-...`) are compatible with this version.
> * **Never Assume Snapshot Ground-Truth:** Always double-check dynamic key strings (like `'internships'`) and form field name markers (like `'internship_major'`) across your actual, active files to prevent spelling mismatches.

> [!TIP]
> Looking to remove a category instead? Jump directly to [Removing a Category](#removing-a-category).

---

## Step-by-Step Instructions (Adding/Updating a Category)

### Part 1: Add the Selection Card (HTML)

1. To get started, please open `Contact_Us.html` in your code editor.
2. Please search for the gateway grid division tag with the ID `"gatewayGrid"`. Inside, you will find:
   **Real code snippet:** `<div class="grid grid-4 gap-md" id="gatewayGrid">`

   <p align="center">
     <img src="../md_file_images/Contact_Form_Categories_%232.png" alt="Locating the gatewayGrid Container" /><br/>
     <em>Locating the main gatewayGrid container element in Contact_Us.html</em>
   </p>

3. Copy an existing card division block and paste it inside the container in your desired order.
4. Each category is represented by a card division tag with the class name `"gateway-card"`.

   **Example Snippet**
   ```html
   <!-- Example of a gateway card (Stage 08: Student Internships) -->
   <div class="gateway-card" onclick="selectCategory('internships', this)">
       <i class="fas fa-user-graduate"></i>
       <h3 class="gateway-title">Internship Opportunities</h3>
       <p class="gateway-desc">Apply for student positions and research associate roles.</p>
   </div>
   ```

5. You can customize the card properties using the guide table below:

| # | What to Change | Description | Options / Example |
|---|---|---|---|
| ① | Category ID | Unique identifier passed to the `selectCategory` click handler. Must be in single quotes, lowercase, and match the key used in JS and PHP. | `<div class="gateway-card" onclick="selectCategory('internships', this)">` |
| ② | FontAwesome Icon | The visual icon class representing the category. Check the official Font Awesome library. | `<i class="fas fa-user-graduate"></i>` |
| ③ | Title | The primary category heading text displayed in the card. | `<h3 class="gateway-title">Internship Opportunities</h3>` |
| ④ | Description | The brief explanation text displayed under the title. | `<p class="gateway-desc">Apply for student positions...</p>` |

> [!NOTE]
> **Understanding the Font Awesome `<i>` Class & Sourcing Icons:**
> *   **What the class means:** The tag `<i class="fas fa-user-graduate"></i>` utilizes Font Awesome classes. Here, `fas` stands for **Font Awesome Solid** (the icon style), and `fa-user-graduate` specifies the **particular glyph** (a student wearing a mortarboard).
> *   **How we got that icon:** 
>     1. Visit the official [Font Awesome Icon Library](https://fontawesome.com/icons).
>     2. Use the search bar to find keywords related to your inquiry category (e.g., `microscope` for equipment, `atom` for collaborations, `file-invoice-dollar` for billing, `user-graduate` for training, or `truck-loading` for sales).
>     3. Ensure you filter by **Free** icons.
>     4. Click on your desired icon, copy the HTML `<i>` tag code snippet, and paste it into your `Contact_Us.html` card markup.

---

### Part 2: Let's Define Form Inputs (JavaScript)

Now, we must define the specific text inputs that slide into view when your card is selected.

1. To get started, please open `JS/script.js` in your code editor.
2. Please search for the `fieldData` object. Inside, you will find:
   **Real code snippet:** `const fieldData = {`

   <p align="center">
     <img src="../md_file_images/Contact_Form_Categories_%233.png" alt="Locating the fieldData Object in script.js" /><br/>
     <em>Locating the fieldData object inside JS/script.js to declare dynamic fields</em>
   </p>

3. Insert a new category key matching the *exact* ID value you declared in Part 1 (e.g., `'internships'`). Custom fields are written as key-value pairs where the `fields` are declared inside a JavaScript template literal.

   **Example Snippet**
   ```javascript
   'internships': {
       title: "Internship Opportunities",
       desc: "Apply for student positions and research associate roles.",
       fields: `
           <div class="form-stack">
               <div>
                   <label class="block font-bold mb-1 required">Major / Field of Study</label>
                   <input type="text" name="internship_major" class="form-control" placeholder="e.g. Electrical Engineering" required>
               </div>
           </div>
       `
   },
   ```

4. **Crucial Rule:** Every input or select tag inside the HTML `fields` template literal MUST have a unique `name` attribute. This is how the server tracks the value.

---

### Part 3: Register, Map, and Validate the Fields (PHP Form & Field Validation)

Finally, we must tell the backend processor to validate these fields and map them into the notification email sent to lab staff.

1. To get started, please open `FormSubmission.php` in your code editor.
2. Please search for the categories array declaration. Inside, you will find:
   **Real code snippet:** `$categories = [`

   <p align="center">
     <img src="../md_file_images/Contact_Form_Categories_%234.png" alt="Locating the categories array in PHP" /><br/>
     <em>Locating the categories array configuration in FormSubmission.php</em>
   </p>

3. Add a new configuration array block matching your category ID exactly.

   **Example Snippet**
   ```php
   'internships' => [
       'title'       => 'Internship Opportunities',
       'required'    => ['internship_major'],    // Inputs required before submitting
       'textFields'  => ['internship_major'],    // Inputs subject to automated anti-spam checks
       'wordLimited' => [],                      // Inputs limited to 500 words
       'fields'      => ['internship_major'],    // The display order of fields in the email
       'labels'      => [
           'internship_major' => 'Student Major', // The label printed next to the data in the email
       ],
   ],
   ```

   > [!IMPORTANT]
   > **Array Ordering Guidelines:**
   > The order of category keys inside the PHP `$categories` array does not impact backend logic (as PHP retrieves it via associative lookup). However, for long-term codebase maintainability and neatness, it is highly recommended to append your new block to the **end of the array**, or place it in the same sequential order as the cards are laid out in `Contact_Us.html`.

---

🎉 **Great work!** You have now successfully created the visual gateway card, defined the dynamic javascript entry fields, and registered the backend validation hooks in PHP. Your new inquiry category is fully operational and synchronized!

---

## Removing a Category

To safely remove an inquiry category (such as `"sales"` / Vendor / Sales):

1. Open `Contact_Us.html` in your code editor.
2. Search for the gateway grid division tag with the ID `"gatewayGrid"`. Inside, you will find:
   `<div class="grid grid-4 gap-md" id="gatewayGrid">`

   <p align="center">
     <img src="../md_file_images/Contact_Form_Categories_%232.png" alt="Locating the gatewayGrid Container" /><br/>
     <em>Locating the main gatewayGrid container element in Contact_Us.html</em>
   </p>

3. Locate the specific card division block (e.g., the `"sales"` card):
   `<div class="gateway-card" onclick="selectCategory('sales', this)">`
4. Delete or comment out the entire visual block.

<p align="center">
  <img src="../md_file_images/Contact_Form_Categories_%235.png" alt="Removing a Category block from HTML" /><br/>
  <em>Commenting out or deleting the target gateway-card division block inside Contact_Us.html</em>
</p>

The associated JS definitions in `script.js` and PHP array mappings in `FormSubmission.php` can safely remain in the codebase; they will simply never trigger because the user has no way to select that category from the front-end user interface.

---

## Design Impact & Layout Solutions

*   **The Design Discrepancy Risk:** Direct styling shifts, unmatched padding heights, or adding excessive list items can cause grids or column layouts to wrap unevenly.
*   **Visual Impact:** Uneven spacing, jagged line-breaks, or overlapping panels that look unpolished.
*   **Recommended Solutions:**
    *   **Strict Alignment:** Maintain matching card lists in even multipliers (such as multiples of 2, 3, or 4).
    *   **Height Uniformity:** Standardize description lengths and use robust layout parameters (like `min-height`) in global CSS files.
    *   **Preview Tools:** Always verify changes locally across desktop and mobile views before merging into the main repository.

---

## Let's Verify Your Changes

1. Open `Contact_Us.html` to verify your changes. You can double-click the file to launch it directly in your web browser, or if you are using a code editor like VS Code, you can run a local preview extension (such as Live Server) to render and test the page in real-time.
2. Click your new inquiry category card. Verify that the correct input fields slide dynamically into view.
3. Fill out the form fields with test parameters and submit the form.
4. Verify that the browser displays a green success confirmation window and check that the generated email arrives with your custom labels mapped correctly.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last Updated: May 22, 2026*
