# Host protocol: where the files live and how to replace them

## Wiring (do not touch)

- `likvid-ui_dev/src/App.tsx` — the "Тест UI" tab (`TEST_TAB`, `page === 'test' && <TestSection />`) is already wired.
- `likvid-ui_dev/src/sections/TestSection.tsx` — renders `<Mockup />` from `../mockups/Mockup`, a caption, and the `.test-host` host.
- `likvid-ui_dev/src/app.css` — `.test-host { width: 100% }`: the host is stretched, the mockup itself dictates width and layout.
- The host entry (`main.tsx`) imports `likvid-ui/theme.css` before project styles; your own token overrides live in a separate CSS file below that import (see `kit-rules.md`, "Kit styles wiring").

If `TestSection` ever points to a different path — fix the reference, do not move the mockup.

## Replacing the mockup down to the root

1. `rm -rf /workspace/likvid-ui_dev/src/mockups`
2. `mkdir -p /workspace/likvid-ui_dev/src/mockups`
3. Re-author exactly three files:
   - `Mockup.tsx` — root `export const Mockup = memo(() => ...)` + `Mockup.displayName = 'Mockup'`. The name and path are fixed — TestSection references them.
   - `Mockup.css` — imported from `Mockup.tsx` (`import './Mockup.css'`).
   - `icons.tsx` — inline SVG icons the mockup uses.
4. No fourth files, no neighboring "old" mockups.

## Mockup conventions

- **Class prefix** — a fresh one, after the mockup's subject (`gt-` for Google Translate, `sp-` for Spotify, etc.). The whole CSS module lives in `Mockup.css` and stays inside the prefix.
- **Mockup root** — `<div className="pfx-screen">`, carrying local `--color-main`/`--color-main-dark` for the original's palette, the mandatory mirror `--likvid-color-main: var(--color-main)` for library components, and `font-family` when needed.
- **Mockup sections** — as on the input: header, modes, panel, footer. DOM order = order on the screenshot.
- **Interactivity** — minimally alive: `useState` for the selected tab/mode, `maxLength` plus a counter for inputs, a swap button that exchanges the selection. Data stubs are file-level constants.

## Writing order

CSS → icons → Mockup. That way the component is written already knowing the finished classes, instead of being rewritten after the styling.
