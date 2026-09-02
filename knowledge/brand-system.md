# Brand System — MING

Machine-readable operating instructions for every agent producing MING work. Human rationale lives in [ming-identity.md](/rooms/353871fb77e1/file/a97b343243f1) (v1.0, confirmed by zhihao feng 2026-09-01); this file is the executable version of it. When the two disagree, the identity doc wins and this file gets fixed.

MING is the only brand in this workspace. There is no separate "Success" corporate identity yet.

## The one rule everything derives from

BaZi is a calendar, not an oracle — so MING is an almanac, not a horoscope. Warm paper instead of void, pigment instead of monochrome, terrestrial instead of celestial, precise-and-kind instead of precise-and-cruel. If a choice does not serve that sentence, it is out.

## CSS custom properties

```css
:root {
  /* base — the almanac page */
  --paper:        #EFEDE4;  /* default background everywhere */
  --paper-raised: #F8F6F0;  /* sheets and panels above the page */
  --paper-sunk:   #E5E2D6;  /* recessed fields, stripes, disabled */
  --rule:         #D6D2C4;  /* hairlines and borders, 1px always */
  --ink:          #171A18;  /* all primary text and the wordmark */
  --ink-soft:     #5A5F59;  /* secondary text, marginalia, timestamps */

  /* the five phases — the data layer, used as fills */
  --wood:  #4A5D3A;
  --fire:  #BE4127;   /* also the accent; see accent rule */
  --earth: #AD8449;
  --metal: #7C8A91;
  --water: #2F5468;

  /* type-only phase variants — chart glyphs only */
  --earth-ink: #7E5C2C;   /* 5.18:1 on paper */
  --metal-ink: #586A72;   /* 4.81:1 on paper */

  /* type */
  --font-display: 'Newsreader', Georgia, serif;
  --font-body:    'Newsreader', Georgia, serif;
  --font-mono:    'IBM Plex Mono', ui-monospace, monospace;

  /* style */
  --radius:  2px;   /* maximum, anywhere */
  --shadow:  none;  /* there are no shadows */
  --border:  1px solid var(--rule);
}
```

Font import (both families, required):

```
https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,200..500;1,6..72,200..500&family=IBM+Plex+Mono:wght@400;500&display=swap
```

## Accent rule (read this before using cinnabar)

`#BE4127` is both 火 Fire and the brand accent, which collides on any surface that shows a chart.

- **Surface renders chart data** → cinnabar means Fire and nothing else. "Now" / active / selected is marked by **inversion**: `background:var(--ink); color:var(--paper)` on a mono stamp. Use the all-ink mark, not the cinnabar one.
- **Surface renders no chart data** (marketing, landing page, poster) → cinnabar is the single accent, one per surface, meaning "now". The brand-neutral mark keeps its cinnabar square.

Never a second accent. Never a gradient.

## Type scale

| Role | Setting |
|---|---|
| Display | Newsreader 300, 44–72px, line-height 1.05, tracking −0.01em |
| Headline | Newsreader 400, 26–34px, line-height 1.2 |
| Body | Newsreader 400, 17px, line-height 1.65, measure 60–66 characters |
| Reading emphasis | Newsreader 400 *italic*, `--ink` — the daily reflection question only |
| Label / stamp | IBM Plex Mono 500, 11–13px, UPPERCASE, tracking +0.12em |
| Data / chart | IBM Plex Mono 400–500, tabular |

No sans-serif anywhere. No weight above 500 and no bold — emphasis is size, space, and color. Mono never sets body copy. One display line per surface.

## Logo

The mark is the user's own eight characters: a 4 × 2 grid of hard-edged squares, four pillars across (right to left: year, month, day, hour), stems on top, branches beneath. It is not a pun on 明 — 明 is [⿰日月](https://en.wiktionary.org/wiki/%E6%98%8E), a left–right compound, and any asset saying "sun over moon" is wrong.

