---
name: "booking-playbook"
description: "Turn a rough-draft trip plan into confirmed reservations — re-pricing flights, lodging, tickets, and transport live, sweeping airports, staging bookings up to (but not through) payment, and maintaining a booking log. Use in Cowork only, when the user is ready to actually book a trip that has already been planned, or asks to \"book the trip,\" \"price this out for real,\" or \"stage these reservations.\" Never enters payment details or completes purchases."
---

# Booking Playbook

Generic instructions for turning a rough-draft Master Trip Doc into confirmed reservations.
Runs **in Cowork only** — it needs live search tools, and it writes back to files.

**Sequence:** `travel-planning-template` (rough draft) → **this playbook** (booking) →
`itinerary-build-playbook` (final itinerary document).

**The prime rule:** Claude searches, prices, compares, and stages every booking up to the
payment screen. Claude never enters payment details and never completes a purchase. The user
executes every transaction themselves.

---

## 1. Pre-Flight Check

Before searching anything, re-read the Master Trip Doc and confirm:

1. **Dates are actually locked.** If the user is still flexible, stop — flexible dates belong in
   Phase 3 of the planning template, not here. Booking against soft dates wastes the search.
2. **Traveler details are known** — full legal names as they appear on passports, DOB, passport
   number and expiry, frequent-flyer/loyalty numbers, known-traveler/redress numbers. Ask for
   anything missing now; discovering a name mismatch after ticketing is expensive.
3. **Passport validity** — many destinations require 3–6 months validity beyond the return date.
   Check the specific requirement for this destination and flag it.
4. **Entry requirements** — visa, ETA/ESTA-equivalent, vaccination. These have their own lead
   times and can gate everything else. Verify current status by search; do not rely on memory.

Do not proceed past a failed check. A missing passport expiry is a blocking item, not a footnote.

---

## 2. Re-Price Everything Before Booking Anything

**Treat every number in the rough-draft Master Trip Doc as an estimate that has already expired.**
Planning-phase price ranges are directional; they are not what the user will pay. Re-price each
line item live, then compare against the draft and report the delta explicitly.

Where the live price exceeds the draft estimate by more than ~20%, stop and surface it as a
decision, not a footnote. The user budgeted against the draft. Options to present:

- Absorb the increase
- Downgrade the item (different property, different fare class)
- Shift dates, if the trip window still has any give
- Cut something else to fund it

Re-verify the **identity** of every named property, venue, and operator, not just its price.
Hotels get sold and rebranded, restaurants close, tour operators fold. If a name doesn't resolve
in inventory, search for a rename or closure before concluding it's unavailable — and update the
Master Trip Doc with the current name.

---

## 3. Booking Order

Book in this order. Each step constrains the next, and the earlier items are the hardest to
change.

1. **Flights** — sets the true first and last usable day of the trip
2. **Lodging** — depends on confirmed arrival/departure times
3. **Inter-city transport** — depends on confirmed lodging locations
4. **Timed-entry tickets and tours** — depends on the day plan surviving steps 1–3
5. **Restaurant reservations** — last, and easiest to move

Do not book downstream items before upstream ones are confirmed. A ticket bought for a day the
flight later invalidates is a real loss.

---

## 4. Flights

### 4a. Sweep both ends before pricing anything

Run this **before** settling on a route. It is four searches and it regularly changes the answer.

**Origin side — search every airport the traveler could plausibly depart from:**

- The primary hub the draft assumed
- The traveler's *local* airport, even when it has no nonstop service. A through-ticket from a smaller origin sometimes prices *below* the hub, because the fare is constructed differently. It often doesn't — but the check is cheap and the failure mode is silent.
- When the local-origin fare is close to the hub fare, **the comparison is not over.** Add the ground cost of reaching the hub — parking for the full trip duration, shuttle fare both ways, or fuel and the driver's time. A fare that looks $35 more expensive can be $100 cheaper all-in.

**Destination side — search the metro area, not the airport:**

- Query the *city*, not a single airport code, so secondary fields surface on their own.
- If secondary airports return nothing, that is a finding worth recording: it means there is no service on that route, not that the search was skipped. Write it down so the next trip doesn't re-litigate it.
- Where a secondary airport does return results, price the **ground transfer into the city** before comparing. A cheaper fare into a distant airport is frequently a worse deal once transfer time and cost are counted.

**Record the negative results.** "We checked X and it doesn't serve this route" is a real output. Without it, the same question resurfaces every trip and nobody can tell whether it was investigated or overlooked.

### 4b. Price and compare

- Re-run the search live. Present the **total** the user will actually pay, not the headline fare.
- **Watch what a broader search silently swaps.** Widening the query (all airports, more stops) can return a lower headline fare that has quietly changed the itinerary — a red-eye connection, a pre-dawn departure, a second stop. Always diff the *schedule* against the baseline, not just the price. Report the cheaper fare **and** what it costs in usability, then let the user choose.
- **Price the fare class honestly.** Basic economy headline prices are frequently a false
  economy. Compute the real total: base fare + seat selection + checked bag(s) each way + change
  fee exposure. A fare that costs more up front but includes a bag and free changes is often
  cheaper in practice — say so explicitly with the arithmetic shown.
- **Report seat inventory.** If a fare shows low or zero remaining seats, that is a booking-urgency
  signal and belongs in the recommendation.
- **Watch for codeshares.** The same physical flight often sells under several marketing airlines
  at different prices with different baggage rules and change penalties. Compare the marketing
  carriers, not just the operating one.
- Verify the arrival and departure times against the **first and last day of the itinerary**, and
  compute realistic door-to-door timing:
  - Arrival day: landing → immigration/baggage (budget 60–90 min at a major international hub) →
    transit to lodging → earliest realistic check-in
  - Departure day: latest usable activity end → transit to airport → check-in cutoff (typically
    3 hrs before an international departure)
