# Board-games app - FULL sound set (stage 2)

Classic club style (wood, brass, ivory, felt, soft piano/jazz). 56 sounds = 12 approved stage-1 sounds (copied byte-for-byte from `../samples/`) + 44 new stage-2 sounds. All are original synthesis (see `../LICENSES.md`).

## Contents
| Path | What |
|---|---|
| `ogg/*.ogg` (56) | OGG Vorbis q6 - **recommended on Android and for the loops (gapless)** |
| `m4a/*.m4a` (56) | AAC-LC 128 kb/s mono / 192 kb/s stereo, faststart - recommended on iOS |
| `sound_events.json` | event_id -> file base, ogg/m4a path, category, game, volume, loop, overlap, throttle_ms, duration, channels |
| `analysis.json` | per file: duration, sample/true peak, momentary-max + integrated LUFS, DC, edges, centroid, limiter GR, encoded OGG/M4A true peak, size; loop seam check for loops |
| `full_preview.mp4` / `.srt` | all 56 sounds grouped by category (UI, Backgammon, Chess, Sudoku, Mahjong, Game result, Music excerpts), Hebrew + English captions burned in; H.264 High@4.0 yuv420p + AAC 192k, 1280x720, faststart |
| `../src/` | `dsp.py`, `sfx.py`, `music.py`, `build.py` (stage 1); `sfx2.py`, `music2.py`, `build_final.py`, `preview_full.py`, `readme_final.py` (stage 2); `masters/*.wav` (float32 masters, all 56) |

Rebuild: `cd sound/src && python build_final.py && python preview_full.py && python readme_final.py` (venv: `requirements.txt`).

## Format and loudness (same rules as stage 1)
- 44.1 kHz. SFX mono except `sd_puzzle_complete`, `sd_mistake_limit`, `mj_board_cleared` (stereo); music stereo.
- SFX: normalized to **-16 LUFS momentary-max** (400 ms BS.1770), true-peak limited to -1.5 dBTP with at most 4 dB gain reduction; after encoding every OGG/M4A is decoded and re-trimmed if its true peak exceeds -1.0 dBTP. Result: every encoded file is <= -1.0 dBTP.
- Very short transients (< 0.3 s ticks/clicks) can't reach -16 LUFS M without crushing the attack, so they sit at -17.5 to -20.5 LUFS M (list: `analysis.json`, momentary_max_lufs).
- Music: `music_menu_loop` -20 LUFS I (approved), **`music_ingame_loop` -22 LUFS I** (calmer, 60 BPM, 64.0 s = 16 bars, circularly rendered so the seam is seamless), `music_victory_sting` -20 LUFS I, 11.5 s.

## EVENT MAP
Volume = suggested player volume (0-1) on top of the master SFX/music sliders. Overlap = may the same sound play over itself (else stop/skip the running instance). Throttle = minimum gap between two triggers of the same event.

