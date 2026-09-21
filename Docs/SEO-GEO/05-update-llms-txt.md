
# Update llms.txt

**Summary:** Add, edit, or remove a knowledge-base entry, or the lab's address and contact lines, in both static copies of llms.txt.

**When to change:** When a knowledge-base page is added, renamed, or removed, or when the lab's address or contact details change.

**Difficulty:** <span class="difficulty-stars" style="color:#E6A800">★★</span>

**Estimated Time:** 10 minutes

---

## Visual Reference

**Real Code Snippet (`/llms.txt` — file header and first index entry, lines 1–15)**

```markdown
# MPaCT Lab - Northern Arizona University

> The Microelectronics Processing and Characterization Testing (MPaCT) Lab is a
> shared-use characterization and fabrication facility at Northern Arizona
> University, Building 98E, 561 E Pine Knoll Dr, Flagstaff, AZ 86001, USA.
> It is open to NAU researchers, external academic users, and industry partners.
> Contact: mpct.nano@nau.edu, +1-928-523-2343.

This file indexes the knowledge base. Every page below states the technique,
the instrument make and model, what it measures or makes, its limits, and the
facility location in Flagstaff, Arizona.

## Techniques

- [Transmission Electron Microscopy](https://nano.nau.edu/knowledge-base/techniques/transmission-electron-microscopy/): Internal structure and crystallography below 0.2 nm. JEOL JEM-F200.
```

*The same opening is the pattern for the whole file: a title, one blockquote, one intro paragraph, then section headings and one-line links.*

---

## Target Files

| Path | Purpose in this task |
|---|---|
| `/llms.txt` | The site-root index. Edit this copy first. |
| `/knowledge-base/llms.txt` | The second copy, beside the knowledge-base pages. It must stay byte-for-byte the same as `/llms.txt`. |
| `/robots.txt` | Already allows `/knowledge-base/` (line 66) and `/llms.txt` (line 67). Leave those lines in place unless crawler rules are the task. |

> [!WARNING]
> **Keep the two copies identical.**
> AI tools may read `https://nano.nau.edu/llms.txt` or `https://nano.nau.edu/knowledge-base/llms.txt`. If you edit only one, the other goes stale. The site still returns HTTP 200 for both, and nothing in the page, the sitemap, or the server log tells you they have drifted.

> [!IMPORTANT]
> **Verify against your local codebase:** Before you edit, confirm the two llms.txt files still match, recount the bullets under each `##` heading, and confirm `Allow: /knowledge-base/` and `Allow: /llms.txt` are still in `/robots.txt`. A new link must be a real knowledge-base page, not a guessed slug.

> [!NOTE]
> **Nothing in this website repository writes either file.**
> Both copies are static text, checked in as the same 75 lines. The HTML under `/knowledge-base/` is a MkDocs build (`mkdocs-1.6.1`, `mkdocs-material-9.7.6` in the generator tag on `/knowledge-base/index.html`). The MkDocs project that produced that HTML is not in this repository, so this page cannot tell whether a later knowledge-base rebuild will replace `/knowledge-base/llms.txt`. After any rebuild, compare the two files again before you deploy.

