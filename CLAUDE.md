# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is a content repository, not a software project — there is no code to build, lint, or test. It contains `.srt` subtitle files for the three extended-edition *Lord of the Rings* films, distinguished from ordinary subtitles by including full transcriptions and English translations of every foreign-language dialogue (Sindarin, Quenya, Adûnaic, Rohirric, Black Speech) and selected song lyrics. See README.md for the full project description, sourcing, and status per movie.

## Repository structure

```
1_Fellowship-of-the-Ring/
2_The-Two-Towers/
3_Return-of-the-King/
```

Each movie directory contains one subdirectory per supported release timing, named `release-<Variant>-<runtime HHMMSS>` (the runtime suffix is the Blu-ray extended-edition runtime that subtitle timing is anchored to):

- `release-ShortPause-<runtime>` — single-file Blu-ray rip, short pause between disk 1 and disk 2 content.
- `release-LongPause-<runtime>` — single-file Blu-ray rip, long pause between disk 1 and disk 2 content.
- `release-TwoDVDs` — two-file set (`Full.Disk1...` / `Full.Disk2...`) matching the original DVD extended edition split.
- `release-TwoBlueRays` — two-file set (`Full.Disk1...` / `Full.Disk2...`) matching the 2021 remastered Blu-ray box set's two-disc split.

Inside each release directory:

- `Full.<movie title/release info>.ZoowlCZ.<Variant>.srt` — full subtitles (English dialogue + foreign transcription/translation + included lyrics).
- `compatible-releases.txt` — the runtime this timing targets and a list of known video releases it lines up with.
- `partial-subtitles/` — derived, narrower variants generated from the full file:
  - `ForeignDialogsAndLyrics...srt` — foreign dialogue and lyrics only.
  - `ForeignDialogs...srt` — foreign dialogue only, no lyrics.
  - `NoLyrics...srt` — everything except song lyrics.

`resources/` holds reference source material (PDFs used for transcription/translation) and is tracked in git; do not expect it to be complete.

## Subtitle content conventions

When editing `.srt` files, preserve the existing conventions found throughout the files:

- Foreign-language lines are prefixed with a bracketed language tag, e.g. `[Sindarin]`, `[Quenya]`, `[Rohirric]`, `[Black Speech]`, `[Adûnaic]`, immediately followed by the transcribed line in `<i>...</i>` italics.
- The English translation of a foreign line is normally given as a separate subsequent subtitle entry (own timestamp block), not inline — follow the pattern of nearby existing entries.
- Song lyrics use `♫` markers around the line, e.g. `♫ <i>Man ammen toltha i dann hen morn?</i> ♫`, and pair a `[Language]` line with a following `[English]` translation line in the same style when both are shown together (see entry 5 in the FotR ShortPause file for an example of simultaneous foreign+English lyric lines using `{\an8}` positioning).
- `{\an8}` is used to force top-of-screen positioning for lines that would otherwise overlap another subtitle shown at the same time (e.g., simultaneous lyric + translation, or lyric + dialogue).
- Files use the `ⓘ` symbol for informational/credit notes (typically only in the opening entries).
- Files rely on ASS-style override tags (`<i>`, `{\an8}`) inside standard `.srt` — call this out if asked, since not all subtitle players support these tags.

## Multi-file consistency

A wording, timing, or translation fix to a dialogue/lyric line generally needs to be propagated across:
1. The `Full...` file of the release it was found in.
2. The corresponding `Full...` files for the other release variants (ShortPause/LongPause/TwoDVDs/TwoBlueRays) of the *same movie*, accounting for each variant's own timestamp offsets.
3. The relevant `partial-subtitles/` derived files for all affected release variants (a foreign-dialogue fix touches `ForeignDialogs`, `ForeignDialogsAndLyrics`, and `NoLyrics`; a lyrics-only fix touches `ForeignDialogsAndLyrics` only, not `ForeignDialogs` or `NoLyrics`).

Check `compatible-releases.txt` in a release directory before adjusting timestamps, to keep changes consistent with the runtime/releases that variant targets.
