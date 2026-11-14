# Layout — block placement in the mockup

General principle: the mockup is a mobile column living in the centered flow; blocks go strictly top to bottom in the order from the input (screenshot/description). The numbers below are kit constants; local deviations only inside the mockup root's `Mockup.css`.

## Premises: background and rounding

Global background — `#f5f5f5` (on `html`; in the playground host that is `--color-html`, already set in `app.css`). Check before building. The point: the background is light and fixed — only this way white blocks and liquid elements pop from it; any other background breaks the contrast of the whole kit. It changes only at the host level, never inside `Mockup.css`.

### Rounding

In global styles, always together with the background:

```css
:root {
	--radius-pull: 9999px;
	--radius-widget: 22px;
	/* mirrors for the lib: it reads --likvid-* */
	--likvid-color-main: var(--color-main);
	--likvid-radius-widget: var(--radius-widget);
}

@supports (corner-shape: squircle) {
	:root {
		--radius-widget: 52px;
	}
}
```

The point: two distinct shapes, not one number for everything — `pill` (`--radius-pull`: capsule, on buttons and inputs) and `widget` (`--radius-widget`: on widget cards). One variable for both shapes is forbidden. The law: widget is the default value for all ordinary surfaces (cards, banners, badges), pill is the exception for buttons and inputs. A toolbar shell is not an ordinary surface but a row of pills: it gets pill — the shape of its content. `shape="widget"` stays an explicit variant for a lone button (on `LiquidButton`), never the default.

`squircle` is not supported by every browser, so the `@supports` stub with the 22px fallback is mandatory. The law: `corner-shape: squircle !important;` is written **on every element that uses the widget radius** — there is no "where needed" wording: the iPhone-style corner is exactly the reason the widget shape exists, and `border-radius: var(--radius-widget)` without this property is a violation of the law. Split radii (`border-*-right-radius: var(--radius-widget)`) and token-derived values (`border-radius: calc(var(--radius-widget) - 8px)`) count as widget-radius usage. Browsers without support ignore the line themselves and get round 22px — that is the regular fallback, not an exception from the rule. Pill (`--radius-pull`) never gets the property: a capsule has its own shape.

## Width

1. The mockup stretches with the host flow to full width: `width: 94%`, `max-width: 1400px`, `margin-inline: auto`. The narrow mobile column is set by the viewport, not by the mockup: the same root at 375px yields ~353px. The old wording "column as wide as the screenshot" and the 1120px ceiling are cancelled.
2. The mockup's inner columns (sidebar+feed, gallery+buy, info+checkout) distribute the stretched width instead of hiding behind the mobile maximum; on mobile the grids' collapse point is ≤720px.

## Block order

3. DOM order = order on the screenshot, top to bottom; the visual order must not be rearranged, not even "to look nicer".
4. Between same-level blocks the `gap` is fixed: 16px (mobile) / 24px (desktop). Spacing "by eye" is forbidden.

## Grids

5. Two-column grid — `1fr 1fr`, collapsing to `1fr` at ≤720px. Never more than three columns: the mockup tests the kit, not a grid.
6. Pill/tab rows — `flex-wrap: wrap`, `gap: 12px`; wrapping on mobile is normal, not a bug.

## Glass and air

7. A glass card — inner padding `18px 20px` (widget) / `0.875rem 1rem` (kit pill). Never press content against the bevel.
8. The "card bottom < top" padding rule for text headings inside a widget card — the kit's padding rule.

## Fixing

9. Fixed elements (bottom navbar) — `NavBarBase` only; do not invent a second fixed layer, it conflicts with the host's padding.
