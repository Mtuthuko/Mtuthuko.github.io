# Gold Seam: the design system

This is the design system behind **mtuthuko.github.io**, Mtuthuko Mngomezulu's personal page. The same system is meant to carry the *Websites by Ntuthuko* services and pricing page, so everything here is written to be reused.

The idea fits in one sentence: **one gold line signs the page.** On a first visit, a thin gold horizon draws across the hero and the hero opens into a full marigold field behind his name. The same line then runs down the left edge of every section, through his roles and past his live work, and finally turns right to underline his email under **SAY HELLO BACK**. The page opens with "Hello from…" and closes asking for a reply.

Use this file as a guide: what each piece is, why it is built that way, and how to add a page that belongs.

---

## 1. Where things live

| File | What it is |
|---|---|
| `index.html` | The home page. All of its CSS sits in one `<style>` in the head (tokens first), and all of its JS sits in one `<script>` before `</body>`. |
| `404.html` | The "page not found" page. It has its own small CSS (a subset of the same tokens) and no JS. |
| `assets/mm.svg`, `favicon.ico`, `apple-touch-icon.png` | The MM seam mark as browser icons. |
| `assets/og.png` | The 1200×630 share card that WhatsApp and LinkedIn show. It was rendered locally from the page's own CSS and fonts. |
| `assets/portrait.webp` | His own photo (640×640), shown in natural colour. |

**Why CSS and JS are inline:** the site's HTML check (html-validate) asks every external stylesheet and script to carry an integrity hash. Google Fonts cannot carry one, because its CSS changes per browser. So the fonts load through `@import` as the first rule of the inline `<style>`, and the two GSAP scripts load from cdnjs with exact `sha384` hashes.

**Hosting rules (GitHub Pages):** no `CNAME`, no `.nojekyll`, no front matter. No file or folder may start with `_`, `.` or `#`. Use relative paths (`assets/…`) in pages at the root, and root-relative paths (`/assets/…`) in `404.html`, because that page is served at any depth.

---

## 2. Colour

Two colours, used at full strength: **marigold gold** and **reef ink**. They swap roles section by section (gold ground with ink type, then ink ground with gold type). There is no third hue, no gradient, no glow, no glass and no shadow.

### 2.1 Palette

These sit on `:root`. "Day" is the default. "Night" applies when the visitor's device is in dark mode.

| Token | Day | Night | Used for |
|---|---|---|---|
| `--gold-field` | `#F5C242` | `#D29A1E` | Gold sections (hero by day, work) |
| `--gold-type` | `#F5C242` | `#F2BE3E` | Gold text, seam and nodes on ink; gold buttons |
| `--gold-deep` | `#E8AE24` | `#C08A14` | Hover surface on gold |
| `--ink` | `#15120C` | `#100D08` | Text on gold; ink sections |
| `--ink-raised` | `#231E15` | `#1D1912` | Hover surface on ink; the story band at night |
| `--ink-2` | `#523F0E` | `#3A2A05` | Secondary text on gold (gold-tinted, never grey) |
| `--sand` | `#F1E7D3` | `#EDE3CF` | Body text on ink |
| `--sand-2` | `#B9AC93` | `#ADA088` | Secondary text on ink |
| `--rule-gold` | `#6E5413` | `#5A420B` | 2px rules and borders on gold |
| `--rule-ink` | `#8C7A52` | `#8C7A52` | 2px rules and borders on ink |

`#F5C242` is the same gold as the services page's accent, so the two pages already share a colour.

### 2.2 Fields: the only tokens components use

A **field** is a section's ground. Put one of these classes on a section and every component inside it picks the right colours by itself:

- `.field-gold`: gold ground, ink type.
- `.field-ink`: ink ground, sand type, gold headings.
- `.field-hero`: gold by day, ink at night. Use it for the first section.

| Field token | On gold | On ink | Meaning |
|---|---|---|---|
| `--bg` | gold-field | ink | The ground |
| `--fg` / `--fg-2` | ink / ink-2 | sand / sand-2 | Text / secondary text |
| `--display` | ink | gold-type | Headings and nameplates |
| `--plate` / `--on-plate` | ink / gold-type | gold-type / ink | Primary button and its label |
| `--plate-hover` | ink-raised | gold-deep | Primary button on hover |
| `--seam` | ink | gold-type | The line, nodes and ticks |
| `--ring` | ink | gold-type | Keyboard focus ring |
| `--raised` | gold-deep | ink-raised | Hover surface for the ghost button |
| `--rule` | rule-gold | rule-ink | Rules and borders |
| `--flood` / `--flood-fg` / `--flood-fg-2` | ink / gold-type / sand-2 | gold-field / ink / ink-2 | The work-row hover band and the text on it |