- Brand-neutral: [ming-mark.svg](/preview/workspaces/fc8a8e44157c/files/e99fa375b31c) — all ink except the day-pillar stem in cinnabar.
- In-product, beside live chart data: [ming-mark-ink.svg](/preview/workspaces/fc8a8e44157c/files/0dc484047fbc) — all ink.
- Personal mark: every square is a phase color, never ink. Ink squares mean "this is the logo, not a person."
- Minimum width 24px. Clear space equals one square on every side. 1px paper gutter between squares, 2px radius maximum.
- Wordmark: MING, Newsreader 300, uppercase, tracking +0.18em, `--ink`. Never on a colored field, never in cinnabar, never with a tagline locked to it.
- Never AI-regenerate, recolor, round into dots, outline, rotate, or animate the mark. Composite from the SVG.

## Imagery

Documentary macro photographs of the five phases as physical matter — split pine end-grain, banked embers, cracked clay, an unpolished blade, still deep water. Flat overcast light, straight-on, frame-filling, no hands, no faces, no props. Each printed as a duotone in `--paper` plus that phase's pigment. Second layer: the chart itself set large as type-as-image. Nothing else is on-brand.

## Voice

Speaker: a knowledgeable friend reading the weather with you. Warm, not soft. Exact, not cold. Never impressed with itself.

Grammar: conditions, then consequence, then choice. Second person, present tense, active voice. Short sentences, one idea each, average under twenty words. Name the mechanism — the pillar, the phase, the month — because the reason is the product. One metaphor per surface, from weather, seasons, materials, or craft.

A headline may run ahead of its mechanism only when the mechanism appears on the same surface within two lines.

**Banned words:** destiny, fate, the universe, energy (as vibes), vibration, abundance, manifest, align/alignment, journey, blessed, cosmic, divine, sacred, unlock, reveal, lucky, unlucky, "the stars," "meant to be," "trust the process," "hold space," "authentic self."

**House words:** conditions, tendency, easier / harder, season, month, pillar, phase, grain, friction, ballast, weight, edge, damp, dry, room, pace.

**Product nouns:** the three areas are **Chart**, **Season**, **Overlap**. The daily ritual is **the note**. Date stamps use the solar term: `白露 · WHITE DEW · DAY 3`. Positioning line: *MING — an almanac for a person.* Supporting: *Four Pillars, in plain English. Your conditions, daily, in two minutes.*

## Ship test

1. Cover the wordmark — is it still obviously MING?
2. Could this line be pasted into another astrology app unchanged? Then it is Barnum.
3. Does it predict an event? Cut it.
4. Is a mechanism visible — pillar, phase, month?
5. More than one accent, or any gradient? Fix it.
6. Would this have looked at home on a 2009 astrology website? Start over.
7. Swap the nouns in the strongest sentence — does it still work? Then it is a template.
8. Can the reader check the reading against their own chart?
9. Would anyone post this? If nothing on the surface is theirs alone, it is not a share asset.

## Hard constraints

- No gradient, glow, bloom, or drop shadow, anywhere.
- No pure white and no pure black surfaces. `#000` belongs to Co-Star.
- Hairlines are 1px `--rule`. Depth comes from paper-tone shifts, never from elevation.
- Phase colors at full strength or 8% tint on paper. No intermediate opacities.
- At most two phase colors plus ink per surface, unless the surface is the chart.
- Nothing is centered except the mark — chart grids included.
- Measure never exceeds 66 characters.
- Radius never exceeds 2px.
- Never a percentage compatibility score, a "lucky day," or a future-tense event claim.
- Never a template sentence whose nouns can be swapped.
- Never assert a fact about the user's week the system cannot know.

## Anti-patterns (disqualifying on sight)

Purple-to-black gradients · star fields · glowing orbs · gold foil · dragons · taiji symbols · zodiac animals · coins · incense · crystals · tarot · brush calligraphy as decoration · red-and-gold luck styling · Inter or any sans-serif · violet gradients · rounded blob illustration · 3D glass · drop shadows · gradient meshes · "Unlock your personalized insights" · exclamation marks · emoji · spa beige · soft-focus hands · breathwork register · Co-Star's pure-black brutalism, NASA cutouts, and deadpan cruelty.

## Machine-readable kit

