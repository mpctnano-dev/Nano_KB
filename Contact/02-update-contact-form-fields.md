# Update Contact Form Fields

**One-line description:** Add or modify input fields in the main Contact form.
**When you'd do this:** You need to collect a new piece of information from general inquiries (e.g., "Department" or "Company Size").
**Difficulty:** Careful
**Estimated time:** 20 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `Contact_Us.html` | The visual HTML form where the field is added. |
| `FormSubmission.php` | The backend script that processes the form submission and emails the lab staff. |

> [!WARNING]
> If you add a field to the HTML but forget to update the PHP file, the data entered by the user will be silently dropped and will never reach the lab staff's email.

## Steps

### Part 1: Update the HTML Form

1. Open `Contact_Us.html`.
2. Locate the form block by searching for `<form id="contactForm" ...>`.
3. Add a new `pf__group` div for your field. 
4. Ensure the `<input>`, `<select>`, or `<textarea>` has a unique `name` attribute.

```html
<!-- Example of adding a new text field for 'Department' -->
<div class="pf__group">
    <label class="pf__label" for="contactDepartment">Department/Organization</label>
    <input type="text" id="contactDepartment" name="department" class="pf__input" placeholder="e.g. Physics Dept">
</div>
```

### Part 2: Update the PHP Processor

1. Open `FormSubmission.php`.
2. Locate the section where variables are extracted from `$_POST`.
3. Add a line to sanitize and store your new field.

```php
// Existing code...
$email = sanitize_input($_POST['email'] ?? '');
// Add your new field:
$department = sanitize_input($_POST['department'] ?? 'Not specified');
```

4. Scroll down to where the HTML email body (`$mail->Body`) is constructed.
5. Add your new field to the table or list so it appears in the email sent to the lab staff.

```php
// Inside the $mail->Body string construction...
"<tr><th>Email:</th><td>{$email}</td></tr>" .
"<tr><th>Department:</th><td>{$department}</td></tr>" . // Your new field
```

## Verify It Worked
*   Load `Contact_Us.html` in your local environment.
*   Fill out the form, including your new field, and submit it.
*   Check the receiving email inbox (or test inbox) to ensure the new field appears correctly formatted in the email body.

---
*After making this change and deploying, update this how-to page if anything about the process changed.*
*Last reviewed: 2026-05-15*
