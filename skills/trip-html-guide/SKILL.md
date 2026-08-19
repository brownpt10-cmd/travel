---
name: trip-html-guide
description: Build a self-contained HTML trip guide in one of two formats — a mobile-first multi-page "pocket guide" (bottom nav, one page per section) or a single-page desktop "long-scroll" guide (sticky in-page nav, hero, magazine-style sections). Use whenever a finished itinerary needs to become a browsable HTML site, whether for a phone or for reading at a computer. Both formats share one warm, parchment-and-accent-color design language so a trip can ship either or both without looking like two different products.
---

# Trip HTML Guide

Turns a finished, confirmed itinerary (flights, lodging, day-by-day plan, reservations, research notes) into a static HTML site. No build step, no JS framework, no server — plain HTML/CSS files that open directly in a browser, work offline, and can be hosted for free on GitHub Pages or any static host.

Two formats live under this one skill because they solve different problems:

| | **Mobile Pocket Guide** | **Desktop Long-Scroll Guide** |
|---|---|---|
| Shape | 5+ separate HTML files, bottom tab nav | 1 HTML file, sticky in-page nav, scroll-to-section |
| Best for | Checking things on your phone during the trip — today's plan, transit directions, confirmation numbers | Planning at a computer, sharing with travel companions, printing/PDF |
| Reference build | `templates/mobile/styles.css` (built for a Munich Oktoberfest trip, Aug 2026) | `templates/desktop/desktop-template.html` (built for the same trip) |

Ask the user which format they want before building — or both, if the trip warrants it. Don't assume; a short trip with one hotel may only need the mobile guide, while a multi-stop trip someone wants to share with a group is a better fit for the desktop guide (or both).

## Shared design language

Both formats use the same warm, editorial palette so they read as one system if a trip ships both:

- **Background**: a light neutral — parchment/cream (`#F6EFDD`–`#f5efe1` family) for a warm trip-book feel, or light grey (`#E9E9E9`) for a cleaner/more neutral feel. Ask the user's preference or default to parchment for leisure trips.
- **Ink**: near-black warm brown/charcoal for body text, never pure black on the cream backgrounds — it looks harsh. On grey backgrounds, a true near-black reads fine.
- **Navy** (`#0A1B33`–`#1B2536` family): headers, nav bars, sticky elements, ticket/day-card headers. Use the *darker* end of that range for anything the eye rests on constantly (top header, bottom nav) — it should feel substantial, not pastel.
- **Accent gold** (`#C9982F`–`#d99a2b`): dividers, chips, badges, active-state highlights. Never body text — gold on light backgrounds fails contrast.
- **Accent maroon/oxblood** (`#7A2E27`–`#b23a2e`): section-title banners, alerts, callout borders. Use as a solid banner background (with white/cream text) for section headers, but for a callout box that needs to stay readable and calm (a strategy note, a warning), prefer a **white box with a maroon border/left-accent** over a solid maroon fill — solid maroon blocks with lots of text get heavy fast. Default to the white-with-maroon-border treatment for any callout longer than 2-3 lines.
- **Font**: a neutral sans-serif for all body copy and headings — Calibri/Arial (system-safe) or Manrope (if loading Google Fonts is acceptable for this delivery). Reserve a display/serif face (DM Serif Display, Georgia) *only* for a hero masthead or trip title if the desktop format wants a touch of editorial flair — never for body text or card content. Uppercase eyebrow labels (section tags, "Day 1 of 5" badges) can use a tighter-tracked sans-serif regardless of the body font choice.
- **Cards over tables**: no `<table>` elements for content — stack data as bordered/shadowed cards (quick-facts row-pairs, flight-leg cards, day cards, transit route cards). This is what makes both formats scan well on a phone and still look intentional on a desktop.

Do not treat the specific hex values above as locked — they're the validated defaults from the reference build. Swap them for a trip's own theme (destination flag colors, a color the user requests, a season) but keep the *relationships*: one dark anchor color, one warm neutral background, one saturated accent for chips/dividers, one deeper accent for banners/alerts, cards always lighter than the page background so they lift off it.

## Format 1 — Mobile Pocket Guide

Multiple linked HTML files, not one long page — this is the point. Someone standing at a train platform wants "today's plan" or "the hotel address," not a document they scroll through.

### Page set (adapt names/count to the trip, this is the default)

1. **Home** (`index.html`) — quick-facts card (hotel, flights, confirmation numbers, party), then a nav-card list linking to every other page, then a one-card trip summary.
2. **Itinerary** (`itinerary.html`) — one day at a time behind a **pager**: "Day X of N" badge, Previous/Next buttons, a dot indicator, and the day's content (a ticket-style card + a timeline of timed stops) swapped via a few lines of vanilla JS — no framework, no build step. If the trip has multiple stops/cities, either page by day within a stop and add a stop-level sub-nav, or page by stop first then day — pick whichever matches how the traveler will actually think about "where am I."
3. **Travel** (`travel.html`) — every flight/train leg, lodging (with a capacity/headcount flag if relevant), a reservations-status checklist, and all point-to-point transit directions, all on one scrolling page. This is the "logistics" page — it should answer "how do I get from A to B" without a search.
4. **More** (`more.html`) — everything else: food, sights, local strategy notes (e.g. a tent-walk-in strategy, a packing list, a weather contingency), open decisions as a checklist. Designed to keep growing — new cards slot in without restructuring.
5. **Files** (`files.html`) — a single large tappable link card to wherever the actual documents live (Google Drive, Dropbox, iCloud). On iOS this hands off to the native app if installed. Add one short paragraph reminding the traveler to mark key files "available offline" before losing signal.

