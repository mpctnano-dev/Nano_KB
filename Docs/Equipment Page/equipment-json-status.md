# Equipment Catalog · `equipment.json` Status System
**Files:** `equipment.json`, `JS/Equipment_Status.js`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This System Is

`equipment.json` is a data file at the project root that stores the live availability status for every instrument. When `Equipment.html` loads, `JS/Equipment_Status.js` fetches this file and rewrites the status dot and Reserve button on every matching card — **overriding whatever is in the HTML**.

This means:
- Changing a card's `status-dot` class in the HTML may have no visible effect on the live page
- The correct place to change availability is `equipment.json`
- The same JSON file also controls status on individual `About_Equipment/*.html` pages

> **Note:** `Equipment_Status.js` matches cards to JSON entries by normalizing the instrument name — lowercasing it and stripping non-alphanumeric characters. `"B-2 Atomic Force Microscope (AFM)"` in JSON must match the `<h3 class="tech-title">` text in the HTML exactly (before normalization). A name mismatch causes the JSON entry to be silently ignored.

---

## SECTION 1 — JSON File Structure

**File location:** `equipment.json` (project root)

```json
{
    "lastUpdated": "2026-02-12",
    "equipment": [
        {
            "id": "EQ-006",
            "name": "Bambu Lab H2D 3D Printer",
            "category": "Fabrication",
            "status": "AVAILABLE",
            "expectedDate": null,
            "warningMessage": null,
            "accessories": [],
            "aboutPage": "About_Equipment/Bambu_H2D.html"
        }
    ]
}
```

### Fields Reference

| Field | Type | Required | Notes |
|-------|------|----------|-------|
| `id` | string | Yes | Equipment ID matching the `Reserve_Equipment.html?equipment=` URL parameter. Format: `EQ-001` through `EQ-XXX`. |
| `name` | string | Yes | Must match the `<h3 class="tech-title">` text in the HTML exactly. Case-sensitive after normalization. |
| `category` | string | Yes | For reference only — the JS does not use it for filtering. |
| `status` | string | Yes | One of: `"AVAILABLE"`, `"UNAVAILABLE"`, `"EXPECTED"` (all caps). |
| `expectedDate` | string or null | Only when `status` is `"EXPECTED"` | Date in `"MM/DD/YY"` or `"YYYY-MM-DD"` format. The JS converts it to `"Month Nth"` (e.g., `"07/01/26"` → `"July 1st"`). Set to `null` when not applicable. |
| `warningMessage` | string or null | No | If set, displays a scrolling warning ticker on the instrument's About page. Set to `null` to hide the ticker. |
| `accessories` | array | No | Currently unused in display logic. Leave as `[]`. |
| `aboutPage` | string | No | Path to the instrument's About page. Used for matching on About pages. |

---

## SECTION 2 — Changing an Instrument's Status

### Step 1 — Open `equipment.json`
Find the entry by searching (Ctrl+F) for the instrument name.

### Step 2 — Change the `status` field

```json
// Before — instrument unavailable
"status": "UNAVAILABLE",
"expectedDate": null,

// After — instrument is now available
"status": "AVAILABLE",
"expectedDate": null,
```

### Step 3 — Update `expectedDate` if applicable

If changing to `"EXPECTED"`, set a date. If changing away from `"EXPECTED"`, set to `null`.

```json
// Coming soon — date will show as "July 1st" on the card
"status": "EXPECTED",
"expectedDate": "07/01/26",

// No longer expected — remove the date
"status": "AVAILABLE",
"expectedDate": null,
```

Accepted date formats:
- `"MM/DD/YY"` → `"07/01/26"` displays as `"July 1st"`
- `"YYYY-MM-DD"` → `"2026-07-01"` also works

### Step 4 — Save the file and verify
Reload `Equipment.html` in a browser. The status dot and Reserve button should reflect the new status within a few seconds of page load.

> **Please double-check:** Open the browser DevTools console (F12 → Console). If you see `"Failed to load equipment.json"`, the file has a syntax error (usually a missing comma or quote). Use a JSON validator (e.g., [jsonlint.com](https://jsonlint.com)) to check the file before deploying.

---

## SECTION 3 — Adding a New Instrument to the JSON

When you add a new card to `Equipment.html`, you must also add a matching entry to `equipment.json`. Without it, the HTML status will not be managed by the status system.

### Step 1 — Find the next available ID
Search `equipment.json` for the highest existing `EQ-XXX` number and increment by one.

### Step 2 — Add the entry
Paste a new entry object inside the `"equipment"` array. Entries can be in any order — the JS matches by name, not position.

```json
{
    "id": "EQ-043",
    "name": "Your Instrument Name",
    "category": "Metrology",
    "status": "AVAILABLE",
    "expectedDate": null,
    "warningMessage": null,
    "accessories": [],
    "aboutPage": "About_Equipment/YourPage.html"
}
```

Add a comma after the closing `}` of the previous entry if this is not the last entry in the array.

> **Note:** JSON does not allow trailing commas. The last entry in the array must NOT have a comma after its closing `}`. A trailing comma will break the entire file silently — no equipment statuses will load on any page.

---

## SECTION 4 — Adding a Warning Message

A `warningMessage` displays as a scrolling ticker banner on the instrument's `About_Equipment/*.html` page. It does not appear on the catalog grid.

```json
"warningMessage": "This instrument is currently undergoing maintenance. Contact staff before scheduling.",
```

To remove the warning, set it back to `null`:
```json
"warningMessage": null,
```

> **Tip:** Warning messages are plain text only — no HTML tags. Keep the message under 120 characters so it reads cleanly in the ticker.

---

## SECTION 5 — How Status Affects What Visitors See

| JSON `status` | Status dot on card | Reserve button | About page button |
|--------------|-------------------|----------------|-------------------|
| `"AVAILABLE"` | Green dot | Enabled `<a>` link | Enabled Reserve link |
| `"UNAVAILABLE"` | Orange dot | Disabled, shows "Unavailable" | Disabled, shows "Currently Unavailable" |
| `"EXPECTED"` | Hidden, replaced by date text (e.g., "July 1st") | Disabled, shows "Unavailable" | Disabled, shows "Currently Unavailable" |

---

## SECTION 6 — Updating `lastUpdated`

The top-level `lastUpdated` field is a date string for reference only. The JS does not use it. Update it when you make significant changes to help track when the file was last edited:

```json
{
    "lastUpdated": "2026-05-19",
    ...
}
```

---

## Quick Reference

| Task | What to edit |
|------|-------------|
| Mark instrument available | `"status": "AVAILABLE"` in `equipment.json` |
| Mark instrument unavailable | `"status": "UNAVAILABLE"` in `equipment.json` |
| Set coming-soon date | `"status": "EXPECTED"`, `"expectedDate": "MM/DD/YY"` |
| Add warning banner on About page | `"warningMessage": "Your message here"` |
| Remove warning banner | `"warningMessage": null` |
| Add new instrument to system | New entry object in `equipment.json` array |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| `name` in `equipment.json` | `<h3 class="tech-title">` in the HTML card (must match exactly) |
| Status to `"EXPECTED"` | Set `"expectedDate"` to a valid date string |
| Status away from `"EXPECTED"` | Set `"expectedDate": null` |
| Add a new instrument entry | Check for trailing comma after the previous last entry |
