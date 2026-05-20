# Material 3 Desktop Flutter Guide

Use this when target includes Windows, macOS, Linux, desktop web, ChromeOS desktop mode, or external-monitor workflows.

## Desktop Goal

Desktop UI is not a wide tablet UI. Support resizable windows, mouse, keyboard, scroll wheel, focus, shortcuts, and denser workflows while keeping Material 3 structure.

## Window Classes

```text
expanded: width >= 840
large:    width >= 1200
extra:    width >= 1600
```

Use app window constraints, not monitor size.

```dart
LayoutBuilder(
  builder: (context, constraints) {
    final width = constraints.maxWidth;
    return width >= 1200 ? const DesktopView() : const TabletView();
  },
);
```

## Navigation

Common desktop patterns:

- `NavigationRail(extended: true)` for primary app sections.
- Permanent side pane only when hierarchy or frequent switching requires it.
- Top app bar for global search, account, sync/status, and primary commands.
- Contextual toolbar for selected table/list items.

Avoid:

- bottom `NavigationBar` on desktop widths.
- hidden mobile drawers for primary desktop nav.
- full-window modal flows when inline panes would be clearer.

## Content Width

Do not let text/forms stretch full screen.

Use constraints:

```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(maxWidth: 960),
    child: body,
  ),
);
```

Guidelines:

- Long copy: about 60 characters per line.
- Forms: usually 480-720 dp max width.
- Data/work surfaces: can use more width, but group into panes/columns.
- Dashboards: prefer responsive grids with min tile widths over fixed columns.

## Mouse And Keyboard

Material widgets cover many defaults. Custom controls must add:

- hover state
- focus state
- tab traversal
- keyboard activation: Enter/Space
- shortcuts for repeated actions
- scroll wheel behavior
- right-click/context menu only when useful

Use:

```dart
FocusableActionDetector(...)
Shortcuts(...)
Actions(...)
FocusTraversalGroup(...)
```

## Density

- Start from touch-safe UI.
- Only tighten spacing for desktop after mobile/tablet works.
- Keep interactive targets accessible; do not shrink critical commands below usability.
- Use icons with tooltips for compact toolbars.
- Prefer visible labels for destructive or ambiguous actions.

## Tables And Data Screens

Desktop can support denser comparison:

- use sortable columns when comparison matters
- sticky/contextual actions for selected rows
- preserve scroll position
- support keyboard selection where appropriate
- avoid horizontal overflow unless table semantics require it

For small desktop windows, degrade to tablet layout before content breaks.

## Dialogs And Windows

- Use non-fullscreen dialogs for short decisions/forms.
- Use side sheets or panes for edit-in-context.
- Avoid mobile bottom sheets for core desktop flows unless app pattern already uses them.
- Ensure Esc/back behavior and focus return.

## Desktop Build Awareness

Pair UI work with platform checks from `references/flutter-builds-by-stack.md`:

```bash
flutter build windows --release
flutter build macos --release
flutter build linux --release
```

Use platform-specific packaging only after UI works at target desktop sizes.

## Verification

Test:

- 840 dp width
- 1200 dp width
- 1600+ dp width
- narrow resized desktop window
- mouse hover
- scroll wheel/trackpad
- keyboard tab order
- shortcuts
- text scale
- focus return after dialogs

Return:

- desktop width classes used
- navigation/toolbar pattern
- max-width constraints
- input/focus/shortcut support
- desktop-specific verification result
