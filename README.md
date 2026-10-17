<p align="center">
  <img src="assets/SanitizeNowPlaying-logo.png" alt="Sanitize NowPlaying logo" width="144" height="144">
</p>

<h1 align="center">Sanitize NowPlaying</h1>

<p align="center"><strong>Current release: v3.1.2</strong> · Windows 10/11 · .NET Framework 4.8</p>

Standalone Windows application that monitors playout metadata and turns it into clean, predictable RDS RadioText (RT), RT+ and separate component files for Stereo Tool, Magic RDS 4 and other file-based RDS workflows.

Sanitize NowPlaying v3.0.0 was the direct successor to **v2.1.8**, the final PowerShell/conhost release. v3 is a complete C# / .NET Framework 4.8 WinForms rewrite. **v3.1.2** adds a saved F9 bypass switch, clearer operating-status feedback and refined footer controls, building on the configurable processing and shared menu experience of v3.1.0/v3.1.1. Bypass defaults to Off, so normal processing remains unchanged. Existing settings load without migration; the six output filenames and RT+ target formats are retained.

Designed for small and semi-professional FM stations that want broadcast-ready RDS metadata without manually retagging an entire music library.

Developed and reviewed with AI assistance, alongside hands-on design, testing and refinement.


## What's new in v3.1.2

- **F9 Bypass:** temporarily preserve source artist/title text while retaining output order, RT+ formatting and length limits; processing preferences are remembered
- **Clear bypass status:** fixed-width On/Off button, orange Bypass On text with a subtle active fill, a hover-only frame and a Bypass On indication in the title bar; F9 retains its neutral shortcut colour
- **Refined footer controls:** clearer spacing and a subtle divider between quick settings and commands, with consistent Tab focus and direct F9 switching that does not move keyboard focus
- **Gradual last-update colours:** green through five minutes, orange at ten minutes and red at fifteen minutes
- **Consistent wording:** Initial caps replaces Capitalize words throughout the footer and menus; ASCII-safe keeps its full name
- **More robust settings:** settings are flushed before atomic replacement, and unknown JSON fields survive saves
- **Safer startup and release packaging:** rejected additional demo starts leave input/settings untouched; release packaging adds a SHA256 checksum and a compact changelog

Settings from v3.1.1 remain compatible. Bypass defaults to Off, all twelve rules to On, text case to Original and output to the input folder. The six output filenames, delimiter options and RT+ target formats are retained.


## Capabilities introduced in v3.1.0

- Twelve configurable sanitizer rules and Original, All caps or Initial caps text case
- An independent output folder, or output that follows the input folder
- Five quick menus: Rules, Chars, Translit, Case and Order, with direct saving and keyboard navigation
- Improved multilingual handling, output recovery, settings diagnostics and folder/file status reporting


## v3.0.0 rewrite highlights

- Standalone **WinForms application** for Windows 10 and Windows 11
- No PowerShell, console host or Windows Terminal required at runtime
- First-run input-folder setup with integrated folder and volume picker
- Configurable input filename; `nowplaying.txt` remains the default
- Low-overhead hybrid input monitoring with immediate wake-up on metadata changes
- Automatic handling of temporarily unavailable mapped drives and UNC input/output folders
- Clear `EMPTY`, `MISSING`, `EXPIRED` and `INVALID DATA` input states, `UNAVAILABLE` folder status and per-file `WRITE FAILED` reporting
- Transactional in-app **F10 Settings** UI with draggable nested overlays
- Atomic settings saves and verified single-worker runtime handover
- Per-file atomic output writes with retry protection
- Startup freshness checks and confirmed output clearing on shutdown to prevent stale RDS data
- Single-instance protection within a Windows logon session
- In-app About panel and polished graphical status presentation


## Metadata processing

The following applies to normal processing (Bypass Off, the default). Initial caps gives each word an initial capital, for example `my song` → `My Song`.

- Intelligent artist/title cleanup for encoders, bitrates, country/year suffixes, platform tags, duplicate information and other common library noise, with twelve independently selectable rules
- Preservation-first handling for ambiguous country codes, natural hyphenation and technical/version tails
- Conservative artist/title splitting
- Balanced bracket handling for `()`, `[]` and `{}`, including cleanup of unmatched or mismatched brackets
- Configurable artist/title order for compact RT/RT+ presentation
- Multilingual or custom prefix text
- Configurable connector text
- Configurable playout delimiter, including U+241F, TAB and custom values
- Optional Greek/Cyrillic transliteration; ASCII-safe mode requires transliteration
- Original, All caps or Initial caps text case
- NFC normalization and improved handling of Vietnamese, combining accents and additional Unicode letters
- Independent 64-character artist/title component limits
- Adaptive trimming of combined RT/RT+ content after conversion, within the existing 64-character budget and without splitting supported letter/accent sequences
- Short 100 ms partial-write guard for empty-title / artist-only input, with later file changes reprocessed normally


## Output files

All output files are written as UTF-8 without BOM in the selected **output folder** and are replaced atomically, one file at a time. By default, output follows the input folder. A separate destination can be selected under **F10 Settings > Output settings > Output folder**. The six files are not updated as one atomic set.

