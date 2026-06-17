Last Updated: 2026-05-27

# Email Rendering

Both email templates (lab notification and user confirmation) are built in `ServiceRequestSubmission.php`.
This page covers how field values get from `$_POST` into the email body — specifically the detail
table rows that show the submission specifics.

For contact fields (`first_name`, `last_name`, `email`, `phone`), see
[service-request-field-lifecycle.md](service-request-field-lifecycle.md) — they take a different path.

---

## Two Rendering Paths

| Path | Fields | How it works |
|---|---|---|
| **Contact section** | `first_name`, `last_name`, `email`, `phone` | Hardcoded HTML rows in the email template. Not configurable via `$services`. |
| **Detail rows loop** | Everything else | Driven by `$meta['fields']` and `$meta['labels']` in the `$services` registry. |

---

## The `$detailRows` Loop

**Ctrl+F to find it:** `foreach ($meta['fields'] as $field)`

```php
foreach ($meta['fields'] as $field) {
    if ($isPickup && in_array($field, $shippingOnlyFields, true)) {
        continue;  // skip shipping rows when delivery = pickup
    }

    $label = $meta['labels'][$field] ?? $field;       // fallback to raw key if label missing
    $raw   = trim($_POST[$field] ?? '');               // read raw — NOT via post()

    // Checkboxes (dropoff_confirm, usb_confirm) post a value only when checked.
    // Map to "Confirmed" / "Not confirmed" so the email doesn't show a blank.
    if ($field === 'dropoff_confirm' || $field === 'usb_confirm') {
        $formatted = !empty($raw) ? 'Confirmed' : 'Not confirmed';
    } else {
        $formatted = formatValue($raw);                // translate option values to readable text
    }

    $plainValue = $formatted !== '' ? $formatted : '—';    // plain-text version for AltBody
    $value      = $formatted !== '' ? htmlspecialchars($formatted, ENT_QUOTES, 'UTF-8') : '—';

    // Full inline styles omitted for brevity — copy them from an existing row in the source.
    $detailRows .= "<tr>
        <td>$label</td>
        <td>$value</td>
    </tr>";
}
```

**Why `$_POST[$field]` directly instead of `post()`:**
`post()` calls `clean()` which runs `htmlspecialchars()`. Then `formatValue()` would call
`htmlspecialchars()` again via its own escaping — double-encoding `&` into `&amp;amp;` in the email.
Reading raw from `$_POST` and calling `htmlspecialchars()` exactly once (after `formatValue()`) is
the rule. There is one call to `htmlspecialchars()` per field in this loop — no more.

---

## Adding a Field to the Email

To make a new field appear in the email detail table:

1. **Add to `$meta['fields']`** — the ordered list controls which fields appear and in what order:

   ```php
   // In $services['printing']['fields']:
   'fields' => [
       'affiliation', 'department', 'project_title',
       'your_new_field',        // ← add here, in the position you want it to appear
       'application_category', ...
   ],
   ```

2. **Add to `$meta['labels']`** — the human-readable column heading:

   ```php
   // In $services['printing']['labels']:
   'labels' => [
       'your_new_field' => 'Your Field Label',   // ← add here
       ...
   ],
   ```

   Without this entry, the raw PHP key (`your_new_field`) appears as the column heading.

3. **If the field is a dropdown or radio:** add its option values to `formatValue()` — see below.

> **Please double-check:** After adding a field, submit a test request and verify the field appears
> in the correct position in the email with the correct label and a readable value.

---

## `formatValue()` — Translating Raw Option Values

**Ctrl+F to find it:** `function formatValue`

HTML `<select>` and `<input type="radio">` elements post their raw `value=` attributes — short
machine-readable keys like `'prototype'` or `'pickup'`. `formatValue()` translates these to
the human-readable text that appears in the email.

```php
function formatValue(string $raw): string
{
    static $map = [
        'prototype'  => 'Prototype Development',
        'functional' => 'Functional Part',
        'pickup'     => 'Lab Pickup (Free)',
        'ship'       => 'Ship to Address',
        // ... many more entries
    ];
    if ($raw === '') return '';
    return $map[$raw] ?? ucwords(str_replace('_', ' ', $raw));
}
```

**The fallback** (`ucwords(str_replace('_', ' ', $raw))`) converts `my_option` to `My Option`.
This looks acceptable in the email but is not reliable — always add new option values to `$map`
explicitly rather than relying on the fallback.

**Adding a new dropdown option:**

1. Add the `<option>` to the HTML `<select>`:
   ```html
   <option value="new_value">New Display Text</option>
   ```

2. Add the mapping to `$map` in `formatValue()`:
   ```php
   'new_value' => 'New Display Text',
   ```

   Both changes are required. If you add the option to HTML but not to `$map`, the raw key
   (`new_value`) appears in the email.

---

## Conditional Shipping Rows

**Ctrl+F to find it:** `$isPickup`

```php
$isPickup = (post('delivery') === 'pickup');
$shippingOnlyFields = [
    'shipping_contact_name', 'shipping_speed',
    'shipping_address_line1', 'shipping_address_line2',
    'shipping_city', 'shipping_state', 'shipping_zip', 'shipping_country'
];
```

When the user chooses "Lab Pickup", all 8 shipping fields are skipped in the loop — no empty rows
appear in the email. When they choose "Ship to Address", all 8 are included.

These 8 field names are hardcoded in both the PHP skip-list above and in the PHP `requireFields()`
shipping validation block. If you rename a shipping field, update both locations.

---

## Plain-Text AltBody

Both emails also have an `AltBody` built from `$plainDetails` — a plain-text version of the same
field list, built in the same loop:

`$plainValue` is the unescaped version of the same value (`$formatted !== '' ? $formatted : '—'`) — plain text does not need HTML encoding, so a second variable is used rather than re-using the HTML-safe `$value`.

```php
$plainDetails .= "  $label: $plainValue\n";
```

This is required because email clients and spam filters expect `multipart/alternative` messages
(HTML + plain text together). A plain-text fallback also renders correctly in screen readers and
text-only clients.

The `AltBody` is built automatically alongside `$detailRows`. If a field is in `$meta['fields']`,
it will appear in both the HTML table and the plain-text body.

---

## Both Email Sends

Two separate `createMailer()` calls send two emails per submission:

| Email | Recipient | Purpose |
|---|---|---|
| Lab notification | `LAB_EMAIL` + `CC_LIST` (from `mpact_config.php`) | Internal notification. `addReplyTo()` is set to the submitter's email — staff can reply directly. |
| User confirmation | Submitter's `$email` | Shows the same `$detailRows` summary so the user sees exactly what was recorded. |

Both emails attach the uploaded files from PHP's temp path. The temp files are deleted when the
script exits — attachments must be added before the script finishes.

---

→ For how specific field types differ: [service-request-field-types.md](service-request-field-types.md)
→ For SharePoint column writes: see the SharePoint integration guide (not yet documented in this knowledge base)
→ For email-related breakpoints: see the responsive breakpoints guide (not yet documented in this knowledge base)
