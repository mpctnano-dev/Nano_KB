Last Updated: 2026-05-27

# Field Type Reference

This page shows what is **different** at each layer for each field type compared to the plain
`<input type="text">` baseline. If a layer is not mentioned for a type, it works identically
to the text baseline.

For the complete text-input walkthrough (the baseline), see
[service-request-field-lifecycle.md](service-request-field-lifecycle.md).

For how fields get into the email detail table, see [service-request-email.md](service-request-email.md).

---

## `text` — Baseline

Fully documented in [service-request-field-lifecycle.md](service-request-field-lifecycle.md).
All other types below are described as diffs against this.

---

## `email`

**Example field:** `email` (`id="p-email"`, `name="email"`)

**Layer 1 (HTML):** Use `type="email"`. The browser validates format natively on submit.
```html
<input type="email" id="p-email" name="email" class="sr-input" required maxlength="50">
```

**Layer 2 (JS):** `getInvalidFields()` calls `MPCT.Validation.isEmail(el.value)` on `input[type="email"]` elements:
```js
Validation.EMAIL_RE = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
Validation.isEmail = function (v) {
    v = (v || '').trim();
    return v === '' || Validation.EMAIL_RE.test(v);
};
```

**Layer 3 (PHP):**
```php
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    respond(false, 'Invalid email address.');
}
```
Note: `email` is a contact field — this validation runs outside the `$services` config block.

**Layer 4 (Email):** Hardcoded in the Contact Information section (contact field — not in `$detailRows` loop).

**Layer 5 (SharePoint):** Written as `'Email' => htmlspecialchars_decode($email, ENT_QUOTES)`.

---

## `tel` — Phone Number

**Example field:** `phone` (`id="p-phone"`, `name="phone"`)

**Layer 1 (HTML):**
```html
<input type="tel" id="p-phone" name="phone" class="sr-input"
       placeholder="(555) 123-4567" inputmode="numeric" maxlength="14">
```
`type="tel"` on mobile shows a numeric keyboard. `maxlength="14"` matches the JS phone mask format
`(XXX) XXX-XXXX`. This field is **optional** — `required` is absent.

**Layer 2 (JS):** A phone mask input listener formats digits as `(XXX) XXX-XXXX` in real time
(Ctrl+F: `p-phone` in the inline script). Non-digit keystrokes are blocked.

**Layer 3 (PHP):**
```php
validatePhoneFormat('phone', 'Phone');
$phone = mb_substr(post('phone'), 0, 25);
```
`validatePhoneFormat()` accepts digits, spaces, parens, hyphens, dots, and a leading `+`. Empty
values pass — phone is optional. The 25-char cap is a safety rail, not a user-facing limit.

**Layer 4 (Email):** Contact field — hardcoded row. Displays `—` if empty:
```php
<td>' . ($phone ?: '—') . '</td>
```

**Layer 5 (SharePoint):** `'PhoneNumber' => htmlspecialchars_decode($phone, ENT_QUOTES)`.

---

## `number`

**Example fields:** `print_size_length`, `quantity`, `filament_estimate`

**Layer 1 (HTML):**
```html
<!-- Dimension with decimal allowed -->
<input type="number" id="p-size-l" name="print_size_length" class="sr-input"
       required min="1" max="325" step="any" placeholder="1 – 325">

<!-- Whole number only -->
<input type="number" id="p-qty" name="quantity" class="sr-input"
       required min="1" max="9999" step="1" value="1">
```
`step="any"` allows decimals. `step="1"` restricts to integers at the browser level.

**Layer 2 (JS):** `getInvalidFields()` checks `[min]` and `[max]` via `el.validity.valid`.

**Layer 3 (PHP):**
```php
// For dimensions (decimal allowed):
validateNumericRange('print_size_length', 1, 325, 'Print Length (mm)');

// For whole-number fields, add this after validateNumericRange:
validateNumericRange('quantity', 1, 9999, 'Quantity');
validateInteger('quantity', 'Quantity');
```
`validateInteger()` rejects `1.5` but accepts `1` or `1.0`.

> **Please double-check:** `min`/`max` in HTML and the second/third arguments to
> `validateNumericRange()` in PHP must be the same values.

**Layer 4 (Email):** Goes through the `$detailRows` loop. `formatValue()` has no entry for numeric
values — the raw number is displayed as-is.

---

## `date`

**Example field:** `deadline` (`id="p-deadline"`, `name="deadline"`)

**Layer 1 (HTML):**
```html
<input type="date" id="p-deadline" name="deadline" class="sr-input" required>
```
The inline JS sets `min` and `max` attributes dynamically to today and today+6 months respectively,
so the browser date picker restricts the selectable range:
```js
// Ctrl+F: p-deadline  in the inline script
const today = new Date().toISOString().split('T')[0];
deadlineInput.min = today;
// max calculated as +6 months
```

