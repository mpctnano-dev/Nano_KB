# Update Form Categories

**One-line description:** Add, remove, or modify the inquiry categories ("cards") on the Contact Us page.
**When you'd do this:** The lab creates a new department (e.g., "Internships") or needs to collect entirely different information for an existing category.
**Difficulty:** Complex
**Estimated time:** 30 minutes

## Files You'll Touch

| File | Purpose in this task |
|---|---|
| `Contact_Us.html` | The visual cards the user clicks to choose a category. |
| `JS/script.js` | The JavaScript `fieldData` object that defines the HTML form fields injected when a card is clicked. |
| `FormSubmission.php` | The backend script's `$categories` array that tells the server how to validate and email the submitted fields. |

> [!WARNING]
> The contact form uses a 3-part system. If you add a category to the HTML but forget to update the JS, the form won't load. If you update the JS but forget the PHP, the server will reject the submission. **You must keep all three files in sync.**

## Steps: Adding a New Category

### Part 1: Add the Visual Card (HTML)

1. Open `Contact_Us.html`.
2. Locate the `<div class="grid grid-4 gap-md" id="gatewayGrid">`.
3. Copy an existing `<div class="gateway-card">` block.
4. Paste it where you want it to appear in the grid.
5. Update the `onclick` handler with your new unique category ID (e.g., `onclick="selectCategory('internships', this)"`).
6. Update the icon (`<i class="...">`), title, and description text.

### Part 2: Add the Form Fields (JavaScript)

1. Open `JS/script.js`.
2. Locate the `const fieldData = {` object (around line 337).
3. Add a new block using your exact category ID from Step 1:
   ```javascript
   'internships': {
       title: "Internship Opportunities",
       desc: "Apply for student positions.",
       fields: `
           <div>
               <label class="block font-bold mb-1 required">Major</label>
               <input type="text" name="internship_major" class="form-control" required>
           </div>
       `
   },
   ```
4. Ensure every `<input>` or `<select>` inside your `fields` HTML has a unique `name` attribute.

### Part 3: Register the Category (PHP)

1. Open `FormSubmission.php`.
2. Locate the `$categories = [` array (around line 191).
3. Add a new block matching your category ID:
   ```php
   'internships' => [
       'title'  => 'Internship Opportunities',
       'required' => ['internship_major'], // Must match the 'name' attributes
       'textFields' => ['internship_major'], // Which fields get anti-spam text checks
       'wordLimited' => [], // Which fields get a 500-word limit
       'fields' => ['internship_major'], // The exact order they appear in the email
       'labels' => [
           'internship_major' => 'Student Major', // How it looks in the email
       ],
   ],
   ```

## Steps: Removing a Category

To safely remove a category (like "Report an Issue"), simply delete its `<div class="gateway-card">` block from `Contact_Us.html`. 
The JavaScript and PHP configurations can remain in the codebase harmlessly—they just won't ever be triggered because the user has no button to click.

## Verify It Worked
1. Load `Contact_Us.html` locally.
2. Click your new category card. Verify the correct fields slide into view.
3. Fill out the form and submit it.
4. Verify the success message appears and the resulting email contains the correct data and labels.

---
*After making this change and deploying, update this how-to page if anything about the process changed.*
*Last reviewed: 2026-05-15*
