# Work order: MING daily-ritual engine

Owner: Mira. Reviewer: Dev. Start immediately; this brief is complete.

## Product context (confirmed by founder zhihao feng, 2026-09-01)

MING turns a person's birth date, time, and location into a Four Pillars (BaZi) chart and translates it into plain contemporary language. Users do not see raw technical terms — Heavenly Stems, Earthly Branches, Ten Gods — up front. The app covers three areas: understanding yourself, understanding your timing (the personal "season" you are in), and understanding relationships (chart-to-chart comparison, which is also the invite loop).

The retention habit is a two-minute daily ritual with exactly four parts:

1. One personal observation
2. One current theme
3. One useful action
4. One reflection question

The founder's own example of the register:

> Today's Signal — You may feel pressure to respond before you understand what you actually want. Delay is not avoidance today.
> Try: Leave one decision open until tomorrow.
> Notice: Who becomes uncomfortable when you take your time?

## Non-negotiable framing

Never predictive. Never "this will happen." Always "these are the conditions around you, here is how you might work with them." No fortune-telling voice, no generic good-luck content, no old-fortune-telling-website aesthetics.

## Audience

English-speaking, astrology-curious people roughly 22-38 who already use Co-Star, The Pattern, CHANI, Human Design, Enneagram, attachment styles, or journaling apps, and who know little or no BaZi. Copy must land for someone who has never heard of BaZi. Secondary audience: Chinese diaspora users whose family mentioned BaZi but nobody ever explained it relevantly.

## Deliverable

A working tool — not a mockup — that:

- Accepts a birth date, birth time, and birth location, plus a target day.
- Computes the Four Pillars chart for those inputs.
- Generates the four-part signal for that day from the interaction between the natal chart and the current year, month, and day pillars.
- Produces different, chart-specific output for different people and different days. Identical text regardless of input is a failure of this task.

Done looks like: zhihao feng can open it, enter real details, and see a real generated signal; you can show your calculation for at least one worked example; and you state plainly which parts are real computation and which parts are language templating.

## Data and honesty rules

- Read `/workspace/shared/knowledge/INDEX.md` first.
- No external connections are available for this work. Do not claim access to any.
- Do not fabricate astrological rules. If you rely on a public BaZi calculation reference, cite its exact source link on the claim it supports.
- Do not invent user numbers, testimonials, or launch dates.

## Review

When the deliverable is ready, @mention Dev (already a member of this thread) for a review against audience truth and framing: does the output read as specific rather than horoscope-generic to someone who uses Co-Star, and does every line stay in conditions-and-choices language rather than prediction? Tell Dev to end the review by @mentioning you with the verdict — a reply that does not @mention you will not wake you. When the review arrives, fold it in. One review round-trip, then deliver.

## Owner rules

Do not re-ask for context already in this brief or in workspace knowledge. Run no discovery or welcome of your own. Create no agents and no rooms. Ask zhihao feng only if genuinely blocked.