**Layer 2 (JS):**
```js
Validation.checkDateInRange = function (v) {
    // Returns null on success, or an error reason string
    // Checks: valid YYYY-MM-DD format, not in past, within 6 months
};
```
For the full implementation, search `Ctrl+F: checkDateInRange` in `JS/validation.js`. The function returns `null` on success or a reason string on failure; `getInvalidFields()` calls it and passes the error reason to the field's error message.

**Layer 3 (PHP):**
```php
validateDateInRange('deadline', 'Requested Completion Date');
```
From `includes/validation.php` — same rules as JS: valid Y-m-d format, not past, within 6 months.

**Layer 4 (Email):** Goes through `$detailRows`. No `formatValue()` entry — date string displays as-is (e.g. `2026-08-15`).

---

## `select` — Dropdown

**Example fields:** `application_category`, `material`, `delivery`

**Layer 1 (HTML):**
```html
<select id="p-appcat" name="application_category" class="sr-select" required>
    <option value="" disabled selected>Select primary use…</option>
    <option value="prototype">Prototype Development</option>
    <option value="functional">Functional Part</option>
    <!-- ... -->
</select>
```
The `value=` on each `<option>` is the POST key — the text between tags is display-only.

**Layer 2 (JS):** `getInvalidFields()` checks `el.validity.valid` — a `<select>` with `required`
is invalid if the selected option has `value=""`.

**Layer 3 (PHP):** No special validation beyond `requireFields()` for required dropdowns. PHP reads
the raw `value=` from `$_POST`.

**Layer 4 (Email):** The raw `value=` goes through `formatValue()`. **If the option value is not in
`$map`, the fallback shows a ucwords version of the key.**

Adding a new option requires two changes:

1. Add `<option>` to the HTML `<select>` in `ServiceRequest.html`
2. Add the value mapping to `formatValue()` in `ServiceRequestSubmission.php`:
   ```php
   'new_value' => 'Human-Readable Label',
   ```

> **Please double-check:** Test the new option by submitting a request with it selected and
> verifying the email shows the human-readable label, not the raw value.

---

## `textarea`

**Example fields:** `project_abstract`, `notes`

**Layer 1 (HTML):**
```html
<textarea id="p-abstract" name="project_abstract" class="sr-textarea"
          rows="3" required maxlength="2500" data-max-words="500"></textarea>
```
`data-max-words="500"` is a custom attribute — the browser ignores it, but JS and PHP both read it.
`maxlength="2500"` is the character cap (browser-enforced). Both limits apply independently.

**Layer 2 (JS):**
```js
form.querySelectorAll('textarea[data-max-words]').forEach(el => {
    const maxWords = parseInt(el.dataset.maxWords, 10);
    if (V.wordCount(el.value) > maxWords) {
        flagEl(el, `(over ${maxWords}-word limit)`);
    }
});
```
A live word counter is also shown below the textarea (updates on every keystroke).

**Layer 3 (PHP):**
```php
validateTextField('project_abstract', 'Project Abstract');  // no HTML, no emoji, no mashing
enforceWordLimit('project_abstract', 500, 'Project Abstract');
enforceMaxLength('project_abstract', 2500);
```

> **Please double-check:** `data-max-words` in HTML and the second argument to `enforceWordLimit()`
> in PHP must be the same number.

**Layer 4 (Email):** Goes through `$detailRows`. `formatValue()` has no entry for textarea values — the raw text displays as-is. Long values wrap naturally in the email table cell.

---

## `radio` group — Color Swatches

**Example field:** `color` (printing form)

**Layer 1 (HTML):** Individual `<input type="radio">` elements share `name="color"`. No `id=` on
individual inputs — the label wraps the input instead of using `for=`:
```html
<label class="sr-color-option">
    <input type="radio" name="color" value="black" required>
    <span class="sr-color-dot" style="background:#1A1A1A;"></span>
    <span class="sr-color-name">Black</span>
</label>
```
`required` is only on the first radio in the group — the browser treats the entire name group
as required when any one of them has `required`.

**Layer 2 (JS):** `getInvalidFields()` handles radio groups specially:
```js
if (el.type === 'radio') {
    if (seenRadioNames.has(el.name)) return;  // only process each name group once
    seenRadioNames.add(el.name);
    const grid = el.closest('.sr-color-grid');
    if (grid) highlightEl = grid;              // outline the whole color grid, not one dot
}
```
This is why `required` appears only on the first radio input — if every radio in the group carried `required`, `getInvalidFields()` would add the same group to the invalid list multiple times.

