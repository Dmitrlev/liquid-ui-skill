# Anatomy — patterns: where liquid, where regular

Block-placement patterns taken from mobile screens (iOS). Text and content on the screenshots are noise; the structure is what matters. The main principle: content is an ordinary surface, liquid marks interaction points only — 2–3 glass accents per screen at most.

## 1. Screen header — regular

Large heading on the left (a subline beneath it), a round icon button or an avatar on the right. No surface: the block lives on the background. The header's round buttons are light liquid/ghost — they are actions, not content.

## 2. Feature card — regular

A wide card across the column at the top of the content: metric/graphic, a round action in the corner. An ordinary solid surface (white/dark). Liquid only if the card is the single one and the screen's key accent.

## 3. Metric grid 2×N — regular

Rows of identical square cards: a heading and a round chevron button at top, a large value, a mini graphic at the bottom. Chevrons are ghost circles (outline, no glass).

## 4. Vertical card list — regular

Repeating full-width row cards: icon, content, an action on the right; below — a row of pill sub-buttons. One gap step between cards.

## 5. Grouped list block — regular

A group of rows inside one card (settings, menu): a colored icon-chip on the left, rows separated by hairline dividers, value/chevron/switch on the right.

## 6. Horizontal card scroller — regular

A row of cards runs past the right edge with the next one half-visible — the half-visible card is the scroll affordance.

## 7. Floating tab bar at the bottom — LIQUID

A glass pill above the content, 2–4 tabs: icon + caption, the active one is accented and has its own pill backing inside. In the lib this is `NavBarBase`. One of the few truly liquid elements on the screen.

## 8. Floating search field — LIQUID

A glass pill/field floating over the background.

## 9. Hero header on a background — mixed

A central round element over a photo/colored backing, round ghost buttons above it; below, the content continues with regular cards.
