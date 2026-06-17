# Homepage (`index.html`) — Documentation Index
**File:** `index.html`  
**Last Updated:** May 2026  
**Internal Use Only**

---

## What This Page Is

<img src="../md_file_images/homepage-hero.png" alt="NAU Nano homepage hero section" />

*The index.html hero — the first full-viewport section visitors see, with the headline, subtitle, and CTA buttons. The header and footer you see in the browser are injected separately by layout.js and are not part of this file.*

`index.html` is the homepage of the NAU Nano website (https://nano.nau.edu). It is the first page most visitors land on and is divided into six visual sections. The header and footer are **not part of this file** — they are injected at load time from `includes/header.html` and `includes/footer.html` by `JS/layout.js`. Do not look for nav or footer content here.

---

## Sections on This Page

| # | Section | Editable? | Doc |
|---|---------|-----------|-----|
| 1 | **Hero** — full-viewport banner, background photo, headline, CTA buttons | Basic edits only — no doc needed | — |
| 2 | **Powering Arizona's High-Tech Future** — mission text, photo, $13M stat box | Basic edits only — no doc needed | — |
| 3 | **Featured Equipment** — horizontal carousel of instrument cards | Yes — non-obvious dependencies | [home-featured-equipment.md](home-featured-equipment.md) |
| 4 | **Workforce Development & Partners** — dark blue section with partner logo grid | Yes — grid count constraint | [home-workforce-partners.md](home-workforce-partners.md) |
| 5 | **Integrated Education Paths** — academic program card carousel with arrows | Yes — JS hardcoded card count | [home-academic-programs.md](home-academic-programs.md) |
| 6 | **Committed to Innovation** — contact info + embedded Google Map | Yes — map embed URL | [home-map-location.md](home-map-location.md) |

> **Note:** Sections 1 and 2 are static content. Changing text, images, or button labels in those sections is straightforward HTML editing with no hidden dependencies. They do not have dedicated doc files.

---

## Section 3 — Featured Equipment Carousel
**Doc:** [home-featured-equipment.md](home-featured-equipment.md)

This is a manually maintained list of instrument cards. Key things to know before editing:
- Cards are hardcoded in HTML — there is no data file feeding them
- Badge color, status dot, and reserve button must always be updated as a set
- Visual order follows HTML source order (unlike `Equipment.html`, there is no JS sort)

Go here when: adding a new instrument card, changing availability status, updating badge category, or swapping an equipment photo.

---

## Section 4 — Workforce Development & Partners
**Doc:** [home-workforce-partners.md](home-workforce-partners.md)

The partner logo grid uses a fixed 2-column CSS layout. Key thing to know:
- An odd number of logos (3 or 5) leaves an empty cell in the last row — this looks broken on desktop
- Safe counts are 2, 4, 6, 8 — or the CSS grid must be changed to accommodate odd numbers

Go here when: adding a new industry partner logo, replacing an existing logo, or changing a partner label.

---

## Section 5 — Integrated Education Paths
**Doc:** [home-academic-programs.md](home-academic-programs.md)

The carousel arrows and pagination dots depend on a hardcoded number (`7`) in `JS/script.js`. Key thing to know:
- Every time you add or remove a card in `index.html`, you must update that number in `script.js` — it does not update itself
- Forgetting this step breaks the arrows silently with no browser error

Go here when: adding a new degree program card, removing a program, or if the carousel arrows stop working.

---

## Section 6 — Committed to Innovation (Map)
**Doc:** [home-map-location.md](home-map-location.md)

The map is a Google Maps embed iframe. Key thing to know:
- Changing the address text in the HTML does not move the map pin — the iframe has its own `src` URL with the location encoded in it
- Both must be updated together if the address changes

Go here when: the lab moves to a new location, or the map pin needs to point somewhere different.

---

## Related Files

| File | Role |
|------|------|
| `CSS/style.css` | All visual styling — partner grid columns, card layout, badge colors |
| `JS/script.js` | Carousel JS, education card pagination, semester auto-update |
| `JS/layout.js` | Injects header and footer — do not modify |
| `includes/header.html` | Site navigation — edit here, not in `index.html` |
| `includes/footer.html` | Site footer — edit here, not in `index.html` |