Adjust the page count/names to what the trip actually needs — a single-city 3-day trip might not need a separate Travel page; a multi-leg trip might split Travel into per-leg pages. The 5-page shape above is a strong default, not a rule.

### Shared chrome

- A `styles.css` file shared by every page (see `templates/mobile/styles.css` for the full reference implementation — copy it as a starting point and reskin the CSS variables at the top).
- A sticky header at the top (trip name + dates/context) and a fixed bottom nav (one tab per page, active page highlighted in gold, `position: fixed` with `env(safe-area-inset-bottom)` padding for iPhone home-indicator clearance).
- `max-width: 520px` centered content column — this is what makes it feel like an app on a phone and still centers reasonably on a tablet/desktop browser.
- No JS dependencies beyond the small inline pager script on the Itinerary page. No `localStorage`/`sessionStorage` — these guides need to survive being opened from a fresh Safari tab with no prior state.

### Build process

1. Gather all confirmed trip content first (flights, lodging, itinerary, reservations, local research) — from uploaded docs, a prior pocket-guide-playbook/itinerary-build-playbook build, or direct conversation. Don't design pages around content you don't have yet.
2. Decide page set with the user if the default 5 doesn't fit.
3. Copy `templates/mobile/styles.css`, adjust the `:root` color variables to the trip's palette.
4. Build each page, reusing the shared component classes (`.facts`, `.leg`, `.hotel-card`, `.ticket` + `.timeline`, `.route`, `.callout-box`, `.info-card`, `.checklist`, `.pager`, `.big-link`, `.nav-card`) rather than inventing new ones per page — consistency across pages is what makes it feel like one guide.
5. Render each page headless (Playwright at ~390px viewport) and screenshot before delivering, to catch layout breaks — especially the day-pager JS and the fixed bottom nav overlapping content.
6. Deliver via `SendUserFile`. If the user wants it hosted (GitHub Pages, etc.), push the files as-is — they're already relative-linked and need no build step.

## Format 2 — Desktop Long-Scroll Guide

One HTML file, meant to be read scrolling top to bottom at a normal desktop width (design around ~1100–1200px max content width), with a **sticky in-page nav** (anchor links to `#itinerary`, `#travel`, `#tents`, etc. — no separate pages, no page reloads) that stays visible as you scroll.

### Reference section order (from the Munich build — adapt names, keep the shape)

Masthead → decorative band (optional flourish, e.g. a destination-flag-pattern stripe) → Hero (trip name, dates, a strong visual statement) → Countdown (optional, if built before departure) → Sticky Nav → Overview → Itinerary (day-by-day, can be a table or stacked cards — more room than the phone format, so richer layouts are fair game) → destination-specific deep-dive sections (in the reference build this was a festival-tent strategy section; for another trip this might be "Neighborhoods," "Excursions," whatever the trip's centerpiece is) → Travel (airport transfer, lodging) → local color sections (restaurants, food & drink, what to wear, useful phrases) → Footer.

This format has room the phone format doesn't — use it for things that don't fit well as phone cards: a full comparison table, a wider photo, a longer narrative passage, a phrase-book grid.

### Build process

1. Same content-gathering step as the mobile format — in fact, build both from the same source material where a trip gets both formats, so they never disagree with each other.
2. Copy `templates/desktop/desktop-template.html` as a starting skeleton and reskin: trip title, palette variables, section list, content.
3. Keep the sticky nav's anchors in sync with actual section `id`s as you add/remove sections.
4. Everything inlines into the one file (styles in a `<style>` block, no external CSS file) — the point of this format is "one file, opens anywhere," so don't split it into partials.
5. Screenshot at a desktop width (1280×900 or similar) before delivering, and once at mobile width too — this format should still be legible on a phone even though it isn't optimized for one; it just won't have the app-like chrome the pocket guide has.
6. Deliver via `SendUserFile`.

## Confirm before shipping (either format)

- Resolve any open items (unconfirmed reservations, TBD headcounts) before calling it done — flag them as a checklist rather than silently omitting them.
- Remove other-traveler references if the guide is being built for a solo trip, or vice versa.
- Verify transit/schedule specifics that came from research (not from a confirmed booking) with a fresh web search if the trip is more than a few weeks out — schedules drift.
- If publishing to GitHub Pages: push to the target repo's default branch, then enable Pages via the repo API (`source: {branch, path: "/"}`) — no separate `gh-pages` branch needed for a small static site.
