# Material 3 Responsive Flutter Guide

Use this reference when migrating or creating Flutter UI. Source principles: Material 3 adaptive layout guidance at `https://m3.material.io/`, Android adaptive Material guidance, and Flutter adaptive/responsive docs.

## Design Goal

Do not copy desktop web layout 1:1 into Flutter. Recompose each feature into a mobile-first task flow, then adapt up to tablet, foldable, desktop, and resizable windows.

## Core Rules

- Use Material 3: `ThemeData(useMaterial3: true)`, `ColorScheme`, Material components, semantic typography.
- Solve compact mobile first, then medium/expanded.
- Use available window width, not hardware category.
- Avoid layout branching by `OrientationBuilder` near app root.
- Avoid portrait lock unless explicitly required.
- Use `SafeArea` around content that can be obscured by notches, status bars, system bars, hinges, or rounded corners.
- Preserve scroll/list state across size changes with stable keys such as `PageStorageKey`.
- Keep widgets small and reusable; abstract shared data before switching widgets per breakpoint.

## Window Size Classes

Use logical pixels.

```text
compact:  width < 600
medium:   600 <= width < 840
expanded: width >= 840
large:    width >= 1200, optional desktop refinement
```

Common mapping:

```text
compact  -> phone portrait, single-column, bottom NavigationBar
medium   -> tablet/foldable portrait, NavigationRail, wider content
expanded -> tablet landscape/desktop, NavigationRail + labels, multi-pane
large    -> desktop, constrained content width, richer shortcuts/input
```

## Measuring Size In Flutter

For full-screen/app-level layout:

```dart
final width = MediaQuery.sizeOf(context).width;
```

For local widget layout:

```dart
LayoutBuilder(
  builder: (context, constraints) {
    final width = constraints.maxWidth;
    return width < 600 ? const CompactView() : const WideView();
  },
);
```

Helper:

```dart
enum WindowClass { compact, medium, expanded }

WindowClass windowClassFor(double width) {
  if (width < 600) return WindowClass.compact;
  if (width < 840) return WindowClass.medium;
  return WindowClass.expanded;
}
```

## Adaptive Scaffold Pattern

Use equivalent navigation data across components.

```dart
class AppDestination {
  const AppDestination(this.icon, this.label);
  final IconData icon;
  final String label;
}
```

Pattern:

```text
compact  -> Scaffold.bottomNavigationBar: NavigationBar
medium   -> Row(NavigationRail, Expanded(body))
expanded -> Row(NavigationRail(extended: true), Expanded(body))
```

Implementation notes:

- Keep selected index and destination list shared.
- Use `NavigationBar` for compact primary destinations.
- Use `NavigationRail` for medium/expanded widths.
- Do not show bottom nav and rail simultaneously.
- Move FAB/primary action near rail or into app bar on wider layouts when ergonomically better.

## Canonical Layouts

Pick one per feature:

- Feed: cards/list content; compact = single column; expanded = grid/featured card with constrained width.
- List/detail: compact = list route then detail route; expanded = persistent list pane + detail pane.
- Supporting pane: compact = supporting info below or separate route; expanded = primary pane + side pane.

Rules:

- Do not let body text stretch across full desktop width.
- Target readable line length around 60 characters for long copy.
- Use panes and spacing to group related content; prefer whitespace before adding unnecessary cards.
- Use max-width constraints for forms, dialogs, and text-heavy panels.

## Spacing, Density, Touch

- Minimum interactive target: 48 x 48 dp.
- Prefer 8 dp spacing rhythm; use 4 dp only for fine internal adjustments.
- Compact mobile: larger tap areas, clear vertical rhythm, one primary action per screen.
- Desktop: lower visual density only after touch UI works; add hover/focus/keyboard accelerators.
- Never reduce text below readable body sizes just to fit a web layout.

## Safe Areas And Insets

Use:

```dart
SafeArea(
  child: body,
)
```

Place `SafeArea` around content at risk of being cut off. Do not wrap the whole `Scaffold` blindly when `AppBar`/system UI already handles its own inset.

Use `MediaQuery` for:

- text scale
- high contrast
- display features such as folds/hinges
- app window size

## Forms

Compact:

- Single column.
- Full-width fields.
- Primary button near end of flow and reachable.
- Validation inline, close to field.

Medium/expanded:

- Constrain max width.
- Use two columns only when field relationships stay obvious.
- Preserve tab traversal order with `FocusTraversalGroup` for complex forms.

## Lists And Tables

Compact:

- Prefer list cards/rows with essential data only.
- Use detail screen for secondary info.

Medium/expanded:

- Add secondary columns or panes.
- Consider `DataTable` only when comparison is the main task.
- Keep horizontal scrolling as last resort.

## Accessibility And Input

- Material widgets usually provide focus, hover, keyboard, and semantic behavior; custom widgets must implement it.
- Use `FocusableActionDetector` for custom interactive controls.
- Use `Shortcuts`/`Actions` for keyboard accelerators on desktop/web.
- Ensure tab traversal matches visual/reading order.
- Test with increased text scale; no clipped labels/buttons.
- Use semantic labels for icon-only controls.

## Migration Checklist Per Screen

1. Identify user task and primary action.
2. Define compact layout first.
3. Choose canonical layout for wider widths.
4. Define nav pattern per width class.
5. Replace fixed web sizes with flexible constraints.
6. Add safe areas and scroll behavior.
7. Verify 48 dp touch targets.
8. Verify text scaling and long labels.
9. Verify keyboard/focus/hover for desktop/web.
10. Verify resize from compact to expanded without losing state.

## Verification Commands

Run:

```bash
flutter analyze
flutter test
flutter run
```

Manual checks:

- compact width around 360-430 dp
- width 600 dp
- width 840 dp
- width 1200+ dp
- landscape
- text scale >= 1.3
- keyboard tab traversal
- mouse hover/scroll when desktop/web exists

## Sources

- Material Design 3: `https://m3.material.io/`
- Android adaptive Material guidance: `https://developer.android.com/codelabs/adaptive-material-guidance`
- Flutter adaptive/responsive docs: `https://docs.flutter.dev/ui/adaptive-responsive`
