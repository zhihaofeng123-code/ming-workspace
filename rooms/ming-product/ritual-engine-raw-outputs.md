# MING Daily Ritual — raw outputs for editorial review

Read-only capture of App `5d0bcf4c384e` ("MING Daily Ritual"), room `f8664cffbf96`, workspace `fc8a8e44157c`.
Source read from the draft checkout of branch `draft/ritual-engine` (draft `ef773e6c6ec2`, commit `c210670`), materialized with `workspace_cli app checkout`.
Nothing in the App was modified, committed, pushed, or published by this capture.

Source files that matter:

- Chart / pillar computation: `lib/ming/bazi.ts` (511 lines) — VSOP87 solar longitude via `astronomia`, solar terms, sexagenary day count, Five Tigers / Five Rats, hidden stems, Ten Gods, element load, day-master strength, branch relations.
- Copy library **and** selection logic: `lib/ming/signal.ts` (228 lines) — the four `Record` literals `OBSERVATION`, `THEME`, `ACTION`, `NOTICE` plus `buildSignal()`. There is no separate content file; copy and selection live in the same module.
- Server entry point: `app/api/signal/route.ts` (POST `/api/signal`).
- UI: `components/ming/ritual-app.tsx` (form + signal view), `components/ming/calculation.tsx` ("Show the calculation" panel + caveat flags), `components/ming/place-field.tsx` (city autocomplete), `app/page.tsx`.
- City data: `lib/ming/cities.json` (2,871 cities, tuple rows `[name, region, country, lat, lon, tz, population]`).
- Self-check script: `scripts/ming/verify.ts` (`pnpm test:ming`).

Method: the engine was imported directly in Node (tsx) and called exactly the way `app/api/signal/route.ts` calls it. The generated lines were then confirmed against the live UI for birth A on 2026-09-02 and birth B on 2026-09-02 — identical text and identical selection keys.

---

## 1. Verbatim copy inventory

Counts actually found in `lib/ming/signal.ts` (also exported by the module itself as `COPY_COUNTS`):

| Slot | UI label | Table | Entries found |
|---|---|---|---|
| observation | (no label; lead paragraph) | `OBSERVATION` | 30 |
| theme | (no label; second paragraph) | `THEME` | 15 |
| action | `Try` | `ACTION` | 10 |
| reflection | `Notice` | `NOTICE` | 17 |
| **total** | | | **72** |

Per-slot counts match the author's stated 30 / 15 / 10 / 17. The stated total of 76 does not: 30 + 15 + 10 + 17 = 72. Every entry is one authored string; observation and theme strings are two sentences each, action and reflection strings are one sentence each. Counting sentences instead of strings gives 30x2 + 15x2 + 10 + 17 = 117, so 76 matches neither count.

All 72 strings are distinct — no string is reused under two keys.

### 1.1 Observation (30) — key `<tenGod>.<loadState>`

Selected by `OBSERVATION[\`${dayStemGod}.${state}\`]` where `dayStemGod = tenGod(natalDayMaster, targetDay.day.stemIdx)` and `state = loadState(natalElementShare[element of the target day's day stem])`. `loadState` bands (`lib/ming/bazi.ts`): `share < 0.12 -> scarce`, `share > 0.30 -> saturated`, otherwise `present`. No other gate.

| Key | Sentence (verbatim) |
|---|---|
| `friend.scarce` | Something today asks you to stand on your own, and that is not your usual default. Independence can feel like exposure before it feels like strength. |
| `friend.present` | You are more sure of your own position today than you may be letting on. The open question is whether you say it plainly or wait to be agreed with first. |
| `friend.saturated` | You already know what you think, and today hands you more of the same conviction. Certainty this smooth is worth testing against one person who does not share it. |
| `rival.scarce` | Someone may want the same thing you want today, and you are not especially practiced at that. Wanting it out loud is not the same as taking it from anyone. |
| `rival.present` | There is a pull today between holding your share and handing it over. Generosity you resent afterwards was not generosity. |
| `rival.saturated` | You may catch yourself measuring — output, pace, who got there first. The comparison is real, and it is still not information about your work. |
| `craft.scarce` | You may want to make something today without being able to say why. Appetite is data, even before it points anywhere useful. |
| `craft.present` | Output comes more easily today than it usually does. Easy is not the same as unimportant: finish something small rather than opening something large. |
| `craft.saturated` | There is more you could produce today than you can actually finish. Enjoying the work is not the same as moving it. |
| `voice.scarce` | Something you normally leave unsaid may get close to the surface today. Sharpness that surprises you is worth examining before it is worth using. |
| `voice.present` | You can see clearly what is wrong with something today. Being accurate and choosing the moment are two separate decisions. |
| `voice.saturated` | Your read on the flaw is probably right and probably early. The cost of being right early is that nobody stays for the second half. |
| `opening.scarce` | An opening may show up today that does not match how you normally get things. Unfamiliar is not the same as unsuitable. |
| `opening.present` | There is movement available today if you go toward it. Not every open door is worth walking through, and not choosing is also a choice. |
| `opening.saturated` | There is more on offer today than you can hold. Reaching for all of it is the dependable way to end up with none of it. |
| `holding.scarce` | Today rewards upkeep over momentum, which is probably not where your attention wants to go. Ten minutes now removes a much larger cost later. |
| `holding.present` | The steady version of what you are building is available today. It is less interesting than the ambitious version and considerably more likely to survive. |
| `holding.saturated` | You may be holding more than needs holding. Worth asking which of it you maintain out of value and which out of habit. |
| `pressure.scarce` | Something may push at you today without following any rule you recognise. Pressure with no format is still pressure — name it before you answer it. |
| `pressure.present` | There is real demand on you today. You can meet it, redirect it, or delay it, and picking one beats defaulting into the first. |
| `pressure.saturated` | Urgency is loud today and you are practiced at absorbing it. Absorbing is also what teaches the next demand to arrive sooner. |
| `structure.scarce` | A rule or an expectation is more visible today than you are used to. Structure you did not ask for can still be worth reading before you push back on it. |
| `structure.present` | What is expected of you is unusually clear today. Clarity about what is expected is also clarity about what is not. |
| `structure.saturated` | You are carrying a lot of should today. Some of it is genuine obligation and some of it is inherited, and they do not deserve equal weight. |
| `refuge.scarce` | Support may arrive today on terms you did not expect. Help that does not look like help is easy to turn down by reflex. |
| `refuge.present` | There is room to step back and think today. Thinking turns into avoidance at a point you can usually feel arriving. |
| `refuge.saturated` | You have no shortage of reasons to stay inside your own head today. Interior work has sharply diminishing returns after the second lap. |
| `support.scarce` | You may need backing today more than you want to admit. Asking early costs much less than asking once it is urgent. |
| `support.present` | There is dependable support around you today. Using it does not draw down anyone’s account. |
| `support.saturated` | Comfort is easy to reach today. The comfort you do not actually need is the expensive kind. |

Ten God key -> classical label (`TEN_GOD_META`, `lib/ming/bazi.ts`): friend = Friend 比肩; rival = Rob Wealth 劫財; craft = Eating God 食神; voice = Hurting Officer 傷官; opening = Indirect Wealth 偏財; holding = Direct Wealth 正財; pressure = Seven Killings 七殺; structure = Direct Officer 正官; refuge = Indirect Resource 偏印; support = Direct Resource 正印.

### 1.2 Theme (15) — key `<phaseDirection>.<strengthBand>`

Selected by `THEME[\`${seasonDir}.${strength.band}\`]` where `seasonDir = phaseDirection(dayMasterElement, targetDay.month.branch.element)` and `strength.band` comes from `dayMasterStrength(natal)`: `ratio < 0.32 -> weak`, `ratio > 0.48 -> strong`, otherwise `balanced`. `ratio = support / (support + drain)`. Because the input is the target day's **month** branch, this line is constant for a whole solar month.

