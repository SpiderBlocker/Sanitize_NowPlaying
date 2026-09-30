# Sanitize NowPlaying

**Current release: v3.0.3**

Standalone Windows application that monitors playout metadata and turns it into clean, predictable RDS RadioText (RT), RT+ and separate component files for Stereo Tool, Magic RDS 4 and other file-based RDS workflows.

Sanitize NowPlaying v3.0.0 was the direct successor to **v2.1.8**, the final PowerShell/conhost release. v3 is a complete C# / .NET Framework 4.8 WinForms rewrite. **v3.0.3** keeps the established settings and six-file output contract while refining responsiveness and status presentation on top of the v3.0.2 runtime/input hardening.

Designed for small and semi-professional FM stations that want broadcast-ready RDS metadata without manually retagging an entire music library.

This project was created through iterative co-development with ChatGPT 5.2 / 5.5 / 5.6, combining AI-assisted development with hands-on design, testing and optimization.


# What's new in v3.0.3

- Reduced the special empty-title / artist-only split-write quiet period from **600 ms to 100 ms**
- A later title write is still detected normally by the hybrid input monitor and republishes the completed metadata
- Kept the existing round status-leader dots but doubled their centre-to-centre pitch to the Colour Lab-selected **200%**
- Replaced the quantized LAST UPDATE colour ramp with explicit states: **healthy below 5 minutes**, **warning from 5:00 through 14:59**, and **error from 15:00 onward**
- Increased `TextMuted` from RGB `105/105/105` to **`125/125/125`**
- Increased separator lines from RGB `82/82/82` to **`100/100/100`**
- Settings schema, input/output filenames, delimiter behaviour and RT+ target formats are unchanged from v3.0.2; no migration is required


# v3.0.0 rewrite highlights

- Standalone **WinForms application** for Windows 10 and Windows 11
- No PowerShell, console host or Windows Terminal required at runtime
- First-run Working Directory setup with integrated folder and volume picker
- Configurable input filename; `nowplaying.txt` remains the default
- Low-overhead hybrid input monitoring with immediate wake-up on metadata changes
- Automatic handling of temporarily unavailable mapped drives and UNC Working Directories
- Clear `EMPTY`, `MISSING`, `EXPIRED`, `OFFLINE` / unavailable and `INVALID DATA` states
- Transactional in-app **F10 Settings** UI with draggable nested overlays
- Atomic settings saves and verified single-worker runtime handover
- Per-file atomic output writes with retry protection
- Startup freshness checks and confirmed output clearing on shutdown to prevent stale RDS data
- Single-instance protection
- In-app About panel and polished graphical status presentation


# Metadata processing

- Intelligent artist/title cleanup for encoders, bitrates, country/year suffixes, platform tags, duplicate information and other common library noise
- Conservative artist/title splitting
- Balanced bracket handling for `()`, `[]` and `{}`, including cleanup of unmatched or mismatched brackets
- Configurable artist/title order for compact RT/RT+ presentation
- Multilingual or custom prefix text
- Configurable connector text
- Configurable playout delimiter, including U+241F, TAB and custom values
- Optional Greek/Cyrillic transliteration and ASCII-safe mode
- Independent 64-character artist/title component limits
- Adaptive trimming of combined RT/RT+ content to the RDS 64-character limit
- Short 100 ms partial-write guard for empty-title / artist-only input, with later file changes reprocessed normally


# Output files

All output files are written as UTF-8 without BOM in the selected Working Directory and are replaced atomically.

| File | Contents |
| --- | --- |
| `prefix.txt` | Selected multilingual prefix or custom prefix text, or empty |
| `artist.txt` | Sanitized artist, independently limited to 64 characters, or empty |
| `connector.txt` | Configured connector when both artist and title are present, or empty |
| `title.txt` | Sanitized title, independently limited to 64 characters, or empty |
| `nowplaying_rt.txt` | Compact combined RadioText in the configured artist/title order, or empty |
| `nowplaying_rtplus.txt` | Compact RT+ output in the selected target syntax, or empty |

For `nowplaying_rtplus.txt`, **Stereo Tool** is the backward-compatible default. **Magic RDS 4** can be selected as an alternative RT+ output target.

