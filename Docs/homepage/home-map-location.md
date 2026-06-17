# Home Page · Committed to Innovation — Map Location
**File:** `index.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## Page Overview

<img src="/md_file_images/homepage-map-location.png" alt="Committed to Innovation section showing contact info and embedded Google Map" />

*The Committed to Innovation section at the bottom of the homepage — contact info on the left and the embedded Google Maps iframe on the right. The map embed URL is the element most likely to need updating.*

The "Committed to Innovation" section at the bottom of the homepage contains an embedded Google Maps iframe. The map shows a pinned location on campus. This is the only element in this section that requires non-obvious steps to update.

| Area | Search For (Ctrl+F) | What You Can Change |
|------|---------------------|---------------------|
| Map embed | `class="map-container"` | The location the map is centered on |

> **Note:** Changing the address text in the HTML (phone, email, street address lines) does not move the map pin. The iframe has its own URL that encodes the location separately — you must update both if the address changes.

---

## SECTION 1 — Updating the Map Location

### Step 1 — Find the map container
In `index.html`, press **Ctrl+F** and search for:
```
class="map-container"
```

You will land on this block:
```html
<div class="map-container">
    <iframe width="100%" height="100%" frameborder="0" scrolling="no" marginheight="0"
        marginwidth="0"
        src="https://maps.google.com/maps?width=100%25&amp;height=100%25&amp;hl=en&amp;q=561%20E%20Pine%20Knoll%20Dr,%20Flagstaff,%20AZ%2086001+(NAU%20Nano%20Facility)&amp;t=&amp;z=15&amp;ie=UTF8&amp;iwloc=B&amp;output=embed">
    </iframe>
</div>
```

### Step 2 — Generate a new embed URL

1. Go to [maps.google.com](https://maps.google.com) and search for the new address.
2. Click **Share** (the icon or right-click → Share).
3. Select **Embed a map**.
4. Click **Copy HTML**.
5. From the copied HTML, extract only the `src="..."` value inside the `<iframe>` tag.

### Step 3 — Replace the src value

Replace the entire `src="..."` attribute on the existing `<iframe>` with the new URL you just copied:

```html
<!-- Before -->
<iframe ... src="https://maps.google.com/maps?...q=561%20E%20Pine%20Knoll%20Dr...">

<!-- After — paste your new src URL here -->
<iframe ... src="https://maps.google.com/maps?...q=NEW+ADDRESS+HERE...">
```

Do not change any other attribute on the `<iframe>` (`width`, `height`, `frameborder`, etc.).

### Step 4 — Adjust zoom level if needed

In the new `src` URL, find the `z=` parameter and adjust:

| Value | Result |
|-------|--------|
| `z=13` | Zoomed out — shows the surrounding neighborhood |
| `z=15` | Current default — shows the block and nearby streets |
| `z=17` | Zoomed in — shows the specific building clearly |

Example — changing from zoom 15 to zoom 17:
```
...&amp;z=15&amp;...
→
...&amp;z=17&amp;...
```

### Step 5 — Verify in the browser
Open `index.html` and scroll to the map. Confirm:
- The correct location is pinned
- The zoom level shows the right amount of context
- The map loads and is not blank

> **Please double-check:** Google Maps embeds sometimes show a gray "This page can't load Google Maps correctly" message when viewed from a local file on your computer (`file://`). This is normal for local preview — the map will load correctly on the live server. Deploy to the staging environment to confirm before going live.

---

## SECTION 2 — Understanding the Embed URL

The `src` URL is a plain Google Maps embed link. No API key is required for this basic embed type. The key parts of the current URL are:

| URL Fragment | Current Value | What It Controls |
|---|---|---|
| `q=561%20E%20Pine%20Knoll%20Dr,...` | Encoded street address | Where the map is centered and pinned |
| `+(NAU%20Nano%20Facility)` | Label in parentheses | The text shown on the map pin popup |
| `z=15` | Zoom level integer | How far in or out the map starts |
| `hl=en` | Language code | Map labels language (`en` = English) |

> **Tip:** URL encoding replaces spaces with `%20`. When Google generates the embed URL for you in Step 2, the encoding is done automatically — you do not need to encode anything by hand.

---

## SECTION 3 — Quick Reference

| Task | Search For (Ctrl+F) |
|------|---------------------|
| Find the map iframe | `class="map-container"` |
| Find the src URL to replace | `src="https://maps.google.com` |

### Changes That Must Always Be Made Together

| If you change… | You must also change… |
|---|---|
| Map location (iframe `src`) | The address text lines in the same section (`561 E Pine Knoll Dr`) |
| Address text in this section | The map iframe `src` URL AND the same address on `Contact_Us.html` and `includes/footer.html` |