| Key | Sentence (verbatim) |
|---|---|
| `peer.weak` | The stretch you are in is on your side even when you do not feel resourced. Leaning on what is already around you counts as strength. |
| `peer.balanced` | You and this stretch want broadly the same things. That agreement is exactly what makes overcommitting easy. |
| `peer.strong` | This period amplifies what you already are. More of yourself is not automatically the answer. |
| `resource.weak` | This period feeds you. Taking the support is the work right now, not a break from it. |
| `resource.balanced` | More is coming in than going out at the moment. Good conditions for learning something you cannot use yet. |
| `resource.strong` | You are well supplied and the conditions keep supplying. At some point receiving becomes a way of not deciding. |
| `output.weak` | This period pulls output out of you and there is less to give than usual. Choosing what not to make is the real decision. |
| `output.balanced` | The conditions want something made. Pick one and let the rest stay unmade. |
| `output.strong` | You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out. |
| `wealth.weak` | There is something workable here and less energy to work it with than you would like. Smaller scope, same direction. |
| `wealth.balanced` | Conditions are workable. Effort converts into result at roughly a fair rate right now. |
| `wealth.strong` | You can take on more than this period strictly requires. Whether you should is a separate question. |
| `authority.weak` | The period presses and you are not at full strength. Reducing what you owe beats trying to meet all of it. |
| `authority.balanced` | There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s. |
| `authority.strong` | You can carry what this period is asking. Being able to carry it is how people end up carrying what was never theirs. |

### 1.3 Action / `Try` (10) — key `<tenGod>`

Selected by `ACTION[tenGod(dayMaster, today.day.branch.hidden[0])]` — the Ten God of the **primary hidden stem** of the target day's branch. No load, strength, or relation gating.

| Key | Sentence (verbatim) |
|---|---|
| `friend` | State your actual position once, without softening it into a question. |
| `rival` | Name one thing you want that someone else also wants. Only name it. |
| `craft` | Finish one small thing you already started. Do not open a new one. |
| `voice` | Write the criticism down in full and do not send it today. |
| `opening` | Say yes to the smallest of the options in front of you and decline the largest. |
| `holding` | Do the ten-minute upkeep you keep moving to tomorrow. |
| `pressure` | Answer one demand with a specific time rather than a specific outcome. |
| `structure` | Take one obligation and check whether anyone still actually expects it. |
| `refuge` | Take twenty minutes with no input at all — no feed, no music, no talking. |
| `support` | Ask one person for something small and specific. |

### 1.4 Reflection / `Notice` (17) — key `<relation>.<pillar>` or `none.<element>`

Two-stage gate in `buildSignal()`:

1. For each natal pillar, in the fixed order `day, month, hour, year`, compute `branchRelation(today.day.branchIdx, natal[pillar].branchIdx)`.
2. Rank them `clash = 3 > harmony = 2 > same = 1 > none = 0` and keep the highest. The comparison is strictly `rank > best.rank`, so **ties resolve to the earliest pillar in `day, month, hour, year` order**.
3. If the best rank is 0 (no clash, harmony or repeat against any natal branch), the key falls back to `none.<element of the target day's branch>`.

`branchRelation` definitions (`lib/ming/bazi.ts`): `same` if identical branch; `clash` if six apart (子午 丑未 寅申 卯酉 辰戌 巳亥); `harmony` if `(i + j) mod 12 === 1` (子丑 寅亥 卯戌 辰酉 巳申 午未); otherwise `none`.

| Key | Sentence (verbatim) |
|---|---|
| `clash.day` | What are you defending that nobody has actually attacked? |
| `clash.month` | Whose expectations are you working against today, and did they ever state them out loud? |
| `clash.hour` | What are you hurrying toward that would survive being a week later? |
| `clash.year` | Which old rule of yours is this friction really about? |
| `harmony.day` | What gets easier the moment you stop treating this as a problem to solve? |
| `harmony.month` | Who is easy to work with right now, and have you ever told them so? |
| `harmony.hour` | What would you do next if this went well — and are you ready for that? |
| `harmony.year` | What are you agreeing to because it is comfortable rather than because it is right? |
| `same.day` | Where are you repeating yourself and calling it consistency? |
| `same.month` | What has shown up more than once this month that you keep filing as coincidence? |
| `same.hour` | What do you do at the same time every day without ever deciding to? |
| `same.year` | What have you believed about yourself for so long that you stopped checking? |
| `none.Wood` | What are you growing that you have not looked at closely in a while? |
| `none.Fire` | What are you visible for at the moment, and did you choose it? |
| `none.Earth` | What are you holding steady for someone else, and who is holding it for you? |
| `none.Metal` | What would you cut if the decision cost you nothing? |
| `none.Water` | What are you waiting to be certain about, and what would "certain enough" look like? |

---

## 2. On-screen text around the signal (verbatim from the templates and confirmed in the UI)

### Entry screen (`components/ming/ritual-app.tsx`)

- Wordmark: `MING`
- H1: `One moment in time, read back to you every morning.`
- Intro: `MING builds your chart from the position of the sun at your birth, then reads each day against it. Two minutes, four lines. It describes conditions and choices, never what will happen.`
- Field labels: `Date of birth`, `Time of birth`, `Place of birth` (rendered uppercase by CSS)
- Under time: `The clock reading on the record. MING converts it to sun time itself.`
- Place field placeholder: `Start typing a city`
- Place field hint, before a city is picked: `Needed for the time zone in force that day and the longitude correction.`
- Place field hint, after a city is picked: `<tz> · <lat to 2dp>, <lon to 2dp>`
- Submit: `Read today`. Secondary, only when details already exist: `Cancel`
- Date input is constrained `min="1902-01-01"` `max="2099-12-31"`.

### Signal screen

- Date line: long date in the viewer's timezone, `en-GB` `weekday, day, month` — e.g. `Wednesday 2 September`. No year is shown.
- Day nav buttons, aria-labels `Previous day` / `Today` / `Next day`, visible glyphs `←`, `Today`, `→`
- H1: `Today’s Signal`
- Observation paragraph, then theme paragraph. Neither has a label.
- Section label: `Try` — then the action sentence.
- Section label: `Notice` — then the reflection sentence.
- Toggles: `Show the calculation` / `Hide the calculation`, and `Change your details`
- Provenance line: `<place> · <day stem pinyin> day master · <year> <month> <day> <hour>` in Chinese characters — e.g. `San Francisco, California, United States of America · Ren day master · 甲戌 丁卯 壬寅 甲辰`
- Footer, on every screen: `MING reads conditions, not outcomes. Nothing here predicts events, and nothing here is medical, legal or financial advice.`

### Error strings

- Client, network failure: `Could not reach the engine. Check your connection and try again.`
- Client, unknown server error: `Could not read that chart.`
- Server (`app/api/signal/route.ts`): `Malformed request.` / `Birth date is required.` / `Birth time is required.` / `Target day is required.` / `Birth place time zone is not recognised.` / `Birth place coordinates are out of range.` / `Birth year must be between 1902 and 2099 for reliable time-zone history.` / `Could not compute a chart for that moment.`

### "Show the calculation" panel (`components/ming/calculation.tsx`)

- Intro: `Everything above the line is chosen by the arithmetic below. The pillars, the relationships between them, and the selection keys are computed. The sentences themselves are written by hand and picked by those keys — MING does not generate language at runtime.`
- Two four-column pillar grids: `Your chart` and `Day chart — <target date>`, columns `Year` / `Month` / `Day` / `Hour`, each showing stem + branch characters, pinyin, and `<stem element> / <branch element>`.
- Section `How the chart was derived`, rows `Birth moment`, `True solar time`, `Year pillar`, `Month pillar`, `Day pillar`, `Hour pillar` (each a computed sentence — see the matrix and screenshots for real instances).
- Section `Element balance in your chart`, with: `Eight visible characters plus the stems hidden inside each branch, weighted 0.6 / 0.3 / 0.15 by qi layer. That weighting is MING’s own scoring, not a classical rule — the classical tables record the order of hidden stems but not numeric weights.`
- Then: `Support versus drain on the day master scores <support> to <drain> — a ratio of <n>%, which MING reads as a <weak|balanced|strong> day master. Those bands are MING’s own too.`
- Section `Why these four lines and not others`, rows `Observation`, `Current theme`, `Action`, `Reflection`, each ending in the literal selection key in a `<code>` element. The `Current theme` row also states: `This line tracks the stretch you are in, so it holds for the season rather than changing daily.`
- Closing note: `Solar positions come from the full VSOP87 series; solar-term instants agree with the Hong Kong Observatory published times to within a minute. Year, month and day pillars were checked against a second independent implementation across 4,000 charts from 1940 to 2035 with no disagreement outside the 23:00 convention noted above.`

### Conditional caveat block — heading `Worth knowing about your birth time`

