# Sound - licences

## Audio content
- **All 56 sounds (stage 1: 12 approved, stage 2: 44 new) are original procedural syntheses** created for this project by `src/dsp.py`, `src/sfx.py`, `src/music.py` (stage 1) and `src/sfx2.py`, `src/music2.py` (stage 2), and owned by the project. This covers everything in `samples/` and `final/`. The methods are modal synthesis, filtered noise, additive piano and pad tones, a Karplus-Strong bass, and a synthetic room impulse response.
- **No recordings or third-party samples are used** (except the 8 `durak_*` Kenney CC0 card sounds listed at the end of this file): no other Kenney, no Freesound, no CC-BY, no CC-BY-NC, no GPL material. No attribution is required.
- **The music is original composition:** `music_menu_loop` is an original melody over a common jazz ii-V-I progression; `music_ingame_loop` (60 BPM ambient pad/piano/bass loop) and `music_victory_sting` (Dmaj9 – G/A – Dmaj9 cadence) are original too. Chord progressions are not copyrightable.
- **CC0 recordings: none used.** The stage-2 brief allowed verified CC0 recordings where synthesis wasn't convincing; it wasn't needed, so there are no third-party URLs, authors or licences to record.

## Build tools (used at build time only; nothing is linked into or shipped with the app)
| Tool | Version | Licence | Use |
|---|---|---|---|
| Python | 3.13 | PSF | scripting |
| NumPy | 2.5.5 | BSD-3-Clause | synthesis maths |
| SciPy | 1.18.1 | BSD-3-Clause | filters, convolution, resampling, WAV I/O |
| pyloudnorm | 0.2.0 | MIT | BS.1770 K-weighting / integrated loudness |
| matplotlib | 3.11.2 | PSF-based (matplotlib licence, permissive) | QA plots only |
| FFmpeg | 7.1.5 (Debian build, configured `--enable-gpl` because it includes libx264) | LGPL/GPL | encoding to OGG Vorbis (libvorbis, BSD), AAC (FFmpeg native, LGPL), H.264 (libx264, GPL) for the preview video; subtitle burn-in (libass ISC, fribidi LGPL) |

- **FFmpeg licensing:** FFmpeg is only a converter here. Its GPL/LGPL terms cover the program, not the media files it outputs, and no FFmpeg code is distributed. The OGG/M4A outputs carry no codec licence obligations for the app.
- **Preview subtitle font:** IBM Plex Sans Hebrew (SIL OFL 1.1), from the system font directory. It is used only in `samples/samples_preview.mp4` and `final/full_preview.mp4`.

## Durak card sounds (added 24.11.2026) - third-party CC0
The 8 `durak_*` events are **not** synthesised: they are trimmed, mono, peak-normalised (-3 dBFS) edits of recordings from **Casino Audio (1.1) by Kenney Vleugels (www.kenney.nl)**, released under **Creative Commons Zero (CC0 1.0 Universal, public domain)**: http://creativecommons.org/publicdomain/zero/1.0/

Bundled `License.txt` (verbatim excerpt): "License (Creative Commons Zero, CC0) ... You may use these assets in personal and commercial projects. Credit (Kenney or www.kenney.nl) would be nice but is not mandatory." Source page: https://kenney.nl/assets/casino-audio (checked 24.11.2026, "License: Creative Commons CC0"). Download zip SHA-256: `f36250766ac5bc378c13708ddf12a23a8e54a3251f8d482c7536e51b5dbafa18`.

Attribution is not required; we credit Kenney anyway.

| Event | Kenney source file | Edit |
|---|---|---|
| `durak_card_deal` | `Audio/card-slide-1.ogg` | first 0.40 s, 80 ms fade-out |
| `durak_card_select` | `Audio/card-slide-4.ogg` | first 0.20 s |
| `durak_card_play` | `Audio/card-place-1.ogg` | first 0.45 s |
| `durak_card_beat` | `Audio/card-place-2.ogg` | first 0.40 s |
| `durak_take` | `Audio/card-shove-2.ogg` | first 0.60 s |
| `durak_bito` | `Audio/card-fan-1.ogg` | first 0.65 s |
| `durak_trump_reveal` | `Audio/card-shove-1.ogg` | first 0.65 s |
| `durak_turn_cue` | `Audio/cards-pack-take-out-1.ogg` | first 0.45 s |

Pipeline (FFmpeg): leading-silence trim (-55 dB), mono 44.1 kHz, fade-out 80 ms, peak-normalise to -3 dBFS, OGG Vorbis q6 + AAC-LC 128 kb/s (faststart).
