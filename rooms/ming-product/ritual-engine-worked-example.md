# MING daily-ritual engine — worked example and honesty statement

Companion to the working tool. Everything below was produced by the same code path the
app runs; no number here was typed by hand.

## What is computation and what is templating

**Real computation — derived from the birth moment, different for every input:**

- Apparent solar position from the full VSOP87 series (via the `astronomia` port of
  Jean Meeus, *Astronomical Algorithms*, 2nd ed.), used to locate solar terms to the second.
- Historical time-zone resolution, including daylight saving and wartime rules, from the
  runtime IANA database — not a fixed offset.
- True solar time: longitude offset plus the equation of time.
- All four pillars: year, month, day, hour.
- Hidden stems, Ten God relations, five-element load, day-master strength, and the
  clash / harmony / repeat relations between the day's branch and the natal branches.
- The four **selection keys** that choose which sentences appear.

**Language templating — authored once, selected by those keys, never generated at runtime:**

- 90 observation sentences (30 keys × 3 rotating variants), 15 season sentences, 10 actions,
  27 reflection questions — **142 authored strings, all of them reachable.**
- No language model is called when a signal is produced. The same chart and the same day
  always yield the same four lines.
- The number that describes a *corpus* is 142. The number that describes a *user* is the one
  worth quoting: for one chart, **271 distinct four-line readings in a year**, and the lead
  observation first repeats after **30 days**. Both are measured by `pnpm test:ming`, not estimated.

**MING's own scoring, not classical rule — flagged in the app as well:**