| File | Contents |
| --- | --- |
| `prefix.txt` | Selected multilingual prefix or custom prefix text, or empty |
| `artist.txt` | Artist text, sanitized normally or preserved in bypass, independently limited to 64 characters, or empty |
| `connector.txt` | Configured connector when both artist and title are present, or empty |
| `title.txt` | Title text, sanitized normally or preserved in bypass, independently limited to 64 characters, or empty |
| `nowplaying_rt.txt` | Compact combined RadioText in the configured artist/title order, or empty |
| `nowplaying_rtplus.txt` | Compact RT+ output in the selected target syntax, or empty |

For `nowplaying_rtplus.txt`, **Stereo Tool** is the backward-compatible default. **Magic RDS 4** can be selected as an alternative RT+ output target.

The component files keep fixed semantic identities: `artist.txt` always contains the artist, `connector.txt` the connector and `title.txt` the title. The **Artist/title order** setting changes presentation order, not file identity.


## Requirements

- Windows 10 or Windows 11
- .NET Framework 4.8

The application is built as AnyCPU. The executable is not digitally signed; Windows SmartScreen or organisational policy may require approval before it can run. A release ZIP's SHA256 checksum checks integrity and is not a digital signature.

`Sanitize-NowPlaying.settings.json` is stored next to `SanitizeNowPlaying.exe`, so run the application from a directory in which the current user has write permission.


## Quick start