Rendered only inside the calculation panel, and only when `meta.lateZi || meta.minutesToHourBoundary <= 15 || meta.degreesToTermBoundary < 0.05`. Up to three bullets, each independently gated:

- `lateZi` (effective sun-time hour === 23): `Your birth falls between 23:00 and 23:59 in sun time. Schools disagree about whether that hour belongs to the current day or the next one. MING keeps it on the current day and applies the hour rule to that day’s stem; a calculator using the other convention will give you a different day and hour pillar.`
- `minutesToHourBoundary <= 15`: `You are within <n> minutes of a double-hour boundary. Birth records round minutes, so the hour pillar here is less certain than the rest of the chart.`
- `degreesToTermBoundary < 0.05`: `Your birth is within a few hours of a solar-term boundary, which is what sets the month branch. Small errors in the recorded time matter more than usual.`

---

## 3. Signal matrix

Four births x ten target days. Day charts are computed the way the API does it: local noon in the viewer timezone, `lon = 0`, true solar time off. `viewerTz` here is set to the birth timezone, which is the API's own fallback when the client sends no `viewerTz`; in the live UI `viewerTz` is the browser's timezone (see observation O-6).

### Birth A — 1994-03-17 07:42 — San Francisco, California, United States of America

- Input: date `1994-03-17`, time `07:42`, tz `America/Los_Angeles`, lat `37.74`, lon `-122.46`
- Birth instant UTC: `1994-03-17T15:42:00.000Z` (zone offset -480 min)
- True solar correction: -18.22 min -> effective sun-time clock `1994-03-17 07:24`
- **Natal pillars: 甲戌 丁卯 壬寅 甲辰 (Jia Xu / Ding Mao / Ren Yin / Jia Chen)**
- Day master: 壬 Ren (Water, yang)
- **Day-master strength: support 0.45 / drain 7.90 = ratio 5.4% -> band `weak`**
- Element share: Wood 45.2%, Fire 18.7%, Earth 17.4%, Metal 3.9%, Water 14.8%
- BaZi year 1994; solar longitude at birth 356.820°
- Caveat flags: lateZi=false, minutesToHourBoundary=24.0, degreesToTermBoundary=11.8196 -> caveat block not shown

#### A · 2026-09-02