- Hidden-stem weights of 0.6 / 0.3 / 0.15 by qi layer. The canonical source records the
  *order* of hidden stems but explicitly states the table
  ["does not establish 60/30 or another universal" weighting](https://wiki.openfate.ai/en/bazi/hidden-stems-roots/twelve-branch-hidden-stems-table)
  and that "a product using numerical weights must document its separate algorithm".
- Day-master strength bands (weak below 0.32, strong above 0.48), set so the three readings
  occur at roughly equal rates across 4,000 random birth moments.
- Whether an element counts as scarce, present or saturated is decided by its **rank within
  that chart**, not an absolute threshold. An absolute cut could never call the day master's
  own element scarce — the day stem alone contributes at least 12.2% of total load — which
  left two observation keys permanently unreachable.
- The mapping from a Ten God to a piece of English copy. That is editorial, not traditional.

**Not built:** relationship / chart-to-chart comparison, luck pillars, and any account or
sync. Birth details live in the browser and in the URL, nowhere else.

## Worked example

Birth: **17 March 1994, 07:42, San Francisco.** Target day: **2 September 2026.**

### Step 1 — the birth moment as an absolute instant

`1994-03-17 07:42` in `America/Los_Angeles`. The IANA database gives UTC−08:00 for that date
(Pacific Standard Time; US daylight saving began 3 April that year), so the instant is
**1994-03-17T15:42:00Z**.

### Step 2 — true solar time

| Term | Value |
|---|---|
| Longitude −122.4194° × 4 min/° | −489.68 min |
| Remove the zone offset (−480 min) | +480.00 min |
| Equation of time | −8.38 min |
| **Net correction** | **−18.05 min** |

Pillars are therefore read at **07:24 sun time** on 17 March 1994. The convention that clock
time must be converted before the hour pillar is set is described by
[MingMing3](https://mingming3.com/en/bazi/articles/bazi_format) and
[bazi8](https://bazi8.net/en/learn/methodology).

### Step 3 — year pillar

The BaZi year turns at Lichun, the instant the sun reaches 315° apparent ecliptic longitude —
not 1 January and not Lunar New Year
([MingMing3](https://mingming3.com/en/bazi/articles/bazi_format),
[bazi8](https://bazi8.net/en/learn/methodology)).

Computed Lichun 1994 = **1994-02-04T01:30:56Z**. The birth is after it, so the BaZi year is 1994.
With 1984 as a *jiazi* year, (1994 − 1984) mod 60 = 10 → stem index 0, branch index 10 →
**甲戌 Jia Xu**.

### Step 4 — month pillar

Sun at birth = **356.820°**, which is 41.820° past Lichun → the second of twelve solar months,
branch **卯 Mao**. Month branches follow the twelve *jie* terms, not lunar months
([MingMing3 solar-term table](https://mingming3.com/en/bazi/articles/bazi_format)).

The Five Tigers rule takes a 甲 year stem to a 丙 stem at the Yin month, advancing one stem per
month ([OpenFate](https://wiki.openfate.ai/en/bazi/calendar/five-tigers-method)); one step on
gives **丁卯 Ding Mao**.

### Step 5 — day pillar

Julian Day Number at noon of the effective date 1994-03-17 = **2449429**.
Using stem = 1 + mod(JDN − 1, 10) and branch = 1 + mod(JDN + 1, 12), anchored on
27 January 2019 = *jiazi*
([ytliu0, Chinese Calendar](https://ytliu0.github.io/ChineseCalendar/sexagenary.html)):

- stem = 1 + mod(2449428, 10) = 9 → 壬 Ren
- branch = 1 + mod(2449430, 12) = 3 → 寅 Yin

**壬寅 Ren Yin.** The day stem 壬 is the day master — the character the whole reading measures
against.

### Step 6 — hour pillar

07:24 sun time falls in the 辰 Chen double-hour (07:00–08:59). The Five Rats rule takes a 壬 day
stem to a 庚 stem at Zi hour ([OpenFate](https://wiki.openfate.ai/en/bazi/calendar/five-rats-method));
advancing to Chen gives **甲辰 Jia Chen**.

### The chart

| Year | Month | Day | Hour |
|---|---|---|---|
| 甲戌 Jia Xu | 丁卯 Ding Mao | 壬寅 Ren Yin | 甲辰 Jia Chen |

Element load across the eight characters plus hidden stems:
Wood 45%, Fire 19%, Earth 17%, Water 15%, Metal 4%.
Support 0.45 against drain 7.90 — a **weak** Water day master, unsurprising for 壬 water born in
a Wood month with two 甲 stems drawing on it.

### Step 7 — reading 2 September 2026 against it

Day pillar for that date is **己卯 Ji Mao**; the season branch is **申 Shen** (Metal).

| Line | Selection | Key |
|---|---|---|
| Observation | Today's stem 己 (Earth) against a 壬 Water day master is Direct Officer 正官 — structure and expectation. Earth ranks in the middle of this chart's five elements, so "present". Variant 3 of 3 today. | `structure.present` |
| Theme | Season branch 申 is Metal; Metal generates Water, so it stands in the *resource* direction to the day master, read against a weak day master. | `resource.weak` |
| Action | Primary hidden stem of today's branch 卯 is 乙 Wood → Hurting Officer 傷官 from a Water day master. | `voice` |
| Reflection | Today's branch 卯 forms a six-harmony with the natal **year** branch 戌 (卯戌 is one of the six harmonies). | `harmony.year` |

Ten Gods are derived by five-phase direction plus yin-yang polarity
([OpenFate](https://wiki.openfate.ai/en/bazi/ten-gods/how-the-ten-gods-are-derived));
hidden stems come from the canonical twelve-branch table
([OpenFate](https://wiki.openfate.ai/en/bazi/hidden-stems-roots/twelve-branch-hidden-stems-table)).

### The signal those four keys produce

> **Today's Signal** — Someone's expectation of you is legible today. Legible ones are the ones
> you can actually negotiate.
>
> Season · until 7 September — This period feeds you. Taking the support is the work right now,
> not a break from it.
>
> Try: Write the criticism down in full and do not send it today.
>
> Notice: What are you agreeing to because it is comfortable rather than because it is right?

## Changes after Dev's review

[Dev's review](/rooms/f8664cffbf96/file/6be64f06bf04) found four things worth blocking on. All four
are fixed, and each now has a regression test that fails if it comes back.

| Finding | Before | After |
|---|---|---|
| Observation looped on the day stem alone | repeated every 10 days, 10 of 30 strings ever seen | three authored variants per key, rotated on the sexagenary index offset by the chart's own — repeats after 30 days, 30 distinct observations a year |
| Reflection fell back to a chart-independent question on ~45% of days | two unrelated charts closed with identical text | fallback is keyed on the day branch's element *read against the chart* — its direction from the day master and its rank in that chart. Two sample charts now share a closing question on 0 of 365 days |
| Coordinates silently defaulted to 0 | a truncated link rendered a confident chart computed at Greenwich | missing or malformed coordinates are rejected by the API and send the browser back to the setup form |
| Day chart used the viewer's time zone | the same shared link gave two friends different seasons | the day chart is read at noon in the **birth** zone, so (chart, date) → reading is a pure function |

Also changed: the season line is labelled and dated on screen (`SEASON · UNTIL 7 SEPTEMBER`) rather
than sitting as an unmarked second paragraph; the five `.scarce` observations that used forecast
grammar ("Support may arrive today…") were reworded as conditions; "Show the calculation" was
promoted from a small text link to the main control under the reading; the element bars now scale
to the chart's own peak instead of saturating at 38.5%; and the solar-term warning now fires within
about six hours of a boundary, matching what its copy claims.

Still true and not fixed by any of this: the underlying day pillar cycles every 60 days, so the
*combination* of lines is drawn from a finite set. What changed is how long it takes to notice —
from about day 10 to a measured 271 distinct readings across a year.

## Verification

Run in the checkout with `pnpm test:ming`. Current result: all checks pass.

| Check | Result |
|---|---|
| Lichun 1994 / 2024 / 2025 / 2026 against [Hong Kong Observatory published solar-term times](https://www.hko.gov.hk/en/gts/astronomy/Solar_Term.htm) | agree to the minute |
| Equinox and solstice instants 2024 | agree to under a minute |
| Day pillar on 1970-01-01, 2000-01-01, 2019-01-27, 2024-06-15 against the [published sexagenary formula](https://ytliu0.github.io/ChineseCalendar/sexagenary.html) | exact |
| Full chart for 2024-06-15 12:00 UT against a [pyswisseph-derived fixture](https://github.com/vedika-io/xalen-ephemeris/blob/main/crates/xalen-chinese/tests/bazi_fixtures.rs) | exact |
| 4,000 random charts, 1940–2035, year / month / day / hour pillars against `lunar-javascript`, an independent implementation | 0 mismatches |
| Shortest observation repeat, 4 charts × 365 days | 30 days (was 10) |
| Distinct four-line readings in a year, one chart | 271 of 365 |
| Closing question shared by two unrelated charts, 365 days | 0 days (was ~45% of sampled days) |
| Every authored key and variant reachable | 30 observation keys, 90 strings, 27 reflections — all hit |

## Known limits, stated plainly

1. **Late-Zi convention.** Births between 23:00 and 23:59 sun time are the one place schools
   genuinely disagree. MING keeps that hour on the current civil day and applies the Five Rats
   rule to that day's stem. A calculator using the other convention will differ on the day and
   hour pillars. The app flags this on any affected chart rather than hiding it.
2. **The theme line holds for the season, not the day.** It tracks the stretch the chart is in,
   so it changes when the solar month changes, roughly every 30 days. That is deliberate; it is
   now labelled and dated on screen so a reader can tell a slow line from a stuck app.
3. **Birth-time precision.** Charts within 15 minutes of a double-hour boundary, or within hours
   of a solar-term boundary, are flagged in the app. Records round minutes and the pillar can move.
4. **Place data.** 2,871 cities above 100,000 population, with coordinates and IANA zone. Smaller
   birthplaces need the nearest listed city; the error is well under a minute of solar time for
   anywhere in the same metro area.
5. **Year range.** 1902–2099, bounded by reliable time-zone history.
6. **Untested with real users.** The copy has had no reader other than its author.
7. **The repeat ceiling is measurable, not eliminated.** The test worth running is Dev's: give
   eight to ten people in the 22-38 band their own chart for two weeks and record the day each
   first says "I've seen this one." That date is the retention ceiling.
