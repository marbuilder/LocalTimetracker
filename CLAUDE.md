# CLAUDE.md — Project Scope

## Project
**Lokaler Zeit-Tracker** — a fully client-side time tracker. The user-facing UI is in German.

## Hard Constraints

- **Single artifact**: the entire app lives in `index.html` (HTML, CSS, and JS in the same file). No additional source folders, no module splits.
- **No external dependencies**: no npm/CDN packages, no web fonts, no trackers, no external API calls. The CSP (`default-src 'self'`) is intentionally strict and must not be weakened.
- **No build step**: the file is opened directly in the browser (or via `tests/*.html` for regression checks). No bundler, no transpiler.
- **Persistence via `localStorage` only** (keys `local-time-tracker-v1` and `local-time-tracker-v1-ticket-suggestions`). No IndexedDB, no server.
- **Language**: all user-facing strings, labels, buttons, hints, and alerts must remain in German. Code, comments, and documentation (including this file) are in English.

## Data Model

Each stored entry in `state.entries`:

```js
{
  id: string,             // cryptoRandomId
  ticket: string,         // sanitized, max 60 characters
  notes: string,          // sanitized, max 2000 characters
  startTs: number,        // ms epoch
  endTs: number,          // ms epoch, > startTs
  pauseMinutes: number,   // 0..1440
  createdAt: number,      // ms epoch
  source: 'timer' | 'manual'
}
```

A running timer (`state.activeTimer`):

```js
{ id, ticket, notes, startTs }
```

## Security

- **Never** inject user data as HTML — only via `textContent` or DOM APIs.
- Normalize input through the existing utilities:
  - `sanitizeText(value, maxLen)` — strip control characters, trim, truncate.
  - `clampNumber(value, min, max)` — clamp to range and round.
  - `localDateTimeToTs(date, time)` — `YYYY-MM-DD` + `HH:MM` → timestamp.
  - `formatDateInput(date)`, `formatTimeInput(ts)` — formatted input values.
- No `innerHTML` assignments with user strings.

## Tests

Standalone HTML pages under `tests/` (e.g. `s01-persistence-regression.html`, `s02-current-day-backdated-timer.html`). Open in the browser and click through manually. No test runner.

## Working Approach

- Minimal, focused diffs. Consistently reuse existing patterns (`row` / `field s*`, `panel`, CSS variables `--bg`, `--panel`, `--accent`, …).
- Before changing code, read the relevant section of `index.html` — the file is large but searchable.
- For UI changes, check the breakpoints (`1050px`, `700px`).
