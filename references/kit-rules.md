# Kit rules and code style for mockups

## Surfaces: what is liquid and what is not

Components prefixed with `Liquid*` (`LiquidButton`, `LiquidContainer`, `LiquidRadio`, `NavBarBase`) are accents. They stand in meaningful places — actions, selection, navigation, key cards. "Liquid everywhere" means "liquid nowhere": when every surface is glass, none of them stands out.

Surface hierarchy, from plain to accent:
1. **Text blocks and content** — plain `background: #fff`, no shadow, no glass, no border. This is the default for paragraphs and text sections; they are not liquid elements.
2. **Grouping on the background** — widget cards (`LiquidContainer.section`) only when a group must be separated from the white fill.
3. **Actions and selection** — `LiquidButton`, `LiquidRadio`; surgically, every instance with a reason.

### Nested accent: liquid inside liquid is forbidden

You may not put a `LiquidButton` into a glass container — neither the glass nor the accent variant. Glass is a one-level surface: a second layer inside reads as garbage, and the accent is lost twice (you can see it: an accent pill inside a glass shell is junk). The rule is absolute, no exceptions.

An action inside a glass container is always a plain button: ghost (transparent, `currentColor` icon) or solid-accent (`var(--color-main)` fill, no glass). Such buttons differ from the mockup's other buttons in no radius way: pill by default (`border-radius: var(--radius-pull)`). It stays gray while the lib has no neutral variant — when one appears, take the variant from the lib, not from CSS.

### Icon-button group (toolbar)

Combine only buttons that are similar in meaning and equal in role (account/orders/favorites) — via `LiquidContainer` (shape="pill"): the container is the group's glass capsule shell, the user sees one toolbar, several spots are clickable.

A first-class button with its own meaning (search, cart, settings, menu) does not join the shared shell: a separate `LiquidButton` plus a shell for the rest — two liquid elements per row. Primacy is judged by the row, not by the icon: the same cart among four equal counter-actions is secondary and groups alongside them; it is split out only when it alone carries the row's main meaning. The split itself is the accent: the separated button need not be accent-colored, glass suffices. A one-button shell is never built — a lonely button is a `LiquidButton`, not a container.

Separate buttons of one row are still one cluster: they sit close with a small step (gap 8–12px), not scattered to the edges of the header. The cluster is pinned to the edge by layout means (a non-liquid wrapper or `margin-left: auto`), not by a second glass shell.

Inside the shell, `LiquidButton`s are forbidden (the nested-accent rule): real toolbar buttons are plain `<button>` — ghost for regular ones, solid-accent for the main one (submit). The toolbar's accent is a `var(--color-main)` fill, not glass.

Toolbar buttons are buttons like all others: pill radius by default (`var(--radius-pull)`), and the corner law from `layout.md` applies to them without concessions. The toolbar shell is pill too: a row of pills is wrapped into a capsule. Widget shape on a button is only possible when the button itself declares it (`shape="widget"`) — an explicit variant, never the default.

Ghost toolbar buttons are transparent: a background inside a glass/white shell is forbidden — a second layer on one surface. A light chip under a button is allowed only above media and gradients (a wishlist heart on a product photo), where the icon would otherwise sink.

### Accent — from the original, submit in accent

The mockup always declares an accent color: if the original (screenshot/description) has one — take it into the mockup root's `--color-main`, do not substitute the playground's default. Right next to it in the root sit the mandatory mirrors for library components: `--likvid-color-main: var(--color-main)`, and when overriding `--radius-widget` also `--likvid-radius-widget: var(--radius-widget)` — the lib reads only `--likvid-*` names, and the bridge at `:root` does not reach into the mockup's scope. Submit/primary buttons are accent (`variant="accent"`): that is where the eye lands first.

This rule is working, not iron: from screenshots we will clarify which other elements pull the accent.

### No liquid on white

A white regular surface is the content base, not a stage for an accent: a liquid element (glass or accent glass) placed on white loses both the glass and the accent — it reads as a foreign stain. Liquid lives on the tinted background (global `#f5f5f5`). So the header/panel hosting a liquid button or a glass toolbar must have no white background of its own; if the surface is white — buttons in it are plain.

Concrete "no liquid here" cases will keep extending this section.

## Install and styles (real project)

```bash
yarn add likvid-ui
```

Import the base tokens once, in the app entry — the order does not matter, the `@layer likvid.tokens` always sits below your unlayered styles:

```js
import 'likvid-ui/theme.css'
```

