# Travel

A five-stage travel-planning workflow built as Claude skills, plus the guides they produce.

**Live site:** https://brownpt10-cmd.github.io/travel/

## The workflow

| Stage | Skill | What it does |
|---|---|---|
| 1 | `bucket-list-builder` | Interviews you and builds a real bucket list of 10–20 items across three effort horizons |
| 2 | `travel-planning-template` | Seven phases from destination to a complete rough-draft Master Trip Doc |
| 3 | `booking-playbook` | Re-prices everything live and stages each reservation up to (never through) payment |
| 4 | `itinerary-build-playbook` | Builds the polished Word itinerary from confirmed logistics, plus a packing list |
| 5 | `trip-html-guide` | Turns the finished itinerary into a mobile pocket guide or a desktop long-scroll guide |

Each stage is standalone — jump in wherever your trip already is.

## Contents

- `index.html` — the workflow guide, explaining each stage and how to start it
- `skills/` — the five skill folders (plain `SKILL.md` markdown, plus HTML templates)
- `downloads/` — zip archives: all five together, or each one individually
- `munich/` — a real stage-5 output: the Munich Oktoberfest 2026 mobile pocket guide

## Installing a skill

Unzip the folder into `~/.claude/skills/` (Claude Code), or upload the `SKILL.md` as a skill in the Claude app. Then describe what you want in plain language — no configuration, no dependencies.