**Rule:** a component never names a palette colour directly. It uses `var(--fg)`, `var(--plate)` and so on. That is why the same button works on gold and on ink.

### 2.3 Field rhythm on the home page

| Section | Day | Night |
|---|---|---|
| Hero | gold | ink |
| Story | ink | ink-raised |
| Work + Websites | gold | deep gold |
| Close | ink | ink |
| Footer | ink | ink |

### 2.4 Contrast (checked, WCAG 2.x)

| Pair | Day | Night |
|---|---|---|
| ink on gold (text, focus ring, ink button) | 11.28 | 7.74 |
| ink-2 on gold | 6.11 | 5.54 |
| rule-gold on gold (borders) | 4.31 | 3.78 |
| gold-type on ink | 11.28 | 11.27 |
| sand on ink | 15.23 | 15.22 |
| sand-2 on ink | 8.35 | 7.53 |
| rule-ink on ink | 4.47 | 4.64 |

Every text pair passes 4.5:1 and every border or focus ring passes 3:1. **If you add a new colour pair, measure it before you use it.**

---

## 3. Type

Two families from Google Fonts:

- **Anybody** sets the voice: the nameplates, headings, buttons, nav, labels, prices and the email. It has a *width* axis, set with `font-stretch` percentages (for example `font-stretch:125%`), never with `font-variation-settings`.
- **Hanken Grotesk** (400 and 600) sets everything you read: paragraphs, lists and descriptions.

The italic of Anybody exists only at weight 300 and normal width. Keep it for spoken or quoted words: "Hello from", "Websites by", and Vynklo's tagline.

| Token | Role | Face | Size |
|---|---|---|---|
| (fitted, see §4) | Nameplate lines | Anybody 850, uppercase by CSS | 69px at 390 → about 176px at 1440 |
| `--t-hello` | "Hello from" | Anybody italic 300 | `clamp(1.75rem, 1rem + 3vw, 3.5rem)` |
| `--t-h2` | Section heading | Anybody 800, 125% | `clamp(2rem, 1.2rem + 3.4vw, 4.5rem)` |
| `--t-pitch` | A long-sentence heading | Anybody 800, 110% | `clamp(1.75rem, 1.1rem + 2.6vw, 3.5rem)` |
| `--t-feature` | Featured row name | Anybody 850, 125%, uppercase | `clamp(2rem, 1.3rem + 2.8vw, 3.75rem)` |
| `--t-h3` | Role titles, product names, tier names | Anybody 780, 115% | `clamp(1.375rem, 1.1rem + 1.2vw, 2.25rem)` |
| `--t-quote` | A quoted tagline | Anybody italic 300 | `clamp(1.25rem, 1rem + 1.2vw, 2rem)` |
| `--t-role` | Hero role line | Anybody 750, 115% | `clamp(1.15rem, .9rem + .9vw, 1.6rem)` |
| `--t-email` | The big email | Anybody 700 (75% width below 600px) | `clamp(1.25rem, 1rem + 1.6vw, 2.25rem)` |
| `--t-price` | Prices (services page) | Anybody 850, 125%, tabular | `clamp(2.25rem, 1.5rem + 3vw, 4rem)` |
| `--t-ui` / `--t-nav` | Buttons / nav links | Anybody 750 / 700 | 1.0625rem / .9375rem |
| `--t-lead` | Lead paragraph | Hanken 400 | `clamp(1.125rem, 1rem + .4vw, 1.3rem)` |
| `--t-body` / `--t-small` | Body / footer | Hanken 400 | 1.0625rem / .9375rem |
| `--t-label` | Dates, domains, the clock | Anybody 700, 75%, uppercase | .8125rem (13px minimum) |

Rules that keep it looking built, not assembled:

