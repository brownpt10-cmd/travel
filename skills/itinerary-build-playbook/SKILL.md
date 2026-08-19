---
name: "itinerary-build-playbook"
description: "Build a final, polished Word-document itinerary from confirmed trip logistics (flights, lodging, bookings) pulled from uploaded docs, Gmail, Drive, or direct input, plus a packing list. Use when the user asks to write up, finalize, or produce a formal itinerary document, or says things like \"put together the itinerary doc\" or \"build the final trip document.\" Runs last in the trip workflow, after bookings are confirmed."
---

# Itinerary Build Playbook

Generic instructions for building any trip itinerary as a Word document. Applies in both
Chat and Cowork. Not trip-specific — reuse this same process for every new trip.

---

## 1. Gather Hard Information First

Before drafting, pull confirmed logistics from available sources, in this order:

1. **Uploaded documents** in the conversation/project (calendars, confirmations, prior
   itinerary drafts, project briefs).
2. **Gmail** — search for airline/hotel/rental confirmation emails.
3. **Google Drive / Cowork project files** — calendar slides, booking screenshots, prior
   drafts.
4. **Direct user input** — anything given in chat.

Hard information to extract for every leg:
- Flights: airline, confirmation #, flight numbers, dates, departure/arrival times and
  airports/cities.
- Lodging: property name, address, phone, check-in/check-out dates and times, room type,
  booking confirmation #, total cost, cancellation status.
- Ground transit already booked: trains, ferries, rental cars, transfers.
- Reservations: restaurants, tours, tickets, tent/venue bookings.

**If hard information is missing, ask for it rather than inventing it.** Do not fabricate
confirmation numbers, times, or prices. If the user has no source for a piece of information,
mark it as an open item (see Section 5) rather than guessing at specifics — but reasonable,
clearly-labeled *recommendations* (transit mode, restaurant picks, timing) are expected and
should be generated, not just flagged as gaps.

---

## 2. Structure: Day-by-Day vs. Block

- **Day-by-day**: trips under 5 days, or any trip anchored to a single base hotel. One
  section per calendar day.
- **Block-by-block**: multi-location trips. One section per location/lodging stay
  (e.g., "Sep 5–10 · Santorini (Oia)"), with day-level detail nested inside where it exists.

Always open with a **Trip at a Glance** table: dates, location, lodging, status
(Confirmed / Tentative / Not booked) for every leg of the trip.

---

## 3. What Every Day or Block Should Contain

Fill in what's known; generate reasonable recommendations for what isn't (labeled as
recommendations, not confirmed fact):

- **Transportation** — mode, route, timing, cost, booking status. If not provided, recommend
  based on typical options for that airport/city pair (taxi vs. transit vs. rideshare) and
  note it's a recommendation, not a booking.
- **Lodging context** — anything relevant to daily logistics (distance to sights, transit at
  the door, laundry availability, room capacity).
- **Things to do** — anchored first around any fixed commitments (flights, reservations,
  known must-sees), then filled with reasonable recommendations if the user hasn't specified
  activities. Flag terrain, heat, crowd, or accessibility considerations relevant to the
  traveler.
- **Where to eat** — recommend specific restaurants/bars if none are chosen, noting whether
  reservations are typically required.
- **Local logistics** — laundry, cash/currency needs, connectivity, tipping norms, safety or
  health notes (tap water, terrain, etc.) as relevant to the destination.

---

## 4. Packing List

Include as the **final section** of the same Word document (not a separate file unless the
user asks for one on a given trip).

- Split **His / Hers** (or per-traveler, if traveler count/names are known).
- Base quantities and layering on:
  - Destination(s) and **time of year** — pull current climate normals for each leg.
  - **Trip length and laundry access** — if there's a laundry gap of several days between
    wash points, that gap (not total trip length) drives clothing quantities. Map out wash
    points explicitly if the itinerary spans multiple laundry-access gaps.
  - Known traveler constraints if provided (mobility/medical considerations, footwear
    requirements, etc.) — do not invent these; only apply if the user has stated them.
- Note gear decisions with brief rationale (e.g., why a synthetic vs. down jacket) rather
  than just listing items.

---

## 5. Status Tagging Convention

Use consistently throughout the document:

- **CONFIRMED** — sourced from a booking confirmation, email, or the user's direct
  statement of a locked-in detail.
- **GAP** — something structurally necessary that has no answer yet (e.g., "Santorini
  lodging — GAP").
- *(recommendation, unlabeled)* — reasonable suggestions Claude is generating to fill in
  where the user hasn't specified (restaurants, transit mode, activities). These read as
  normal prose/recommendations, not tagged as gaps, since they're not blocking — just offer
  them and let the user accept, swap, or ignore.
- **Open Items** — collect all GAPs and unresolved questions in a running list, and also
  compile them into a final **"What's Missing" / "Open Items"** section at the end of the
  document, split into:
  - **Real unknowns** — conflicting or implausible source data (e.g., a ferry time that
    doesn't match any real schedule) that needs the user to verify against the actual
    booking.
  - **Still open** — decisions or bookings not yet made at all.

**On request, strip all status tags and the Open Items section out for a "final" clean
version** — same content, but presented as a settled itinerary rather than a working draft.

---

## 6. Format & Output

- **Output format:** Word document (.docx), using the docx skill.
- **Tables** where information is naturally tabular (flight legs, lodging summary,
  transportation summary, laundry reference, comparison options). Prose/subsections
  elsewhere.
- **No photos, no HTML-style visual formatting** at this stage — that comes later as a
  separate pocket-guide/HTML pass (see the `pocket-guide-playbook` skill), not part of this
  playbook.
- Include a short **"Latest additions" / changelog** line near the top when revising an
  existing itinerary, noting what's new since the last draft.
- Close with a **Sources** line noting what was used (uploaded docs, Gmail, Drive, direct
  input, web verification) and the as-of date for anything price- or schedule-sensitive.

---

## 7. Verification

- Web-search to verify anything time-sensitive or easily wrong: current-year event dates
  (festivals, holidays), transit schedules/prices, restaurant/venue operating status
  (confirm not permanently closed), visa/entry requirement status.
- Flag prices and schedules as approximate and dated ("as of [date] — reconfirm closer to
  travel") rather than presenting them as fixed.

---

## 8. Before Finalizing

Ask the user directly if any of the following are unclear rather than guessing:
- Exact traveler names/count, if not evident from source material.
- Whether a leg the user calls "out of scope" should actually be included (booked dates
  supersede earlier scoping notes).
- Whether to keep or strip status tags/open items in this pass.
- Whether packing should be per-traveler or combined, if traveler count is ambiguous.

