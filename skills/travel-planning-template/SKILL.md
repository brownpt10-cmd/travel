---
name: "travel-planning-template"
description: "Kick off rough-draft trip planning for a new trip — destination/pacing options, flights, lodging, point-to-point travel, activities, and dining, building a running Master Trip Doc through Phase 7. Use when the user wants to start planning a new trip, asks to plan a vacation/itinerary from scratch, or says things like \"let's plan a trip to X\" or \"help me plan our vacation.\" Stops at a planning-stage draft — does not book anything."
---

# Travel Planning Process — Instructions for Claude

Follow this step by step. Do not skip ahead. Update the **Master Trip Doc** at the bottom of this file after every phase — it is the single running record of the trip. This template covers rough-draft planning only (Phases 1-7); it stops at a planning-stage Master Trip Doc, not booking-ready reservations — see **Handoff to Cowork** at the end.

---

## Session Setup (before Phase 1)

Ask this up front, before or alongside the Phase 1 batch:
1. **Chat vs. Cowork — strongly prefer Cowork, and say so.** If running in regular Chat, recommend switching now rather than "before booking." If already in Cowork, skip this question.
2. **Project folder** — Ask if Claude should create a folder for this trip under `~/Documents/Claude/Projects/Travel`, and what to name it. Skip if the user declines or a folder for this trip already exists.

### Why Cowork, from Phase 3 onward

Phases 1–2 are generative — comparing itinerary shapes, pacing, and regions. Chat handles that fine.

**From Phase 3 on, every output is a factual claim about the world**: what a flight costs, what a hotel is called, which Tube line serves it, what time a museum opens on a Saturday in October, how far ahead a venue releases tickets. Chat cannot verify a single one of those. It will produce confident, plausible, specific answers anyway.

A Chat-built London draft was carried into a Cowork booking pass and **eight separate errors surfaced, none of which were catchable without live tools**:

- Flight estimate 30–86% low; lodging estimate 41–190% low
- The chosen hotel had been sold and renamed two and a half years earlier
- The hotel's stated Tube line was wrong — it wasn't on the line the plan depended on
- A Day 7 museum visit was physically impossible against the departure flight
- Saturday opening hours were half what the plan assumed
- One venue only sells tickets 8 weeks out, which the plan never accounted for
- Neither departure nor arrival airports had been swept for alternatives

The draft wasn't careless. It was written without the ability to check itself. **Treat any Chat-produced draft as a set of hypotheses, not a plan** — and re-verify all of it in Cowork before anything gets booked.

---

