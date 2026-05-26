
# Update Contact Form Fields

**Summary:** Add, remove, or modify input fields in the inquiry forms on the Contact Us portal, and register them in both front-end files and the dynamic back-end email processor.

**When to change:** When the laboratory needs to collect new information (such as "Department", "Company Size", or "Preferred Equipment") from visitors submitting inquiries.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★★★★</span>

**Estimated Time:** 30 minutes

---

## Visual Reference

<p align="center">
  <img src="../../md_file_images/Contact_Form_Fields_%231.png" alt="Contact Us Form Fields Overview" /><br/>
  <em>Overview of the interactive inquiry gateway on the Contact Us portal</em>
</p>

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/Contact_Us.html` | The public HTML contact page containing the static/common user-facing form inputs. |
| `/JS/script.js` | The client-side script containing the `fieldData` map that defines and injects category-specific dynamic form inputs. |
| `/FormSubmission.php` | The backend PHP mail processor that dynamically validates inputs, translates options, and generates notification/confirmation emails. |

> [!WARNING]
> **Keep all three layers synchronized!**
> The contact form operates as a unified multi-part system. If you add an input field in the front-end (HTML or JS) but forget to register and map it in the backend PHP script's `$categories` registry, the server will ignore the field, and the data will be silently dropped upon submission. You must complete all parts sequentially!

> [!IMPORTANT]
> **Codebase Integrity & Local Verification:**
> Before carrying out any modifications described in this guide, **always verify these instructions and code snippets against the active, local files in your codebase**.
> * **Verify Exact Class & Tag Matches:** Stylesheets (`CSS/style.css`) and grid architectures can be updated over time. Make sure form layout grid containers (`grid grid-2`, `col-span-2`, etc.) and general control classes (`form-control`, `required`) match exactly.
> * **Front-end Script Dependencies:** The validation framework relies on shared assets in `JS/layout.js` (for general structure), `JS/validation.js` (which handles custom input filtering like `data-name-field`, `data-phone-field`, and `data-org-field`), and `JS/script.js` (which injects dynamic fields). Ensure these are imported correctly in `Contact_Us.html`.
> * **Backend validation dependencies:** Backend sanitization and security helpers (such as `post()`, `requireFields()`, and `enforceMaxLength()`) are loaded from `includes/validation.php`. Always verify their function signatures before introducing new fields.
> * **Never Assume Snapshot Ground-Truth:** Always double-check dynamic key strings (like `'other'`, `'equipment'`) and form field name markers (like `'department'`, `'message'`) across your actual, active files to prevent spelling mismatches.

> [!TIP]
> Looking to remove a field instead? Jump directly to [Removing a Field](#removing-a-field).

---

## Step-by-Step Instructions (Adding/Updating a Field)

The Contact Us form is split into two types of fields: **Common Fields** (which appear for all categories, such as First/Last Name and Email) and **Category-Specific Fields** (which slide into view depending on which gateway card is selected).

---

### Part 1: Update the Front-End Form Fields (HTML & JavaScript)

Choose **Case A** if you are updating a field shared by all inquiries, or **Case B** if you are updating a field specific to a single category.

#### Case A: Update Common Fields (Static HTML)

1. To get started, please open `Contact_Us.html` in your code editor.
2. Search for the main form tag with the ID `"contactForm"`. Inside, locate the common fields grid container:
   **Real code snippet:** `<div class="grid grid-2 gap-lg" style="margin-bottom: 25px;">`

   <p align="center">
     <img src="../../md_file_images/Contact_Form_Fields_%232.png" alt="Locating the Common Fields Grid Container" /><br/>
     <em>Locating the common fields grid container inside Contact_Us.html</em>
   </p>

3. Add a wrapper division element for your new input. You can customize the field layout properties using the guide table below:

| # | Parameter | Description | Class / Example |
|---|---|---|---|
| ① | Grid Column Wrap | Wrapper division tag that aligns the input. Use `<div>` for half-width or `<div class="col-span-2">` for full-width. | `<div class="col-span-2">` |
| ② | Label Class | Class on the `<label>` element. Standard is `block font-bold mb-1`. Add `required` to display a red asterisk. | `<label class="block font-bold mb-1 required">` |
| ③ | Input Class | Class on the `<input>`, `<select>`, or `<textarea>`. Must be exactly `form-control`. | `<input type="text" class="form-control" ...>` |
| ④ | Name Attribute | The unique identifier key used by PHP to extract the field. Must be lowercase `snake_case`. | `name="department"` |

4. Define your new label and input elements using our layout system.

   **Example Markup**
   ```html
   <div>
       <label class="block font-bold mb-1 required">Department / Group</label>
       <input type="text" name="department" class="form-control" placeholder="e.g. Physics Lab" required maxlength="100">
   </div>
   ```

   > [!WARNING]
   > **Common Fields bypass the `$categories` registry!**
   > Because Common Fields are hardcoded inside the static HTML of `Contact_Us.html` and are submitted by **every** inquiry category, they bypass the dynamic `$categories` registry mapping loop. If you add or modify a Common Field in the HTML, you cannot rely on the automatic registry loop. Instead, you must manually complete these three extra tasks inside `FormSubmission.php`:
   > 1. **Extract and validate globally:** Extract your new variable at the top of the file (e.g., `$department = post('department');`) and validate it using the validation helpers in `includes/validation.php`.
   > 2. **Update the email templates:** Manually insert matching table rows for your new field in both the **Lab Notification Email** (`$labBody`/`$labPlain`) and **User Confirmation Email** (`$userBody`/`$userPlain`) tables.
   > 3. **Sync with SharePoint:** Add the field mapping to the `$sp_List_fields` array at the bottom of the script to prevent it from being omitted from the SharePoint database logs.

---

#### Case B: Update Category-Specific Fields (Dynamic JS)

Dynamic fields are stored as HTML string templates inside `/JS/script.js` and injected on demand into `Contact_Us.html` when a category card is clicked.

1. To get started, please open `JS/script.js` in your code editor.
2. Search for the `fieldData` object declaration:
   **Real code snippet:** `const fieldData = {`

   <p align="center">
     <img src="../../md_file_images/Contact_Form_Fields_%233.png" alt="Locating the fieldData Object in script.js" /><br/>
     <em>Locating the fieldData object inside JS/script.js to declare dynamic fields</em>
   </p>

3. Find the target category key (e.g., `'other'`, `'equipment'`, `'billing'`) and locate its `fields` template literal.
4. Insert your new input markup inside the `fields` template literal. Maintain the same CSS styling conventions as Case A (using `.form-control` for inputs, `.block.font-bold.mb-1` for labels, and `.grid.grid-2.gap-lg` or `.form-stack` for container styling).
5. **Crucial Rule:** The input's `name` attribute is the exact key that the backend PHP processor uses to look up the data. Ensure it is unique, lowercase, and uses `snake_case`.

   **Example JS template block:**
   ```javascript
   'other': {
       title: "General Inquiry",
       desc: "How can we help you?",
       fields: `
           <div>
               <label class="block font-bold mb-1 required">Message / Question</label>
               <textarea name="message" rows="5" class="form-control" placeholder="Please describe your question or issue..." required></textarea>
           </div>
       `
   },
   ```

---

### Part 2: Register, Map, and Validate the Fields (Form & Field Validation)

Both client-side and server-side validation layers must be updated to keep your form secure, reliable, and user-friendly. Choose the appropriate validation structure depending on whether your field is static (Common) or dynamic (Category-Specific).

---

#### Case A: Registering & Validating Common Fields (Static / Global)

Since Common Fields appear statically inside `Contact_Us.html` for **every** inquiry and bypass the category loop in PHP, their validation rules are wired both automatically on the client side and manually in the global section of the backend.

##### 1. Client-Side Validation (JavaScript)
The form script inside `JS/script.js` automatically scans and applies basic validation constraints (such as `required` and `maxlength`) on form load. For specialized inputs, you can attach custom data attributes directly to your HTML element in `Contact_Us.html` to leverage pre-wired live validators:

*   **`data-name-field`:** Restricts inputs to standard letters, spaces, hyphens, and apostrophes (blocking numbers, special symbols, and emoji).
*   **`data-phone-field`:** Attaches an automatic US telephone layout mask: `(XXX) XXX-XXXX`.
*   **`data-org-field`:** Automatically blocks emoji and displays character-mashing alerts for keyboard spam.

##### 2. Server-Side Validation (PHP)
Because these fields are processed globally outside of the dynamic categories configuration block, you must add manual validation checks inside `FormSubmission.php`:

1. Open `FormSubmission.php` in your code editor.
2. Scroll to the global extraction block near the top where standard fields are fetched and validated (around line 310):
   **Real code snippet:**
   ```php
   $firstName    = post('first_name');
   $lastName     = post('last_name');
   $email        = post('email');
   ```

   <p align="center">
     <img src="../../md_file_images/Contact_Form_Fields_%235.png" alt="Locating the Global Extraction Block in PHP" /><br/>
     <em>Locating the global extraction and validation block inside FormSubmission.php</em>
   </p>

3. Extract your new variable using the `post()` sanitation helper:
   ```php
   $department   = post('department'); // Sanitizes and trims input
   ```
4. If your field is mandatory, add it to the `requireFields()` array:
   ```php
   requireFields(['first_name', 'last_name', 'email', 'category', 'department']);
   ```
5. Apply specific validation helper functions imported from `includes/validation.php` to secure the input:
   *   **Character Limits:** `enforceMaxLength('department', 100, 'Department');`
   *   **Free-Text Checks (Spam, HTML, Emoji):** `validateTextField('department', 'Department');`
   *   **Name-Format Checks:** `validateNameField('department', 'Department');`
   *   **Phone-Format Checks:** `validatePhoneFormat('department', 'Department');`

---

#### Case B: Registering & Validating Category-Specific Fields (Dynamic / Registry-Based)

Unlike other forms in the codebase, `FormSubmission.php` uses a completely automated and dynamic mapping engine for dynamic fields. You do not need to manually parse individual category-specific fields or write manual HTML email table rows. Instead, you simply register your new field key inside the PHP category configuration block.

1. Open `FormSubmission.php` in your code editor.
2. Search for the categories array declaration:
   **Real code snippet:** `$categories = [`

   <p align="center">
     <img src="../../md_file_images/Contact_Form_Fields_%234.png" alt="Locating the categories Array in PHP" /><br/>
     <em>Locating the categories array configuration inside FormSubmission.php</em>
   </p>

3. Locate the configuration array corresponding to your category (or search for its `'title'`).
4. Update the category configuration parameters using the guide table below to register your new field and its validation rules:

| Parameter | Type | Purpose | Validation Rules / Example |
|---|---|---|---|
| `'fields'` | Array | Displays the listed fields in the specific order in outgoing emails. | `['department', 'message']` |
| `'labels'` | Assoc Array | Maps field `name` keys to friendly labels displayed in emails. | `'department' => 'Department / Group'` |
| `'required'` | Array | Optional. Field keys that must be filled before submission (triggers backend required gates). | `['department']` |
| `'textFields'` | Array | Optional. Fields checked for keyboard mashing, HTML tags, and emoji. | `['department']` |
| `'wordLimited'` | Array | Optional. Textareas capped at 500 words. | `['message']` |

   **Example configuration update:**
   ```php
   'other' => [
       'title'       => 'General Inquiry',
       'required'    => ['message', 'department'], // 'department' is now required
       'textFields'  => ['message', 'department'], // 'department' will undergo spam checks
       'wordLimited' => ['message'],
       'fields'      => ['department', 'message'],  // 'department' will be emailed before the message
       'labels'      => [
           'department' => 'Department / Group',
           'message'    => 'Message',
       ],
   ],
   ```

5. **Optional - Map Dropdown Option Labels:**
   If your field is a `<select>` dropdown with custom machine-readable option values (e.g. `<option value="physics_dept">`), scroll up to the static `$map` array inside the `formatValue()` helper function:
   **Real code snippet:** `static $map = [`
   You can add a clean translation mapping (e.g. `'physics_dept' => 'Department of Physics'`) to translate machine keys into friendly email labels.
   > [!NOTE]
   > For fields not registered in the `$map` array, the system automatically falls back to replacing underscores with spaces and title-casing the value (e.g., `'engineering_dept'` becomes `'Engineering Dept'`).

---

## Removing a Field

To safely remove an existing field from the Contact Us form, choose the appropriate case based on the field type.

### Case A: Removing a Common Field (Static HTML)

1. Open `Contact_Us.html` in your code editor.
2. Locate the static form tag `<form id="contactForm">`.
3. Find the division element containing the target input and label (e.g. the phone field) and delete or comment out the block.

   <p align="center">
     <img src="../../md_file_images/Contact_Form_Fields_%236.png" alt="Removing a Common Field from HTML" /><br/>
     <em>Commenting out or deleting a common field block inside Contact_Us.html</em>
   </p>

4. Open `FormSubmission.php` in your code editor:
   * **Remove Global Extraction:** Remove the global extraction and validation lines corresponding to your field at the top of the file (e.g. `validatePhoneFormat('phone', 'Phone');`).
   * **Clean Email Layouts:** Remove the custom HTML `<tr>` row and the plain-text equivalent for that field inside the lab notification (`$labBody`/`$labPlain`) and user confirmation (`$userBody`/`$userPlain`) templates.
   * **Remove SharePoint Mapping:** Locate the `$sp_List_fields` array at the bottom of the script and delete the line mapping your removed field (e.g., `'Phone' => ...`).

---

### Case B: Removing a Category-Specific Field (Dynamic JS)

1. Open `JS/script.js` in your code editor.
2. Search for the `fieldData` object, locate your target category key, and delete the specific input wrapper element from the `fields` template literal.
3. Open `FormSubmission.php` in your code editor:
   * Locate the `$categories` registry array.
   * Find the target category key configuration block.
   * Remove the field name key from the `'fields'` array.
   * Remove the field key and its label mapping from the `'labels'` associative array.
   * Remove the field name key from the `'required'`, `'textFields'`, and `'wordLimited'` arrays (if present).
   * *(Optional)* If the field was a select dropdown, you can safely remove its options map inside the `formatValue()` helper's static `$map` array.

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

1. Open `Contact_Us.html` in your local environment browser.
2. Click on the category card you updated. Verify that the new layout flows elegantly without any uneven grid wraps.
3. Fill out the form completely, ensuring you input valid test details in your new field (or verifying that removed fields no longer appear), and submit the message.
4. Verify that the browser displays a green success confirmation window.
5. Check the receiving email inbox (or verify local server mail logs) to ensure the email is successfully generated and that your changes display accurate user-entered information under the correct column headers in the dynamic email table.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last Updated: May 22, 2026*
