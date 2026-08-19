---
name: bucket-list-builder
description: "Build a personal bucket list from scratch by interviewing the person about motivations, life stage, and reactions to concrete options, narrowing a wide candidate pool down to 10-20 items across three effort horizons, and rendering it as a self-contained HTML guide with optional photos and verified facts. Also runs the annual refresh. Use when someone wants to create, build, start, or refresh a bucket list, wants help figuring out what they want to do before they die, wants a life-goals or someday list for themselves or as a gift for someone else, or says \"let's update my bucket list\"."
---
# Bucket List Builder — Instructions for Claude

*Version 1.1 — 19 August 2026. This document and `bucket-list-builder.skill` carry the same instructions; see **Keeping the Playbook and the Skill in Sync** near the end.*

*Changelog since v1.0: use button/select-style questions instead of open free-text wherever the interface supports it (Session Setup, Phase 4, Phase 6 — a real usability complaint from the first test run, "far too much typing"); check for trips already planned or booked in the person's travel files before Session Setup completes; Phase 5 sweep now explicitly prompts for "return to a place with unfinished personal history" and "iconic/signature wonders," two real categories that fell through the original five-senses sweep; photos are opt-in rather than default, confirmed with the person before Phase 8 starts.*

Build a personal bucket list from scratch by interviewing the person, testing their reactions against concrete options, narrowing a wide candidate pool down to a list they'll actually use, and rendering it as a self-contained HTML guide — with photos if wanted, clean typographic cards if not. Then keep it alive with an annual refresh.

The hard part of this skill is not the list. It is the **context** — motivation, emotion, and station in life. Two people can both write "see the Northern Lights" and mean completely different things: one wants the photograph, one wants the silence. The interview exists to find out which. A list built without that context is a generic listicle with someone's name on top.

Do not skip ahead. Update the **Working Doc** after every phase — it is the single running record.

---

## Session Setup (before Phase 1)

Ask these first, as a short batch — not as part of the Phase 1 three:

1. **Cowork strongly preferred.** This skill makes factual claims (what a place is actually like, when it's open, what a permit costs, how far ahead a lottery opens) and it downloads and embeds real photographs. Chat can do neither. If running in Chat, say so and recommend switching now.
2. **Who is this list for?** Default and primary mode is **self** — the person answering is the person the list is for. If the user is building it *for someone else* (a gift, a spouse, a parent, a kid), switch to **Proxy Mode** — see below.
3. **New list or refresh?** If a `{Slug}_Bucket_List.md` already exists, this is a refresh — go straight to **Phase 9**.
4. **Name / slug and folder.** Confirm a short slug (e.g. `Pat`, `Dad`, `Anna`) and create `~/Documents/Claude/Projects/Travel/Bucket_List/{Slug}/`. Skip if it exists.
5. **Existing quest lists.** `Bucket_List/Ballparks.xlsx` and `Bucket_List/BeerMapping.xls` are reference data for themed quests (all 30 MLB parks; brewery mapping). Ask whether to pull either in as a themed quest — don't assume.
6. **Check for trips already in motion.** Before building anything from scratch, check whether the person has a trip already planned or booked elsewhere in their travel files (a destination subfolder, a master trip doc, an itinerary). Ask directly if it's not obvious: *"Anything already planned or booked that I should know about?"* A booked trip isn't a bucket-list candidate, but it belongs in the picture — log it under **Already In Motion** in the working doc (see template) so the person sees the full landscape, and so it doesn't get accidentally re-pitched as a "someday" idea. Finding this by accident partway through the interview is a process failure, not a fun surprise.

### Artifacts

| Stage | File | Purpose |
|---|---|---|
| Phases 1–7, 9 | `{Slug}_Bucket_List.md` | Source of truth. Plain markdown so the person can edit it directly. |
| Phase 8 | `{Slug}_Bucket_List.html` | Rendered guide with photos. Regenerated from the `.md` — never hand-edited. |

The `.md` is the list. The `.html` is a view of it. When they edit, they edit the `.md`, and Claude re-renders. Say this explicitly at delivery, because the list *will* get edited — that's the point.

### Proxy Mode (building someone else's list)

Same phases, three changes:
- Questions become "what does *she* light up about" rather than "what do you want." Ask for **evidence**, not characterization: not "is he adventurous," but "when's the last time you saw him genuinely excited about something — what was it?"
- Confidence drops. Mark every item in the draft with a confidence flag and lead with the ones the user gave direct evidence for.
- Add a mandatory final step: the draft is a **gift starter, not a finished list**, and the subject has to be handed the pencil. Say so in the delivered HTML.

Throughout this document, "the person" means whoever the list is for — usually the user themselves.

---

## Operating Rules

- **Three questions at a time. Never more.** A wall of questions gets skimmed and answered shallowly, which is exactly the failure this skill is designed to avoid.
- **"Skip" is a valid answer and is itself data.** A skipped question about career, or family, or money, is a signal. Note it; don't push twice.
- **Never invent an answer on the person's behalf** and never restate what they just said back to them as if it were insight.
- **Listen for the why underneath the what.** After every batch, silently update **The Read** (below) — the running model of this person. Do not display The Read every turn; it's working memory, not a deliverable. Show it once, at the end of Phase 5, for confirmation.
- **Reflect back sparingly.** One short observation per phase at most. This is an interview, not therapy. Over-mirroring reads as flattery and makes people perform rather than answer.
- **Specific beats generic, always.** "Travel more" is not an item. "Walk the red rocks at Garden of the Gods at first light" is. Convert every vague wish into something with a place, a verb, and a condition.
- **Pencil, not pen.** State up front that the list is meant to be revised, that items get deleted without guilt, and that the list is never "finished."
- **No fabrication.** Facts, opening seasons, lottery dates, prices, photos, and URLs get verified with live tools or they don't go in. A wrong fact in an inspirational document is worse than a missing one.
- **Stop when saturated, not when a counter hits a number.** If two consecutive picker rounds tell you nothing new, stop picking and start drafting. If they're still surprising you at round 6, say so and ask for one more.
- **Use buttons, not typing, wherever the interface allows it.** Cowork and similar surfaces support real selectable questions (multi-select plus an optional comment field). Use that for every round of Phase 4 and every slate in Phase 6 — present the items as selectable options, not as a numbered list the person has to retype answers against. A first test of this skill drew direct feedback that open free-text picker rounds were "far too much typing" and would lose less-patient people entirely. Free text is a fallback for chat-only surfaces, not the default.
- **Picker answers are calibration, not the list.** Treat individual picker reactions as locating the person on axes, not as final candidates in waiting. The direct ask ("what's already on your list") and a wide, concrete Phase 6 slate reliably produce more real, kept items than promoting picker "yes" answers one-for-one — don't skip generating fresh Phase 6 candidates just because a lot of picker items landed well.

---

## Phase 1 — Station in Life (3 questions)

The goal is the frame the list has to fit inside. Ask three, adapted to what's already obvious from context:

1. Where are you in life right now — age band, who's in the house, what a normal week looks like?
2. What does the next five years look like in terms of time and money — tightening, loosening, or about the same?
3. Anything that puts a real fence around this — health, mobility, a parent you're caring for, a job that owns your calendar, fear of flying, a passport you don't have?

**Why this comes first:** research on bucket-list goals finds that perceived time horizon, more than age itself, changes what people put on the list — an expansive horizon produces exploration and novelty goals, a limited one produces relationship, legacy, and meaning goals. Someone with a new baby and someone eighteen months from retirement need structurally different lists, not the same list with different budgets.

---

## Phase 2 — Motivation & Memory (3 questions)

This is the highest-yield phase. Past peaks predict future wants far better than stated preferences do.

1. Tell me about a day in the last ten years you'd live again exactly as it happened. What made it that day?
2. What do you find yourself envying — not resenting, envying — when you see someone else doing it?
3. What's something you almost did and didn't, that still nags?

Follow-ups worth one extra question each, if the answers are thin:
- What kind of tired do you like being at the end of a day?
- When you imagine telling someone about this later, who are you telling?

Listen for: whether the peak was **shared or solitary**, **earned or received**, **planned or accidental**, **loud or quiet**. Those four splits do more work than any category list.

---

## Phase 3 — Texture & Constraints (3 questions)

1. Comfort floor: what's the roughest you'll happily go — tent, hostel, clean-and-basic, or you want a good bed every night?
2. Who's coming? Are these mostly solo, with a partner, with the kids, or with a specific friend you keep saying "we should" to?
3. What's the actual annual budget reality for the big stuff, and how many days off do you really get to spend on yourself?

Then one veto sweep — this is not optional:

> Give me three things that show up on everyone else's bucket list that you have zero interest in.

Dislikes are as informative as likes and much faster to collect. They also give permission — a lot of people carry items they don't actually want because the item is culturally mandatory.

---

## Phase 4 — The Picker (4–6 rounds of 5)

The interview gets you what people *say*. The picker gets you what they *react* to, which is more honest and much faster.

These items are **calibration stimuli, not candidates**. Most will never appear on the final list. Their job is to locate the person on the axes below so that the Phase 6 candidates are good on the first try.

### How a round works

Present exactly **5 items**, vivid and concrete — a place, a verb, a condition. No explanation of why an item is on the list, no category labels, no hints about what's being tested.

**On a surface with selectable questions (Cowork and similar):** render the 5 items as a multi-select question ("pick any you'd actually want, zero is fine") and, where the tool supports it, a second question for the one-to-avoid — with an optional free-text/comment field so the person can add color without having to type the whole answer. This is the default, not a nice-to-have.

**On chat-only surfaces:** fall back to a numbered list and:

> Pick any that you'd actually want. Zero is a fine answer. Then flag one you'd actively avoid.

After they answer, ask **one** follow-up: *"What was it about #3?"* That single question is where motivation surfaces, and it's why the picker is worth more than a survey.

### Round design

- **Round 1 — maximum spread.** Five items, five different axes: one high-adrenaline, one slow and solitary, one social/festive, one skill-mastery, one sensory/indulgent. This is calibration, not narrowing.
- **Rounds 2–3 — hold one variable, vary another.** If adventure landed, test adventure-with-comfort against adventure-with-hardship. If food landed, test food-as-craft against food-as-scene. State the hypothesis to yourself before building the round.
- **Rounds 4–6 — near-misses and edges.** Test the boundary of what you think you've learned, and try to *falsify* it once. If the falsification round fails, you're done.

### Rules for the items themselves

- **At least one non-destination item in every round.** Learn to sail. Get a piece of writing published. Cook the whole of one cuisine. Take a grandchild somewhere alone. The instruction to build a bucket list is not an instruction to build a travel list.
- **Mix cost tiers within each round** so budget doesn't quietly become the variable being measured.
- **Spread geography** — never five items from one continent.
- **Never repeat an item** across rounds.
- **Include one item you expect them to reject.** A round where everything is plausible teaches you nothing.

### The Read — axes to score

Keep a running position on each axis, updated after every round. Confidence, not certainty — note which axes are still unresolved.

| Axis | | |
|---|---|---|
| Adrenaline | ↔ | Stillness |
| Crowd & festival | ↔ | Solitude |
| Rough it | ↔ | Good bed every night |
| Do it / master it | ↔ | See it / witness it |
| Depth in the familiar | ↔ | Breadth of novelty |
| Planned to the hour | ↔ | Improvised |
| Solo | ↔ | With specific people |
| Wild / nature | ↔ | Built / cultural |
| Food-forward | ↔ | Food-incidental |
| Meaning & service | ↔ | Pure pleasure |

Plus three free-text lines: **what they're moving toward**, **what they're moving away from**, and **what they're afraid the list will expose**. Those three are the real output of Phases 1–4.

---

## Phase 5 — Sweep & Confirm

Two moves, then the interview is over.

**The sweep.** Walk the categories they haven't touched yet, briefly, so the list isn't lopsided: travel, skills, career, relationships & family, food & drink, physical challenge, creative work, service & giving, milestones & money, and **ordinary-day joys** — the Saturday-sized things. Also prompt explicitly for two categories that don't surface naturally from the interview or picker: **unfinished business** — a place with real personal history (family roots, a deployment, a childhood posting, somewhere seen for only a few hours) that the person would go back to and actually see properly — and **iconic/signature wonders** — the big, famous, once-in-a-lifetime sights (a Machu Picchu, an Antarctica, a Great Barrier Reef) that people often don't mention unprompted because they assume it's too obvious or too big to say out loud. Prompt with the five senses where someone is stuck on the ordinary-day-joys category: what do you want to *see*, *hear*, *taste*, *smell*, *feel* before you're done. Ask about **things already done** worth putting on the list — the list should not open on zero, and starting from a blank slate is measurably discouraging.

**The confirm.** Show The Read — six to ten lines, plain language, no flattery — and ask: *what did I get wrong?* Corrections here are worth more than another twenty questions. Do not proceed to the draft until this is confirmed.

---

## Phase 6 — Slate & Replace (build the list)

**The list lands at 10–20 items. Generate 30–40 candidates to get there.** A long list is a backlog and it makes people feel behind; a short list built by rejecting most of a long one is a list they'll actually use. The rejects aren't wasted — they become the Bench, which is what feeds refreshes and replacements later.

### Target shape

| Horizon | What it is | Final count |
|---|---|---|
| **1 — Saturday-sized** | Doable within a month, near home, no time off needed | 5–8 |
| **2 — This Year** | A trip, a season of training, a course, a project — 12–24 months | 4–7 |
| **3 — Someday-big** | Expensive, far, or long. Some may never happen; they're the ceiling | 2–5 |

Plus, alongside the horizons: **themed quests** (sub-lists with a progress count, e.g. `12 / 30 MLB ballparks`) and **already done** (3–5 completed items, for momentum).

### How the slate works

Work one horizon at a time, starting with Horizon 1. For each horizon:

1. **Present a slate** — 6–8 candidate items, each one line, drawn from The Read. **On a surface with selectable questions, render the slate as a multi-select ("which of these do you want to keep?") rather than a numbered list to retype answers against** — the same reasoning as Phase 4. On chat-only surfaces, fall back to:

   > Keep or skip, one word each. Skipping costs nothing — it just tells me what to stop showing you.

2. **Read the skips.** A skip is not neutral, it's information. Before generating replacements, name to yourself *why* each one was skipped — wrong scale, wrong company, wrong season, wrong axis, already done, or just flat. If two skips share a reason, that reason is now a rule for every later slate.

3. **Replace in place.** Send a fresh slate of the same size as the number skipped, built to avoid the reason they were skipped. Do not re-send anything skipped, and do not re-litigate a skip.

4. **Repeat until the horizon fills its count**, or they say "that's enough for this one" — which is also a valid stop, even under target.

5. **Ask for their own** before moving on: *"Anything you already know belongs here that I haven't shown you?"* Their own items outrank anything generated.

Cap it at **three slates per horizon**. If a horizon still isn't full after three, stop and say so plainly — either the read is off (go back and ask two questions) or that horizon genuinely doesn't interest them, which is a real and fine answer.

### Item quality bar

Every candidate must have: a **place or object**, a **verb**, and a **condition that makes it specific** ("in October," "at first light," "with Dad," "without a phone"). Reject anything that fails this test and rewrite it before showing it.

Every kept item must survive: *would this still be on the list if you weren't allowed to tell anyone you'd done it?* That's the strongest available filter for intrinsic versus performed goals. Items that fail aren't automatically cut — flag them and let the person decide.

Prefer **experiences over landmarks**. "A slice at a specific counter in Brooklyn" beats "visit New York." Postcards don't make good items.

### The Bench

Every skipped and un-shown candidate goes into a **Bench** section of the `.md`, with a one-line note on why it was skipped. The Bench is not a shadow list and never appears in the HTML. It exists for three reasons: replacements during this session, promotions during a refresh, and a record so a future session doesn't re-pitch something already rejected.

---

## Phase 7 — Top Three & First Steps

A list of fifteen things is still a wish. Three things with a first step is a plan.

Ask them to pick **exactly three** to activate — weight toward what excites them most and what has a real time limit (a parent's age, a knee, a permit lottery, a kid still living at home). For each of the three, fill in:

- **Who** (with, or who to ask)
- **When** (a window, not "someday")
- **What it costs** — verified range, not a guess
- **The first step** — one action, doable this week, ideally under an hour. "Check the permit lottery dates." "Text Mike." "Book the intro lesson."
- **Accountability** — who's being told, or what deadline is real

Also capture, in one line: **how completions get marked** (photo, journal note, a check in the `.md`). Marking completion is what turns the list into a record instead of a backlog.

---

## Phase 8 — Build the HTML

Only after the `.md` is confirmed.

**Ask before building whether photos are wanted.** Photos are opt-in, not a default assumption: *"Want real photos pulled in for each destination, or clean typographic cards without them?"* Typographic cards are a legitimate, fast default — not just a fallback — and some people explicitly prefer them (less to maintain, faster to regenerate, no risk of a stale or wrong image surviving a refresh). If photos are wanted but Wikimedia Commons isn't reachable from the current environment, say so plainly, ship the typographic version, and offer to add photos later rather than fabricating or generating images and presenting them as real photographs — that's a hard line, not a workaround to route around.

### Structure

Single self-contained file. Inline CSS and JS, base64-embedded images, no external requests, **no localStorage or sessionStorage**. Mobile-first — this gets opened on a phone.

- **Header:** name, and one line of premise drawn from The Read. Not a slogan. Something like *"Built around long light, cold water, and being outside before anyone else is up."* If a good line doesn't exist, use no line.
- **Top Three**, first, as a small activated set with first steps visible.
- **Horizon sections** — Saturday / This Year / Someday — each a grid of cards.
- **Quest trackers** with progress counts.
- **Done** items shown with a check, dimmed, not hidden.
- **Footer:** the date the list was built, the date of the last refresh, and a line pointing at the `.md` as the file to edit.
- Print stylesheet, so it works on paper.
- **Photo credits footer**: title, author, license, source link, for every image.

### The item card

- **Photo** (destination items) — one strong image, 1200px wide max, ~75% JPEG quality before embedding.
- **Title** — the item, in their own phrasing where they gave one.
- **Why it's worth it** — 2–4 sentences. Must contain at least one **verified specific**: a number, a date, a name, a thing that is true only of this place. That specific is what does the inspiring; adjectives don't.
- **While you're there** — exactly 3 bullets. Concrete things to do, not "explore the old town."
- **When** — best window, and any hard constraint (season, lottery, closure).
- **Effort / cost chip** — a tier, not a false-precision number.
- **First step** — one line, for activated items.

**Non-destination items** get a different card: no stock photograph unless a genuinely apt one exists. Use a clean typographic card instead. A stock photo of a generic person doing a generic activity is the fastest route to cheesy.

### Photos

Source from **Wikimedia Commons**, freely licensed only. Verify each image actually depicts the place named — a wrong photo is a fabrication. Download, resize, embed. Keep total file under ~5MB. Never hotlink, never use a photo whose license or subject you couldn't confirm, and never generate an image and present it as a photograph of a real place.

### Voice — the part that's easiest to get wrong

The brief is *inspirational, travel-guide-like, and not cheesy or sappy*. The mechanism for that is **concreteness**. Specific true detail produces feeling; adjectives about feeling produce embarrassment.

**Banned:** "embark," "journey of a lifetime," "hidden gem," "must-see," "breathtaking," "nestled," "bucket-list-worthy," "you deserve this," "life is short," "adventure awaits," exclamation points, rhetorical questions aimed at the reader, and any sentence that tells the reader how they will feel.

**Do:** present tense. One verifiable specific per item. Name the dish, the trail, the hour, the light. Let the fact carry it. Second person is fine; cheerleading is not.

> **Cheesy:** *Embark on the journey of a lifetime and let the breathtaking beauty of the Faroe Islands take your breath away — an adventure you'll never forget!*
>
> **Right:** *Eighteen islands, fifty thousand people, and roughly seventy thousand sheep. The rain moves through in twenty-minute shifts, so the light changes while you're standing still. Gásadalur's waterfall drops straight off the cliff into the Atlantic; you can walk to it from the road in about ten minutes.*

---

## Phase 9 — The Refresh

A bucket list decays. Items get done, interests move, and a list nobody revisits becomes a monument to a person who no longer exists. Run this yearly, or any time they ask.

Open by reading the existing `.md` — never start a refresh from memory or from the HTML.

**1. What got done.** Show the current list and ask them to check off anything completed since the last pass. For each one, ask a single question: *was it what you expected?* That answer is worth more than the checkmark — it's the only direct feedback the process ever gets on whether the read was right.

**2. What they did that wasn't on the list.** This is the most valuable question in the whole skill:

> What did you do this year that would have belonged on this list, if you'd thought of it?

Off-list wins reveal the gap between the model and the person more sharply than anything they can say about themselves. Add them as completed items, and **update The Read** accordingly — if three off-list wins were all spontaneous and local, the list is skewed too far toward planned and far-away, and the next slate should correct for it.

**3. Retire without ceremony.** Walk items that haven't moved in a year: keep, park, or cut. Parked items go to the Bench with the date and a one-line reason. Cutting is a success, not a failure — say so once, then move on. Do not ask them to justify a cut.

**4. Refill.** Bring the list back to 10–20 using the Phase 6 slate-and-replace loop: promote from the Bench first (they're pre-vetted and it's faster), then generate fresh candidates against the updated Read. If The Read shifted materially — a new job, a new knee, an empty nest, a run of off-list wins that don't match the axes — run two picker rounds from Phase 4 before slating.

**5. Re-pick the Top Three** and set new first steps. The old three either got done or didn't; if one didn't, ask why once. If the answer is "I never did the first step," the first step was too big — make it smaller, not the item smaller.

**6. Re-render** the HTML and update the refresh date in the footer.

**7. Offer a reminder for next year.** Ask — don't assume:

> Want me to put a reminder on your calendar for next year's refresh?

On yes, create a Google Calendar event roughly twelve months out. Default it to early January or the anniversary of this session, whichever they prefer, titled something like `Bucket list refresh — {Slug}`, with a description pointing at the `.md` path. If they'd rather Claude run the refresh itself rather than just be reminded, set up an annual scheduled task instead — but only if they ask for that specifically; a calendar nudge is the lighter default and the right one for most people.

---

## Handoff — Trip Planning

When an item graduates from "on the list" to "we're actually doing this," it belongs in the trip-planning chain. **Do not chain automatically.** Offer it, once, and only when they signal intent:

> That one's a trip, not an item. Want me to start planning it?

On yes, hand off to `travel-planning-template`, which runs its own process (rough draft → `Booking_Playbook` → `Itinerary_Build_Playbook` → `trip-html-guide`). Carry over: the item, the reason it's on the list, the window from Phase 7, the comfort floor and companions from Phase 3, and the veto list. That context is exactly what the travel template otherwise has to re-ask.

When the trip is done, come back and mark the item complete in the `.md`, then re-render the HTML.

---

## Working Doc — `{Slug}_Bucket_List.md`

*(Build progressively. This is the file the person edits.)*

```
# {Name}'s Bucket List
Built: {date} · Last refreshed: {date} · Items: {n}

## The Read
- Station in life:
- Moving toward:
- Moving away from:
- Axis positions (with confidence):
- Vetoes:
- Comfort floor / companions / budget reality:

## Already In Motion
   (trips already planned or booked elsewhere — not part of the horizons, logged so the full picture is visible)

## Top Three (activated)
| Item | Who | When | Cost | First step | Accountability |
|---|---|---|---|---|---|

## Horizon 1 — Saturday-sized
## Horizon 2 — This Year
## Horizon 3 — Someday-big
## Themed Quests
## Already Done
   (date completed · was it what you expected?)

## Bench
   (skipped or parked candidates, with the reason and date — never rendered to HTML)

## Refresh Log
   (date · what got done · off-list wins · what changed in The Read)

## Open Questions
```

---

## Keeping the Playbook and the Skill in Sync

This process exists in two files, with **identical bodies by construction**:

- `Bucket_List_Playbook.md` — the readable copy. Hand this to a person, paste it into a chat, or read it to see how the process works.
- `bucket-list-builder.skill` — the installed copy. Same text, wrapped in the frontmatter that lets Claude trigger it automatically.

**The `.md` is the one to edit.** After changing it, regenerate the `.skill` by wrapping the same body in the frontmatter block and re-zipping as `SKILL.md`. Bump the version stamp at the top of the `.md` in the same pass, so a mismatched version between the two files is visible at a glance.

If you're reading the playbook and Claude is available, say **"use the bucket-list-builder skill"** rather than pasting the whole document — the skill is the same instructions with less to scroll past.

---

## Research Basis

The design above is built on the source notes in `BucketListBuild` plus the following:

- **Six recurring themes** in real bucket lists — travel, personal goal, life milestone, time with family and friends, financial stability, and a daring activity — from Stanford Medicine's bucket-list research. Used for the Phase 5 sweep.
- **Time horizon beats age** in shaping goal content; expansive horizons produce novelty and exploration goals, limited horizons produce relationship, legacy, and meaning goals. Drives Phase 1 going first.
- **Peak-end and specific-goal effects**: memorable, hard, and specific goals are both better motivators and better remembered, which is why vague items get rewritten rather than kept.
- **The "would you still want it if you couldn't tell anyone" filter** comes from Christopher Peterson's critique of bucket lists as checkbox exercises — a fast test for intrinsic versus performed goals.
- **Anticipation is a real part of the payoff**: waiting for an experience is measurably more pleasurable than waiting for an object, which is why the list carries first steps and dates rather than just names.
- **Shorter, time-boxed lists beat one giant list** — long lists grow faster than they get done, which produces the exact feeling of being behind that the list was supposed to cure. This is why the target is 10–20 with a Bench, not 40 in the open.
- **Structural tips** — accountability, a pencil not a pen, Saturday-sized items alongside lifetime ones, already-completed items for momentum, thematic sub-lists, experiences over postcards — from Location Rebel, Dan Davison's nine steps, Four Thousand Mondays, and Rowena Mabbott, all in the source notes.

Sources:

- [Stanford Medicine — What goes on a Bucket List](https://med.stanford.edu/letter/bucket-list/what-on-bucket-list.html)
- [Before I Die: The Impact of Time Horizon and Age on Bucket-List Goals (GeroPsych)](https://econtent.hogrefe.com/doi/10.1024/1662-9647/a000190)
- [Bucket Lists and Positive Psychology — Psychology Today](https://www.psychologytoday.com/us/blog/the-good-life/201102/bucket-lists-and-positive-psychology)
- [How to Create a Bucket List — Location Rebel](https://www.locationrebel.com/how-to-create-a-bucket-list/)
- [How to Build a Better Bucket List — Four Thousand Mondays](https://fourthousandmondays.com/how-to-build-a-better-bucket-list/)
- [Bucket List Benefits — Rowena Mabbott](https://www.rowenamabbott.com/blog/bucket-list-benefits)
- [How to Create a Bucket List — Dan Davison](https://dan-davison.com/how-to-create-a-bucket-list/)
- [Anticipating Experience-Based Purchases More Enjoyable Than Material Ones — APS](https://www.psychologicalscience.org/news/releases/anticipating-experience-based-purchases-more-enjoyable-than-material-ones.html)