**Operating rules:**
- Ask only what's needed to move to the next phase. No filler, no restating what the user just said.
- Phases 1–2 must produce 3 distinct rough-draft itineraries before anything else happens, **unless the destination is already decided by the user** — in that case, skip straight to drafting 2-3 distinct *pacing/style* variants of that destination (e.g. concentrated core vs. wider loop vs. slower/deeper), not alternate places. User picks one (or blends) before Phase 3 starts.
- From Phase 3 onward, Claude researches and proposes specific options (flights, lodging, activities) but the user decides. Never book or finalize anything without explicit confirmation.
- Every phase updates the Master Trip Doc in place — don't create a new file per phase.
- When a phase hits a blocking decision (budget ceiling, a date that doesn't work, a neighborhood tradeoff), stop and ask. Don't guess past a real fork.
- Verify anything time-sensitive (venue open/closed, price ranges, travel times, opening days) before it goes in the doc — don't rely on memory for current status.
- When new information surfaces mid-planning that could change an already-locked day (a newly discovered site, a better routing), present it as a swap: Option A (current plan) vs. Option B (the change), and flag explicitly what else it displaces or moves. Don't silently rewrite a locked day.
- Where a reasonable simpler or backup version of a route/plan exists alongside the optimized one, note both — the optimized route for the default plan, the backup for if the user needs to audible on the day.
- Include hyperlinks to official sites, booking pages, or verified sources in the Master Trip Doc wherever a URL was actually confirmed during research. Never fabricate or guess a URL — if a verified link isn't available, list the name and location only.
- Before presenting the final Master Trip Doc (or any major update to it), do one pass across all phases to confirm dates, operating days/hours, and travel times are still mutually consistent with each other — e.g. a venue scheduled on a day it's actually open, a travel leg that fits inside the day's available time. Resolve any conflicts before presenting; don't leave that check to the user.

---

## Phase 1–2: Where & When (produces 3 draft itineraries)

Ask up front, as a single batch, only what's not already obvious from context:
1. Who's traveling (adults/kids, any mobility or dietary constraints)
2. Rough trip length or date flexibility (fixed dates? a window? a duration only?)
3. Budget tier (shoestring / mid / splurge) — just a tier, not a number
4. Trip character: 2-4 words (e.g. "food + hiking," "history-heavy," "beach reset")
5. Any hard excludes or must-includes (a place they've already been, a place they're set on)

Then, for candidate destinations:
- Pull must-see sites / signature experiences per candidate location using current guidebook-equivalent sources
- Estimate a reasonable day count per location based on those sites (don't pad or compress artificially)
- Draft **3 distinct itineraries** — vary them meaningfully (different regions, different pacing, different mix of must-see vs. white space), not 3 minor variations of the same trip. **If the destination is already fixed, vary pacing/style instead (see operating rules above).**
- Each draft itinerary includes: locations + day split, dates (or date window), 3-5 signature "why this stop" items per location, and the key tradeoff it makes (e.g. "more days in fewer places" vs "wider loop, faster pace")

Present the 3 drafts as a short comparison, not three full documents. User picks one or asks for a blend/adjustment. Iterate here until locked — this is the foundation everything else builds on.

Once locked: write the chosen itinerary into the Master Trip Doc as the Overview + Day Split, and negotiate final day counts per location against must-see lists (what's cut, what's kept, why).

Dates can lock as a specific window up front, or emerge iteratively (e.g. picking a rough window first, then shifting it once flight-price patterns are known in Phase 3) — either is fine as long as the Day-by-Day Skeleton is updated to match whenever dates shift.

---

## Phase 3: Flights

- Once dates/locations are locked, research flight options across a **2–6 month price window** for the given route, not just the exact target dates
- Show a rough price grid (flexible ±3 days depart/arrive) if data supports it, and flag if fares look unusually high/low for the route
- Default to open-jaw / multi-city routing (fly into city A, home from city B) over point-to-point round trip when the itinerary spans multiple cities — flag if point-to-point is actually cheaper/better for this specific case
- Note realistic connection times and red-flag tight connections
- If the user is flexible on which day they fly, check whether shifting the whole window by a day or two aligns with statistically cheaper travel days (e.g. Thursday departures) — this can shift the entire Day-by-Day Skeleton, not just the flight dates, so update both together
- Present 2-3 flight framings (cheapest, best routing, best schedule) — user decides, Claude does not book

---

## Phase 4: Lodging

- Start with neighborhood, not property — research 2-3 neighborhoods per city based on itinerary fit (proximity to must-sees, transit, walkability), present tradeoffs
- Once neighborhood is picked, find 2-3 specific properties per city in that neighborhood
- Source from hotel direct sites, verified travel sites, or direct search — not Booking.com/Expedia aggregator listings and not anything sourced from social media ads (Booking.com and similar OTAs add markup — note the direct-booking price if findable)
- Verify each property's actual location against the map, not just its listed neighborhood name
- Pull reviews from the **last 30-90 days only** — older reviews are noted as unreliable for current conditions and skipped
- Present options with location verified, price range, and a one-line "why this one" — user picks

---

## Phase 5: Point-to-Point Travel

- For every inter-city/inter-region leg in the itinerary, research realistic travel time (train, ferry, car, domestic flight) door-to-door from the locked lodging location, not just transit time
- Flag any leg that's a bad use of a day (e.g. a travel day that eats most of the daylight)
- Where a leg can combine two nearby points of interest more efficiently than routing back through a central hub, check the direct connection between them, not just each one's link back to base
- Adjust day counts per location in the Master Doc if travel time meaningfully changes what's feasible — flag this adjustment explicitly to the user, don't silently change the plan

---

## Phase 6: What to Do

- Split into: big-ticket items (need advance booking — timed tickets, tours, reservations), flexible/walk-up items, indoor options, outdoor options
- Flag anything with known booking lead times (e.g. "book 60+ days out") explicitly with dates — a table of site / day / book-by-date works well
- Deliberately leave white space in the day plan — don't over-schedule; note where white space is intentional vs. where it's still open to fill
- This is where the Master Doc gets its day-by-day detail

---

## Phase 7: Where to Eat

- Focus on regional specialties/what the area is actually known for, not generic "best restaurants" lists
- Cross-reference Reddit/local-forum threads against review sites rather than relying on either alone
- Filter to 4+ star ratings with a meaningful review count (several hundred+) to avoid outlier noise
- Where possible, distinguish "great for tourists" from "where locals actually eat"
- Present a working shortlist per city/day, not an exhaustive list — this feeds directly into the day-by-day plan from Phase 6

---

## Handoff to Cowork — Booking

The Master Trip Doc produced through Phase 7 is a rough draft, not a booking-ready plan. Do not use this template to make actual lodging or airline reservations.

**Treat every price in the rough draft as an estimate that will not survive contact with live inventory.** Planning-phase ranges are directional only — a dry run on a London trip found flights 6–86% and lodging 41–190% above the drafted ranges. Say this to the user when handing off, so the budget expectation resets before booking rather than during it.

Once Phase 7 is complete:
- Confirm with the user that the rough-draft Master Trip Doc is finalized
- If not already in Cowork, recommend switching now — booking needs live search tools
- Point to the `booking-playbook` skill as the next step — that playbook governs re-pricing, staging, and confirming actual reservations
- Treat this as a real stop, not a formality — the rough-draft doc and the confirmed bookings are separate artifacts produced by separate processes

**Full sequence:** this skill (rough draft) → `booking-playbook` (reservations) → `itinerary-build-playbook` (final itinerary document). The itinerary build runs last, once confirmation numbers exist to fold into it.

---

## Master Trip Doc

*(Claude: build this out progressively as phases complete. Keep it scannable — overview + key assumptions, not exhaustive detail. This is the one doc that persists across the whole planning process. Include hyperlinks to official/verified sources where confirmed during research; never invent a URL.)*

### Overview
- Destinations & dates:
- Travelers:
- Trip character:
- Budget tier:

### Day-by-Day Skeleton
*(location, dates, day count, key tradeoff — fill in as Phase 2 locks; re-verify against Phase 6/7 details for consistency before final delivery)*

### Flights
*(routing, price range, dates — fill in Phase 3)*

### Lodging
*(city, neighborhood, property, price range, source — fill in Phase 4)*

### Inter-City Travel
*(leg, mode, time — fill in Phase 5; include a backup/simpler route alongside an optimized one where relevant)*

### Activities
*(big-ticket/book-ahead items with dates, flexible items, indoor/outdoor split — fill in Phase 6)*

### Dining Shortlist
*(by city/day — fill in Phase 7)*

### Open Questions / Blocking Items
*(running list — anything still unresolved)*

