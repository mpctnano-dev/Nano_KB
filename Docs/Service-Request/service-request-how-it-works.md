Last Updated: 2026-05-21

# How ServiceRequest.html Works

`ServiceRequest.html` contains three independent service request forms — 3D Printing, Laser Structuring,
and 3D Scanning — all backed by one PHP handler: `ServiceRequestSubmission.php`. When a user submits
any of these forms, their data travels through five layers before it lands anywhere permanent. If any
layer is misconfigured, data fails silently at that layer — no error appears to the user or in the browser console.

---

## The 5-Layer Lifecycle

Every field in every form follows this path in order:

| # | Layer | Location | What it does |
|---|---|---|---|
| 1 | **HTML definition** | `ServiceRequest.html` | Declares the field: `name=` (the POST key used by everything downstream), `id=` (JS handle), and native constraints: `maxlength`, `min`, `max`, `required` |
| 2 | **JS pre-submit validation** | Inline `<script>` in `ServiceRequest.html`; shared primitives in `JS/validation.js` | Runs before the network request. `getInvalidFields()` checks HTML validity constraints and calls `MPCT.Validation` helpers for name patterns, email format, word count, keyboard mashing, and HTML injection. Invalid fields are outlined in red. Nothing reaches the server until this passes. |
| 3 | **PHP validation** | `ServiceRequestSubmission.php` + `includes/validation.php` | Server-side mirror of Layer 2. Runs even if JS is disabled or bypassed with a hand-crafted POST. Required fields and content rules are defined per service in the `$services` registry. |
| 4 | **Email rendering** | `ServiceRequestSubmission.php` — `$detailRows` loop | Builds the HTML table rows for both the lab notification and the user confirmation email. A field must be in `$meta['fields']` (controls inclusion and order) and `$meta['labels']` (controls the column heading). Values pass through `formatValue()` to convert raw option keys to readable text (see [service-request-email.md](service-request-email.md)). |
| 5 | **SharePoint** | `ServiceRequestSubmission.php` — `$sp_List_fields` block | Writes a record to the SharePoint Services list via the Microsoft Graph API. Only 8 fields are written here — they are hardcoded. A new field does not appear in SharePoint unless explicitly added. |

---

## The `$services` Registry

The `$services` array near the top of `ServiceRequestSubmission.php` is the configuration hub for
Layers 3 and 4. Each service key (`'printing'`, `'laser'`, `'scanning'`) maps to a block:

| Sub-key | Controls |
|---|---|
| `title` | Email subject lines and headings |
| `required` | Fields PHP enforces as non-empty (Layer 3 required check) |
| `fields` | Which fields appear in the email table, and in what order (Layer 4) |
| `labels` | Human-readable column headings for the email table |
| `uploadField` | The `name=` of the `<input type="file">` for this service |
| `allowedExtensions` | Accepted file extension whitelist for this service's uploads |

> **Note:** A field missing from `fields` will never appear in the email — even if it is in `required`
> and the user filled it in. A field in `fields` but not in `labels` falls back to the raw PHP key
> name as the column heading (e.g. `application_category` instead of `Application Category`).

---

## Two Special Field Categories

**Contact fields** (`first_name`, `last_name`, `email`, `phone`) behave differently from all other fields:
- They are in `$meta['required']` — PHP enforces them as non-empty
- They are NOT in `$meta['fields']` — they skip the `$detailRows` loop entirely
- They appear in the hardcoded "Contact Information" section at the top of both email templates
- They go to SharePoint as `FirstName`, `LastName`, `Email`, `PhoneNumber`

**`service_type`** never exists in the HTML form at all:
- JS appends it just before the fetch: `formData.append('service_type', serviceType)`
- The map `serviceTypeByFormId` in the inline script resolves form IDs to service keys:
  `printingForm → 'printing'`, `laserForm → 'laser'`, `scanningForm → 'scanning'`
- PHP reads it as `$serviceType = post('service_type')` and uses it to select the right `$services` entry

---

## JS / PHP Regex Parity

`JS/validation.js` and `includes/validation.php` share identical regex patterns. Both files contain
the comment: *"Regex parity with PHP/JS: any change must be mirrored in the other file."*

| Rule | JS constant | PHP function |
|---|---|---|
| Valid name characters | `NAME_RE` | `validateNameField()` |
| Emoji detection | `EMOJI_RE` | `containsEmoji()` |
| Keyboard mashing | `MASHING_RE` | `looksLikeMashing()` |
| HTML/script injection | `HTML_TAG_RE` + `HTML_ATTR_RE` | `containsHtmlTags()` |

> **Note:** `EMAIL_RE` is defined in `JS/validation.js` but is not in the table above. On the PHP
> side, email is validated with `filter_var($email, FILTER_VALIDATE_EMAIL)` rather than a mirrored
> regex function — this is intentional. If you change email validation logic, update
> `ServiceRequestSubmission.php` directly rather than looking for a PHP regex constant to match.

> **Warning:** Changing a regex in one file without updating the other means the server will reject
> input the browser approved — the user sees a server error after passing client-side validation,
> with no indication of what went wrong.

---

## Email Is Sent Before SharePoint Runs

`ServiceRequestSubmission.php` sends both emails first, then calls `respondAndContinue()` to flush
the JSON success response to the browser. PHP keeps running after that to sync with SharePoint.

This means:
- A SharePoint failure does **not** affect the user — they already have their confirmation email
- SharePoint errors are caught, logged to the server error log, and trigger a failure alert email
  to the lab team via `includes/sharepoint_alert.php`
- If a submission looks successful on the front end but the SharePoint record is missing, check
  the server error log and the failure alert inbox

---
