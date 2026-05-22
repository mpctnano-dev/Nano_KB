Last Updated: 2026-05-21

# Complete Field Lifecycle: First Name

This page traces a single field — `first_name` — through all five layers of the ServiceRequest
pipeline, from the HTML element to the SharePoint column. Every layer is shown with the actual
code from the live codebase.

Read [service-request-how-it-works.md](service-request-how-it-works.md) first if you have not
seen the 5-layer overview.

**Why First Name as the example:**
`first_name` is a contact field — it follows a slightly different path than most fields (it skips
the `$detailRows` loop and goes directly into the hardcoded email header). This makes it the most
instructive example: it shows both the standard rules and the contact-field exception in one walkthrough.
For how regular detail fields work (fields that do go through the loop), see
[service-request-email.md](service-request-email.md).

---

## Layer 1 — HTML Definition

**Ctrl+F to find it:** `id="p-fname"`

```html
<input type="text" id="p-fname" name="first_name" class="sr-input"
       required maxlength="25" placeholder="Alex">
```

| Attribute | Purpose |
|---|---|
| `name="first_name"` | This exact string becomes the `$_POST` key. Every layer downstream references this name. |
| `id="p-fname"` | Used by `<label for="p-fname">` and by `getFieldLabel()` in the inline JS to retrieve the display name for error messages. |
| `required` | Triggers `el.validity.valid` check in `getInvalidFields()`. The browser will not submit the form if this is missing, and JS will highlight the field before the fetch fires. |
| `maxlength="25"` | Browser-enforced hard cap. PHP mirrors this with `enforceMaxLength('first_name', 25)`. Both must be updated together if the limit changes. |
| `placeholder="Alex"` | UX hint only — has no effect on validation or submission. |

---

## Layer 2 — JS Pre-Submit Validation

**Ctrl+F to find it:** `getInvalidFields`

The inline `<script>` at the bottom of `ServiceRequest.html` runs `getInvalidFields(form)` on every
submit attempt. For `first_name` it performs two checks:

**Check 1 — Required and maxlength (HTML validity API)**
```js
form.querySelectorAll('[required], [min], [max]').forEach(el => {
    if (el.closest('.sr-hidden')) return;   // skip hidden conditional fields
    if (el.validity && el.validity.valid) return;
    // ... add to invalid list, outline in red
});
```
`el.validity.valid` is false if the field is empty (`required`) or over the character limit (`maxlength`).

**Check 2 — Name pattern (MPCT.Validation)**
```js
const V = window.MPCT && window.MPCT.Validation;

// The inline script checks name fields using:
if (!V.isValidName(el.value)) {
    flagEl(el, '(letters, spaces, hyphens, apostrophes only)');
}
```
`V.isValidName()` uses `NAME_RE = /^[\p{L}\s'\-\.]+$/u` — Unicode letters, spaces, hyphens,
apostrophes, and dots. Emoji are caught separately by `EMOJI_RE`.

If either check fails:
- The input gets `outline: 2px solid var(--nau-red, #c0392b)`
- The outline clears automatically on the next `input` or `change` event
- A list of failed field labels is shown in the `.bk-feedback` div above the submit button
- The `fetch()` to `ServiceRequestSubmission.php` is not called

**Serialization — how the value reaches PHP:**
```js
const formData = new FormData(form);
formData.append('service_type', serviceType);  // 'printing', 'laser', or 'scanning'

const response = await fetch('ServiceRequestSubmission.php', {
    method: 'POST',
    body: formData
});
```
`new FormData(form)` collects all named inputs including `first_name`. `service_type` is the only
field appended programmatically — it does not exist in the HTML form.

---

## Layer 3 — PHP Validation

**Ctrl+F to find it:** `validateNameField('first_name'`

`ServiceRequestSubmission.php` performs three checks on `first_name` in this order:

**Check 1 — Required field**
```php
requireFields($meta['required'], $meta['labels']);
// $meta['required'] for printing includes 'first_name'
```
`requireFields()` calls `respond(false, 'Missing required field: First Name.')` and exits if the
field is empty or whitespace-only after trimming. Nothing else runs after this.

