# Material 3 Tablet Flutter Guide

Use this when target includes tablet, foldable, ChromeOS tablet mode, or large mobile landscape.

## Tablet Goal

Tablet UI is not stretched phone UI. Keep touch-first ergonomics while using extra width for context, preview, comparison, and fewer route changes.

## Size Classes

```text
medium:   600 <= width < 840
expanded: width >= 840
```

Use available app window width, not device model.

```dart
final width = MediaQuery.sizeOf(context).width;
final isTabletWidth = width >= 600;
```

## Navigation

```text
compact phone -> NavigationBar
tablet medium -> NavigationRail
tablet expanded -> NavigationRail(extended: true) when labels improve scan speed
```

Rules:

- Keep same destinations across `NavigationBar` and `NavigationRail`.
- Do not show bottom nav and rail together.
- Use 3-7 top-level destinations.
- Keep primary action reachable: FAB near nav rail or in screen action area.

## Layout Patterns

Prefer canonical layouts:

- List/detail: list pane + selected detail pane.
- Supporting pane: primary task + side context/actions.
- Feed: grid/featured cards with consistent spacing.

Compact route model:

```text
List screen -> Detail screen
```

Tablet route model:

```text
List pane | Detail pane
```

## Panes

Use panes when they reduce navigation hops:

- master list + item detail
- inbox + message
- catalog + product detail
- settings categories + setting detail
- dashboard summary + focused chart

Avoid panes when compact task is sequential and extra content distracts.

## Spacing And Width

- Keep touch targets >= 48 dp.
- Use 16-24 dp pane padding.
- Use 24 dp gaps between major panes.
- Avoid full-width paragraphs; keep readable line length around 60 chars.
- Use cards only for repeated items or clear containment, not entire screen sections.

## Foldables And Rotation

- Preserve state when window changes, rotates, folds, or unfolds.
- Do not branch only on portrait/landscape.
- Watch for hinges/display features via `MediaQuery` where relevant.
- Keep current selection visible when list/detail changes between one-pane and two-pane.

## Forms On Tablet

- Keep simple forms single-column with constrained max width.
- Use two columns only for obviously paired fields.
- Keep validation near fields.
- Preserve tab/focus order even though touch is primary.

## Verification

Test:

- 600 dp width
- 840 dp width
- landscape and portrait
- split-screen/multi-window if available
- text scale >= 1.3
- selection state across resize
- touch reachability and 48 dp targets

Return:

- window classes used
- navigation pattern
- pane strategy
- state preservation behavior
- tablet-specific verification result