- **Flag any activity the flight times kill.** A morning museum on a departure day with a midday
  flight does not work. Surface this as a conflict, resolve it, and update the day plan.
- Stage the booking to the payment screen and hand off. Note the fare's hold/cancellation window
  (many carriers offer a 24-hour free cancellation) so the user knows how much time they have.

---

## 5. Lodging

- Re-verify the property still exists under the name in the draft. If renamed or sold, confirm
  it's the same physical address before assuming continuity — and check whether reviews predate
  the ownership change, since they may no longer describe the current operation.
- **Compare at least three price sources**: the hotel's own site, and two independent
  aggregators. Report the spread. Aggregator prices frequently differ from each other and from
  direct by material amounts on the same room and dates.
- **Cross-check the guest rating across sources.** A property rated 7.9 on one platform and 8.4 on
  another is a signal to look closer, not to average the two. Prefer the source with the larger
  review count and check whether recent reviews diverge from the lifetime score.
- **Re-run the value comparison against the neighborhood.** The draft picked this property against
  estimated prices. If the live price has moved, the property may no longer be the best option in
  its own neighborhood. Pull the current field and compare price against rating — surface it if a
  better-rated property is now cheaper.
- **Verify transit claims against the map.** If the draft justifies the property by proximity to a
  specific station or line, confirm the actual walking distance and that the station is on the
  line claimed. Nearest-station claims in planning docs are often approximate.
- Capture for the Master Trip Doc: exact rate, taxes and fees, total, **cancellation deadline and
  penalty**, room type, bed type, check-in/check-out times.
- Prefer a free-cancellation rate when the trip is more than a few weeks out and any part of the
  itinerary is still soft. The premium over a non-refundable rate is usually small relative to the
  optionality.

---

## 6. Timed-Entry Tickets and Tours

- Build a table: **site / date / price / booking window opens / must-book-by / booked?**
- **Check the booking window, not just the lead time.** Some venues only release tickets a fixed
  number of weeks ahead. If the window hasn't opened, the item is not bookable yet — set a
  reminder rather than treating it as done.
- **Verify day-of-week operating hours for the specific date, not general hours.** Seasonal and
  day-specific variation is the single most common source of broken itineraries:
  - Weekend hours are often shorter than weekday hours
  - Religious sites close to visitors for services
  - Many museums have one closed day per week
  - Shoulder-season hours differ from summer hours
- **Compute the real time cost of each visit**, including the walk from the nearest station.
  A site "at" a transit stop may be a mile from it. Add this to the day's timing.
- **Check advance vs. gate pricing.** Several venues charge a premium at the door; advance booking
  is both cheaper and safer.
- **Check whether a pass or membership beats individual tickets** for this specific set of sites
  and dates. Show the arithmetic — passes are often worse than they look, and occasionally much
  better.
- Note anything seasonal that *adds* value on these dates (rooms or wings open only part of the
  year) — this is worth telling the user, not just the risks.

---

## 7. Ground Transport

- Price the actual legs, including airport transfers in both directions.
- Note where a stored-value card, contactless payment, or daily/weekly fare cap makes point
  tickets unnecessary — and what the cap is.
- For rail, check whether advance fares are materially cheaper than walk-up, and whether an
  advance fare's inflexibility is acceptable given the day's other bookings.
- Confirm that any connection the day plan depends on actually runs on that day of the week and
  at that time.

---

## 8. Restaurant Reservations

- Book last. Confirm each venue is still trading before recommending it.
- Distinguish venues that **require** a reservation from those that don't take them at all.
  For casual pubs and walk-up spots, say so rather than manufacturing a booking step.
- For solo travelers, note where the bar counter is a better and faster option than a table.
- Check that opening hours cover the meal slot the day plan assumes.

---

## 9. Conflict Resolution Pass

Before handing off, run one pass across every confirmed booking and check:

- Every activity falls on a day the venue is actually open, within its hours for that day
- Travel legs fit inside the available daylight/opening window, walks included
- Arrival and departure day plans survive the confirmed flight times
- No two timed bookings overlap or require impossible transit between them

Where a conflict exists, present it as an explicit choice — Option A (keep the plan, move the
booking) vs. Option B (keep the booking, change the plan) — and state what each displaces.
Do not silently rewrite a day the user has already approved.

---

## 10. Booking Log

Maintain a `{Trip}_Booking_Log.md` in the trip folder recording, for every line item:

| Item | Status | Price | Source | Confirmation # | Cancellation deadline |
|---|---|---|---|---|---|

Status values: **BOOKED** / **STAGED** (ready for the user to execute) / **BLOCKED** (waiting on
something) / **NOT YET BOOKABLE** (window hasn't opened).

Also record, separately from the table:
- **Draft vs. actual price deltas** — this is the feedback loop that makes the next trip's
  planning estimates better
- **Process friction** — what didn't resolve, what was stale, what took extra steps

Write confirmation numbers into the log as the user completes each booking. This log is the
primary input to `itinerary-build-playbook`.

---

## 11. Handoff to the Itinerary Build

Once bookings are confirmed (or staged and consciously deferred):

1. Update the Master Trip Doc with confirmed prices, times, property names, and any day-plan
   changes forced by the conflict pass
2. Summarize for the user: what's booked, what's still open, total committed spend vs. the draft
   estimate
3. **Ask directly whether they're ready to kick off the itinerary build**, pointing to the
   `itinerary-build-playbook` skill

Treat that as a real question with a real stop, not a formality. The itinerary build is a separate
process producing a separate document, and it works best once confirmation numbers exist to
fold in.