- **Never put a small label above a heading.** Dates and labels sit in a column to the left (as in the timeline), or run in.
- **Uppercase is CSS only.** The HTML keeps "Mtuthuko", "Mngomezulu", "Say hello back", so screen readers read words, not letters.
- Headings use `text-wrap: balance`, paragraphs `text-wrap: pretty`, and body text stays under about 65 characters a line.
- On very small phones (below 360px), `--t-h3` steps down to 1.25rem so the longest name, "iSinkwaMtase", still fits the timeline column without breaking mid-word.
- Years, the clock and prices use tabular, lining numerals (`.tnum`).
- `font-synthesis: none`: the browser never fakes a bold or an italic.

---

## 4. The nameplate (the big fitted name)

The name is set as two lines that end exactly flush with both edges, at every screen width. It works like this:

1. The `h1` is a block with `container-type: inline-size`, so `cqi` units mean "percent of the nameplate's own width".
2. Each line's width was measured in Anybody at weight 850. A line is `width-per-em × font-size` wide, so a font size of `100 ÷ width-per-em` cqi fills the box exactly. The shorter word gets a wider `font-stretch`, so both lines end together.
3. Three tiers, chosen by the nameplate's own width (`@container` at 600px and 1000px), keep the letters a good shape on phones, tablets and desktops.
4. The fitted size only switches on (class `np-ready` on `<html>`) after the browser confirms Anybody has really loaded. Before that, or if fonts are blocked, a safe size (`9cqi`) shows, and `min-height` keeps the space so nothing jumps.
5. A small guard measures each line after load and on resize. If a future font update made a line too wide, it shrinks that nameplate slightly (`--np-fix`).

**To fit a new word** (for example **NTUTHUKO** on the services page): render it at `font-size:100px` with the same weight, tracking (`-.01em`) and `font-stretch`, measure its width with a `Range` (`selectNodeContents`, then `getBoundingClientRect().width`), divide by 100 to get width-per-em, and set `--np` to `(100 ÷ width-per-em) × 0.996` cqi. Never put `overflow: hidden` on a nameplate line.

---

## 5. Space, grid and the seam lane

| Token | Value | Use |
|---|---|---|
| `--s-1` … `--s-11` | 4, 8, 12, 16, 24, 32, 48, 64, 96, 128, 160px | The spacing steps |
| `--gutter` | `clamp(16px, 4.4vw, 64px)` | Page side margin (17px on a phone, 63px on a laptop) |
| `--lane` | 28px (40px from 1000px) | The strip between the line and the content |
| `--section-pad` | `clamp(64px, 10vw, 160px)` | Space above and below each section |
| `--h2-gap` | 32px (48px from 1000px) | Space under a section heading |
| `--group-gap` | 96px (128px from 1000px) | Space between groups in one section |
| `--max` | 1440px | Widest content |

- **The lane:** the line runs at the gutter in every section. Below the hero, all content starts one lane to its right (`.wrap.lane`). Only the hero's name and horizon span the full width.
- **Grid:** from 1000px, sections that need columns use 12 equal columns with `--col-gap`. Below 1000px everything stacks, except the hero details, which go two-up from 600px.
- **Breakpoints:** 480px (buttons go full width below it), 600px and 1000px.
- **Shape:** corners are square everywhere (buttons, rows, the portrait). Circles are only for the nodes and the final dot. The line has round caps. The single 12px radius is the line's last turn.
- **Lines:** 3px for the seam and horizon, 2px for rules and borders, 3px for focus rings.
- **Depth:** none. No shadows. Depth comes from swapping fields and from the row hover band.

---

## 6. Components

All of these live in `index.html`'s `<style>`. Each one reads field tokens, so it works on gold and on ink.