**Check 2 — Name character rules**
```php
validateNameField('first_name', 'First Name');
```
From `includes/validation.php` — checks the same `NAME_RE` pattern as JS:
```php
function validateNameField(string $field, string $label): void
{
    $val = trim($_POST[$field] ?? '');
    if ($val === '') return;
    if (containsEmoji($val)) {
        respond(false, "$label cannot contain emoji.");
    }
    if (!preg_match('/^[\p{L}\s\'\-\.]+$/u', $val)) {
        respond(false, "$label should contain letters, spaces, hyphens, or apostrophes only.");
    }
}
```

**Check 3 — Maximum length**
```php
enforceMaxLength('first_name', 25);
```
Uses `mb_strlen()` so multi-byte UTF-8 characters count as one character, matching the browser's
`maxlength` behavior.

> **Please double-check:** If you change the 25-char limit, update BOTH `maxlength="25"` in
> `ServiceRequest.html` AND `enforceMaxLength('first_name', 25)` in `ServiceRequestSubmission.php`.

---

## Layer 4 — Email Rendering

**Ctrl+F to find it:** `$firstName = post('first_name')`

`first_name` is a **contact field** — it does not go through the `$detailRows` loop. Instead:

```php
$firstName = post('first_name');   // clean() → trim + htmlspecialchars
$lastName  = post('last_name');
$fullName  = trim($firstName . ' ' . $lastName);
```

`post()` calls `clean()`, which trims the value and runs `htmlspecialchars(..., ENT_QUOTES, 'UTF-8')`.
This HTML-encodes characters like `'` as `&#039;` — safe for embedding in the email HTML.

`$firstName` is then used in two places in both email templates:

**Lab notification email — Contact Information table (hardcoded row):**
```php
<td>Name</td>
<td>' . $fullName . '</td>
```

> Note: This is a simplified excerpt — the actual `<td>` elements carry extensive inline styles.
> Use `Ctrl+F: $fullName` in `ServiceRequestSubmission.php` to find the exact source lines.

**User confirmation email — greeting line:**
```php
<h2>Thank you, ' . $firstName . '!</h2>
```

Note: `first_name` is NOT listed in `$services['printing']['fields']`. Adding it there would cause
it to appear twice in the email — once in the hardcoded Contact section and again in the detail table.

---

## Layer 5 — SharePoint

**Ctrl+F to find it:** `'FirstName'`

```php
$sp_List_fields = [
    'Title'     => $meta['title'],
    'FirstName' => htmlspecialchars_decode($firstName, ENT_QUOTES),
    // ...
];
```

| Detail | Value |
|---|---|
| SharePoint column name | `FirstName` — must match the internal column name exactly (case-sensitive) |
| Source variable | `$firstName` (set at Layer 4) |
| Why `htmlspecialchars_decode()` | `post()` HTML-encoded the value for email safety. SharePoint stores and renders plain text — inserting `O&#039;Brien` would show entity codes in the list view. The decode reverses the encoding before insert. |

The record is written to the SharePoint list defined by `SERVICES_LIST_NAME` in `mpact_config.php`
via a POST to `/sites/{siteId}/lists/{listId}/items` using the Microsoft Graph API.

---

## Summary: What Must Be True for First Name to Work End-to-End

| Layer | Requirement |
|---|---|
| HTML | `name="first_name"`, `required`, `maxlength="25"` present |
| JS | `getInvalidFields()` must find it via `[required]` selector; `isValidName()` must be called for it |
| PHP required | `'first_name'` must be in `$services['printing']['required']` |
| PHP content | `validateNameField('first_name', 'First Name')` and `enforceMaxLength('first_name', 25)` must be called |
| Email | `$firstName = post('first_name')` must be set before the email body strings are built |
| SharePoint | `'FirstName' => htmlspecialchars_decode($firstName, ENT_QUOTES)` must be in `$sp_List_fields` |

If any row in this table is missing or wrong, that layer silently fails for this field.

---

