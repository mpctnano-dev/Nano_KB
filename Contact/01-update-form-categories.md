# Update Form Categories

**Summary:** Add, remove, or modify the interactive inquiry categories displayed as selectable cards on the Contact Us page, and register their fields in both front-end scripts and back-end processors.

**Trigger:** When the laboratory introduces a new operational division (such as "Student Internships") or re-evaluates the key inquiry topics available to site visitors.

**Difficulty:** Coordinate-first

**Estimated Time:** 30 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `Contact_Us.html` | The contact gateway document hosting the visual category selection cards. |
| `JS/script.js` | The client-side script containing the `fieldData` database that defines and injects custom inputs when a category card is clicked. |
| `FormSubmission.php` | The backend PHP mail processor that registers the valid categories and maps input names to formatted email labels. |

> [!WARNING]
> **Keep all three layers synchronized!**
> The contact portal operates as a unified three-part system. If you add a card in the HTML but forget to define its input fields in JavaScript, clicking the card will cause an injection error and fail to load. If you update the front-end fields but forget to register them in the PHP script, the server will block the form submission as invalid. You must complete all parts sequentially!

---

## Step-by-Step Instructions

### Part 1: Add the Selection Card (HTML)

1. To get started, please open `Contact_Us.html` in your code editor.
2. Please search for the gateway grid division tag with the ID `"gatewayGrid"`. Inside, you will find:
   `<div class="grid grid-4 gap-md" id="gatewayGrid">`
3. Copy an existing card division block and paste it inside the container in your desired order.
4. Each category is represented by a card division tag with the class name `"gateway-card"`:
   `<div class="gateway-card" onclick="selectCategory('internships', this)">`
5. You can customize the card properties using these guidelines:
   * **The Category ID:** Update the unique ID key passed into the `selectCategory` click handler (for example, change `'report'` to `'internships'`).
   * **The Icon:** Swap the icon symbol tag (`<i>`) inside the card:
     `<i class="fas fa-user-grad"></i>`
   * **Title & Description:** Modify the header (`<h4>`) and paragraph description (`<p>`) tags.

---

### Part 2: Let's Define Form Inputs (JavaScript)

Now, we must define the specific text inputs that slide into view when your card is selected.

1. To get started, please open `JS/script.js` in your code editor.
2. Please search for the key-value database variable. Inside, you will find:
   `const fieldData = {`
3. Insert a new category key matching the *exact* ID value you declared in Part 1 (e.g. `'internships'`). Custom fields are written using template literals in Javascript:
   ```javascript
   'internships': {
       title: "Internship Opportunities",
       desc: "Apply for student positions and research associate roles.",
       fields: `
           <div>
               <label class="block font-bold mb-1 required">Major</label>
               <input type="text" name="internship_major" class="form-control" required>
           </div>
       `
   },
   ```
4. **Crucial Rule:** Every input or select tag inside the HTML `fields` template literal MUST have a unique `name` attribute. This is how the server tracks the value.

---

### Part 3: Let's Register and Map the Fields (PHP)

Finally, we must tell the backend processor to validate these fields and map them into the notification email sent to lab staff.

1. To get started, please open `FormSubmission.php` in your code editor.
2. Please search for the categories array declaration. Inside, you will find:
   `$categories = [`
3. Add a new configuration array block matching your category ID exactly:
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

---

## Removing a Category

To safely remove an inquiry category (such as "Feedback"), you only need to delete or comment out its visual `<div class="gateway-card">` block inside `Contact_Us.html`. The associated JS and PHP array definitions can safely remain in the codebase; they will simply never trigger because the user has no button to select them.

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
*Last reviewed: May 19, 2026*
