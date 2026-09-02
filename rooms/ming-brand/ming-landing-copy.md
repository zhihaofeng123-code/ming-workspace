# MING — landing page copy, v1.0

Every line here is final copy, in MING's confirmed voice ([brand-system.md](/workspace/file/440cc25ee791)). Set what is in quotes; the notes beside each block are for whoever builds the page, not for the page. Anything not specified defaults to the brand system: paper `#EFEDE4`, Newsreader, mono labels, 1px rules, nothing centered except the mark.

The page has one job: make a stranger understand that this is a calendar, not a prediction, and want their eight characters anyway.

---

## 1 · Hero

> **An almanac for a person.**
>
> Your birth date, time, and place make eight characters — four pillars of the Chinese calendar. They describe the conditions you work with, not the events ahead of you. MING reads them in plain English.
>
> `[ Get your eight characters ]`
>
> Free while we build. No card.

Notes — display line is the only one at 44–72px on the page. The mono stamp `處暑 · END OF HEAT · DAY 10` sits above the headline, computed live from the visitor's own date, not hardcoded; it is what stops "almanac" reading as decoration. Cinnabar is allowed here — no chart data on this surface — as exactly one accent, on the stamp. Button is ink fill, paper text, 2px radius.

## 2 · What it is not

> Not a horoscope.
>
> A horoscope tells you what happens. An almanac tells you what season it is and lets you decide what to plant. Four Pillars is the second kind. It has been used to time harvests, moves, and marriages for about a thousand years, and it works the same way for a Tuesday.

Notes — this block earns the rest of the page, so it goes second, above the feature sections. Set as body, one column, 60–66 characters. No illustration.

## 3 · The chart

> **Chart**
>
> Eight characters: a stem and a branch for the year, month, day, and hour you were born. Each one carries a phase — wood, fire, earth, metal, water. The mix is not a personality type. It is a set of materials, and it tells you what you are working with and what you are short of.
>
> Yours might read: heavy metal, no fire. That is a person who can cut cleanly and struggles to start things warm.

Notes — beside this block, the visitor's chart grid, or a sample chart if they have not entered a birth date. This surface renders chart data, so the all-ink mark, and cinnabar is Fire only. Glyphs carry their phase colour using `--earth-ink` and `--metal-ink` where those two appear.

## 4 · The season

> **Season**
>
> The calendar keeps moving after you are born. Every month, and every ten years, the conditions around your chart change — more water, less fire, a branch that clashes with one of yours.
>
> That is what a hard year is. Not a punishment. A season your materials are badly suited to, which is worth knowing in advance the way a frost date is worth knowing.

Notes — no wellness register, no reassurance. The word "punishment" is doing real work; keep it.

## 5 · The overlap

> **Overlap**
>
> Put two charts side by side and you can see where they help and where they grind. Earth against earth is not incompatibility — it is two people who both want to be the stable one.
>
> No score. No percentage. No verdict. Just the mechanism, so you can both read it.

Notes — the three-mono-label row `NO SCORE · NO PERCENTAGE · NO VERDICT` reuses the overlap card exactly. This is also the invite loop; the CTA under it is `[ Compare with someone ]`, greyed until they have a chart.

## 6 · The daily note

> Two minutes, every morning.
>
> One thing today's pillar does to yours. One theme running under the month. One thing worth doing about it. One question, which nobody sees but you.
>
> It always shows its work: which pillar, which phase, which month. If it does not match your day, you can see exactly where it got that, and so can we.

Notes — the daily note card sits beside this, at real size. The reflection question is italic ink, never cinnabar.

## 7 · The mark

> Your eight characters make a mark that is only yours.
>
> Four pillars across, stems above branches, coloured by phase. It is your chart at a glance, and it is the only part of MING built to be shared. The reading stays yours.

Notes — export card preview here, 4×2 grid plus solar-term stamp plus small wordmark. This block replaces the screenshot-a-brutal-sentence loop; it must look like something a person would put on a profile.

## 8 · Waitlist

> **Get your eight characters.**
>
> Enter your birth date, time, and place. We will build your chart and send it when MING opens.
>
> `[ email ]` `[ Join the waitlist ]`
>
> One email when it is ready. Nothing else.

Notes — birth time optional with the honest inline note: "Don't know your birth time? Enter the date. The hour pillar will be missing and we'll say so." Never guess a pillar silently. Sunk-paper field, 1px rule, no placeholder text inside the input doing the label's job.

## 9 · Footer

> MING — an almanac for a person.
>
> Four Pillars, also called BaZi (八字, "eight characters"). We do not predict events, tell fortunes, or sell luck. Conditions and choices.

---

## Rules for this page

Present tense throughout — the page describes what MING does, not what it will do for you. No sentence may work with its nouns swapped out. No exclamation marks, no emoji, no "unlock", "reveal", "journey", "align", or "the universe". One metaphor per section, from weather, seasons, materials, or craft. Every claim about a person names the pillar, phase, or month it came from within two lines. Nothing on the page promises a future event, including in microcopy and button labels.

Measure caps at 66 characters. Nothing is centered except the mark. Cinnabar appears once per screenful and never on a surface showing chart data — there, "now" is ink-fill inversion.