- **Exports:** `likvid-ui` — `LiquidContainer`, `LiquidButton`, `ButtonLoader`; `likvid-ui/next` — `NavBarBase`, `LiquidRadio` (peer deps `next`, `swiper`); `likvid-ui/theme.css` — kit tokens.
- **Peer deps:** `react >= 18`, `react-dom >= 18` (plus `next >= 13`, `swiper >= 11` for `/next`).
- **Token bridge:** components consume only `--likvid-*`; `theme.css` derives them from project variables with fallbacks — `--likvid-radius-widget ← --radius-widget (24px)`, `--likvid-radius-pill ← --radius-pull (9999px)`, `--likvid-color-main ← --color-main (#ff0030)` — and sets `--likvid-color-html: #f5f5f5` + the `html` background.
- **Overriding tokens** — in a separate stylesheet imported below the theme import (or a non-layered `:root` block), never by editing `theme.css`: an unlayered rule always beats the layer. A mockup's local accent still needs the root mirror `--likvid-color-main: var(--color-main)` because the theme computes at `:root`, not inside the mockup subtree.
- **Project-provided:** `--likvid-padding-inline-mobile` (bleed insets for `NavBarBase`/`LiquidRadio`; theme does not set it, fallback `1rem`).
- **Publishing pipeline (maintainers):** `yarn build` = tsc + CSS copy into `dist`; `prepack` runs the build, so `yarn pub` ships fresh `dist`; `files: ["dist"]`. Publishing is the owner's job — an agent builds but never publishes.
- In the dev playground this install step does not apply: vite aliases `likvid-ui` to the library `src` (iron rule 2), but the import specifiers (`likvid-ui`, `likvid-ui/next`, `likvid-ui/theme.css`) stay identical.

## Components from `likvid-ui`

```ts
import { LiquidButton, LiquidContainer } from 'likvid-ui'
import { NavBarBase, LiquidRadio, type I_liquidRadioItem } from 'likvid-ui/next'
```

- `LiquidButton` — pill/widget: `shape="pill"|"widget"`, `variant="glass"|"accent"|"accent-dark"`, `isActive`, `disabled`, `isLoading`, `width="100%"`, `callback`. Polymorphic: `as="a"|"div"|...` (`href` is required for `as="a"`). Icon+label go through `children`, not `title`.
- `LiquidContainer` — glass surface with a dot-tag: `LiquidContainer.div`, `.section`, `.button`, `.label`, `.li`, `.a`. Props: `shape`, `className`.
- `LiquidRadio` — segment control: `items: readonly I_liquidRadioItem[]` (`{ value, label, href }`), `value`, `onChange`.
- `NavBarBase` — bottom fixed navbar: `tabs: I_mobileNavTab[]` (`{ title, path, icon, isActive, onPress? }`). In a mockup only if it is bottom navigation; never in the page flow.

## Kit CSS rules (full contract — `/workspace/.zcode/skills/likvid-glaz/references/`)

- Background `#f5f5f5` is fixed and dark mode does not cancel it.
- Glass carries no `border`: `rgba(250,250,250,0.75)` + `backdrop-filter: blur(2px)` + `::before` with the inner white bevel (`inset 1px 1px 1px 0 white, inset -1px -1px 1px 0 white`) + soft shadow `0 0 20px -5px #5353531f`.
- Presses: `transition: 0.1s ease-in-out`, `:active { transform: scale(1.02–1.1) }`.
- Radii: `--radius-pull: 9999px` (pill), `--radius-widget` (widget, squircle with an `@supports` stub) — every widget-radius site requires `corner-shape: squircle !important;` (the law from `layout.md`); end of build: the count of declarations equals the count of usage sites, a mismatch means the mockup is not done.
- Page paddings: `--site-horizontal-padding` / `-mobile` are host-side; the lib's bleed insets read `--likvid-padding-inline-mobile` (fallback 1rem), the project sets the value.
- Kit styles wiring: the lib's token base is imported once — `@import 'likvid-ui/theme.css'` in global styles; its `@layer` sits below any unlayered host styles, so import order does not matter. Token overrides live in a separate stylesheet below the theme import — not edits to theme.css, not inline in components.

## React style (from the `code-style` skill)

- Functional components only; `interface` for props; named exports; `memo` on the root `Mockup`.
- `useCallback` — for callbacks that flow into memoized children (`LiquidButton` is memoized!): inline arrows in `callback` break its `memo`. List pattern: a `Map` of stable callbacks in `useMemo`.
- `key` — a stable id, never an index.
- `any`/`as unknown as` are forbidden; `JSX.Element` for icons is fine.
- Icons: `() => JSX.Element` with `stroke="currentColor"` — the color inherits from the button.

## Responsive

- Two-column grids — `grid-template-columns: 1fr 1fr` + `@media (max-width: 720px) { 1fr }`.
- Pill rows — `flex-wrap: wrap`.
- Headers and panels must not fall apart at 320px.