```json
{
  "brand_name": "MING",
  "domain": null,
  "industry": "Consumer app — Four Pillars (BaZi) self-knowledge and daily ritual",
  "style_keywords": ["almanac", "warm paper", "semantic pigment", "reading serif", "instrument mono", "terrestrial", "documentary", "restrained", "explanatory"],
  "logo": {
    "primary_file": "/workspace/shared/brand/ming/ming-mark.svg",
    "icon_file": "/workspace/shared/brand/ming/ming-mark.svg",
    "dark_variant": null,
    "light_variant": "/workspace/shared/brand/ming/ming-mark-ink.svg",
    "min_width_px": 24,
    "clear_space": "One grid square on every side",
    "source": "user-selected",
    "confidence": "high",
    "rule": "Never let AI regenerate. Always composite from source file. Personal marks recolor squares to phase colors only, never ink."
  },
  "colors": {
    "primary":        { "hex": "#171A18", "usage": "All primary text, wordmark, mark squares, inverted 'now' stamps", "source": "user-selected", "confidence": "high" },
    "secondary":      { "hex": "#5A5F59", "usage": "Secondary text, marginalia, timestamps", "source": "user-selected", "confidence": "high" },
    "accent":         { "hex": "#BE4127", "usage": "Cinnabar. 火 Fire in the data layer; single accent meaning 'now' only on surfaces with no chart data", "source": "user-selected", "confidence": "high" },
    "background":     { "hex": "#EFEDE4", "usage": "Default page background everywhere", "source": "user-selected", "confidence": "high" },
    "surface":        { "hex": "#F8F6F0", "usage": "Raised sheets and panels", "source": "user-selected", "confidence": "high" },
    "surface_sunk":   { "hex": "#E5E2D6", "usage": "Recessed fields, table stripes, disabled states", "source": "user-selected", "confidence": "high" },
    "border":         { "hex": "#D6D2C4", "usage": "1px hairline rules and borders", "source": "user-selected", "confidence": "high" },
    "text_primary":   { "hex": "#171A18", "usage": "Body and display type on paper", "source": "user-selected", "confidence": "high" },
    "text_secondary": { "hex": "#5A5F59", "usage": "Captions, mono labels, marginalia", "source": "user-selected", "confidence": "high" },
    "phase_wood":     { "hex": "#4A5D3A", "usage": "木 Wood fill; may set chart glyphs (6.14:1 on paper)", "source": "user-selected", "confidence": "high" },
    "phase_fire":     { "hex": "#BE4127", "usage": "火 Fire fill; may set chart glyphs (4.50:1 on paper)", "source": "user-selected", "confidence": "high" },
    "phase_earth":    { "hex": "#AD8449", "usage": "土 Earth fill only; 2.90:1 on paper, never as type", "source": "user-selected", "confidence": "high" },
    "phase_metal":    { "hex": "#7C8A91", "usage": "金 Metal fill only; 3.03:1 on paper, never as type", "source": "user-selected", "confidence": "high" },
    "phase_water":    { "hex": "#2F5468", "usage": "水 Water fill; may set chart glyphs (6.92:1 on paper)", "source": "user-selected", "confidence": "high" },
    "phase_earth_type": { "hex": "#7E5C2C", "usage": "土 Earth chart glyphs only, 5.18:1 on paper", "source": "user-selected", "confidence": "high" },
    "phase_metal_type": { "hex": "#586A72", "usage": "金 Metal chart glyphs only, 4.81:1 on paper", "source": "user-selected", "confidence": "high" }
  },
  "typography": {
    "heading": { "family": "Newsreader", "weights": [200, 300, 400, 500], "fallback": "Georgia, 'Times New Roman', serif", "source": "user-selected", "confidence": "high" },
    "body":    { "family": "Newsreader", "weights": [300, 400], "fallback": "Georgia, 'Times New Roman', serif", "source": "user-selected", "confidence": "high" },
    "import_url": "https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,200..500;1,6..72,200..500&family=IBM+Plex+Mono:wght@400;500&display=swap",
    "scale": {
      "h1":      { "size": "clamp(44px, 6vw, 72px)", "line_height": 1.05, "weight": 300 },
      "h2":      { "size": "34px", "line_height": 1.2,  "weight": 400 },
      "h3":      { "size": "26px", "line_height": 1.25, "weight": 400 },
      "h4":      { "size": "21px", "line_height": 1.35, "weight": 400 },
      "body":    { "size": "17px", "line_height": 1.65, "weight": 400 },
      "label":   { "size": "12px", "line_height": 1.4,  "weight": 500 },
      "caption": { "size": "13px", "line_height": 1.5,  "weight": 400 }
    }
  },
  "style": {
    "border_radius": { "none": "0", "default": "2px", "max": "2px" },
    "shadows": { "none": "none" },
    "spacing": { "hairline": "1px", "tight": "8px", "default": "26px", "section": "56px", "page_margin": "76px" },
    "illustration_style": "No illustration. Duotone documentary macro photography of matter in paper plus one phase pigment, or the chart set large as type-as-image.",
    "icon_style": "Almost none. Mono uppercase labels and 1px rules do the work icons would do."
  },
  "rules": [
    "Cinnabar means Fire and nothing else on any surface rendering chart data; 'now' is marked by ink/paper inversion there.",
    "Cinnabar is the single accent, meaning 'now', only on surfaces with no chart data.",
    "Phase colors are fills; only chart glyphs may set type, using earth/metal type variants.",
    "Italic is reserved for the daily reflection question, set in ink.",
    "Mono sets labels, data, dates, and marginalia; never body copy.",
    "Nothing is centered except the mark, chart grids included.",
    "Every note shows its derivation — pillar, phase, month — so the reader can check it.",
    "The personal eight-square mark is the share surface: export card and profile mark, no note copy on either.",
    "'Almanac' never travels without a solar-term stamp beside it.",
    "Long-cycle 大運 content is sold retrospectively first: the decades already lived are checkable."
  ],
  "anti_patterns": [
    "Gradients, glows, blooms, drop shadows",
    "Pure white or pure black surfaces",
    "Any sans-serif",
    "Star fields, constellations, planets, moons, nebulae",
    "Gold foil, red-and-gold luck styling, dragons, taiji, zodiac animals, coins, incense, crystals, tarot",
    "Brush calligraphy as decoration",
    "Spa beige, soft-focus hands, wellness or breathwork register",
    "Percentage compatibility scores, 'lucky day', future-tense event claims",
    "Template sentences whose nouns can be swapped",
    "Claims about the user's recent week that the system cannot know",
    "Emoji, exclamation marks, ALL-CAPS emphasis",
    "Co-Star's pure-black brutalism, NASA cutouts, and deadpan cruelty"
  ],
  "templates": { "image": null, "ppt": null, "web": null },
  "metadata": {
    "created_at": "2026-09-02",
    "last_updated": "2026-09-02",
    "sources": [
      "/workspace/rooms/353871fb77e1/ming-identity.md v1.0 — authored by Nora, confirmed by zhihao feng 2026-09-01",
      "/workspace/rooms/353871fb77e1/ming-identity-review-dev.md — Dev's sourced review, 2026-09-02",
      "/workspace/rooms/353871fb77e1/ming-assets/ — rendered v1.0 assets",
      "WCAG 2.x relative-luminance ratios computed from the hex values above"
    ]
  }
}
```

## Reference assets

Rendered v1.0 surfaces showing the system applied: [mark system](/preview/workspaces/fc8a8e44157c/files/69821af5ce51?room=353871fb77e1), [identity board](/preview/workspaces/fc8a8e44157c/files/5d6e38f042bf?room=353871fb77e1), [daily note card](/preview/workspaces/fc8a8e44157c/files/c2ba9e012e44?room=353871fb77e1), [season card](/preview/workspaces/fc8a8e44157c/files/0d1268616f71?room=353871fb77e1), [overlap card](/preview/workspaces/fc8a8e44157c/files/300ecf75b8fb?room=353871fb77e1), [positioning poster](/preview/workspaces/fc8a8e44157c/files/68286d9c234e?room=353871fb77e1), [product screens](/preview/workspaces/fc8a8e44157c/files/710893d663cc?room=353871fb77e1).