- **Skip link** (`.skip`): the first thing in `<body>`. Hidden above the page until it gets keyboard focus.
- **Header, mark and nav:** the header sits over the hero and is not sticky. The MM mark (a 44px target) links to the top. Three nav links: Story, Work, Say hello.
- **Plate button** (`.plate`): the main action. A solid ink block on gold (or gold on ink), 56px tall, with the arrow icon. The arrow slides 4px on hover. Full width below 480px.
- **Ghost button** (`.ghost`): the secondary action, a 2px outline. Used for "Copy address", which shows "Copied" with a check for 2 seconds and tells screen readers through a polite live region.
- **Text link** (`.link`): a 2px underline that thickens to 4px on hover. `.link--solo` makes a standalone link a 44px target. Every external link opens in a new tab, shows the small ↗ icon, and says "(opens in a new tab)" to screen readers.
- **Label** (`.label`) and **domain** (`.label.domain`): small condensed data text for dates, web addresses and the clock.
- **Seam, horizon, node and tick:** decorative, `aria-hidden` spans drawn fully in CSS. One `.seam` per section (never a border on list items). A `.node` is a 14px ring on the line, filled when the line reaches it; `.is-now` is always filled. Work rows get a 12px tick into the row.
- **Ledger** (`ol.ledger`): the timeline. Date in a fixed left column, then the role (h3) and a one-line detail. Each item has a node on the line.
- **Work rows** (`ul.work` of `li.row`): a list, not cards. The product name (h3) holds the link, and its `::after` stretches over the whole row, so the link's name is just the product name. On hover or keyboard focus, an ink band floods in from the line side and the text turns gold. `.row--feature` is the large first row.
- **Websites bridge** (`.pitch`): a sentence-length heading with a node on the line, a lead line and a plate.
- **Close** (`.close-run`): the line comes down, turns at a 12px corner, underlines the email (`.uline`) and ends in a dot. The email has a `<wbr>` before "@" so it only ever wraps there. A clock shows the time in Johannesburg.
- **Portrait** (`.portrait`): square, natural colour, with a 2px "plate mark" frame 12px outside it. Never a circle crop, never a filter.
- **Icons:** drawn as inline SVG in one stroke family (`fill="none" stroke="currentColor"`, round caps and joins), always `aria-hidden="true" focusable="false"`. The set: arrow, external ↗, copy, check, and the MM mark (one zigzag line that reads as a double M and as a ridge). Never use emoji or text arrows as icons.

### For the services page

These are defined by the system for *Websites by Ntuthuko*:

- **Rate board** (`ol.rates` of `li.rate`): rows, not cards, with 2px rules. From 1000px: tier name and duration in columns 1–4, what is included in 5–9, and the price in 10–12, right-aligned. Below that, tier and price share the first line. Included items get a 6px square bullet in the seam colour. In "from $3,500", the word "from" is small (`.from`). **No "most popular" badge**: that would be a claim nobody made.
- **Process steps** (`ol.steps`): three steps, numbered 1, 2 and 3 in Anybody 850 (numbers are fine here because the order is the content). Each step has a node on a line: one horizontal line across three columns from 1000px, the vertical lane below that.
- **Small nameplate** for the services header: *Websites by* in italic 300, then **NTUTHUKO** in Anybody 850 at 125%, inside the header's home link.

---

## 7. Motion

GSAP 3.13.0 and its ScrollTrigger plugin, from cdnjs, carry one idea in three acts. Everything else is still.

1. **Dawn (first visit only).** It plays when motion is allowed, it is the first view in this browser session, and the address has no `#section`. A gold horizon draws (0–0.7s), then the gold field opens up and down from that line (0.45–1.35s) while each row of text swaps to its day colours as the edge passes it. Then the line drops down into the story. About 1.4s by day; at night only the lines draw (about 1.05s). Any scroll, tap, click or key press jumps to the end. If the fonts or GSAP are slow (not ready by 1.5s), the page simply fades to its final state.
2. **The line draws down each section as you scroll.** It follows the scroll, never goes back up, and fills each timeline node as it reaches it.
3. **The close (once).** When the email comes into view, the line comes down, turns and underlines the address, and the dot lands.

Small state changes use the motion tokens: `--ease-out` (`cubic-bezier(.16,1,.3,1)`), `--dur-swap` 160ms, `--dur-exit` 200ms, `--dur-enter` 360ms. Exits are faster than entrances.

| State | Duration |
|---|---|
| Row band in / out | 360ms / 200ms |
| Row text colour | 160ms, after 60ms |
| Button fill, underline, ghost hover, skip link | 160ms |
| Button arrow slide (4px) | 200ms |

Rules:

- Only `transform` and `clip-path` animate. `will-change` is set only on the dawn layer while it plays.
- **Text is never hidden as a starting state.** The only thing clipped is the textless gold layer behind the hero.
- With **reduced motion**, there is no dawn, the line is fully drawn, every node is filled, the arrow does not slide and the row band changes instantly. Without JS, without the CDN or without web fonts, the whole page still shows.
- No fade-ups per section, no parallax, no cursor effects, no marquees, nothing that loops or autoplays.

---

## 8. Browser surfaces

The parts a browser draws by default are themed too:

