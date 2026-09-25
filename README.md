# ICS Merger

A single-file, offline web app that merges multiple `.ics` calendar files into one `merged.ics` you can import into Google Calendar.

## Usage

Open `index.html` in your browser (double-click it — no server, no install, no dependencies).

1. Drag and drop `.ics` files onto the drop area, or click it to browse.
2. Click **Start merge** — a progress bar shows reading/merging progress.
3. Click **Download merged.ics** when the merge completes.
4. **Clear files** empties the list to start over.

## What the merge does

- Unfolds and re-folds lines per RFC 5545 (75-octet limit, CRLF endings).
- Keeps `VEVENT`, `VTODO`, `VJOURNAL` and `VFREEBUSY` components, including nested parts such as `VALARM`.
- De-duplicates `VTIMEZONE` blocks by `TZID`.
- Removes identical entries that share a `UID` (and `RECURRENCE-ID`).
- Renames the `UID` when two entries share a `UID` but differ, so Google Calendar imports both instead of overwriting one.
- Fixes recurring events whose `RRULE:UNTIL` is missing the RFC 5545-required UTC `Z` suffix (a common Outlook/Exchange export defect) by converting it to UTC using the file's own `VTIMEZONE` data. Without this, importers can silently stop generating instances at that date.
- Wraps everything in a fresh `VCALENDAR` with `VERSION:2.0` and `METHOD:PUBLISH`.

Everything runs locally in the browser; no file ever leaves your machine.
