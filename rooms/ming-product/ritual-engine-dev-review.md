# MING daily ritual — audience & framing review

Reviewer: Dev. Subject: App `5d0bcf4c384e`, commit `c210670` (the `draft/ritual-engine` build, now published to
[https://ming-daily-ritual.kylon.app](https://ming-daily-ritual.kylon.app) — the draft was released while this review was in progress, so
everything below applies to live copy).

Reviewed against two questions from the owner: does the output read chart-specific rather than horoscope-generic to
someone who uses Co-Star, and does every line stay in conditions-and-choices language rather than prediction.

## Method

I did not read the copy off the screen and form an impression. The engine was imported directly and called the way
`app/api/signal/route.ts` calls it, for four births × ten days:

- A — 1994-03-17 07:42 San Francisco · natal 甲戌 丁卯 壬寅 甲辰 · weak Water day master
- B — 1988-11-02 23:15 London · natal 戊辰 壬戌 辛酉 戊子 · strong Metal day master (late-Zi case)
- C — 2001-06-24 14:05 New York · natal 辛巳 甲午 戊午 己未 · strong Earth day master
- D — 1996-01-09 04:30 Taipei · natal 乙亥 己丑 乙巳 戊寅 · balanced Wood day master

Days: 2026-09-02 → 09-05, 09-07 → 09-09 (across the 白露 term boundary), 09-15, 09-22, 10-10. Generated lines were
confirmed identical in the live UI for A and B on 09-02. All 40 signals, all 72 authored strings verbatim, and the
selection logic are in the capture file:
[ritual-engine-raw-outputs.md](/preview/workspaces/fc8a8e44157c/files/aaf4cfd252f6?room=f8664cffbf96).

## Verdict

The framing holds — nothing here predicts an event, and five lines need rewording rather than replacing. The
chart-specificity claim holds on any single day and fails over time, for a structural reason that is checkable in
about ten lines of code and is the most important thing in this review: **each of the four lines is a pure function of
the target day's pillar, so the reading repeats on a fixed 10-to-12-day loop for every user, forever.** That is not a
copy problem and no amount of new sentences fixes it by itself.

Ship-blocking in my view: items 1, 2 and 4 in the fix list. Everything else can follow.

## 1. Specific vs generic

### Same day, different people — passes cleanly

On 2026-09-02 the four charts produced four different observations, three different themes, three different actions and
three different reflection questions. On 09-04, four different observations again. Nothing in the 40-signal matrix
looks like one text with a name swapped into it, which is the failure mode the brief calls out. Two unrelated charts
never shared more than one of the four lines on the same day.

That is a genuinely better result than the category. A Co-Star user's baseline is text they suspect is not really
theirs — an astrologer quoted in Teen Vogue on the daily notifications: *"I think the alerts are pretty vague and
general… I'm not sure what their basis is for the advice they're giving or if it's even catered to individuals"*
([Teen Vogue, 2019](https://www.teenvogue.com/story/astrology-app-co-star-bizarre-alerts)). MING can answer that
question on screen, which no competitor does — of Co-Star and The Pattern, *"Ask either app why it said what it said
and you get silence"* ([Routines comparison, 2026](https://app.routines.club/blogs/cosmic/the-pattern-vs-co-star)).
"Show the calculation" is the strongest thing in this build and it is currently a small text link under the fold.

### Same person, over time — fails, and predictably

Reading `lib/ming/signal.ts` against `lib/ming/bazi.ts`, once a natal chart is fixed:

| Line | Key depends on | Distinct values that user can ever see | Repeat period |
|---|---|---|---|
| Observation | today's **day stem** only (the load band is a constant per element for a fixed chart) | 10 of the 30 | exactly 10 days |
| Action (`Try`) | today's **day branch** (primary hidden stem) | ≤10 of the 10 | 12 days, often sooner |
| Reflection (`Notice`) | today's **day branch** vs the four natal branches | ≤12 of the 17 | 12 days |
| Theme | the solar **month branch** + a fixed strength band | **5 of the 15, per year** | ~30 days |

So the (observation, action, reflection) triple has exactly 60 states — one per sexagenary day pillar — and repeats
every 60 days for the rest of the user's life. A daily user sees roughly 35 of the 72 sentences in a year, each one
between 30 and 36 times.

The matrix confirms it without needing the code: every one of the four births received its 09-05 observation again on
09-15 (10 days) and its 09-02 observation again on 09-22 (20 days). Four for four. The author's own dispersion check —
120 signals from 3 charts × 40 days, 98% distinct — could not have caught this, because it sampled fewer days than the
cycle is long.

Why this specifically matters for this audience: repetition is the thing they detect and it is the thing they leave
over. A Co-Star review from February 2026: *"For years, I have heartily enjoyed the costar daily takes… but this year
in 2026 things have fallen off a cliff! the daily takes repeat regularly. It's boring. There's no insight… Time to
delete."* ([reviews reproduced at WorldsApps](https://worldsapps.com/reviews-co-star-personalized-astrology)). And on
The Pattern: *"it doesn't really tell me anything new… it'll be like 'hey you're bad at picking mates'… it's not
growing with me"* ([reviews reproduced at JustUseApp](https://justuseapp.com/en/app/1071085727/the-pattern/reviews)).
A ten-day loop is detectable inside the first two weeks — well before the daily habit forms.

### The reflection line is the weak point, and it is the line that ends the ritual

When the day's branch has no clash, harmony or repeat against any natal branch, the key falls back to
`none.<element of the day branch>` — five sentences that **ignore the natal chart completely**. That fallback fired on
18 of the 40 sampled days (45%). Consequences visible in the matrix:

- On 09-04 births A (San Francisco 1994) and B (London 1988) both closed with *"What are you visible for at the moment,
  and did you choose it?"* — identical text, unrelated charts.
- On 09-03 and again on 09-09, births C and D both got `none.Earth`.
- Birth C got the same `none.Earth` question three times in thirteen days.

Chart-to-chart comparison is the invite loop. Two friends screenshotting the same closing question on the same day is a
direct hit on the growth mechanic, not just a copy blemish.

## 2. Framing — is anything predictive

The corpus is clean of hard prediction: no `will`, `going to`, `shall`, and no `luck`, `fortune`, `destiny` or `fate` in
any of the 72 strings. All ten actions are imperatives, all seventeen reflections are questions. The disclaimer runs on
every screen. Against the brief's rule, nothing here says "this will happen."

Two constructions drift, both fixable by rewording:

**Hedged event forecasts, concentrated in the `.scarce` band.** Five observations open by asserting an occurrence rather
than a condition:

- `rival.scarce` — *"Someone may want the same thing you want today, and you are not especially practiced at that."*
  This is the worst line in the set: it is a claim about a third party's wants, on a specific day. It is also the exact
  register that gets Co-Star called paranoid-inducing. Suggested: *"You are not especially practiced at wanting
  something that someone else also wants. Wanting it out loud is not the same as taking it from anyone."*
- `refuge.scarce` — *"Support may arrive today on terms you did not expect."* → *"Support is available today in a shape
  you would not have picked."*
- `opening.scarce` — *"An opening may show up today that does not match how you normally get things."* → *"The openings
  around you today do not match how you normally get things."*
- `pressure.scarce` — *"Something may push at you today without following any rule you recognise."* → *"Pressure today
  is not following a rule you recognise."*
- `voice.scarce` — *"Something you normally leave unsaid may get close to the surface today."* Mildest of the five;
  it is about the reader's own interior, so I would leave it.

The pattern is worth naming to whoever writes the next tranche: whenever the element was scarce, the writer reached for
"something may happen to you," which is forecast grammar. The `.present` and `.saturated` lines almost never do this.

**Certain-outcome claims.** *"Reaching for all of it is the dependable way to end up with none of it"* (`opening.saturated`)
states a result as guaranteed. The others in this family — *"Ten minutes now removes a much larger cost later,"*
*"Absorbing is also what teaches the next demand to arrive sooner"* — are consequences of the reader's own choice, which
is the "choices" half of the frame, and they read fine.

Not prediction, but worth watching: the confident interior-state assertions (*"You already know what you think,"*
*"You are more sure of your own position today than you may be letting on"*) are where Barnum risk lives rather than
where prediction lives. The best-calibrated line in the whole set is `voice.saturated` — *"Your read on the flaw is
probably right and probably early"* — because it hedges and stays concrete at the same time. More of that.

## 3. The theme line holding for ~30 days — intentional or broken

Intentional as computation, broken as presentation. Three reasons, in order of weight:

1. **No time marker, and the surrounding copy contradicts it.** The word "today" appears in 27 of the 30 observation
   strings and in 0 of the 15 theme strings, but the theme sits as an unlabelled second paragraph under a heading that
   says "Today's Signal," in the same weight as the line above it. The reader is given no way to tell a 30-day arc from
   a stuck app. Line 2 never changing is exactly what "this app repeats itself" feels like from the outside.
2. **It is the most repeated line in the product.** Five theme strings per user per year, held about two months each.
   The slowest-moving line is the one with no label explaining why it is slow.
3. **It is carrying "understanding your timing," one of MING's three named areas** (per `INDEX.md`), and burying it as
   an unlabelled paragraph wastes the piece of the product Co-Star structurally does not have — the daily apps are
   built for the ninety-second morning check-in and nothing longer ([Routines
   comparison](https://app.routines.club/blogs/cosmic/the-pattern-vs-co-star)).

Do not make it daily. Label it and date it: a small `SEASON · until 8 October` above the line converts the defect into
the differentiator. The calculation panel already says the right thing; the signal screen should not make the reader
open a panel to learn it.

## 4. Numbers being quoted publicly that are wrong

Flagging these because the founder is repeating them and the app's credibility claim is honesty about its own method.

- **72 authored strings, not 76.** 30 + 15 + 10 + 17 = 72, and all 72 are distinct. (Counting sentences instead gives
  117, so 76 matches neither.) The module exports `COPY_COUNTS`; per-slot counts are correct.
- **Two observation keys are structurally unreachable:** `friend.scarce` and `rival.scarce` require the day-master
  element's natal share below 12%, but the day stem alone contributes at least 12.2% of total load. 70 strings are
  reachable, not 72. 18,000 sampled pairs hit 28 of 30 observations and never these two.
- **"76,500 combinations"** is 30 × 15 × 10 × 17 — a corpus cross-product, not an experience. The number that
  describes a user is 60 distinct (observation, action, reflection) triples, ever, plus five themes a year. Both
  numbers can be stated honestly; only one of them should be said to a founder or a user.
- Minor: the solar-term caveat fires below `0.05°` (~1.2 hours) while its copy says *"within a few hours"*; and the
  element bars saturate at a 38.5% share, so A's Wood 45% and B's Earth 44% draw the same full bar.

## 5. What I would fix, in order

1. **Kill the chart-independent reflection fallback.** 45% of days currently end on a sentence that any two users share.
   Key it on something natal — the chart's scarcest element, or the day branch against the day-master element — so the
   closing question is at minimum different for different people.
2. **Break the 10-day observation loop.** Cheapest real fix, no model call: 2–3 deterministic variants per key, rotated
   by the sexagenary cycle index the engine already computes. 30 keys × 3 moves the personal repeat from 10 days to 30
   and costs 60 more sentences.
3. **Label the theme line as a season with an end date.**
4. **Stop the silent `lat`/`lon` → 0 fallback.** A shared or hand-edited URL missing coordinates currently renders a
   confident chart computed at longitude 0 with no warning. For a product whose entire claim is "computed from your
   exact moment," silently substituting Greenwich is the one failure that cannot be explained away — and the URL is the
   sharing mechanism.
5. **Pin the viewer timezone.** On 2026-09-07 the same chart reads month branch 申 for a London viewer and 酉 for a Los
   Angeles viewer, so the same shared link gives two friends different themes. Use the birth timezone or an explicit
   setting.
6. **Reword the five `.scarce` event-forecast openings** as above.
7. **Correct the public counts** and either write or delete the two dead keys.
8. Promote "Show the calculation" — it is the answer to the category's loudest complaint and it is hidden.

## 6. What this review cannot tell you

Whether the copy lands. I am one more reader who is not the audience, and the register is good enough that I would not
trust my own reaction to it. The test worth running before any of it is polished: give eight to ten people in the
22-38 band their own chart and let them open it daily for two weeks, and record the day on which each of them first
says "wait, I've seen this one." That date is the retention ceiling, and it is currently predicted to be day 10 or 11.
A second, cheaper test: show one person two consecutive days and a friend's same day, and ask which two of the three
belong to the same person. If they cannot tell, the specificity is not reaching the surface even though it is real in
the arithmetic.

## Sources

External, all quoted above at the claim they support:

- Teen Vogue on Co-Star's "Your day at a glance" notifications and the vagueness criticism —
  https://www.teenvogue.com/story/astrology-app-co-star-bizarre-alerts
- Routines, The Pattern vs Co-Star (2026), on the ninety-second daily format, the absence of ritual or journaling in
  both, and neither app explaining its own output —
  https://app.routines.club/blogs/cosmic/the-pattern-vs-co-star
- Co-Star user reviews reproduced at WorldsApps, including the February 2026 "daily takes repeat regularly" review —
  https://worldsapps.com/reviews-co-star-personalized-astrology
- The Pattern user reviews reproduced at JustUseApp, on repetition and "it's not growing with me" —
  https://justuseapp.com/en/app/1071085727/the-pattern/reviews

Internal:

- Verbatim copy inventory, 40-signal matrix, selection logic, URL scheme, screenshots —
  [ritual-engine-raw-outputs.md](/preview/workspaces/fc8a8e44157c/files/aaf4cfd252f6?room=f8664cffbf96)
- Author's derivation and honesty statement —
  [ritual-engine-worked-example.md](/preview/workspaces/fc8a8e44157c/files/ab8de2dd849d?room=f8664cffbf96)
- Brief — [ritual-engine-brief.md](/preview/workspaces/fc8a8e44157c/files/25ad4ded0b21?room=f8664cffbf96)
