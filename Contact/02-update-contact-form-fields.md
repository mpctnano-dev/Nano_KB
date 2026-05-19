# Update Contact Form Fields

**Summary:** Add, remove, or modify input fields in the general inquiry form on the Contact Us page and map them to the backend mailer.

**Trigger:** When the lab needs to collect new information (such as "Department" or "Company Size") from visitors submitting general inquiries.

**Difficulty:** Careful

**Estimated Time:** 20 minutes

---

## Target Files

| File | Purpose in this task |
|---|---|
| `Contact_Us.html` | The public HTML contact document containing the user-facing form inputs. |
| `FormSubmission.php` | The backend PHP mail processor that extracts form data, validates inputs, and formats emails for the lab staff. |

> [!WARNING]
> **Map both layers!**
> If you add an input field in the HTML contact form but forget to register and extract it in the backend PHP script, the data entered by the user will be silently dropped upon submission and will never reach the lab staff's inbox.

---

## Step-by-Step Instructions

### Part 1: Update the HTML Form

1. To get started, please open `Contact_Us.html` in your code editor.
2. Please search for the main form tag with the ID `"contactForm"`. Inside, you will find:
   `<form id="contactForm" ...>`
3. Locate the appropriate section within the form where you wish to insert the new input.
4. Add a new division tag with the class name `"pf__group"` to wrap your text input and label:
   `<div class="pf__group">`
5. Inside this division, define a descriptive label tag (`<label>`) and a unique input tag (`<input>`):
   ```html
   <div class="pf__group">
       <label class="pf__label" for="contactDepartment">Department/Organization</label>
       <input type="text" id="contactDepartment" name="department" class="pf__input" placeholder="e.g. Physics Dept">
   </div>
   ```
   * **Crucial Rule:** The input's `name` attribute (here, `"department"`) is the exact key that the backend PHP processor uses to look up the data. Ensure it is unique and uses clean, lowercase snake_case naming.

---

### Part 2: Register the Field in the PHP Processor

Now that the HTML is capturing the data, we must tell PHP to extract and email it.

1. To get started, please open `FormSubmission.php` in your code editor.
2. Scroll to the sanitization block where variables are pulled from the global `$_POST` array:
   `// Extract and sanitize form inputs`
3. Add a line to sanitize and store your new field. Ensure that the key inside `$_POST` matches the exact `name` attribute from your HTML input:
   ```php
   $email = sanitize_input($_POST['email'] ?? '');
   $department = sanitize_input($_POST['department'] ?? 'Not specified'); // Your new field mapping
   ```
4. Please scroll down to where the HTML email body (`$mail->Body`) is constructed for the notification email.
5. Insert a new row in the HTML email table to output your new variable clearly to the lab staff:
   ```php
   // Inside the $mail->Body string construction...
   "<tr><th>Email:</th><td>{$email}</td></tr>" .
   "<tr><th>Department:</th><td>{$department}</td></tr>" . // Your new email line
   ```

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
2. Fill out the form completely, ensuring you input valid details in your new field, and submit the message.
3. Check the receiving email inbox (or verify local server mail logs) to ensure the email is successfully generated and that your new field displays accurate user-entered information under the correct column header.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!
*Last reviewed: May 19, 2026*