**Layer 3 (PHP):** `requireFields()` checks `color` in `$meta['required']` as normal — radio
groups POST their selected `value=` just like a text input.

**Layer 4 (Email):** The selected `value=` (e.g. `'light_green'`) goes through `formatValue()`:
```php
'light_green' => 'Light Green',
'dark_gray'   => 'Dark Gray',
```
Adding a new color swatch requires:
1. A new `<label class="sr-color-option">` block in the HTML with `name="color"` and a `value=`
2. A new entry in `formatValue()` if the value contains underscores or is not ucwords-friendly

---

## `file` upload

**Example field:** `files[]` (printing form) — `name="files[]"` with `multiple`

**Layer 1 (HTML):** The visible upload UI is a `<div class="sr-file-upload" data-required-upload>`.
The actual file input is hidden inside it:
```html
<div class="sr-file-upload" id="p-file-box" data-required-upload>
    <input type="file" id="p-file" name="files[]"
           accept=".stl,.3mf,.obj,..." multiple style="display:none;">
</div>
```
`data-required-upload` on the wrapper div (not the input) drives the JS required check.
`name="files[]"` with `multiple` creates a PHP `$_FILES['files']` array.

**Layer 2 (JS):** The required check uses `box._getFileCount()` — a closure maintained by
`setupFileUpload()`:
```js
form.querySelectorAll('[data-required-upload]').forEach(box => {
    const fileCount = box._getFileCount ? box._getFileCount() : fileInput?.files.length ?? 0;
    if (fileCount === 0) {
        // outline box, add to invalid list
    }
});
```

**Layer 3 (PHP):**
```php
$uploadedFiles  = normalizeUploadFiles($meta['uploadField']);  // flatten $_FILES shape
// For printing: also enforce at least one file
if ($serviceType === 'printing' && empty($uploadedFiles)) {
    respond(false, '3D model files are required...');
}
$validatedFiles = validateUploads($uploadedFiles, $meta['allowedExtensions']);
// validateUploads checks: error code, file size, total size, extension allowlist, magic-byte MIME
```

**Layer 4 (Email):** Files do NOT go through the `$detailRows` loop. An "Uploaded Files" row is
appended after the loop as an HTML `<ul>` list (or plain dash if no files):
```php
$detailRows .= "<tr><td>Uploaded Files</td><td>$uploadedFilesDisplay</td></tr>";
```

**Layer 5 (SharePoint):** Files go to a Drive folder (not the list). See the SharePoint integration guide (not yet documented in this knowledge base).

> **Warning:** `$meta['uploadField']` must be the bare field name without `[]`. For printing,
> `uploadField => 'files'` matches `name="files[]"` — PHP strips the `[]`. If you set
> `uploadField => 'files[]'` (with brackets), `normalizeUploadFiles()` will not find any files
> and the upload will silently fail.

---

## Conditional block — Shipping fields

**Example fields:** `shipping_contact_name`, `shipping_address_line1`, etc.

These fields are wrapped in a div that is hidden by default:

**Layer 1 (HTML):**
```html
<div id="p-ship-fields" class="sr-hidden">
    <input name="shipping_contact_name" ...>
    <!-- etc. -->
</div>
```

**Layer 2 (JS):** `getInvalidFields()` skips any field inside `.sr-hidden`:
```js
if (el.closest('.sr-hidden')) return;
```
The `toggleShipping()` function shows/hides the block when the delivery select changes:
```js
deliverySelect.addEventListener('change', toggleShipping);
```

**Layer 3 (PHP):**
```php
if (post('delivery') === 'ship') {
    requireFields([
        'shipping_contact_name', 'shipping_speed',
        'shipping_address_line1', 'shipping_city',
        'shipping_state', 'shipping_zip'
    ]);
    enforceMaxLength('shipping_contact_name', 100);
    // ... additional length and format checks
}
```
Shipping fields are only required when `delivery === 'ship'`. Adding a new shipping field means
adding it here inside the `if` block, not to the main `requireFields()` call.

**Layer 4 (Email):** `$isPickup` controls whether shipping rows appear in `$detailRows`:
```php
if ($isPickup && in_array($field, $shippingOnlyFields, true)) { continue; }
```
If you add a new shipping field, add its name to `$shippingOnlyFields` so it is skipped for
pickup deliveries.

---

→ Complete worked example: [service-request-field-lifecycle.md](service-request-field-lifecycle.md)
→ Email rendering details: [service-request-email.md](service-request-email.md)
→ SharePoint column writes: see the SharePoint integration guide (not yet documented in this knowledge base)
