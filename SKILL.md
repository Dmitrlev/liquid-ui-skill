---
name: liquid-ui-skill
description: "Use when asked to build «ликвид глаз» (liquid glass): a mockup, component or page in this style from the likvid-ui component kit."
---

# liquid-ui-skill — mockups by the rules

The skill's job is UI design: input is a screenshot or a description, output is a design mockup built by the rules and fully replacing the previous one. The rules themselves (kit, host, layout) live in `references/`; here is only the workflow and the boundaries.

## Workflow

1. **Intake.** Parse the input: screenshot/description. List the blocks top to bottom (header, navigation, content, footer), the style, the accent color, the interactivity. If the screenshot leaves anything ambiguous — decide in favor of a "live" prototype: tabs and buttons switch, counters work.
2. **Read `references/protocol.md`** — where the files live, how replace-to-the-root works, how not to break the wiring.
3. **Read `references/kit-rules.md`** — kit rules and code style for the mockup.
4. **Read `references/layout.md`, `references/sizes.md` and `references/anatomy.md`** — layout, metrics and patterns: what is liquid, what is regular.
5. **Build the mockup.** Write `Mockup.css` first (name all styles with a fresh prefix), then `icons.tsx`, then `Mockup.tsx`. Write one file at a time, never rewriting a finished one.

## Iron rules

1. One mockup at a time: before building, the old contents of `src/mockups/` are deleted, no neighboring new file appears.
2. Components — only from `likvid-ui` (in dev it resolves to the lib's `src`, reinstalling is not needed).
3. The mockup's accent is a local `--color-main` override at the mockup root, not an edit of the playground's global tokens. Right next to it in the root there is always the mirror `--likvid-color-main: var(--color-main)`: the lib consumes only `--likvid-*` names.
4. The mockup is responsive: the two-column grid collapses to one at ≤720px.
5. Mockup CSS classes carry a unique prefix (the old one had `gt-`) so they never cross demo sections.
6. Before starting work, the host entry must contain `import 'likvid-ui/theme.css'` — tokens and the `html` background come from there; overrides go in a separate stylesheet imported below theme.css (it sits in an `@layer`, unlayered styles always beat it).
7. Wherever the widget radius is used, `corner-shape: squircle !important;` stands there without exceptions. End of build: the count of `corner-shape` declarations equals the count of widget-radius sites; a mismatch means the mockup is not done.

## Structure

```
.
├── SKILL.md
└── references/
    ├── protocol.md   — where files live, how to replace, how not to break TestSection
    ├── kit-rules.md  — the kit + react style for mockups
    ├── layout.md     — block layout: background, widths, order, grids
    ├── sizes.md      — metrics: typography, control sizes
    └── anatomy.md    — screen patterns: liquid vs regular
```