| File | Category | Game | Trigger | Duration | Loop | Volume | Overlap | Throttle | LUFS | dBTP | Stage |
|---|---|---|---|---|---|---|---|---|---|---|---|
| `ui_button_brass` | ui | all | any primary button tap | 0.34 s | no | 0.80 | no | 60 ms | -17.87 M | -1.54 | 1 (approved) |
| `ui_screen_transition` | ui | all | screen push/pop transition | 0.62 s | no | 0.70 | no | 300 ms | -16.0 M | -2.83 | 1 (approved) |
| `ui_error` | ui | all | generic error / invalid action | 0.55 s | no | 0.80 | no | 250 ms | -16.01 M | -1.54 | 1 (approved) |
| `ui_level_unlock` | ui | all | new level / game unlocked | 1.80 s | no | 0.90 | no | 1000 ms | -16.32 M | -1.54 | 1 (approved) |
| `ui_back` | ui | all | back button, close (X) | 0.30 s | no | 0.80 | no | 120 ms | -19.82 M | -1.55 | 2 |
| `ui_toggle_on` | ui | all | switch turned on | 0.45 s | no | 0.80 | no | 80 ms | -19.32 M | -1.54 | 2 |
| `ui_toggle_off` | ui | all | switch turned off | 0.45 s | no | 0.80 | no | 80 ms | -19.46 M | -1.54 | 2 |
| `ui_dialog_open` | ui | all | modal/dialog appears (with motion start) | 0.69 s | no | 0.70 | no | 300 ms | -16.18 M | -1.54 | 2 |
| `ui_dialog_close` | ui | all | modal/dialog dismissed | 0.50 s | no | 0.70 | no | 300 ms | -16.24 M | -1.54 | 2 |
| `ui_tab_switch` | ui | all | tab / segmented control change | 0.28 s | no | 0.70 | no | 100 ms | -20.24 M | -1.54 | 2 |
| `ui_coin_reward` | ui | all | coins added to balance | 1.00 s | no | 0.85 | yes | 250 ms | -16.0 M | -6.46 | 2 |
| `ui_daily_reward_claim` | ui | all | daily reward collected | 2.18 s | no | 0.90 | no | 1000 ms | -16.0 M | -7.37 | 2 |
| `ui_achievement` | ui | all | achievement unlocked toast | 2.38 s | no | 0.90 | no | 1500 ms | -16.0 M | -5.29 | 2 |
| `ui_notification` | ui | all | in-app notification, your-turn reminder | 1.30 s | no | 0.70 | no | 2000 ms | -16.0 M | -5.54 | 2 |
| `ui_timer_low_tick` | ui | all | once per second in the last 10 s | 0.20 s | no | 0.60 | no | 900 ms | -18.56 M | -1.54 | 2 |
| `ui_purchase_success` | ui | all | store purchase confirmed | 1.70 s | no | 0.90 | no | 1000 ms | -16.0 M | -7.12 | 2 |
| `ui_disabled_tap` | ui | all | tap on a disabled control | 0.16 s | no | 0.60 | no | 150 ms | -18.83 M | -1.53 | 2 |
| `bg_dice_roll` | backgammon | backgammon | dice thrown | 1.38 s | no | 0.90 | no | 800 ms | -16.4 M | -1.54 | 1 (approved) |
| `bg_checker_place` | backgammon | backgammon | checker dropped on a point | 0.34 s | no | 0.80 | yes | 50 ms | -16.43 M | -1.54 | 1 (approved) |
| `bg_checker_hit` | backgammon | backgammon | opponent blot hit to the bar | 0.56 s | no | 0.90 | no | 100 ms | -18.21 M | -1.54 | 1 (approved) |
| `bg_bear_off` | backgammon | backgammon | checker borne off | 0.65 s | no | 0.85 | yes | 80 ms | -18.54 M | -1.54 | 1 (approved) |
| `bg_illegal_move` | backgammon | backgammon | checker dropped on an illegal point, snaps back | 0.55 s | no | 0.80 | no | 200 ms | -16.03 M | -1.54 | 2 |
| `bg_turn_change` | backgammon | backgammon | turn passes to the other player | 0.90 s | no | 0.70 | no | 500 ms | -16.0 M | -4.0 | 2 |
| `bg_double_offered` | backgammon | backgammon | doubling cube offered | 0.85 s | no | 0.85 | no | 500 ms | -17.49 M | -1.54 | 2 |
| `bg_double_accepted` | backgammon | backgammon | double accepted, cube turned | 1.30 s | no | 0.85 | no | 500 ms | -16.52 M | -1.54 | 2 |
| `bg_checker_pickup` | backgammon | backgammon | drag starts on a checker | 0.20 s | no | 0.60 | yes | 60 ms | -16.96 M | -1.54 | 2 |
| `bg_all_home` | backgammon | backgammon | last checker enters the home board (bear-off now possible) | 1.60 s | no | 0.80 | no | 2000 ms | -16.0 M | -5.83 | 2 |
| `bg_doubles_sparkle` | backgammon | backgammon | dice show doubles (play after dice land) | 1.10 s | no | 0.70 | no | 1000 ms | -16.0 M | -5.37 | 2 |
| `ch_piece_pickup` | chess | chess | piece selected / drag starts | 0.18 s | no | 0.60 | yes | 60 ms | -18.64 M | -1.54 | 2 |
| `ch_move` | chess | chess | piece lands on a square | 0.38 s | no | 0.85 | yes | 60 ms | -16.16 M | -1.54 | 2 |
| `ch_capture` | chess | chess | piece captures | 0.62 s | no | 0.90 | no | 100 ms | -18.57 M | -1.54 | 2 |
| `ch_check` | chess | chess | move gives check (instead of ch_move) | 0.90 s | no | 0.90 | no | 300 ms | -16.0 M | -2.75 | 2 |
| `ch_checkmate` | chess | chess | checkmate on board (then game_win/game_lose) | 2.60 s | no | 1.00 | no | 2000 ms | -16.0 M | -4.86 | 2 |
| `ch_castling` | chess | chess | castling move (king + rook) | 0.54 s | no | 0.85 | no | 300 ms | -16.04 M | -1.54 | 2 |
| `ch_promotion` | chess | chess | pawn promoted | 1.40 s | no | 0.90 | no | 500 ms | -16.0 M | -5.34 | 2 |
| `ch_draw` | chess | chess | draw or stalemate result | 1.79 s | no | 0.90 | no | 2000 ms | -16.0 M | -6.39 | 2 |
| `sd_digit_tap` | sudoku | sudoku | digit entered in a cell | 0.24 s | no | 0.70 | yes | 40 ms | -19.92 M | -1.54 | 1 (approved) |
| `sd_note_mark` | sudoku | sudoku | pencil note added/removed | 0.18 s | no | 0.60 | yes | 50 ms | -20.47 M | -1.54 | 2 |
| `sd_erase` | sudoku | sudoku | erase tool used | 0.35 s | no | 0.70 | no | 120 ms | -16.46 M | -1.54 | 2 |
| `sd_hint` | sudoku | sudoku | hint revealed | 1.10 s | no | 0.80 | no | 500 ms | -16.0 M | -6.11 | 2 |
| `sd_unit_complete` | sudoku | sudoku | row, column or box completed | 1.20 s | no | 0.75 | yes | 150 ms | -16.0 M | -5.61 | 2 |
| `sd_puzzle_complete` | sudoku | sudoku | sudoku solved (short win) | 2.20 s | no | 1.00 | no | 2000 ms | -16.0 M | -7.86 | 2 |
| `sd_mistake_limit` | sudoku | sudoku | last heart lost | 2.08 s | no | 1.00 | no | 2000 ms | -16.0 M | -8.16 | 2 |
| `mj_tile_select` | mahjong | mahjong | tile selected | 0.20 s | no | 0.75 | yes | 50 ms | -19.87 M | -1.54 | 2 |
| `mj_tile_deselect` | mahjong | mahjong | tile deselected | 0.17 s | no | 0.65 | yes | 50 ms | -19.39 M | -1.54 | 2 |
| `mj_match` | mahjong | mahjong | pair matched and removed | 0.90 s | no | 0.85 | yes | 120 ms | -16.0 M | -1.5 | 2 |
| `mj_no_match` | mahjong | mahjong | second tile does not match | 0.37 s | no | 0.75 | no | 200 ms | -19.41 M | -1.54 | 2 |
| `mj_shuffle` | mahjong | mahjong | board shuffled | 1.13 s | no | 0.80 | no | 1000 ms | -16.12 M | -1.54 | 2 |
| `mj_hint` | mahjong | mahjong | hint highlights a pair | 0.90 s | no | 0.80 | no | 500 ms | -16.0 M | -4.84 | 2 |
| `mj_board_cleared` | mahjong | mahjong | all tiles cleared (win) | 2.80 s | no | 1.00 | no | 2000 ms | -16.0 M | -7.42 | 2 |
| `mj_no_moves_left` | mahjong | mahjong | no matching pairs remain (offer shuffle) | 1.09 s | no | 0.85 | no | 2000 ms | -16.0 M | -2.85 | 2 |
| `game_win` | game | all | result screen - player won (all games) | 3.38 s | no | 1.00 | no | 3000 ms | -16.0 M | -6.72 | 1 (approved) |
| `game_lose` | game | all | result screen - player lost (all games) | 3.29 s | no | 1.00 | no | 3000 ms | -16.0 M | -9.08 | 1 (approved) |
| `music_menu_loop` | music | menus | home + menus, loop forever | 72.00 s | yes | 0.50 | no | - | -20.0 I | -5.25 | 1 (approved) |
| `music_ingame_loop` | music | all | during play (all games), loop forever | 64.00 s | yes | 0.40 | no | - | -22.0 I | -11.23 | 2 |
| `music_victory_sting` | music | all | result screen after a win (after game_win, or instead of it) | 11.50 s | no | 0.70 | no | - | -20.0 I | -4.88 | 2 |

## Integration notes
- Ducking: while `game_win`, `game_lose`, `music_victory_sting`, `ch_checkmate`, `sd_puzzle_complete`, `mj_board_cleared`, `sd_mistake_limit` or `mj_no_moves_left` play, duck music by about -8 dB (300 ms in, 800 ms out).
- `music_victory_sting` is meant to replace the in-game loop on the win screen (stop the loop with a 0.5 s fade, then play the sting once), and can follow `game_win`.
- Use `music_ingame_loop` during play and `music_menu_loop` in menus; crossfade 1.5 s between them.
- Loops: OGG is sample-exact (64.000 s / 72.000 s). AAC has encoder priming/padding (~5 ms) that some players don't trim. On iOS use AVAudioPlayer `numberOfLoops = -1` with the M4A, or the OGG via a Vorbis-capable plugin if a click is audible.
- `ui_timer_low_tick`: play once per second for the last 10 s; it is not a loop.
- Random pitch ±3 % on `bg_checker_place`, `bg_checker_pickup`, `ch_move`, `ch_piece_pickup`, `mj_tile_select`, `sd_digit_tap`, `sd_note_mark` avoids machine-gun repetition.
