# Who Should Start? — Module Handoff

## Current Prototype
`sit-start-explorations/sit-start-41.html`
Live (short link): `https://arjun-nara.github.io/fantasy-swipe/`
Direct: `https://arjun-nara.github.io/fantasy-swipe/sit-start-explorations/sit-start-41.html`

`index.html` at repo root redirects to the current latest — update it whenever the canonical prototype changes.

---

## What It Is
A Tinder-style sit/start decision module embedded in the Yahoo Fantasy home feed. Users swipe left/right to pick between two players. Community vote data is shown after each pick. After 6 picks (one page), a "Your picks this session" summary screen appears.

---

## Module Structure
```
[Header] Who should start? | Y+ Ask yours
[Stack]  Card stack (3 visible, front + 2 back cards)
[Done]   "Your picks this session" card (replaces stack)
```

### Key Heights (4px grid)
- Stack wrap: `290px`, `overflow: hidden`
- No poll-below strip — removed in sit-start-38
- Done card: `290px` (matches stack height exactly)
- Card border-radius: `8px`
- Page size: 6 cards per round

---

## Card Design
- **Background:** Dark split — left/right halves use darkened team colours
- **Headshots:** ESPN CDN — `https://a.espncdn.com/i/headshots/nfl/players/full/{espnId}.png`
- **Logos:** ESPN CDN — `https://a.espncdn.com/i/teamlogos/nfl/500/{abbr}.png`
- **Swipe interaction:** Dragging toward a player triggers full-card expand overlay — team colour fills, logo watermark at ~18% opacity, headshot scales 82% → 105%
- **Position chips:** Per-player `pos` field (QB, RB, WR, TE) — never show "FLEX" (fantasy slot, not NFL position)
- **Back cards:** `translateY` only (12px, 22px) — NO width change on promotion, `overflow: hidden` on stack-wrap clips peek
- **Progress ring:** SVG ring depletes from 6→0 as user swipes. Colours: >4 purple, ≤4 amber, ≤2 red

### Swipe Hint Overlay (sit-start-32)
- Dark frosted pill (`rgba(0,0,0,0.58)` + `backdrop-filter: blur(3px)`) centered on the card body
- "👈 Swipe to pick 👉" — fingers animate with `nudgeLeft`/`nudgeRight` (±7px, 1.5s infinite)
- Or-circle has a `::after` pulse ring — scales 1→1.5, 2.4s infinite
- Dismissed on first drag: fades out (`.gone`), removed from DOM after 300ms
- `hintShown` flag — overlay never re-appears within the same session
- Coach marks (external callout arrows) were removed in sit-start-32; card overlay replaces them

### Card Data Model
```js
{
  id, week, context, asker,
  ou: 47.5,
  left:  { name, last, pos, team, espnId, color, logo, proj, opp, defRank, emoji },
  right: { ... },
  lPct: 57,       // % of community votes for left player
  baseVotes: 430
}
```

### defRank Colour Coding
- ≤10 → red (hard matchup)
- 11–20 → amber (mid)
- >20 → green (easy)

---

## Result Moment (between-card reveal)
Replaces the old below-card result bar. After each swipe:

1. Card flies off (~400ms)
2. **Result moment** fades in over the stack area — split card showing both players
3. Halves animate from 50/50 → actual vote proportions (`flex-grow` transition, 0.85s spring)
4. % numbers count up from 0 → actual values (820ms ease-out cubic)
5. 5px colour bar at bottom also animates proportionally (80ms delay behind halves)
6. Footer: vote count + "✓ With the crowd" or "⚡ Bold pick" sentiment
7. Auto-dismisses after 1.6s — **tap anywhere to skip**
8. Next card slides in after dismiss

**Key values:**
- Transition: `flex-grow 0.85s cubic-bezier(0.34, 1.12, 0.64, 1)` (subtle spring overshoot)
- Bar delay: `0.08s`
- Count-up: 820ms, ease-out cubic (`1 - (1-t)^3`)
- Min-width on halves: `96px` (prevents extreme splits from breaking layout)

---

## Your Picks This Session (Done Screen)
**Triggered:** After all 6 cards are swiped
**Size:** 386px — fills full module (stack 290px + poll-below 96px)
**Poll-below:** Hidden when done screen shows, restored on new round

**Design (sit-start-39, tight 290px):**
- White card, `border: 1px solid rgba(0,0,0,0.08)`, subtle shadow
- "YOUR PICKS THIS SESSION" — 12px, uppercase, letter-spacing
- Aggregate "agreed with crowd" strip **removed** — redundant now that result moment shows per-pick sentiment

**Player chips:**
- 2-column grid, `gap: 6px`
- `#f2f4f8` pill, 28px headshot with team colour ring
- Green checkmark or ⚡ for bold pick

**Bold pick section:**
- Only shows if user's lowest-agreement pick was <50% crowd agreement
- Warm amber on white (`#FFFBEB` / `#FDE68A` border)
- Shows: "🔥 Bold pick of the session / [Player] over [Player] / Only X% agreed"

**CTA:** Full-width purple gradient "↻ Pick more players" — `margin-top: auto` pins to bottom

---

## Onboarding Sheet
- Triggers on page load (400ms delay)
- Title: "Who should start?" (removed "NEW!" — was asked twice, keep it gone)
- Body: "Vote on tough calls from around your league. Find out if you're with the crowd — or going bold."
- CTA: "Let's vote"

---

## Design System
- **Grid:** 4px grid — all spacing/sizing must be multiples of 4
- **Purple:** `#7d2eff` (primary), `#9b4fff` (light)
- **Background:** `#f2f4f8`
- **Card:** `#fff`
- **Text:** `#24272e`
- **Dim:** `#61697c`
- **Font:** Plus Jakarta Sans (400, 500, 600, 700, 800, 900)
- **No dark mode** on prototypes unless specified

---

## File Structure
```
fantasy-swipe/
├── index.html                  ← redirects to current latest prototype
├── sit-start-explorations/     ← all numbered prototypes (02–32)
│   └── sit-start-41.html       ← CURRENT LATEST
├── card-explorations/          ← design direction experiments
│   ├── done-screen-option-a.html  ← light done screen (merged into sit-start-30)
│   ├── swipe-hint-animation.html  ← infinite hands + or-circle pulse exploration
│   └── result-direction-1.html    ← dark result bar exploration (not shipped)
├── References/                 ← design reference PNGs
└── HANDOFF.md                  ← this file
```

### Versioning rule
Never overwrite an existing prototype file. Each iteration gets the next number (sit-start-33, 34, …). Update `index.html` to point to the new latest when ready to share.

---

## Next Up
- **Push to GitHub** — sit-start-40 not yet pushed, still local

---

## Open / Backlog
- **Done card scroll bug** — internal scrolling still present on "Your picks this session" (noted in sit-start-30/32). Done card is 386px, content may slightly overflow. Fix: reduce padding or bump height to 400px
- **Fantasy+ blur gate** on result bar — explored in sit-start-27, not in sit-start-32 yet. Needs product sign-off
- **Padding/spacing audit** against 4px grid — some legacy values (10px, 14px etc.) exist in older CSS
- **Real matchup data** via API — all data is currently hardcoded
- **Injury status indicator** on player cards
- **Trending arrows** on projections
- **Share result / post to league chat**
