# Handoff: Theory@UMass club website

## Overview
A six-page public website for Theory@UMass, a theoretical computer science club in the UMass Amherst College of Information and Computer Sciences. It tells a visitor what the club is, when it meets, who runs it, and gives them the club's material: talk slides, recommended courses, reading, and every weekly problem set as a PDF.

Content is static. There is no auth, no forms, no backend. The only dynamic requirement is that officers can update text and add PDFs without a rebuild being painful.

## About the design files
The files in `design/` are **design references created in HTML** — prototypes showing intended look and behavior, not production code to copy directly. They are authored in a component format that wraps the markup in `<x-dc>` and loads `support.js`; ignore that scaffolding. What matters is the markup inside and its inline styles.

The task is to **recreate these designs in the target codebase's existing environment**, using its established patterns, routing, and component conventions. If no codebase exists yet, pick the simplest thing that fits: this is a static content site, so a static-site generator (Astro, Eleventy, Next.js static export) with Markdown or data files per page is the natural choice. Do not ship the HTML in `design/` as-is.

## Fidelity
**High-fidelity.** Colors, type, spacing, and copy are final. Recreate the UI pixel-perfectly (using the codebase's own primitives where they exist). Every value you need is listed under Design Tokens below and visible inline in the design files.

## Layout skeleton (shared by all six pages)

Every page is the same vertical stack on a cream page:

```
<page wrapper>   min-height: 100vh; background #f6f2e9; color #2b2724;
                 font-family 'Public Sans'; font-weight 300; padding 0 40px 96px
  <header>       max-width 1000px; centered; flex; align-items: baseline;
                 justify-content: space-between; gap 32px;
                 padding 36px 0 20px; border-bottom 1px solid #ddd5c4
  <hero>         max-width 1000px; padding 76px 0 60px;
                 grid 1fr 1fr; gap 80px; align-items: end
                 left = <h1>, right = one intro paragraph
  <sections>     max-width 1000px; each preceded by a mono eyebrow <h2>
  <footer>       max-width 1000px; margin-top 76px; border-top 1px solid #ddd5c4;
                 padding-top 22px; flex; space-between; gap 24px; wrap
```

Content column is capped at **1000px** and centered; page gutter is **40px**.

### Header
- Logo lockup is a single link to Home. The wordmark is Spectral 21px, `#2b2724`, reading `Theory@UMass` with the `@` in maroon `#6d1f26`. The 68×68px logo image is absolutely positioned at `left: 0; top: 50%; transform: translateY(-50%)` inside a link with `padding-left: 82px`, so the logo hangs to the left of the text and the text baseline aligns with the nav. Reproduce that baseline alignment — do not vertically center the lockup against the nav.
- Nav: flex, gap 26px, 14px, `letter-spacing: 0.04em`, `text-transform: uppercase`. Items in order: Talks, People, Courses, Reading, Resources, Other. Home is reachable only through the logo.

### Footer
IBM Plex Mono 12px, `letter-spacing: 0.08em`, color `#8a8272`. Left: `Theory@UMass · College of Information and Computer Sciences`. Right: link `@umasstheoryclub` → `https://www.instagram.com/umasstheoryclub/` (`target="_blank" rel="noopener noreferrer"`). Identical on all six pages.

### Section eyebrow (used ~15 times)
`<h2>` — IBM Plex Mono 12px, `letter-spacing: 0.14em`, uppercase, color `#8a8272`, margin-bottom 8–34px depending on how dense the section below is. This is the site's only section label pattern; build it as one component.

### Row/list pattern
Most content is a list of rows, not cards. There are no card surfaces, no shadows, and no border radius anywhere on the site. Rows are separated by `1px solid #ddd5c4` top borders with 22–40px vertical padding, and are laid out as a CSS grid with a fixed left meta column, a fluid middle, and a fixed right column:

- Talks / Home event rows: `150px | minmax(0,1fr) | 180px`, gap 40px, padding 30px 0, `align-items: start`
- Courses rows: `130px | minmax(0,1fr) | minmax(0,0.9fr)`, gap 40px, padding 22px 0, `align-items: baseline`
- Other/project rows: `minmax(0,1fr) | 180px`, gap 40px, padding 24px 0
- Reading rows: single column, padding 16px 0

## Screens

### 1. Home (`Home.dc.html`)
Purpose: orient a first-time visitor and give them the next thing happening.

- Hero: h1 `Anything Theory.` (Spectral 400, 54px, line-height 1.1, `letter-spacing: -0.015em`). Right paragraph 17px/1.65, `#4a443d`, `max-width: 44ch`.
- **Weekly meetings**: one row. Left column: Spectral 22px date line, then mono 12px `#8a8272` time line. Middle: Spectral 25px/1.3 title (`First Weekly Meeting`). Right: 15px location, `#4a443d`. All three currently read `TBA` — these are editable content fields, not placeholders to remove.
- **Next talk**: same row shape; middle has Spectral 25px title `Introduction to Quantum Algorithms` plus a 16px/1.6 speaker line. Below the row, an 18px-offset link `All upcoming talks →` to Talks.
- **What we do**: 2-column grid, gap 56px. Each column: Spectral 24px/1.25 heading, 16px/1.68 `#4a443d` body.
- **Closing band**: `border-top` + 40px padding-top, grid `1fr | 1.2fr`, gap 72px. Left: Spectral 400 26px/1.3 line. Right: paragraph (17px/1.65, `max-width: 50ch`) plus the site's only button — an inline-block link, padding 12px 24px, `1px solid #c9c0ad`, no radius, 15px, text `Instagram — @umasstheoryclub`. Hover: border and text go maroon `#6d1f26`.

### 2. Talks (`Talks.dc.html`)
Hero h1 `Talks` + intro about hosting faculty and guest talks with slides and notes posted after.
- **Upcoming**: rows in the `150 | 1fr | 180` grid — date/time meta, title + speaker, location.
- **Archive**: eyebrow, then a 16px `#4a443d` line `Past talks, most recent first.`, then the same rows, newest first. Past-talk rows carry a PDF slides link (see `design/talks/`).

### 3. People (`People.dc.html`)
Hero h1 `People` only — no intro paragraph. Then eyebrow **Officers** and four officer rows.

Rows **alternate portrait side**: rows 1 and 3 are `260px | minmax(0,1fr)` (portrait left), rows 2 and 4 are `minmax(0,1fr) | 260px` (portrait right). Gap 48px, padding 40px 0, `border-top: 1px solid #ddd5c4`, `align-items: start`. Preserve the alternation — it is the page's structure.

Portrait block: image `display:block; width:100%; aspect-ratio: 4/5; object-fit: cover`, no radius. Under it, Spectral 22px/1.25 name with pronouns appended as a 15px `#8a8272` span (`&nbsp;` before it), then a 15px contact link.

Text block: role label in IBM Plex Mono 12px, `letter-spacing: 0.1em`, uppercase, **maroon `#6d1f26`** (the only place role color appears), then bio at 17px/1.7, `#4a443d`, `max-width: 62ch`. Quinn's block adds a `Personal website →` link at 15px, margin-top 16px.

Roster, in order: Isik Ulusan (he/him, Co-President), Toby Kahn (he/him, Co-President), August Singh (they/them, Officer), Quinn Mayo (he/him, Officer).

Emails are deliberately obfuscated as display text (`iulusan [at] umass [dot] edu`) while `href` values are placeholders (`mailto:sample@umass.edu`). **The real mailto targets must be filled in by the club before launch** — keep the obfuscated display text.

**Open content gaps:** Toby Kahn's row still has the hatched portrait placeholder (`repeating-linear-gradient(135deg, #e7e0d0 0 8px, #efe9dc 8px 16px)`, 4:5, mono caption bottom-left) and a literal `[PLACEHOLDER.]` bio. Build the placeholder treatment as a real fallback state for a missing portrait; the bio string needs real copy.

### 4. Courses (`Courses.dc.html`)
Hero h1 `Courses` + intro. Eyebrow **Courses at UMass**, then eight rows sorted by course number in the `130 | 1fr | 0.9fr` baseline-aligned grid: course code (mono), title + prerequisites, and a short "why it matters" note.

### 5. Reading (`Reading.dc.html`)
Hero h1 `Reading` + intro `Our favorite papers and books about theory!`. Two sections, eyebrows **Books & chapters** and **Papers**, each a `list-style: none` `<ul>` whose `<li>`s have padding 16px 0 and a 1px top border. Each entry gives title, author, and topic. Papers section is currently empty of entries — it should render nothing (not an empty-state message) until items exist.

### 6. Resources (`Resources.dc.html`)
Hero h1 `Resources` + intro about every problem set written for weekly meetings, newest first.

Eyebrow **Problem sets**, then one group per term. Each group header is a flex row, `align-items: baseline`, gap 32px, `border-bottom: 2px solid #2b2724`, padding-bottom 10px: Spectral 400 30px term name (`Spring '26`, `Fall '25`) plus a mono 12px uppercase `#8a8272` count (`3 SETS`, `5 SETS`). The heavier 2px dark rule distinguishes a group header from an ordinary row border. Rows below link to PDFs in `design/psets/` (8 files: F25 sets 1–5, S26 sets 1–3). Counts should be derived from the data, not hardcoded.

### 7. Other (`Other.dc.html`)
Hero h1 `Other` + intro `Side projects, games, and other things club members have made.` Eyebrow **Projects**, then rows in a `minmax(0,1fr) | 180px` grid with both top and bottom borders. One entry: Complexle, with a description and an external link.

## Interactions & behavior
This is a static site. The full interaction inventory:
- **Links**: default `#6d1f26`, `text-decoration: none`. Hover: color `#2b2724`, `text-decoration: underline` with `text-underline-offset: 3px`. Applies globally to `a`. Implement it as a global rule — every link on the site follows it.
- **Instagram button** (Home only): hover flips border-color and text to `#6d1f26`, no underline.
- **External links** (Instagram, Quinn's site, Complexle, PDFs): `target="_blank" rel="noopener noreferrer"`. PDFs open in a new tab rather than downloading.
- No animations, transitions, modals, tabs, accordions, or scroll effects. Do not add any.
- Nav has no active-page indicator in the current design. If the codebase's conventions call for one, ask before adding.

## Responsive behavior
The designs are drawn at desktop width only and were not specified below ~1000px. Rules to carry down:
- Content column 1000px max, 40px gutters.
- Below roughly 860px, collapse every multi-column grid to a single column: hero, "What we do", the closing band, row grids (meta above content), and People rows (portrait above text, cap portrait width around 260px).
- Keep People's alternation on desktop only; stacked mobile rows all read portrait-first.
- Type scale can hold; only the 54px h1 needs to come down (≈38px on phones).
Confirm mobile specifics with the designer rather than inventing new layouts.

## State management
None. No client state, no data fetching at runtime. Model the content as data (a collection per page: talks, officers, courses, reading, problem sets, projects) so officers edit records rather than markup, and let the build render it. Problem-set counts and archive ordering derive from that data.

## Design tokens

Colors
| Token | Hex | Use |
|---|---|---|
| Page ground | `#f6f2e9` | body background (also on `html`) |
| Ink | `#2b2724` | primary text, link hover, 2px group rules |
| Ink secondary | `#4a443d` | body copy, descriptions, meta |
| Muted | `#8a8272` | eyebrows, footer, pronouns, mono meta |
| Maroon accent | `#6d1f26` | links, the `@` in the wordmark, role labels |
| Rule | `#ddd5c4` | all 1px borders |
| Button border | `#c9c0ad` | Instagram button, resting |
| Placeholder stripes | `#e7e0d0` / `#efe9dc` | missing-portrait hatch |

Type — Google Fonts: `Spectral` (300, 400, 600, 400 italic), `Public Sans` (300, 400, 500), `IBM Plex Mono` (400)
| Role | Spec |
|---|---|
| Page h1 | Spectral 400, 54px / 1.1, `-0.015em` |
| Term heading | Spectral 400, 30px, `-0.01em` |
| Closing line | Spectral 400, 26px / 1.3, `-0.01em` |
| Row title | Spectral 25px / 1.3 |
| Card/column heading | Spectral 24px / 1.25 |
| Name, date | Spectral 22px (/1.25 for names) |
| Wordmark | Spectral 21px |
| Lead paragraph | Public Sans 300, 17px / 1.65, `max-width: 44ch` |
| Bio | Public Sans 300, 17px / 1.7, `max-width: 62ch` |
| Body | Public Sans 300, 16px / 1.6–1.68 |
| Small / links / location | Public Sans 300, 15px |
| Nav | Public Sans 300, 14px, `0.04em`, uppercase |
| Eyebrow | IBM Plex Mono 400, 12px, `0.14em`, uppercase |
| Role label | IBM Plex Mono 400, 12px, `0.1em`, uppercase, maroon |
| Footer / mono meta | IBM Plex Mono 400, 12px, `0.08em` |
| Placeholder caption | IBM Plex Mono 400, 11px, `0.08em` |

Base body weight is **300**. There are no bold weights in use.

Spacing — vertical rhythm between sections: 48 / 72 / 76 / 78 / 82px. Column gaps: 26 (nav), 32, 40, 48, 56, 72, 80px. Row padding: 16 / 22 / 24 / 30 / 40px. Page padding: `0 40px 96px`.

Radius: `0` everywhere. Shadows: none. Borders: `1px solid #ddd5c4`, plus `2px solid #2b2724` for term group headers.

Body sets `-webkit-font-smoothing: antialiased`; long-form text uses `text-wrap: pretty`.

## Assets
In `design/assets/`:
- `logo.png` — club logo, rendered at 68×68 in the header; also the favicon (`<link rel="icon" type="image/png">`).
- `isik.jpg`, `august.jpg`, `quinn.jpg` — officer portraits, cropped to 640×800 (4:5, 2× for retina at the 260px display width). Any new portrait should match that crop and size.
- Toby Kahn's portrait does not exist yet.

In `design/psets/` — 8 problem-set PDFs, named `Theory@UMass <TERM> Problem Set <N>.pdf`. Filenames contain `@` and spaces; either keep them and URL-encode, or slugify on ingest and keep the display title in data.

In `design/talks/` — 1 talk-slides PDF.

## Files
- `design/Home.dc.html`, `Talks.dc.html`, `People.dc.html`, `Courses.dc.html`, `Reading.dc.html`, `Resources.dc.html`, `Other.dc.html` — the seven design references (six nav pages plus Home).
- `design/support.js` — runtime for the prototype format. **Not part of the design.** Do not port it.
- `design/assets/`, `design/psets/`, `design/talks/` — real assets, port as-is.

Open the `.dc.html` files in a browser to see them render.
