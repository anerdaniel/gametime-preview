# Sound - licences

## Audio content
- **All 56 sounds (stage 1: 12 approved, stage 2: 44 new) are original procedural syntheses** created for this project by `src/dsp.py`, `src/sfx.py`, `src/music.py` (stage 1) and `src/sfx2.py`, `src/music2.py` (stage 2), and owned by the project. This covers everything in `samples/` and `final/`. The methods are modal synthesis, filtered noise, additive piano and pad tones, a Karplus-Strong bass, and a synthetic room impulse response.
- **No recordings or third-party samples are used:** no Kenney, no Freesound, no CC-BY, no CC-BY-NC, no GPL material. No attribution is required.
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
