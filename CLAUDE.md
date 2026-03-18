# Claude Projects — Persistent Context

## About Me
- Product designer / PM working on consumer sports/fantasy apps
- Prototyping features for a Yahoo Fantasy-style product
- Preference: build fast, match designs closely to reference PNGs, swipe-only interactions
- Design references stored in `fantasy-swipe/references/` (moved from Desktop)

---

## How to Start a Session
```bash
cd ~/Desktop/Claude\ Projects
claude
```
Then tell me which project to work on or start a new one.

---

## Projects

### fantasy-swipe/
**What it is:** Tinder-style sit/start decision tool for fantasy football rosters, embedded as a module inside a Yahoo Fantasy home screen.

**Stack:** Single-file vanilla HTML/CSS/JS — no build tools, no dependencies. Open `index.html` directly in a browser.

**Key design decisions:**
- Matches Yahoo Fantasy visual language (purple `#5B0DCC`, white cards, Plus Jakarta Sans font)
- Card design: dark split halves with darkened team colours, ESPN headshots, "or" circle center
- Swipe interaction: dragging toward a player triggers a full-card expand overlay — team colour fills card, team logo appears at ~18% opacity watermark, headshot grows from 82% → 105% scale
- Post-swipe: vote % result bar fades in below the card (matches screen 736)
- Live vote counter ticks up every ~1.5s to create social proof
- Circular SVG progress ring replaces text card counter
- 8-card pool split into pages of 4; loops infinitely with a "More Decisions!" flash
- No sit/start buttons — swipe only

**Data model per card:**
```js
{
  id, position, week, context,
  ou: 47.5,                         // game over/under
  left:  { name, last, team, espnId, color, logo, proj, opp, defRank, emoji },
  right: { ... },
  lPct: 57,       // % of community votes for left player
  baseVotes: 430
}
```

**defRank colour coding:** ≤10 = red (hard), 11–20 = amber (mid), >20 = green (easy)

**Image CDNs:**
- Headshots: `https://a.espncdn.com/i/headshots/nfl/players/full/{espnId}.png`
- Logos:     `https://a.espncdn.com/i/teamlogos/nfl/500/{abbr}.png`

**Known ESPN player IDs:**
| Player | ID |
|---|---|
| Josh Allen | 3918298 |
| Patrick Mahomes | 3139477 |
| Derrick Henry | 3043078 |
| Saquon Barkley | 3929630 |
| Tyreek Hill | 3116406 |
| CeeDee Lamb | 4241389 |
| Travis Kelce | 15847 |
| Sam LaPorta | 4430027 |
| Davante Adams | 16800 |
| Jaylen Waddle | 4372016 |
| Tony Pollard | 3916148 |
| Joe Mixon | 3116385 |
| Lamar Jackson | 3916387 |
| Dak Prescott | 2577417 |
| DJ Moore | 3915416 |
| Amari Cooper | 2976499 |

**Design reference files (`fantasy-swipe/references/`):**
- `729.png` — Yahoo Fantasy home feed, original reference
- `Sit-Start Card - 1.png` — Card default state (dark split, player info, team logos bottom corner)
- `Sit-Start Card - 2.png` — Post-swipe result bar (names above bar, % inside, team colours)
- `Sit-Start Card - 3.png` — First-load state showing "Swipe to pick" hint overlay
- `Screenshot 2026-03-16 at *.png` — Various in-progress prototype screenshots

**Upcoming / backlog ideas:**
- Add player injury status indicator
- Trending up/down arrows on projections
- Share result / post to league chat
- Real matchup data via API

---

## My Preferences
- **Communication:** Short and direct, no filler
- **Code:** Minimal, no unnecessary abstractions, edit existing files rather than creating new ones
- **Design:** Always read reference PNGs before building UI, match them closely
- **Figma:** Share frames as exported PNGs — more reliable than Figma links for design matching
- **No dark mode** on prototypes unless specified — match Yahoo's light theme
- **Prototypes** are single HTML files unless the project clearly needs more
