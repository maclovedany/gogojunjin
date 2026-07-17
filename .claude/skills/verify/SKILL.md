---
name: verify
description: How to build/launch/drive this project (고고전진 놀이판) to verify changes end-to-end.
---

# Verifying 고고전진 놀이판

Static single-file page — no build step. The surface is the browser at
`file:///Users/danymac/Desktop/test/gogojunjin/index.html`.

## Drive it with Playwright

Install Playwright in the session scratchpad (not the repo). The npm-latest
Playwright may expect a newer browser revision than what's cached in
`~/Library/Caches/ms-playwright/`. If `chromium.launch()` fails with
"Executable doesn't exist", point at the cached headless shell explicitly:

```js
const browser = await chromium.launch({
  executablePath: process.env.HOME +
    '/Library/Caches/ms-playwright/chromium_headless_shell-1223/chrome-headless-shell-mac-arm64/chrome-headless-shell',
});
```

(Adjust the revision number to whatever `ls ~/Library/Caches/ms-playwright` shows.)

## Flows worth driving

- Click cells in `#grid-a` / `#grid-b` → stamp appears, `#count-a`/`#count-b` increments.
- 모두 지우기 (`[data-clear="a"|"b"]`) resets stamps and count.
- Row/col dropdowns (`#rows-select`, `#cols-select`) rebuild both grids;
  cell count = rows×cols, `grid-template-columns` = cols+1 tracks (row-number column).
- Sound toggle `#sound-toggle` flips label/icon (Web Audio — can't hear it headless;
  just check no JS errors).
- Mobile: viewport 390×844, assert `scrollWidth <= clientWidth` (no horizontal overflow).

Capture screenshots at desktop (1500×950) and mobile widths; listen for
`pageerror`/console errors throughout.
