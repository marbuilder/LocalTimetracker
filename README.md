# LocalTimetracker

Track your work with this local HTML/JavaScript file. It was built with AI.

## Runtime

- Open `index.html` directly in a browser.
- The app remains a single standalone HTML artifact with embedded vanilla JavaScript and CSS.
- Persistence uses browser `localStorage` only.

## Persistence regression check

Open `tests/s01-persistence-regression.html` in a browser to verify the storage normalization contract for malformed state, active timer restoration, and remembered ticket suggestions.

## Timer regression check

Open `tests/s02-current-day-backdated-timer.html` through a local web server to verify the current-day default and backdated timer-start contract.

## Notes

- Development-only regression pages are allowed under `tests/`.
- The shipped app artifact stays `index.html`.

---