The component files keep fixed semantic identities: `artist.txt` always contains the artist, `connector.txt` the connector and `title.txt` the title. The **Artist/title order** setting changes presentation order, not file identity.


# Requirements

- Windows 10 or Windows 11
- .NET Framework 4.8

The application is built as AnyCPU.

`Sanitize-NowPlaying.settings.json` is stored next to `SanitizeNowPlaying.exe`, so run the application from a directory in which the current user has write permission.


# Quick start

1. Download the current `Sanitize-NowPlaying.zip`.
2. Extract it to a writable folder.
3. Run `SanitizeNowPlaying.exe`.
4. On first start, select the Working Directory used by your playout software.
5. Configure the playout application to write artist/title metadata to the selected input file. The default filename is `nowplaying.txt`.
6. Open **F10 Settings** when you want to change the Working Directory, input filename, prefix, artist/title order, connector, ASCII/transliteration behaviour, delimiter or RT+ output target.
7. Configure Stereo Tool, Magic RDS 4 or another RDS application to read the ready-made RT/RT+ file or the separate component files required by your workflow.

When using the recommended U+241F separator, a typical playout metadata format is:

    %artist␟%title


# Stereo Tool component example

The separate component files can be combined as an alternating RadioText sequence in Stereo Tool. For example:

    5s:\r"C:\RDS\prefix.txt"/10s:\+AR\r"C:\RDS\artist.txt"\-/5s:\r"C:\RDS\connector.txt"/10s:\+TI\r"C:\RDS\title.txt"\-

This displays the prefix for 5 seconds, the artist for 10 seconds with an RT+ artist tag, the connector for 5 seconds, and the title for 10 seconds with an RT+ title tag.

**Synchronization note:** Stereo Tool reads each referenced component file when that section becomes active rather than taking one shared snapshot of all component files. If metadata changes mid-sequence, components from adjacent songs can therefore be mixed. Use the ready-made `nowplaying_rt.txt` / `nowplaying_rtplus.txt` output when guaranteed artist/title consistency is more important than separate component rotation.


# Release lineage

**v3.0.0** established the C# / WinForms generation as a continuation of the existing project rather than a separate product. **v3.0.1** added conservative metadata-preservation hardening. **v3.0.2** strengthened malformed-input handling, decoding, RT+ escaping, worker recovery and diagnostics. **v3.0.3** retains the same settings schema, output filenames and file-interface contract while reducing the artist-only quiet period and refining status/contrast presentation. PowerShell/conhost **v2.1.8** remains the compatibility baseline for unaffected processing cases.

For users of v2.1.8, the important continuity points are:

- existing settings schema retained;
- existing output filenames retained;
- unaffected baseline processing cases remain parity-tested against v2.1.8;
- `nowplaying.txt` remains the default input filename;
- Stereo Tool remains the default RT+ output target.

Sanitize NowPlaying v3 is distributed as prebuilt Windows software. The v3 source code is not publicly distributed.


# License

Sanitize NowPlaying **v3.0.0 and later** is distributed under the proprietary Sanitize NowPlaying license included with the release package.

The v3 executable may be used for private, commercial, broadcast, educational and organizational purposes under those terms. The complete, unmodified official binary package may also be redistributed subject to the license conditions.

**Legacy note:** Sanitize NowPlaying **v2.x** was previously released under GNU GPLv3. Those historical v2.x releases remain under GPLv3; the v3 license does not revoke or alter rights already granted for them.

The v3 source code is not publicly distributed.


# Verification

For v3.0.3, the release verification chain includes:

- 73 processing-engine self-tests, including v3.0.1 metadata-preservation and v3.0.2 malformed-input / RT+ hardening regressions;
- 35 unaffected baseline cases × 6 output fields against the v2.1.8 processing reference;
- 34 runtime/settings/file-I/O tests;
- 9 application-state regression tests;
- 26 settings/overlay regression tests;
- 25 picker/network/burn-in tests;
- 13 accelerated stress/soak tests;
- final v3.0.3 production application compile.


# Disclaimer

Stereo Tool is a product of Thimeo Audio Technology B.V. Magic RDS 4 is a product of Pira.cz. This project is not affiliated with or endorsed by either vendor.