> [!TIP]
> **The step that is easy to skip:** after the edit, go to [Part 3: Copy the file over the second location](#part-3-copy-the-file-over-the-second-location) and compare the files. Editing only the root copy fails silently.

---

## Step-by-Step Instructions

### Part 1: Read the file format

1. To get started, please open `/llms.txt` in your code editor. Do not start in `/knowledge-base/llms.txt`. Part 3 copies the root file over that second location so the two cannot be edited apart.
2. Read the file from top to bottom. The order is fixed:

   * Line 1 is a single `#` title: the lab name.
   * Lines 3–7 are one `>` blockquote. It holds the street address, who the lab is open to, the email `mpct.nano@nau.edu`, and the phone `+1-928-523-2343`.
   * Lines 9–11 are one intro paragraph, with no heading.
   * The rest of the file is `##` sections. Each entry is one bullet.

3. These are the `##` sections in the file, in this order. Please recount them in your copy before you rely on the bullet totals:

   | Section heading | Heading line | Bullets | Bullet lines |
   |---|---|---|---|
   | `## Techniques` | 13 | 32 | 15–46 |
   | `## By sample type` | 48 | 6 | 50–55 |
   | `## Comparisons` | 57 | 4 | 59–62 |
   | `## Instruments` | 64 | 5 | 66–70 |
   | `## Reference` | 72 | 2 | 74–75 |

   That is 49 bullets. 48 of them point at a knowledge-base `index.html`. The last one points at `/Equipment.html`. The set of knowledge-base URLs matches the child pages in the knowledge-base sidebar, plus the Concepts page. The four section landing pages and the knowledge-base home are not listed: `/knowledge-base/`, `/knowledge-base/techniques/`, `/knowledge-base/samples/`, `/knowledge-base/compare/`, and `/knowledge-base/instruments/`. `concepts/` is listed because that page has no child pages.

4. The Reference lines show the two URL shapes. A knowledge-base directory ends in a slash. The equipment catalogue is a file, so it does not:

   **Real Code Snippet (`/llms.txt` — Reference section links, lines 74–75)**
   ```markdown
   - [Concepts and Definitions](https://nano.nau.edu/knowledge-base/concepts/): Terms used in these guides, grouped by domain.
   - [Equipment catalogue](https://nano.nau.edu/Equipment.html): Tools held by the lab, with live status.
   ```

5. Please leave the crawler rules alone unless you are changing who may fetch the file. The shared rule set in `/robots.txt` already names both paths. How to add or block an agent is in [Update robots.txt and AI Crawler Access](01-update-robots-and-ai-crawlers.md).

   **Real Code Snippet (`/robots.txt` — Allow rules for the knowledge base and llms.txt, lines 66–67)**
   ```text
   Allow: /knowledge-base/
   Allow: /llms.txt
   ```

   Deleting `Allow: /llms.txt` does not hide the file. `/robots.txt` has no `Disallow: /`, so agents that honor robots.txt can still fetch it. Adding `Disallow: /llms.txt` or `Disallow: /knowledge-base/` would hide the index from those agents while a browser still opened the file. That mistake is easy to miss. Do not add either Disallow as part of this task.

6. A new knowledge-base page is picked up by the sitemap because `/sitemap.php` walks `index.html` files under `/knowledge-base/` and lists each folder with a trailing slash. `llms.txt` itself is not a sitemap URL. To confirm a new page is listed, or to unlist one, follow [Add or Unlist a Page in the Sitemap](02-update-sitemap.md). You do not add a sitemap line by hand for a normal knowledge-base page. If the page is missing from the sitemap, the index can still advertise a URL that search crawlers were not given. The site does not warn you.

---

### Part 2: Add or edit an entry

Edit `/llms.txt` only. Please use **Case A** for a link, or **Case B** for the lab name, address, email, or phone.

#### Case A: Add or edit a link

1. Choose the section that matches the knowledge-base navigation. Keep the five headings spelled as they are in the file. The nav tabs use different labels, and that is expected. The sections are also in a different order from the tabs: Techniques comes before By sample type in this file, while By Sample Type is the first tab. Please do not rename a heading or reorder the five sections unless that is the task. Either change still has to land in both copies.

   | # | llms.txt heading | Knowledge-base navigation | Where the page lives |
   |---|---|---|---|
   | ① | `## Techniques` | Techniques | `/knowledge-base/techniques/[page-slug]/` |
   | ② | `## By sample type` | By Sample Type | `/knowledge-base/samples/[page-slug]/` |
   | ③ | `## Comparisons` | Choosing Between Methods | `/knowledge-base/compare/[page-slug]/` |
   | ④ | `## Instruments` | Instruments | `/knowledge-base/instruments/[page-slug]/` |
   | ⑤ | `## Reference` | Concepts, plus the equipment catalogue on the main site | `/knowledge-base/concepts/` and `/Equipment.html` |

   Inside Techniques, the URL set matches the sidebar, but the order does not. Nanoimprint lithography and industrial automation sit in different places in `/llms.txt` than they do in the sidebar. Please put a new bullet beside the same neighbours the sidebar uses. Do not reshuffle the existing Techniques bullets unless reordering is the task you mean to do. Samples, Comparisons, and Instruments already follow their sidebar order.

2. Add or change one bullet. Use the guide below.

   | # | What to Change | Description | Options / Example |
   |---|---|---|---|
   | ① | Link title | The name in brackets. Use a plain page name, in the style of the bullets already in that section. Do not paste the short sidebar label (`TEM`, `SEM`) and do not paste the HTML `<title>` suffix ` - MPaCT Lab Knowledge Base`. The TEM page's H1 is `Transmission Electron Microscopy (TEM)`; the bullet drops the parenthetical. | `[Transmission Electron Microscopy]` |
   | ② | URL | An absolute `https://nano.nau.edu/...` URL. Knowledge-base pages are directories and end with `/`. `/Equipment.html` is a file and has no trailing slash. A relative URL is wrong on one of the two copies: from `/knowledge-base/llms.txt` it gains an extra `knowledge-base/` segment, and the file still looks valid. | `https://nano.nau.edu/knowledge-base/techniques/[page-slug]/` |
   | ③ | Fact line | One clause after the colon. State what the page measures or makes, and a limit when the page gives one. On a technique or instrument line, name the make and model as the knowledge-base page names them. Sample, comparison, and reference lines name a make and model only when the page is about one instrument. No slogans. | `Internal structure and crystallography below 0.2 nm. JEOL JEM-F200.` |
   | ④ | Section | The existing `##` heading from the table above. Do not add a sixth heading for a normal new page, and do not add a bullet for a section landing page. | `## Techniques` |

   **Example Snippet (new or edited llms.txt bullet)**
   ```markdown
   ## Techniques <!-- this heading is already in the file; do not add it again -->
   - [[Page title]](https://nano.nau.edu/knowledge-base/techniques/[page-slug]/): [What it measures or makes, and one limit]. [Make] [Model]. <!-- change the title, the slug, and the fact line -->
   ```

3. Every URL must resolve to a page that exists. Open the knowledge-base folder and confirm `[page-slug]/index.html` is there before you save the bullet. A bullet that points nowhere does not break `/llms.txt`: the file still loads, and the bad link fails only when someone follows it.
4. If a page is renamed, change the title and the URL in the same bullet. Leaving the old slug in place is a silent 404 for anyone who trusts the index.
5. When the bullet is saved, please go to Part 3. Do not paste the same line into `/knowledge-base/llms.txt` by hand.

#### Case B: Edit the name, address, email, or phone

1. In `/llms.txt`, edit the `#` line or the `>` blockquote from the Visual Reference. Keep it as one blockquote. A second `>` block, or a blank line inside the quote, splits the summary, and an AI reader will treat the rest as a new paragraph. The file will still open.
2. Change only the fact that changed. The current blockquote reads `Building 98E, 561 E Pine Knoll Dr, Flagstaff, AZ 86001, USA` and `Contact: mpct.nano@nau.edu, +1-928-523-2343.`

   | # | What to Change | Description | Options / Example |
   |---|---|---|---|
   | ① | Building and street | The address inside the blockquote. This file puts the building on the same line as the street and adds `USA`. | `Building 98E, 561 E Pine Knoll Dr, Flagstaff, AZ 86001, USA` |
   | ② | Email | The public lab address. It is `mpct.nano@nau.edu` on the homepage and on the knowledge-base contact paragraph. | `mpct.nano@nau.edu` |
   | ③ | Phone | Write the international form in this file. Other pages show the same number with different spacing. | `+1-928-523-2343` |

3. Those other pages are not character-for-character copies, and they will not update themselves. After you change the blockquote, update the same facts in each place that still shows the old ones. If you skip a place, every page still loads, and the lab is described two ways:

   * `/index.html`, in the location block: `Building 98E`, `South Engineering Lab`, `561 E Pine Knoll Dr, Flagstaff, AZ 86001`, the visible phone `928 523-2343`, and `mpct.nano@nau.edu`.
   * `/JS/organization.json`: place name `Building 98E, South Engineering Lab`, street `561 E Pine Knoll Dr`, Flagstaff, AZ, `86001`, country `US`. That file has no email and no phone.
   * The hand-copied JSON-LD blocks on the equipment pages. Follow [Update Equipment Structured Data (JSON-LD)](03-update-equipment-structured-data.md) so those copies stay together. One missed block is silent: validators may still pass the other pages.
   * The knowledge-base contact paragraph (`Building 98E, South Engineering Lab`, `561 E Pine Knoll Dr, Flagstaff, AZ 86001`, visible phone `928-523-2343`, email `mpct.nano@nau.edu`). The same built pages also put `telephone` `+1-928-523-2343` in their JSON-LD. That HTML is MkDocs output. The source project is not in this repository. Change it there and rebuild, rather than hand-editing every built page. A hand edit is wiped by the next rebuild, with no warning in this repo.

4. Then complete Part 3.

---

### Part 3: Copy the file over the second location

1. Please save `/llms.txt`. From the website root (the folder that contains `llms.txt` and the `knowledge-base` folder), overwrite the second copy. Do not merge the files by hand. A hand merge is how the copies drift, and the site will not tell you.

   **Example Snippet (overwrite the knowledge-base copy)**
   ```powershell
   Copy-Item -Path .\llms.txt -Destination .\knowledge-base\llms.txt -Force # overwrite the second copy from the root file
   ```

2. Compare the two files before you deploy. Either command is enough. Run it from the same website root.

   **Example Snippet (compare the two copies)**
   ```powershell
   fc.exe /b llms.txt knowledge-base\llms.txt # no differences means the copies match
   git diff --no-index -- llms.txt knowledge-base/llms.txt # empty output means the copies match
   ```

   `fc.exe /b` prints `FC: no differences encountered` when the bytes match. Any hex dump means they differ. `git diff --no-index` prints nothing and exits 0 when they match. When they differ it prints a diff and exits 1. PowerShell may then show a native-command error for that exit code. The exit code means the files differ. It does not mean git failed to run.
3. After any knowledge-base rebuild, run the compare again before you deploy. If the rebuild replaced `/knowledge-base/llms.txt`, read the diff and keep one text in both places. Copy the root file over the knowledge-base file only when the root file is the text you want to keep. Copying the other way without reading the diff can throw away a rebuilt index, or can publish a rebuilt index that dropped your new bullet. Neither mistake raises an error on the site.

---

## Removing an Entry

Please do this when a knowledge-base page is retired or renamed and the old URL should disappear from the index. Removing the bullet does not remove the HTML page, and deleting the HTML page does not remove the bullet. Each half fails on its own, with no warning from the other file.

1. Open `/llms.txt` and delete the whole bullet for that page, including the leading `- `.
2. Do not leave the old URL "just in case." A leftover link still looks like a live page. The index returns HTTP 200, and the link 404s only when it is followed.
3. If the page moved, add the new bullet in Part 2, Case A, in the same edit. Do not keep both the old slug and the new slug.
4. Complete Part 3 so the second copy loses the same line.
5. Unlist the retired HTML page with [Add or Unlist a Page in the Sitemap](02-update-sitemap.md). The sitemap is a separate list. Taking a line out of llms.txt does not take the URL out of `/sitemap.php`, and the sitemap will keep advertising a page you meant to retire.

---

## Technical best practices and validation

* **Facts, not marketing copy.** Each fact line should be something the knowledge-base page already says: the measurement or process, a limit, and the make and model when the page is about an instrument. Adjectives that are not on the page do not break the file. They do teach an assistant a claim the lab page does not make, and nothing flags the difference.
* **Every URL resolves.** Use `https://nano.nau.edu/` plus the real folder of an `index.html`. Keep the trailing slash on directory URLs so the link matches the canonical URL and the sitemap entry. Do not add a trailing slash to `/Equipment.html`. A slash there requests a different path. Please check each new or edited URL with `curl.exe -I` or in a browser. A 404 means the bullet is wrong even though llms.txt itself loads.
* **Stay in step with the knowledge-base navigation.** The bullet list should name the same child pages as the sidebar, plus Concepts and the equipment catalogue. It should not list the knowledge-base home or the four section landing pages. Link text follows the longer names already in llms.txt, not the short sidebar labels. Place a new bullet with its sidebar neighbours. Do not reorder Techniques as a side effect: nanoimprint lithography and industrial automation already differ from the sidebar, and a drive-by sort makes the review harder without any error from the site.
* **Address and contact details match the rest of the site.** The email is `mpct.nano@nau.edu` and the phone is `+1-928-523-2343` in this file. The homepage, `/JS/organization.json`, the equipment JSON-LD, and the knowledge-base contact paragraph carry the same place, with the wording differences in Part 2, Case B. Update those when the blockquote changes. A mismatch does not break either page.
* **The two copies stay one text.** Compare them after every edit and after every knowledge-base rebuild. A rebuild can replace `/knowledge-base/llms.txt` and leave `/llms.txt` untouched. `Allow: /knowledge-base/` in `/robots.txt` covers the copy inside the knowledge base, and `Allow: /llms.txt` names the root file. Agents may read either one.

---

## Let's Verify Your Changes

1. From the website root, compare the two files with `fc.exe /b llms.txt knowledge-base\llms.txt` or `git diff --no-index -- llms.txt knowledge-base/llms.txt`. Please do not deploy while they differ. The live site will serve both versions and will not mark either one as stale.
2. Start PHP's built-in server from the website root, on a port you can stop when you are done:

   ```powershell
   php -S 127.0.0.1:8080 -t .
   ```

   `php -S` does not read `.htaccess`. That does not affect this check: `/llms.txt` and `/knowledge-base/llms.txt` are static files, not rewrite rules. In a second window:

   ```powershell
   curl.exe -I http://127.0.0.1:8080/llms.txt
   curl.exe -I http://127.0.0.1:8080/knowledge-base/llms.txt
   ```

   Each response should be HTTP 200. Then open both URLs in a browser and confirm the new, edited, or removed line is the same in both bodies. `curl.exe -I` shows headers only, so the body check is the browser (or `curl.exe` without `-I`). Stop the server with Ctrl+C when you are finished. Blocked paths and Apache headers still have to be checked on Apache (production, staging, or a local Apache such as XAMPP).
3. Click-test every new or edited link, or request it with `curl.exe -I`. From the local server, a knowledge-base page looks like this (keep the trailing slash):

   ```powershell
   curl.exe -I http://127.0.0.1:8080/knowledge-base/techniques/[page-slug]/
   ```

   HTTP 200 means the page is there. HTTP 404 means the bullet does not match a real `index.html`. Please fix the bullet and copy the file again. The index will not report the 404 for you.
4. After deploy, open both public URLs, `https://nano.nau.edu/llms.txt` and `https://nano.nau.edu/knowledge-base/llms.txt`, and compare the bodies:

   ```powershell
   curl.exe -s https://nano.nau.edu/llms.txt -o $env:TEMP\root-llms.txt
   curl.exe -s https://nano.nau.edu/knowledge-base/llms.txt -o $env:TEMP\kb-llms.txt
   fc.exe /b $env:TEMP\root-llms.txt $env:TEMP\kb-llms.txt
   ```

   `FC: no differences encountered` means the deployed copies match. Then `curl.exe -I` each new link on `https://nano.nau.edu/...` and confirm it is HTTP 200. The two downloads stay in `%TEMP%`, not in the website root, so they cannot be deployed by mistake.
5. If the change was a new or retired knowledge-base page, open `/sitemap.php` and confirm the directory URL was added or removed, using [Add or Unlist a Page in the Sitemap](02-update-sitemap.md). llms.txt and the sitemap are separate lists. Updating one does not update the other.

---

🔔 **Documentation Update Reminder:** Please make sure to update any How-To procedures or relevant documentation every time you make a change to the website and deploy it. This keeps our operational manual healthy and helpful for the entire team!

*Last reviewed: September 21, 2026*