1. Download the current [SanitizeNowPlaying.zip](https://github.com/SpiderBlocker/Sanitize_NowPlaying/raw/main/SanitizeNowPlaying.zip).
2. Extract it to a writable folder.
3. Run `SanitizeNowPlaying.exe`.
4. On first start, select the input folder used by your playout software. Output follows this folder by default.
5. Configure the playout application to write artist/title metadata to the selected input file. The default filename is `nowplaying.txt`.
6. Open **F10 Settings** to configure the input/output folders, input filename, sanitizer rules, text case, prefix, artist/title order, connector, character set, transliteration, delimiter or RT+ output target. **Save & exit** applies the draft. Closing unchanged settings returns directly; with pending edits, choose **Save & exit**, **Discard changes** or **Cancel**.
7. Configure Stereo Tool, Magic RDS 4 or another RDS application to read the ready-made RT/RT+ file or the separate component files required by your workflow.

The **Rules, Chars, Translit, Case and Order** fields share the bottom row with **F9 Bypass**, **F10 Settings** and **Alt+F4 Exit**. The fields offer quick access while normal processing is active. Rules stays open for successive immediately saved changes; single-choice menus close after a successful selection. Tab/Shift+Tab moves footer focus, Enter/Space opens a menu, Up/Down moves choices and Left/Right switches between quick menus. Esc dismisses the popup without undoing successfully saved quick changes. In ASCII-safe mode, Translit is required: its quick menu is unavailable, keyboard navigation skips it and the tooltip explains the forced setting.

Folder selection requires **Use this folder**. In both Input file pickers, clicking a filename only selects it; **Use this file**, U or Enter confirms it. **Cancel** discards the current unconfirmed choice.

| Header help | Action |
| --- | --- |
| `Esc: close` | Close the Settings root; offer the save/discard/cancel decision if the draft changed |
| `Esc: back` | Return one menu level, preserving earlier F10 draft choices |
| `Esc: cancel` | Perform the same cancellation as the dialog's Cancel button |

Initial setup follows the same picker/editor conventions. Cancel in its root aborts unfinished setup without saving settings.

When using the recommended U+241F separator, a typical playout metadata format is:

    %artist␟%title


## Bypass and operating status

Press **F9**, click the Bypass button, or use Tab followed by Enter/Space to switch bypass. **On/Off describes the current mode**. Bypass is saved; a saved On setting remains On after restart. A failed save leaves the active mode unchanged.

While bypass is On, **Rules, Chars, Translit and Case** are suspended and their controls are disabled. Their preferences are retained; turning bypass Off immediately reprocesses the current input with those choices. Artist/title order and RT+ target formatting remain active. Source text is preserved within the existing length and target-syntax limits, with simple left-to-right clipping and an ellipsis when needed. Bypass does not replay expired or unavailable input. Check that the encoder supports any preserved Unicode characters.

The fixed-width button shows **Bypass On** in orange on a subtle neutral fill. F9 retains its normal shortcut colour; the button frame appears only on hover. Tab focus has its own separate dotted outline. The title bar adds **— Bypass On**, including while the F10 menu is open. F9 works on the main screen and in quick menus; it does not toggle bypass behind modal settings or confirmation dialogs. Holding F9 toggles only once per press.

The last-update timestamp and elapsed-time indicator remain green through **5:00**, gradually reach orange at **10:00** and red at **15:00**, and return to green after a new good output update. Input expiration and file/folder health retain their separate status reporting.


## Stereo Tool component example

The separate component files can be combined as an alternating RadioText sequence in Stereo Tool. For example:

    5s:\r"C:\RDS\prefix.txt"/10s:\+AR\r"C:\RDS\artist.txt"\-/5s:\r"C:\RDS\connector.txt"/10s:\+TI\r"C:\RDS\title.txt"\-

This displays the prefix for 5 seconds, the artist for 10 seconds with an RT+ artist tag, the connector for 5 seconds, and the title for 10 seconds with an RT+ title tag.

**Synchronization note:** Stereo Tool reads each referenced component file when that section becomes active rather than taking one shared snapshot of all component files. If metadata changes mid-sequence, components from adjacent songs can therefore be mixed. Each ready-made `nowplaying_rt.txt` or `nowplaying_rtplus.txt` file contains one complete combined payload and avoids that component-rotation issue. The two files themselves are still replaced independently.


## Folder status and recovery

The input and output folders are monitored independently. Missing or inaccessible folders show **UNAVAILABLE**; the output availability check can briefly show **CHECKING**. A reachable folder remains available when file writes fail, while affected output files show **WRITE FAILED**. The latest metadata is retried after recovery.

An unwritable shared input/output folder is refused during setup or folder selection. Read-only input can be used with a separate output destination. A deleted or renamed configured folder is not automatically recreated during monitoring, retries or shutdown; restore its original path or select another folder explicitly.

Short messages remain on one line. Longer messages wrap; unusually long diagnostics appear in an owned OK notice with scrolling and copying when needed. The main window retains its size.

The application log is available at `%LOCALAPPDATA%\Sanitize NowPlaying\SanitizeNowPlaying.log`; the previous rotated log is `SanitizeNowPlaying.log.old`.


## Release lineage

**v3.0.0** established the C# / WinForms generation as a continuation of the existing project rather than a separate product. **v3.0.1** added conservative metadata-preservation hardening. **v3.0.2** strengthened malformed-input handling, decoding, RT+ escaping, worker recovery and diagnostics. **v3.0.3** reduced the artist-only quiet period and refined status/contrast presentation. **v3.0.4** adds focused sanitizer correctness, Unicode/entity consistency and regex-safety hardening while retaining the same settings schema, output filenames and file-interface contract. **v3.1.0** added configurable sanitizer rules, text case, separate output destinations and quick access. **v3.1.1** refined rule interactions, length handling, explanatory help and consistent dialog/navigation behavior. **v3.1.2** adds bypass control, clearer status feedback, footer refinements and more robust settings persistence while retaining the established file-interface contract. PowerShell/conhost **v2.1.8** remains the compatibility baseline for unaffected processing cases.

For users of v2.1.8, the important continuity points are:

- existing valid settings load without migration; v3.1.0 added optional fields with compatible defaults, retained by v3.1.1/v3.1.2;
- existing output filenames retained;
- unaffected baseline processing cases remain parity-tested against v2.1.8;
- `nowplaying.txt` remains the default input filename;
- Stereo Tool remains the default RT+ output target.

**Downgrade note:** v3.0.4 ignores the new fields and uses the input folder for output again. Saving settings with the older version can discard the new fields; retain a settings backup before downgrading.

Sanitize NowPlaying v3 is distributed as prebuilt Windows software. The v3 source code is not publicly distributed.


## License

Sanitize NowPlaying **v3.0.0 and later, including v3.1.2,** is distributed under the proprietary [Sanitize NowPlaying license](LICENSE). Include the license with the official binary package.

The v3 executable may be used for private, commercial, broadcast, educational and organizational purposes under those terms. The complete, unmodified official binary package may also be redistributed subject to the license conditions.

**Legacy note:** Sanitize NowPlaying **v2.x** was previously released under GNU GPLv3. Those historical v2.x releases remain under GPLv3; the v3 license does not revoke or alter rights already granted for them.

The v3 source code is not publicly distributed.


## Verification

A complete native Windows v3.1.2 verification run passed on **16 October 2026**:

| Suite | Passed | Failed |
| --- | ---: | ---: |
| Processing engine | 2,831 | 0 |
| PowerShell v2.1.8 baseline parity, six fields per case | 34 | 0 |
| Runtime/settings/file I/O | 62 | 0 |
| Application state | 36 | 0 |
| Settings/overlay | 38 | 0 |
| Picker/network/burn-in | 28 | 0 |
| Accelerated stress/soak | 13 | 0 |
| **Total counted cases** | **3,042** | **0** |

The production build, full native UI probe and startup/cleanup probes also passed. The QUICK stress run completed **14,921 simulated operations**. UI coverage includes all 144 footer setting combinations, F9/Tab focus behavior, native tooltips, dialogs and measured startup width.

The final bypass appearance and title-bar refinements passed local build and targeted UI checks, including saved On/Off startup, failed-save handling and unchanged F10/Save & exit rendering. **A fresh native Windows UI/startup check remains outstanding for those final refinements.** The counted native results above precede that presentation-only change.

Display scaling, actual network shares and the encoder's character repertoire remain deployment-specific checks. Single-instance protection is per Windows logon session; use one writer per output folder. Output files are individually atomic, not atomic as a six-file set.


## Disclaimer

Stereo Tool is a product of Thimeo Audio Technology B.V. Magic RDS 4 is a product of Pira.cz. This project is not affiliated with or endorsed by either vendor.