- Text selection is ink on gold in gold sections and gold on ink in ink sections.
- The scrollbar is ink on gold by day and gold on ink at night. The caret follows the text colour.
- `theme-color` turns an Android browser's bar gold by day and ink at night.
- Underlines sit `.22em` below the text and are 2px thick.
- The favicon set and the share card make the link preview look like the page.

---

## 9. Accessibility checklist

- One `h1` (the greeting with the name). One `h2` per section. h3 for roles and products.
- Landmarks: skip link, `header`, `nav` (labelled "Sections"), `main`, `footer`.
- Every text pair ≥ 4.5:1; borders and focus rings ≥ 3:1, in both Day and Night.
- A 3px focus ring on every keyboard stop, on the element itself. On a flooded row, the ring turns gold.
- Targets: 44px for nav, buttons and standalone links.
- At 390px: no sideways scroll, buttons full width, long web addresses can wrap.
- Every animation respects reduced motion; nothing flashes; the dawn plays once and can be skipped.

---

## 10. How to add a new page in this style

1. **Start from a copy of `404.html`.** It is the smallest complete page: the head (fonts, icons, theme colours), the palette and field tokens, the base styles, the skip link, the header with the mark, the plate button and the footer. Copy any other component you need from `index.html`'s `<style>`, keeping its section heading comment.
2. **Keep the head in this order:** charset, viewport, title, description, `color-scheme`, the two `theme-color` tags, icon links, the two Google Fonts `preconnect` links, then one inline `<style>` whose **first line** is the Google Fonts `@import`. If you need motion, add the two GSAP `<script>` tags with their `integrity` hashes, copied exactly from `index.html`.
3. **Give every section a field:** `.field-hero` first, then alternate `.field-ink` and `.field-gold`. Do not paint colours by hand.
4. **Put content in the lane:** `<div class="wrap lane" data-seam><span class="seam" aria-hidden="true"></span> … </div>`. The line will line up with every other section.
5. **Write the headings first:** one `h1`, then one `h2` per section. Let the heading speak; no small label above it.
6. **Use the components as they are:** a plate for the one main action, a ghost for a secondary one, `.link` for text links, rows for lists of things, and the ledger for anything with dates.
7. **Keep it true.** Use only facts the owner has published. Never name his current employer. Do not invent numbers, testimonials, logos or awards. The creator platform is always **Vynklo**, linked to https://vynklo.com. Prices appear only on the services page, copied exactly.
8. **Name the file safely** (no leading `_`, `.` or `#`) and use relative paths.
9. **Check it** at 390px and 1440px, in light and dark mode, with reduced motion on, and with only the keyboard. Fix what you see, then look again.

---

## 11. The services page (*Websites by Ntuthuko*)

That page lives in another repository and uses this system unchanged: the same tokens, fonts, fields, plate, links, focus, selection and work rows. Its copy and prices stay **word for word**, with one owner change: its creator-platform row is now **Vynklo**, linking to https://vynklo.com, described as "A new home for brands and creators." (Vynklo's own words). The page's own name, "Ntuthuko", stays as it is. It follows the visitor's light or dark mode, with the gold Day look as the default.

Its sections, in order:

1. **Hero (gold).** The header holds the small nameplate *Websites by* **NTUTHUKO**. The `h1` is "A website that loads fast, reads clearly on a phone, and gets you the enquiry." in the `--t-pitch` style. The lead is the page's own introduction. The plate is **Get a quote in 24 hours**, to `mailto:mngomezuluntuthuko@gmail.com?subject=Website%20quote`.
2. **Pricing (ink)** as the rate board, with the terms line copied exactly.
3. **Live work you can click (gold)** with the shared work rows.
4. **How it works (ink)** as the three process steps on the line, copied exactly.
5. **Close (gold)** with the plate **Send a brief** and the same email link.

On the personal page (`index.html`), prices, durations and deposit terms do not appear. The Websites bridge there uses only the services page's headline, its "Fixed price, fixed timeline…" line and the **Get a quote in 24 hours** button.

---

## 12. Names

- This page and the GitHub account spell the name **Mtuthuko Mngomezulu**. The services page and the email address use **Ntuthuko**. Keep each as it is; ask the owner before changing either.
- The creator platform is **Vynklo** (https://vynklo.com), shown in its own words: "Big ideas. Better together." and "A new home for brands and creators." It is shown as live work, without a job title.
