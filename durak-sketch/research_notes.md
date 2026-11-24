# Durak Redesign Research Notes — GAME TIME

## 1. Competitive Analysis (Top Durak Apps)
- **Durak Online (Rstgames / Magic Board, 50M+ downloads):**
  - Player hand sits **bottom-center** in a wide, gently curved fan in both portrait and landscape (never tucked into a side column).
  - Large readable corner rank+suit indices; legal/selected cards lift clearly.
  - **28 distinct sound effects** (deal rustle, card place, select tick, turn tick, match end).
  - Clear visual separation between talon (left), active attack/defence pairs (center), and discard/bito (right).
- **Durak (Kaufcom / Playvision 3D / cmyksoft):**
  - Classic **face-up trump card rotated 90° underneath the talon**, protruding 55–75% so the rank and suit pip are unmistakable at a glance.
  - When the talon empties, a persistent **trump-suit watermark/badge** stays visible in the talon spot.
  - Attack/defence pairs overlap with a clear diagonal offset (`+18pt, +22pt`) and slight natural angle so both cards remain readable.
  - Empty discard pile never displays a distracting `"0"` badge.
- **UX Case Study (xDurak):**
  - Emphasize player roles clearly (**Attacker sword** vs **Defender shield**) on opponent chips.
  - Smoother pacing between turns so players can read the defence before cards sweep away.

## 2. Current Implementation Audit (`main` @ `996f8e6`)
- **Card sizes (`durak_table.dart` / `durak_card_painter.dart`):**
  - Table pairs (`_Pair`): `54.0pt` width (`54 × 76.3pt`), scaled further down by `FittedBox` on multi-bout tables.
  - Player hand (`_humanHand`): capped at `68.0pt` (`68 × 96.1pt`); overflows/clips on >9 cards; in landscape shoved into a `min(300, 0.40 * W)` right-side column fighting with the discard pile.
  - Talon / Trump (`_Talon`): `deckW = 42pt`, `trumpW = 44pt`, `peek = 26pt` (rank barely peeks out).
  - Opponent backs (`_OpponentSeat`): `30pt` (`26pt` compact).
  - Empty Discard (`_Discard`): shows a floating `"0"` badge when empty.
- **Why it feels like "no animations, runs too fast":**
  - `DurakPlayController.aiDelay` is a flat `550ms` and runs concurrently with `DurakMotion` flights (`280–320ms`), often cutting off the initial deal (`676ms`) or sweeping cards away before the player reads them.
  - Hand card selection uses `Positioned` + `AnimatedSlide(offset: Offset.zero)` — snapping instantaneously with no vertical lift animation.
  - No 3D card flip (trump reveal / bito face-down flip), no post-defence pause before sweep, and no sound calls anywhere in Durak (`Sfx` has 0 `durak_*` events).

## 3. Card Size Upgrades (Logical pt)
| Element | Today | Proposed Portrait | Proposed Landscape | Growth |
|---|---|---|---|---|
| Table Cards (`_Pair`) | `54pt` | `76pt` (`64pt` in 2x2/2x3 grid) | `68pt` | **+41% width (+98% area)** in portrait, **+26% (+58% area)** in landscape |
| Player Hand | `68pt` (right col in landscape) | `84pt` (bottom center fan) | `74pt` (bottom center fan) | **+24% width (+53% area)**, adaptive overlap prevents clipping |
| Talon & Trump | `42pt` / peek `26pt` | `56pt` / peek `46pt` | `54pt` / peek `42pt` | **+33% deck, +77% trump exposure** + warm golden glow |
| Opponent Backs | `30pt` (`26pt` compact) | `40pt` | `34pt` | **+33%** + avatar frame & role icon |

## 4. Timing & Pacing Plan
- **Initial Deal:** `360ms` flight per card × `75ms` stagger, followed by **Trump Reveal** (`380ms` slide-out + 3D flip + `500ms` gold pulse). AI input waits until deal+reveal finishes.
- **Hand Select:** `180ms` (`Curves.easeOutBack`) smooth `+18pt` lift + brass glow.
- **Play / Attack:** `360ms` parabolic arc (`1.10x` lift scale, `±4°` tilt, deeper drop shadow).
- **Defence Beat:** `380ms` arc landing at `+18, +22pt` offset (`5°` tilt).
- **Bout Resolution Sequencing:**
  1. Wait for final card flight (`380ms`) + **`280ms` readability pause**.
  2. **Take (`440ms`)** or **Bito Discard (`420ms` with 3D Y-flip to face-down & `±6°` scatter)**.
  3. **`200ms` beat**, then **Draw-Up** (`340ms` per card × `75ms` stagger, attacker -> others -> defender; final trump card rotates `90° -> 0°`).
- **AI Think Delay:** Randomized **`750–1,250ms`** (plus guaranteed wait for any active flight + `250ms` settle), replacing flat `550ms`.

## 5. Sound Plan (Reusing `SoundService` + `sound_events.json` v2)
- **Reuse existing:** `ui_button_brass`, `ui_error`, `game_win`, `game_lose`, `music_ingame_loop`.
- **Add 8 `durak_*` events (OGG + M4A):**
  1. `durak_card_deal` (`~120ms`, `overlap: true`, `throttle_ms: 45`)
  2. `durak_card_select` (`~80ms`, `overlap: false`, `throttle_ms: 40`)
  3. `durak_card_play` (`~180ms`, `overlap: true`, `throttle_ms: 50`)
  4. `durak_card_beat` (`~200ms`, `overlap: true`, `throttle_ms: 50`)
  5. `durak_take` (`~350ms`, `overlap: false`, `throttle_ms: 250`)
  6. `durak_bito` (`~320ms`, `overlap: false`, `throttle_ms: 250`)
  7. `durak_trump_reveal` (`~450ms`, `overlap: false`, `throttle_ms: 500`)
  8. `durak_turn_cue` (`~220ms`, `overlap: false`, `throttle_ms: 400`)
- **Free / CC0 Candidate Sources:**
  - **Kenney.nl Casino Audio & Boardgame Pack** (`CC0 1.0 Public Domain`, commercial use OK, no attribution required): `card-slide-1..8.ogg`, `card-place-1..4.ogg`, `card-fan-1..2.ogg`, `card-shove-1..4.ogg`, `card-shuffle.ogg`.
  - **OpenGameArt StarNinjas Page/Card Flips** (`CC0 1.0`) for soft flip/reveal texture.