Day chart: 丙午 丙申 己卯 庚午 (Bing Wu / Bing Shen / Ji Mao / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 160.327°

Keys: `observation=structure.present` · `theme=resource.weak` · `action=voice` · `notice=harmony.year`

Why: today's day stem 己 vs day master 壬 = Direct Officer (正官), natal Earth share 17.4% -> `present`; season branch 申 (Metal) is `resource` to the day master; primary hidden stem of 卯 is 乙 = Hurting Officer; branch relation `harmony` with the natal year branch.

> What is expected of you is unusually clear today. Clarity about what is expected is also clarity about what is not.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Write the criticism down in full and do not send it today.
>
> **Notice** — What are you agreeing to because it is comfortable rather than because it is right?

#### A · 2026-09-03

Day chart: 丙午 丙申 庚辰 壬午 (Bing Wu / Bing Shen / Geng Chen / Ren Wu) — month branch 申 (Shen, Metal), solar longitude 161.295°

Keys: `observation=refuge.scarce` · `theme=resource.weak` · `action=pressure` · `notice=clash.year`

Why: today's day stem 庚 vs day master 壬 = Indirect Resource (偏印), natal Metal share 3.9% -> `scarce`; season branch 申 (Metal) is `resource` to the day master; primary hidden stem of 辰 is 戊 = Seven Killings; branch relation `clash` with the natal year branch.

> Support may arrive today on terms you did not expect. Help that does not look like help is easy to turn down by reflex.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Answer one demand with a specific time rather than a specific outcome.
>
> **Notice** — Which old rule of yours is this friction really about?

#### A · 2026-09-04

Day chart: 丙午 丙申 辛巳 甲午 (Bing Wu / Bing Shen / Xin Si / Jia Wu) — month branch 申 (Shen, Metal), solar longitude 162.264°

Keys: `observation=support.scarce` · `theme=resource.weak` · `action=opening` · `notice=none.Fire`

Why: today's day stem 辛 vs day master 壬 = Direct Resource (正印), natal Metal share 3.9% -> `scarce`; season branch 申 (Metal) is `resource` to the day master; primary hidden stem of 巳 is 丙 = Indirect Wealth; branch relation `none` with any natal branch.

> You may need backing today more than you want to admit. Asking early costs much less than asking once it is urgent.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Say yes to the smallest of the options in front of you and decline the largest.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

#### A · 2026-09-05

Day chart: 丙午 丙申 壬午 丙午 (Bing Wu / Bing Shen / Ren Wu / Bing Wu) — month branch 申 (Shen, Metal), solar longitude 163.234°

Keys: `observation=friend.present` · `theme=resource.weak` · `action=holding` · `notice=none.Fire`

Why: today's day stem 壬 vs day master 壬 = Friend (比肩), natal Water share 14.8% -> `present`; season branch 申 (Metal) is `resource` to the day master; primary hidden stem of 午 is 丁 = Direct Wealth; branch relation `none` with any natal branch.

> You are more sure of your own position today than you may be letting on. The open question is whether you say it plainly or wait to be agreed with first.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Do the ten-minute upkeep you keep moving to tomorrow.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

#### A · 2026-09-07

Day chart: 丙午 丁酉 甲申 庚午 (Bing Wu / Ding You / Jia Shen / Geng Wu) — month branch 酉 (You, Metal), solar longitude 165.174°

Keys: `observation=craft.saturated` · `theme=resource.weak` · `action=refuge` · `notice=clash.day`

Why: today's day stem 甲 vs day master 壬 = Eating God (食神), natal Wood share 45.2% -> `saturated`; season branch 酉 (Metal) is `resource` to the day master; primary hidden stem of 申 is 庚 = Indirect Resource; branch relation `clash` with the natal day branch.

> There is more you could produce today than you can actually finish. Enjoying the work is not the same as moving it.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Take twenty minutes with no input at all — no feed, no music, no talking.
>
> **Notice** — What are you defending that nobody has actually attacked?

#### A · 2026-09-08

Day chart: 丙午 丁酉 乙酉 壬午 (Bing Wu / Ding You / Yi You / Ren Wu) — month branch 酉 (You, Metal), solar longitude 166.146°

Keys: `observation=voice.saturated` · `theme=resource.weak` · `action=support` · `notice=clash.month`

Why: today's day stem 乙 vs day master 壬 = Hurting Officer (傷官), natal Wood share 45.2% -> `saturated`; season branch 酉 (Metal) is `resource` to the day master; primary hidden stem of 酉 is 辛 = Direct Resource; branch relation `clash` with the natal month branch.

> Your read on the flaw is probably right and probably early. The cost of being right early is that nobody stays for the second half.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — Whose expectations are you working against today, and did they ever state them out loud?

#### A · 2026-09-09

Day chart: 丙午 丁酉 丙戌 甲午 (Bing Wu / Ding You / Bing Xu / Jia Wu) — month branch 酉 (You, Metal), solar longitude 167.117°

Keys: `observation=opening.present` · `theme=resource.weak` · `action=pressure` · `notice=clash.hour`

Why: today's day stem 丙 vs day master 壬 = Indirect Wealth (偏財), natal Fire share 18.7% -> `present`; season branch 酉 (Metal) is `resource` to the day master; primary hidden stem of 戌 is 戊 = Seven Killings; branch relation `clash` with the natal hour branch.

> There is movement available today if you go toward it. Not every open door is worth walking through, and not choosing is also a choice.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Answer one demand with a specific time rather than a specific outcome.
>
> **Notice** — What are you hurrying toward that would survive being a week later?

#### A · 2026-09-15

Day chart: 丙午 丁酉 壬辰 丙午 (Bing Wu / Ding You / Ren Chen / Bing Wu) — month branch 酉 (You, Metal), solar longitude 172.958°

Keys: `observation=friend.present` · `theme=resource.weak` · `action=pressure` · `notice=clash.year`

Why: today's day stem 壬 vs day master 壬 = Friend (比肩), natal Water share 14.8% -> `present`; season branch 酉 (Metal) is `resource` to the day master; primary hidden stem of 辰 is 戊 = Seven Killings; branch relation `clash` with the natal year branch.

> You are more sure of your own position today than you may be letting on. The open question is whether you say it plainly or wait to be agreed with first.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — Answer one demand with a specific time rather than a specific outcome.
>
> **Notice** — Which old rule of yours is this friction really about?

#### A · 2026-09-22

Day chart: 丙午 丁酉 己亥 庚午 (Bing Wu / Ding You / Ji Hai / Geng Wu) — month branch 酉 (You, Metal), solar longitude 179.793°

Keys: `observation=structure.present` · `theme=resource.weak` · `action=friend` · `notice=harmony.day`

Why: today's day stem 己 vs day master 壬 = Direct Officer (正官), natal Earth share 17.4% -> `present`; season branch 酉 (Metal) is `resource` to the day master; primary hidden stem of 亥 is 壬 = Friend; branch relation `harmony` with the natal day branch.

> What is expected of you is unusually clear today. Clarity about what is expected is also clarity about what is not.
>
> This period feeds you. Taking the support is the work right now, not a break from it.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What gets easier the moment you stop treating this as a problem to solve?

#### A · 2026-10-10

Day chart: 丙午 戊戌 丁巳 丙午 (Bing Wu / Wu Xu / Ding Si / Bing Wu) — month branch 戌 (Xu, Earth), solar longitude 197.491°

Keys: `observation=holding.present` · `theme=authority.weak` · `action=opening` · `notice=none.Fire`

Why: today's day stem 丁 vs day master 壬 = Direct Wealth (正財), natal Fire share 18.7% -> `present`; season branch 戌 (Earth) is `authority` to the day master; primary hidden stem of 巳 is 丙 = Indirect Wealth; branch relation `none` with any natal branch.

> The steady version of what you are building is available today. It is less interesting than the ambitious version and considerably more likely to survive.
>
> The period presses and you are not at full strength. Reducing what you owe beats trying to meet all of it.
>
> **Try** — Say yes to the smallest of the options in front of you and decline the largest.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

### Birth B — 1988-11-02 23:15 — London, Westminster, United Kingdom

- Input: date `1988-11-02`, time `23:15`, tz `Europe/London`, lat `51.5`, lon `-0.1167`
- Birth instant UTC: `1988-11-02T23:15:00.000Z` (zone offset 0 min)
- True solar correction: 15.96 min -> effective sun-time clock `1988-11-02 23:31`
- **Natal pillars: 戊辰 壬戌 辛酉 戊子 (Wu Chen / Ren Xu / Xin You / Wu Zi)**
- Day master: 辛 Xin (Metal, yin)
- **Day-master strength: support 5.00 / drain 3.35 = ratio 59.9% -> band `strong`**
- Element share: Wood 4.1%, Fire 2.1%, Earth 43.8%, Metal 26.0%, Water 24.0%
- BaZi year 1988; solar longitude at birth 220.756°
- Caveat flags: lateZi=true, minutesToHourBoundary=31.0, degreesToTermBoundary=4.2441 -> caveat block **shown** (late-Zi bullet)

#### B · 2026-09-02

Day chart: 丙午 丙申 己卯 庚午 (Bing Wu / Bing Shen / Ji Mao / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 160.004°

Keys: `observation=refuge.saturated` · `theme=peer.strong` · `action=opening` · `notice=clash.day`

Why: today's day stem 己 vs day master 辛 = Indirect Resource (偏印), natal Earth share 43.8% -> `saturated`; season branch 申 (Metal) is `peer` to the day master; primary hidden stem of 卯 is 乙 = Indirect Wealth; branch relation `clash` with the natal day branch.

> You have no shortage of reasons to stay inside your own head today. Interior work has sharply diminishing returns after the second lap.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Say yes to the smallest of the options in front of you and decline the largest.
>
> **Notice** — What are you defending that nobody has actually attacked?

#### B · 2026-09-03

Day chart: 丙午 丙申 庚辰 壬午 (Bing Wu / Bing Shen / Geng Chen / Ren Wu) — month branch 申 (Shen, Metal), solar longitude 160.972°

Keys: `observation=rival.present` · `theme=peer.strong` · `action=support` · `notice=clash.month`

Why: today's day stem 庚 vs day master 辛 = Rob Wealth (劫財), natal Metal share 26.0% -> `present`; season branch 申 (Metal) is `peer` to the day master; primary hidden stem of 辰 is 戊 = Direct Resource; branch relation `clash` with the natal month branch.

> There is a pull today between holding your share and handing it over. Generosity you resent afterwards was not generosity.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — Whose expectations are you working against today, and did they ever state them out loud?

#### B · 2026-09-04

Day chart: 丙午 丙申 辛巳 甲午 (Bing Wu / Bing Shen / Xin Si / Jia Wu) — month branch 申 (Shen, Metal), solar longitude 161.941°

Keys: `observation=friend.present` · `theme=peer.strong` · `action=structure` · `notice=none.Fire`

Why: today's day stem 辛 vs day master 辛 = Friend (比肩), natal Metal share 26.0% -> `present`; season branch 申 (Metal) is `peer` to the day master; primary hidden stem of 巳 is 丙 = Direct Officer; branch relation `none` with any natal branch.

> You are more sure of your own position today than you may be letting on. The open question is whether you say it plainly or wait to be agreed with first.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Take one obligation and check whether anyone still actually expects it.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

#### B · 2026-09-05

Day chart: 丙午 丙申 壬午 丙午 (Bing Wu / Bing Shen / Ren Wu / Bing Wu) — month branch 申 (Shen, Metal), solar longitude 162.911°

Keys: `observation=voice.present` · `theme=peer.strong` · `action=pressure` · `notice=clash.hour`

Why: today's day stem 壬 vs day master 辛 = Hurting Officer (傷官), natal Water share 24.0% -> `present`; season branch 申 (Metal) is `peer` to the day master; primary hidden stem of 午 is 丁 = Seven Killings; branch relation `clash` with the natal hour branch.

> You can see clearly what is wrong with something today. Being accurate and choosing the moment are two separate decisions.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Answer one demand with a specific time rather than a specific outcome.
>
> **Notice** — What are you hurrying toward that would survive being a week later?

#### B · 2026-09-07

Day chart: 丙午 丙申 甲申 庚午 (Bing Wu / Bing Shen / Jia Shen / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 164.851°

Keys: `observation=holding.scarce` · `theme=peer.strong` · `action=rival` · `notice=none.Metal`

Why: today's day stem 甲 vs day master 辛 = Direct Wealth (正財), natal Wood share 4.1% -> `scarce`; season branch 申 (Metal) is `peer` to the day master; primary hidden stem of 申 is 庚 = Rob Wealth; branch relation `none` with any natal branch.

> Today rewards upkeep over momentum, which is probably not where your attention wants to go. Ten minutes now removes a much larger cost later.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Name one thing you want that someone else also wants. Only name it.
>
> **Notice** — What would you cut if the decision cost you nothing?

#### B · 2026-09-08

Day chart: 丙午 丁酉 乙酉 壬午 (Bing Wu / Ding You / Yi You / Ren Wu) — month branch 酉 (You, Metal), solar longitude 165.822°

Keys: `observation=opening.scarce` · `theme=peer.strong` · `action=friend` · `notice=harmony.year`

Why: today's day stem 乙 vs day master 辛 = Indirect Wealth (偏財), natal Wood share 4.1% -> `scarce`; season branch 酉 (Metal) is `peer` to the day master; primary hidden stem of 酉 is 辛 = Friend; branch relation `harmony` with the natal year branch.

> An opening may show up today that does not match how you normally get things. Unfamiliar is not the same as unsuitable.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What are you agreeing to because it is comfortable rather than because it is right?

#### B · 2026-09-09

Day chart: 丙午 丁酉 丙戌 甲午 (Bing Wu / Ding You / Bing Xu / Jia Wu) — month branch 酉 (You, Metal), solar longitude 166.793°

Keys: `observation=structure.scarce` · `theme=peer.strong` · `action=support` · `notice=clash.year`

Why: today's day stem 丙 vs day master 辛 = Direct Officer (正官), natal Fire share 2.1% -> `scarce`; season branch 酉 (Metal) is `peer` to the day master; primary hidden stem of 戌 is 戊 = Direct Resource; branch relation `clash` with the natal year branch.

> A rule or an expectation is more visible today than you are used to. Structure you did not ask for can still be worth reading before you push back on it.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — Which old rule of yours is this friction really about?

#### B · 2026-09-15

Day chart: 丙午 丁酉 壬辰 丙午 (Bing Wu / Ding You / Ren Chen / Bing Wu) — month branch 酉 (You, Metal), solar longitude 172.633°

Keys: `observation=voice.present` · `theme=peer.strong` · `action=support` · `notice=clash.month`

Why: today's day stem 壬 vs day master 辛 = Hurting Officer (傷官), natal Water share 24.0% -> `present`; season branch 酉 (Metal) is `peer` to the day master; primary hidden stem of 辰 is 戊 = Direct Resource; branch relation `clash` with the natal month branch.

> You can see clearly what is wrong with something today. Being accurate and choosing the moment are two separate decisions.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — Whose expectations are you working against today, and did they ever state them out loud?

#### B · 2026-09-22

Day chart: 丙午 丁酉 己亥 庚午 (Bing Wu / Ding You / Ji Hai / Geng Wu) — month branch 酉 (You, Metal), solar longitude 179.467°

Keys: `observation=refuge.saturated` · `theme=peer.strong` · `action=voice` · `notice=none.Water`

Why: today's day stem 己 vs day master 辛 = Indirect Resource (偏印), natal Earth share 43.8% -> `saturated`; season branch 酉 (Metal) is `peer` to the day master; primary hidden stem of 亥 is 壬 = Hurting Officer; branch relation `none` with any natal branch.

> You have no shortage of reasons to stay inside your own head today. Interior work has sharply diminishing returns after the second lap.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Write the criticism down in full and do not send it today.
>
> **Notice** — What are you waiting to be certain about, and what would "certain enough" look like?

#### B · 2026-10-10

Day chart: 丙午 戊戌 丁巳 丙午 (Bing Wu / Wu Xu / Ding Si / Bing Wu) — month branch 戌 (Xu, Earth), solar longitude 197.162°

Keys: `observation=pressure.scarce` · `theme=resource.strong` · `action=structure` · `notice=none.Fire`

Why: today's day stem 丁 vs day master 辛 = Seven Killings (七殺), natal Fire share 2.1% -> `scarce`; season branch 戌 (Earth) is `resource` to the day master; primary hidden stem of 巳 is 丙 = Direct Officer; branch relation `none` with any natal branch.

> Something may push at you today without following any rule you recognise. Pressure with no format is still pressure — name it before you answer it.
>
> You are well supplied and the conditions keep supplying. At some point receiving becomes a way of not deciding.
>
> **Try** — Take one obligation and check whether anyone still actually expects it.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

### Birth C — 2001-06-24 14:05 — New York, New York, United States of America

- Input: date `2001-06-24`, time `14:05`, tz `America/New_York`, lat `40.75`, lon `-73.98`
- Birth instant UTC: `2001-06-24T18:05:00.000Z` (zone offset -240 min)
- True solar correction: -58.39 min -> effective sun-time clock `2001-06-24 13:07`
- **Natal pillars: 辛巳 甲午 戊午 己未 (Xin Si / Jia Wu / Wu Wu / Ji Wei)**
- Day master: 戊 Wu (Earth, yang)
- **Day-master strength: support 5.50 / drain 3.30 = ratio 62.5% -> band `strong`**
- Element share: Wood 14.6%, Fire 26.6%, Earth 44.3%, Metal 14.6%, Water 0.0%
- BaZi year 2001; solar longitude at birth 93.279°
- Caveat flags: lateZi=false, minutesToHourBoundary=7.0, degreesToTermBoundary=11.7207 -> caveat block **shown** (double-hour bullet)

#### C · 2026-09-02

Day chart: 丙午 丙申 己卯 庚午 (Bing Wu / Bing Shen / Ji Mao / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 160.206°

Keys: `observation=rival.saturated` · `theme=output.strong` · `action=structure` · `notice=none.Wood`

Why: today's day stem 己 vs day master 戊 = Rob Wealth (劫財), natal Earth share 44.3% -> `saturated`; season branch 申 (Metal) is `output` to the day master; primary hidden stem of 卯 is 乙 = Direct Officer; branch relation `none` with any natal branch.

> You may catch yourself measuring — output, pace, who got there first. The comparison is real, and it is still not information about your work.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Take one obligation and check whether anyone still actually expects it.
>
> **Notice** — What are you growing that you have not looked at closely in a while?

#### C · 2026-09-03

Day chart: 丙午 丙申 庚辰 壬午 (Bing Wu / Bing Shen / Geng Chen / Ren Wu) — month branch 申 (Shen, Metal), solar longitude 161.174°

Keys: `observation=craft.present` · `theme=output.strong` · `action=friend` · `notice=none.Earth`

Why: today's day stem 庚 vs day master 戊 = Eating God (食神), natal Metal share 14.6% -> `present`; season branch 申 (Metal) is `output` to the day master; primary hidden stem of 辰 is 戊 = Friend; branch relation `none` with any natal branch.

> Output comes more easily today than it usually does. Easy is not the same as unimportant: finish something small rather than opening something large.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### C · 2026-09-04

Day chart: 丙午 丙申 辛巳 甲午 (Bing Wu / Bing Shen / Xin Si / Jia Wu) — month branch 申 (Shen, Metal), solar longitude 162.143°

Keys: `observation=voice.present` · `theme=output.strong` · `action=refuge` · `notice=same.year`

Why: today's day stem 辛 vs day master 戊 = Hurting Officer (傷官), natal Metal share 14.6% -> `present`; season branch 申 (Metal) is `output` to the day master; primary hidden stem of 巳 is 丙 = Indirect Resource; branch relation `same` with the natal year branch.

> You can see clearly what is wrong with something today. Being accurate and choosing the moment are two separate decisions.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Take twenty minutes with no input at all — no feed, no music, no talking.
>
> **Notice** — What have you believed about yourself for so long that you stopped checking?

#### C · 2026-09-05

Day chart: 丙午 丙申 壬午 丙午 (Bing Wu / Bing Shen / Ren Wu / Bing Wu) — month branch 申 (Shen, Metal), solar longitude 163.113°

Keys: `observation=opening.scarce` · `theme=output.strong` · `action=support` · `notice=harmony.hour`

Why: today's day stem 壬 vs day master 戊 = Indirect Wealth (偏財), natal Water share 0.0% -> `scarce`; season branch 申 (Metal) is `output` to the day master; primary hidden stem of 午 is 丁 = Direct Resource; branch relation `harmony` with the natal hour branch.

> An opening may show up today that does not match how you normally get things. Unfamiliar is not the same as unsuitable.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — What would you do next if this went well — and are you ready for that?

#### C · 2026-09-07

Day chart: 丙午 丁酉 甲申 庚午 (Bing Wu / Ding You / Jia Shen / Geng Wu) — month branch 酉 (You, Metal), solar longitude 165.053°

Keys: `observation=pressure.present` · `theme=output.strong` · `action=craft` · `notice=harmony.year`

Why: today's day stem 甲 vs day master 戊 = Seven Killings (七殺), natal Wood share 14.6% -> `present`; season branch 酉 (Metal) is `output` to the day master; primary hidden stem of 申 is 庚 = Eating God; branch relation `harmony` with the natal year branch.

> There is real demand on you today. You can meet it, redirect it, or delay it, and picking one beats defaulting into the first.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Finish one small thing you already started. Do not open a new one.
>
> **Notice** — What are you agreeing to because it is comfortable rather than because it is right?

#### C · 2026-09-08

Day chart: 丙午 丁酉 乙酉 壬午 (Bing Wu / Ding You / Yi You / Ren Wu) — month branch 酉 (You, Metal), solar longitude 166.024°

Keys: `observation=structure.present` · `theme=output.strong` · `action=voice` · `notice=none.Metal`

Why: today's day stem 乙 vs day master 戊 = Direct Officer (正官), natal Wood share 14.6% -> `present`; season branch 酉 (Metal) is `output` to the day master; primary hidden stem of 酉 is 辛 = Hurting Officer; branch relation `none` with any natal branch.

> What is expected of you is unusually clear today. Clarity about what is expected is also clarity about what is not.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Write the criticism down in full and do not send it today.
>
> **Notice** — What would you cut if the decision cost you nothing?

#### C · 2026-09-09

Day chart: 丙午 丁酉 丙戌 甲午 (Bing Wu / Ding You / Bing Xu / Jia Wu) — month branch 酉 (You, Metal), solar longitude 166.996°

Keys: `observation=refuge.present` · `theme=output.strong` · `action=friend` · `notice=none.Earth`

Why: today's day stem 丙 vs day master 戊 = Indirect Resource (偏印), natal Fire share 26.6% -> `present`; season branch 酉 (Metal) is `output` to the day master; primary hidden stem of 戌 is 戊 = Friend; branch relation `none` with any natal branch.

> There is room to step back and think today. Thinking turns into avoidance at a point you can usually feel arriving.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### C · 2026-09-15

Day chart: 丙午 丁酉 壬辰 丙午 (Bing Wu / Ding You / Ren Chen / Bing Wu) — month branch 酉 (You, Metal), solar longitude 172.836°

Keys: `observation=opening.scarce` · `theme=output.strong` · `action=friend` · `notice=none.Earth`

Why: today's day stem 壬 vs day master 戊 = Indirect Wealth (偏財), natal Water share 0.0% -> `scarce`; season branch 酉 (Metal) is `output` to the day master; primary hidden stem of 辰 is 戊 = Friend; branch relation `none` with any natal branch.

> An opening may show up today that does not match how you normally get things. Unfamiliar is not the same as unsuitable.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### C · 2026-09-22

Day chart: 丙午 丁酉 己亥 庚午 (Bing Wu / Ding You / Ji Hai / Geng Wu) — month branch 酉 (You, Metal), solar longitude 179.670°

Keys: `observation=rival.saturated` · `theme=output.strong` · `action=opening` · `notice=clash.year`

Why: today's day stem 己 vs day master 戊 = Rob Wealth (劫財), natal Earth share 44.3% -> `saturated`; season branch 酉 (Metal) is `output` to the day master; primary hidden stem of 亥 is 壬 = Indirect Wealth; branch relation `clash` with the natal year branch.

> You may catch yourself measuring — output, pace, who got there first. The comparison is real, and it is still not information about your work.
>
> You have the capacity and the period wants it spent. Spending it on the wrong thing is the risk, not running out.
>
> **Try** — Say yes to the smallest of the options in front of you and decline the largest.
>
> **Notice** — Which old rule of yours is this friction really about?

#### C · 2026-10-10

Day chart: 丙午 戊戌 丁巳 丙午 (Bing Wu / Wu Xu / Ding Si / Bing Wu) — month branch 戌 (Xu, Earth), solar longitude 197.367°

Keys: `observation=support.present` · `theme=peer.strong` · `action=refuge` · `notice=same.year`

Why: today's day stem 丁 vs day master 戊 = Direct Resource (正印), natal Fire share 26.6% -> `present`; season branch 戌 (Earth) is `peer` to the day master; primary hidden stem of 巳 is 丙 = Indirect Resource; branch relation `same` with the natal year branch.

> There is dependable support around you today. Using it does not draw down anyone’s account.
>
> This period amplifies what you already are. More of yourself is not automatically the answer.
>
> **Try** — Take twenty minutes with no input at all — no feed, no music, no talking.
>
> **Notice** — What have you believed about yourself for so long that you stopped checking?

### Birth D — 1996-01-09 04:30 — Taipei, Taipei City, Taiwan

- Input: date `1996-01-09`, time `04:30`, tz `Asia/Taipei`, lat `25.0358`, lon `121.5683`
- Birth instant UTC: `1996-01-08T20:30:00.000Z` (zone offset 480 min)
- True solar correction: -0.34 min -> effective sun-time clock `1996-01-09 04:30`
- **Natal pillars: 乙亥 己丑 乙巳 戊寅 (Yi Hai / Ji Chou / Yi Si / Wu Yin)**
- Day master: 乙 Yi (Wood, yin)
- **Day-master strength: support 3.10 / drain 6.00 = ratio 34.1% -> band `balanced`**
- Element share: Wood 36.0%, Fire 11.2%, Earth 37.9%, Metal 3.7%, Water 11.2%
- BaZi year 1995; solar longitude at birth 287.844°
- Caveat flags: lateZi=false, minutesToHourBoundary=30.0, degreesToTermBoundary=2.8436 -> caveat block not shown

#### D · 2026-09-02

Day chart: 丙午 丙申 己卯 庚午 (Bing Wu / Bing Shen / Ji Mao / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 159.722°

Keys: `observation=opening.saturated` · `theme=authority.balanced` · `action=friend` · `notice=none.Wood`

Why: today's day stem 己 vs day master 乙 = Indirect Wealth (偏財), natal Earth share 37.9% -> `saturated`; season branch 申 (Metal) is `authority` to the day master; primary hidden stem of 卯 is 乙 = Friend; branch relation `none` with any natal branch.

> There is more on offer today than you can hold. Reaching for all of it is the dependable way to end up with none of it.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — State your actual position once, without softening it into a question.
>
> **Notice** — What are you growing that you have not looked at closely in a while?

#### D · 2026-09-03

Day chart: 丙午 丙申 庚辰 壬午 (Bing Wu / Bing Shen / Geng Chen / Ren Wu) — month branch 申 (Shen, Metal), solar longitude 160.690°

Keys: `observation=structure.scarce` · `theme=authority.balanced` · `action=holding` · `notice=none.Earth`

Why: today's day stem 庚 vs day master 乙 = Direct Officer (正官), natal Metal share 3.7% -> `scarce`; season branch 申 (Metal) is `authority` to the day master; primary hidden stem of 辰 is 戊 = Direct Wealth; branch relation `none` with any natal branch.

> A rule or an expectation is more visible today than you are used to. Structure you did not ask for can still be worth reading before you push back on it.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Do the ten-minute upkeep you keep moving to tomorrow.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### D · 2026-09-04

Day chart: 丙午 丙申 辛巳 甲午 (Bing Wu / Bing Shen / Xin Si / Jia Wu) — month branch 申 (Shen, Metal), solar longitude 161.659°

Keys: `observation=pressure.scarce` · `theme=authority.balanced` · `action=voice` · `notice=clash.year`

Why: today's day stem 辛 vs day master 乙 = Seven Killings (七殺), natal Metal share 3.7% -> `scarce`; season branch 申 (Metal) is `authority` to the day master; primary hidden stem of 巳 is 丙 = Hurting Officer; branch relation `clash` with the natal year branch.

> Something may push at you today without following any rule you recognise. Pressure with no format is still pressure — name it before you answer it.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Write the criticism down in full and do not send it today.
>
> **Notice** — Which old rule of yours is this friction really about?

#### D · 2026-09-05

Day chart: 丙午 丙申 壬午 丙午 (Bing Wu / Bing Shen / Ren Wu / Bing Wu) — month branch 申 (Shen, Metal), solar longitude 162.628°

Keys: `observation=support.scarce` · `theme=authority.balanced` · `action=craft` · `notice=none.Fire`

Why: today's day stem 壬 vs day master 乙 = Direct Resource (正印), natal Water share 11.2% -> `scarce`; season branch 申 (Metal) is `authority` to the day master; primary hidden stem of 午 is 丁 = Eating God; branch relation `none` with any natal branch.

> You may need backing today more than you want to admit. Asking early costs much less than asking once it is urgent.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Finish one small thing you already started. Do not open a new one.
>
> **Notice** — What are you visible for at the moment, and did you choose it?

#### D · 2026-09-07

Day chart: 丙午 丙申 甲申 庚午 (Bing Wu / Bing Shen / Jia Shen / Geng Wu) — month branch 申 (Shen, Metal), solar longitude 164.568°

Keys: `observation=rival.saturated` · `theme=authority.balanced` · `action=structure` · `notice=clash.hour`

Why: today's day stem 甲 vs day master 乙 = Rob Wealth (劫財), natal Wood share 36.0% -> `saturated`; season branch 申 (Metal) is `authority` to the day master; primary hidden stem of 申 is 庚 = Direct Officer; branch relation `clash` with the natal hour branch.

> You may catch yourself measuring — output, pace, who got there first. The comparison is real, and it is still not information about your work.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Take one obligation and check whether anyone still actually expects it.
>
> **Notice** — What are you hurrying toward that would survive being a week later?

#### D · 2026-09-08

Day chart: 丙午 丁酉 乙酉 壬午 (Bing Wu / Ding You / Yi You / Ren Wu) — month branch 酉 (You, Metal), solar longitude 165.539°

Keys: `observation=friend.saturated` · `theme=authority.balanced` · `action=pressure` · `notice=none.Metal`

Why: today's day stem 乙 vs day master 乙 = Friend (比肩), natal Wood share 36.0% -> `saturated`; season branch 酉 (Metal) is `authority` to the day master; primary hidden stem of 酉 is 辛 = Seven Killings; branch relation `none` with any natal branch.

> You already know what you think, and today hands you more of the same conviction. Certainty this smooth is worth testing against one person who does not share it.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Answer one demand with a specific time rather than a specific outcome.
>
> **Notice** — What would you cut if the decision cost you nothing?

#### D · 2026-09-09

Day chart: 丙午 丁酉 丙戌 甲午 (Bing Wu / Ding You / Bing Xu / Jia Wu) — month branch 酉 (You, Metal), solar longitude 166.510°

Keys: `observation=voice.scarce` · `theme=authority.balanced` · `action=holding` · `notice=none.Earth`

Why: today's day stem 丙 vs day master 乙 = Hurting Officer (傷官), natal Fire share 11.2% -> `scarce`; season branch 酉 (Metal) is `authority` to the day master; primary hidden stem of 戌 is 戊 = Direct Wealth; branch relation `none` with any natal branch.

> Something you normally leave unsaid may get close to the surface today. Sharpness that surprises you is worth examining before it is worth using.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Do the ten-minute upkeep you keep moving to tomorrow.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### D · 2026-09-15

Day chart: 丙午 丁酉 壬辰 丙午 (Bing Wu / Ding You / Ren Chen / Bing Wu) — month branch 酉 (You, Metal), solar longitude 172.348°

Keys: `observation=support.scarce` · `theme=authority.balanced` · `action=holding` · `notice=none.Earth`

Why: today's day stem 壬 vs day master 乙 = Direct Resource (正印), natal Water share 11.2% -> `scarce`; season branch 酉 (Metal) is `authority` to the day master; primary hidden stem of 辰 is 戊 = Direct Wealth; branch relation `none` with any natal branch.

> You may need backing today more than you want to admit. Asking early costs much less than asking once it is urgent.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Do the ten-minute upkeep you keep moving to tomorrow.
>
> **Notice** — What are you holding steady for someone else, and who is holding it for you?

#### D · 2026-09-22

Day chart: 丙午 丁酉 己亥 庚午 (Bing Wu / Ding You / Ji Hai / Geng Wu) — month branch 酉 (You, Metal), solar longitude 179.181°

Keys: `observation=opening.saturated` · `theme=authority.balanced` · `action=support` · `notice=clash.day`

Why: today's day stem 己 vs day master 乙 = Indirect Wealth (偏財), natal Earth share 37.9% -> `saturated`; season branch 酉 (Metal) is `authority` to the day master; primary hidden stem of 亥 is 壬 = Direct Resource; branch relation `clash` with the natal day branch.

> There is more on offer today than you can hold. Reaching for all of it is the dependable way to end up with none of it.
>
> There is genuine demand right now and you can meet a decent share of it. Deciding the share is your job, not the demand’s.
>
> **Try** — Ask one person for something small and specific.
>
> **Notice** — What are you defending that nobody has actually attacked?

#### D · 2026-10-10

Day chart: 丙午 戊戌 丁巳 丙午 (Bing Wu / Wu Xu / Ding Si / Bing Wu) — month branch 戌 (Xu, Earth), solar longitude 196.873°

Keys: `observation=craft.scarce` · `theme=wealth.balanced` · `action=voice` · `notice=clash.year`

Why: today's day stem 丁 vs day master 乙 = Eating God (食神), natal Fire share 11.2% -> `scarce`; season branch 戌 (Earth) is `wealth` to the day master; primary hidden stem of 巳 is 丙 = Hurting Officer; branch relation `clash` with the natal year branch.

> You may want to make something today without being able to say why. Appetite is data, even before it points anywhere useful.
>
> Conditions are workable. Effort converts into result at roughly a fair rate right now.
>
> **Try** — Write the criticism down in full and do not send it today.
>
> **Notice** — Which old rule of yours is this friction really about?

---

## 4. URL scheme

State lives entirely in the query string of the App root path `/`. There is no hash/fragment state (`buildUrlStateHref` in `lib/url-state.ts` preserves a fragment but nothing ever writes one), and no path segments.

| Param | Meaning | Format | Validation |
|---|---|---|---|
| `d` | birth date | `YYYY-MM-DD` | `/^\d{4}-\d{2}-\d{2}$/` client-side (`isBirthDetails`) and server-side; server additionally requires year 1902–2099 |
| `t` | birth time | `HH:MM`, 24-hour, zero-padded | `/^\d{2}:\d{2}$/` |
| `tz` | birth-place IANA timezone id | e.g. `America/Los_Angeles` | non-empty string client-side; server checks it with `Intl.DateTimeFormat` |
| `lat` | birth-place latitude | decimal degrees | `Number()`; server rejects outside ±90 |
| `lon` | birth-place longitude | decimal degrees | `Number()`; server rejects outside ±180; drives the true-solar-time correction |
| `place` | display label only | free text | any string, including empty, is accepted |
| `day` | target day | `YYYY-MM-DD` | applied only if it matches the regex; otherwise the app falls back to today in the viewer's timezone |

Behaviour notes:

- All six birth params (`d`, `t`, `tz`, `lat`, `lon`, `place`) must validate together. If any fails, the whole URL state is discarded and the app falls back to `localStorage` key `ming.birth.v1`, then to the empty entry form. `day` is read independently of that.
- After hydration the app rewrites the address bar with `router.replace` into canonical key order `d, t, tz, lat, lon, place, day`.
- Encoding is whatever `URLSearchParams.toString()` produces: space as `+`, `:` as `%3A`, `/` as `%2F`, `,` as `%2C`.
- The App is private; unauthenticated requests to either host redirect to Kylon sign-in.

### Working example — birth A on 2026-09-02

Preview host (draft `ef773e6c6ec2`, deployment `1393a2fbf58e`):

```
https://ming-daily-ritual-ritual-engine-ef773e6c.preview.kylon.app/?d=1994-03-17&t=07%3A42&tz=America%2FLos_Angeles&lat=37.74&lon=-122.46&place=San+Francisco%2C+California%2C+United+States+of+America&day=2026-09-02
```

Release host (the draft was published mid-capture — see observation O-16), same query string:

```
https://ming-daily-ritual.kylon.app/?d=1994-03-17&t=07%3A42&tz=America%2FLos_Angeles&lat=37.74&lon=-122.46&place=San+Francisco%2C+California%2C+United+States+of+America&day=2026-09-02
```

Both were opened and both rendered keys `structure.present` / `resource.weak` / `voice` / `harmony.year`, identical to the engine output in section 3.

---

## 5. Screenshots

All captured at a 420 x 900 phone viewport. Paths are workspace paths under `/workspace/rooms/f8664cffbf96/review/`.

| File | Path | fileId | What it shows |
|---|---|---|---|
| ming-01-entry-form-420x900.jpg | `/workspace/rooms/f8664cffbf96/review/ming-01-entry-form-420x900.jpg` | `c509c9957042` | Entry form: headline, intro, the three inputs, hints, `Read today`, footer disclaimer |
| ming-02-signal-birth-a-2026-09-02.jpg | `/workspace/rooms/f8664cffbf96/review/ming-02-signal-birth-a-2026-09-02.jpg` | `aa3197f0e353` | Generated signal for birth A on 2026-09-02: date line, day nav, `Today's Signal`, four lines, `Try` / `Notice` labels, toggles, provenance line, footer |
| ming-03-calculation-panel-birth-a.png | `/workspace/rooms/f8664cffbf96/review/ming-03-calculation-panel-birth-a.png` | `b49a63bf23fa` | `Show the calculation` expanded for birth A: the two pillar grids and the top of `How the chart was derived` |
| ming-04-selection-keys-birth-a.png | `/workspace/rooms/f8664cffbf96/review/ming-04-selection-keys-birth-a.png` | `67ba7de709c7` | Element balance bars, the strength-band sentence, and `Why these four lines and not others` with all four literal selection keys |
| ming-05-birth-time-caveat-birth-b.png | `/workspace/rooms/f8664cffbf96/review/ming-05-birth-time-caveat-birth-b.png` | `a234908b12a9` | Birth B: selection keys plus the conditional `Worth knowing about your birth time` late-Zi caveat block |

---

## 6. Factual observations

**O-1 — Copy total does not match the stated figure.** 72 authored strings exist, not 76. Per-slot counts do match the stated 30 / 15 / 10 / 17; 30 + 15 + 10 + 17 = 72. Counting sentences rather than strings gives 117.

**O-2 — Two dead keys.** `friend.scarce` and `rival.scarce` in `OBSERVATION` can never be selected. Those two keys require the target day's day-stem element to equal the natal day-master element (`phaseDirection` = `peer`) while the natal share of that element is below the 0.12 `scarce` threshold. The natal day-master stem itself contributes 1.0 to that element in `elementLoad`, and the maximum possible total load is 4 stems + 4 branches x 1.05 hidden weight = 8.2, so the day-master element's share is always at least 1 / 8.2 = 0.1220. Confirmed empirically: 18,000 (birth, day) pairs across 6,000 random births reached 28 of 30 `OBSERVATION` keys, missing exactly these two. All 15 `THEME`, all 10 `ACTION` and all 17 `NOTICE` keys were reached.

**O-3 — The theme line is monthly, not daily.** Its inputs are the target day's *month* branch and the natal strength band; the band never changes for a given birth. In the matrix birth A shows the same theme sentence on 9 of 10 days, birth B on 9 of 10, birth C on 9 of 10, birth D on 9 of 10. The calculation panel discloses this ("it holds for the season rather than changing daily"); the signal screen itself does not.

**O-4 — The reflection tie-break systematically favours the day pillar.** `PILLAR_ORDER` is `day, month, hour, year` and the loop keeps a new best only on a strict `rank >`, so when the target day's branch has the same top-ranked relation with two or more natal branches the earlier pillar always wins. Measured on 2,000 random (birth, day) pairs: 168 of the 1,365 pairs with a non-`none` relation had two or more pillars tied at the top rank.

**O-5 — Individual lines repeat within a short window for the same person.** Birth A gets the identical observation sentence on 2026-09-02 and 2026-09-22 (`structure.present`) and again on 09-05 and 09-15 (`friend.present`); the `none.Fire` reflection appears for A on 09-04, 09-05 and 10-10. Across the full 40-row matrix all 40 four-line combinations are distinct, but single lines repeat up to 6 times.

**O-6 — The same calendar day can produce a different theme depending on the viewer's timezone.** The day chart is read at local noon in `viewerTz`, so on a solar-term boundary day the month branch differs by region. For 2026-09-07: month branch 申 (Shen, `monthIdx` 6) at Pacific/Kiritimati, Asia/Taipei and Europe/London; 酉 (You, `monthIdx` 7) at America/Los_Angeles and Pacific/Midway. The day pillar (甲申) is identical everywhere. In the live UI `viewerTz` is the browser's timezone, so two people opening the same chart URL for 2026-09-07 from London and San Francisco get different theme lines. When the client sends no `viewerTz`, the API silently falls back to the *birth* timezone instead.

**O-7 — Silent coordinate fallback to 0, 0.** `app/api/signal/route.ts` uses `typeof body.lat === "number" ? body.lat : 0` (same for `lon`), and the client's `detailsFromParams` reads `Number(params.get("lat"))`, which is `0` when the parameter is absent. A URL carrying `d`, `t`, `tz` and `place` but no `lat`/`lon` therefore renders a full signal computed at longitude 0 with no warning, and the true-solar-time correction is wrong by up to about 12 hours of longitude offset.

**O-8 — `place` is unvalidated display text.** `isBirthDetails` accepts any string, including empty, and the API echoes `body.place ?? ""`. The provenance line under the signal prints whatever the URL claims, independent of `tz`, `lat` and `lon`. An empty `place` renders the line as a leading ` · Ren day master · ...`.

**O-9 — No gender, sex, pronoun, or name field anywhere.** A case-insensitive grep for `gender|pronoun|male|female|\bhe\b|\bshe\b` across `lib/`, `components/` and `app/` returns nothing. All copy is second-person. The App computes no luck pillars (`大運`), which is the classical construct whose direction depends on sex.

**O-10 — The day master is excluded from its own strength score.** `dayMasterStrength` skips the day pillar's stem entirely, so ratios can be extreme: birth A scores support 0.45 / drain 7.90 = 5.4%. Bands are `< 0.32 weak`, `> 0.48 strong`, else `balanced`, described in-app as MING's own. The most recent commit is `c210670 Recentre day-master strength bands so all three readings occur at similar rates`; measured over 6,000 random births with the checked-in bands the split is weak 2,116 / balanced 1,947 / strong 1,937.

**O-11 — Late-Zi affects the day master, not just the hour.** Birth B (23:15 Europe/London) is shifted to 23:31 sun time, sets `lateZi = true`, and shows the caveat bullet. Because the App keeps the birth on the same civil day, the other convention would produce a different day pillar and therefore a different day master, a different strength band, and a different observation, action and reflection for every day.

**O-12 — The solar-term caveat threshold is much tighter than its copy implies.** The gate is `degreesToTermBoundary < 0.05` degrees of solar longitude, roughly 1.2 hours; the bullet text says "within a few hours of a solar-term boundary". None of the four test births triggered it — closest were D at 2.84° and B at 4.24°.

**O-13 — Birth C triggers the double-hour caveat.** 2001-06-24 14:05 New York has `minutesToHourBoundary = 7.0`, so the caveat block appears with the double-hour bullet. Its true-solar-time correction is −58.39 min, moving the reading clock from 14:05 to 13:07.

**O-14 — Dead helper.** `lib/url-state.ts` exports `readUrlState`, which the ritual App never calls; `ritual-app.tsx` reads `new URLSearchParams(window.location.search)` directly. `readUrlState` is still covered by `lib/url-state.test.ts`.

**O-15 — The independent cross-check skips the disputed case.** `scripts/ming/verify.ts` compares 4,000 random charts against `lunar-javascript` but does `if (h === 23) { lateZi++; continue; }`, so the late-Zi convention — the one place the App knowingly diverges from other calculators — is never compared. Its "different inputs must give different output" check uses three hardcoded people (which do not match the four in this matrix) and a 0.6 uniqueness floor.

**O-16 — App state changed mid-capture.** At checkout, draft `ef773e6c6ec2` was active with READY preview `1393a2fbf58e`. Partway through this capture `app draft list 5d0bcf4c384e` returned "No active drafts" and `app show` reported release URL `https://ming-daily-ritual.kylon.app`, i.e. the draft was published by someone else while this read-only capture was running. Screenshots 1–2 are from the preview host, screenshots 3–5 from the release host; the rendered text matched the checked-out `c210670` source in both cases.

**O-17 — Prediction-word scan.** Across all 72 strings there is no occurrence of `will`, `going to`, `shall`, `predict`, `luck`, `fortune`, `destiny` or `fate`. One string contains "expect" (`refuge.scarce`: "on terms you did not expect"). All 10 `ACTION` strings are imperatives; all 17 `NOTICE` strings end in a question mark. The word "today" appears in 27 of the 30 observations, 1 action and 1 reflection, and in none of the 15 themes (themes use "this period", "right now", "at the moment").

**O-18 — Element bars are non-linear above 38.5%.** The calculation panel renders each element bar as `Math.min(100, share * 260)` percent, so any share at or above 38.5% draws a full bar. Birth B's Earth (43.8%) and birth A's Wood (45.2%) both render as 100% bars while the numeric label next to them reads 44% and 45%.

**O-19 — The `none.*` reflection fallback is chart-independent.** When the target day's branch has no clash, harmony or repeat with any natal branch, the reflection depends only on that branch's element, so unrelated users get the same question on the same day. `none.Fire` occurred 6 times across the matrix, spanning three different births.

**O-20 — Hardcoded fallbacks in the request path.** `viewerTz` falls back to the birth timezone; `lat`/`lon` fall back to 0; `place` falls back to `""`; `trueSolarTime` defaults to on (`body.trueSolarTime !== false`) and is not exposed in the UI; hidden-stem weights fall back to `0.15` (`HIDDEN_WEIGHTS[i] ?? 0.15`). Birth year is hard-limited to 1902–2099 server-side and by the date input's `min`/`max`.
