---
name: migration-webapp-to-flutter
description: "Trigger: migrar webapp a Flutter, convertir React/Vue/Svelte/Angular a Flutter, webapp to mobile/desktop app, crear builds Flutter Android APK/AAB, iOS IPA, Linux, Windows o macOS, diseñar UI móvil Material 3 responsive/adaptive, seguridad Flutter, tests y release checks. Guía migración incremental, diseño responsive, seguridad y entrega por stack."
license: Apache-2.0
metadata:
  author: senasdesktop
  version: "1.0.0"
---

## Activation Contract

Use this skill when migrating an existing React, Vue, Svelte, Angular, or similar webapp to native Flutter, especially when the user wants an incremental migration that keeps the web version alive during the transition.

Also use it when the user needs to create, configure, sign, build, or publish the Flutter app for Android, iOS, Linux, Windows, or macOS after or during the migration.

Also use it when the user asks for mobile UI, Material 3, responsive/adaptive layout, tablet/desktop adaptation, navigation behavior, accessibility, or touch-friendly design for the migrated Flutter app.

Also use it when the user asks for security hardening, release readiness, test strategy, QA checklist, warnings, or validation for a migrated Flutter app.

## Hard Rules

- Do not treat the migration as a rewrite; migrate feature by feature and keep the webapp working.
- Analyze user-facing features before files; a feature may span domain, state, UI, and API code.
- Migrate domain models and pure logic before UI widgets.
- Keep backend APIs and data contracts unchanged unless the user explicitly asks otherwise.
- Use a WebView shell only when the webapp is already deployed and the user needs a working app from day one.
- For each migrated feature, hide the duplicated web section in the WebView and show the native Flutter version.
- Keep `domain/` free of Flutter imports; UI belongs in `features/` or `widgets/`.
- Prefer simple `StatefulWidget` state; do not add Provider, Riverpod, BLoC, or Redux unless the app clearly needs it.
- Treat app creation and release builds as stack-specific work: Android APK/AAB, iOS IPA, Linux, Windows, and macOS each require different prerequisites, commands, signing, and artifacts.
- Never commit signing secrets, keystores, passwords, provisioning profiles, `.env`, or `android/key.properties`.
- Prefer `--dart-define-from-file=env/<env>.json` for build-time config when multiple environments exist.
- Design mobile-first, touch-first, and adaptive from the start; do not port fixed web layouts directly into Flutter.
- Use Material 3 window size classes and available app window width for layout decisions, not device names, hardware type, or raw orientation.
- Use `MediaQuery.sizeOf(context)` for app-window breakpoints and `LayoutBuilder` for local widget constraints.
- Keep interactive targets at least 48 dp and preserve focus, hover, keyboard, text scaling, and safe-area behavior.
- Apply security and test checks from `references/flutter-security-testing.md` before release or store submission.
- Surface blocking warnings explicitly: missing signing backups, hardcoded secrets, weak TLS/API handling, unsafe storage, missing permissions rationale, or untested auth/payment/data-loss flows.

## Decision Gates

| Situation | Action |
|---|---|
| Webapp deployed and must keep working | Start with a Flutter WebView shell and feature flags. |
| Full native migration requested | Still migrate in feature order; remove WebView only after all features are native. |
| Feature has business formulas | Port models and pure functions first, then compare outputs with known web values. |
| Feature calls APIs | Reuse the same endpoints with Dart `http`, `Uri.https()`, timeouts, and error states. |
| Feature uses browser APIs | Map them deliberately: localStorage to SharedPreferences, geolocation to geolocator, charts to fl_chart. |
| User asks "create app" without platform | Ask for target stack only if not inferable; otherwise default to Android APK for direct install plus AAB for Play Store. |
| Android release needed | Configure version, env file, keystore, `key.properties`, Gradle signing, then build APK/AAB. |
| Play Store needed | Produce signed AAB, verify `version`/build number, export SHA1 if Firebase/Google APIs are involved. |
| iOS release needed | Require macOS, Xcode, Apple Developer account, certificates, provisioning profile, pods, then build IPA. |
| Desktop release needed | Enable exact platform with `flutter create --platforms=<platform> .`, then build and report artifact path. |
| Mobile UI requested | Start from compact width, use Material 3 components, verify touch targets, safe areas, text scaling, and overflow. |
| Tablet responsive UI requested | Use `references/material3-tablet-flutter.md`: medium/expanded layouts, panes, foldables, touch-first, nav rail. |
| PC/desktop responsive UI requested | Use `references/material3-desktop-flutter.md`: resizable windows, constrained widths, mouse/keyboard, density, shortcuts. |
| Tablet/desktop responsive UI requested | Switch layout at width classes: compact `<600`, medium `600-839`, expanded `>=840`; read both tablet and desktop guides when target includes both. |
| Web layout uses sidebars/cards/grids | Recompose into mobile task flows first, then expand into list/detail, feed, or supporting-pane layouts. |
| Security/release readiness requested | Use `references/flutter-security-testing.md`; report blockers, warnings, tests run, and residual risk. |
| Auth, payments, personal data, files, location, camera, notifications, or deep links involved | Require explicit permission/data handling review and targeted tests before release. |

## Execution Steps

1. Inventory all user-facing features and classify them as migrable, web-only, or hybrid.
2. Build a dependency order from least risky to most complex: static components, forms, calculators, API features, then complex state.
3. Create or update a Flutter project in `flutter/` at the repo root; if none exists, scaffold it with `flutter create flutter`.
4. Add only required Flutter dependencies and document why each is needed.
5. For each feature, port models, domain logic, service/API code, state, UI, WebView hiding logic, and verification in that order.
6. For each native UI, apply Material 3 responsive guidance in `references/material3-responsive-flutter.md`; if target includes tablet or PC, also apply the specific tablet/desktop reference.
7. Apply security and QA checks from `references/flutter-security-testing.md` for changed features.
8. Run `flutter analyze`, relevant tests, and a debug build before marking a feature migrated.
9. When user asks for an installable app, create the requested stack using the stack guide in `references/flutter-builds-by-stack.md`.
10. Remove WebView dependencies and bridge files only after every migrable feature has a native Flutter replacement.

## App Creation / Build Handoff

When delivering platform builds, return an explicit stack block:

- Target stack: Android APK, Android AAB, iOS IPA, Linux, Windows, or macOS.
- Required local tools: Flutter SDK, Android SDK, Java 17, Xcode, CocoaPods, desktop toolchain, as applicable.
- Config files touched: `pubspec.yaml`, `env/*.json`, `android/key.properties`, `android/app/build.gradle`, platform folders.
- Build command: exact command run or exact command user must run.
- Artifact path: exact expected output path.
- Signing state: debug, unsigned release, signed release, store-ready, or blocked by missing credentials.
- Security notes: secrets that must stay out of Git and backups required.

## Output Contract

Return:
- Feature inventory and migration order.
- Files created or changed for the current feature.
- WebView/feature-flag changes, if any.
- Verification commands run and results.
- Responsive UI decisions: window classes, navigation pattern, canonical layout, accessibility/touch checks.
- Security/test status: blockers, warnings, tests run, skipped tests, and residual risk.
- Platform build commands, artifact paths, signing status, and missing local prerequisites.
- Remaining unmigrated, hybrid, or web-only features.

## References

- `references/flutter-builds-by-stack.md` — app creation, environment config, signing, build commands, artifact paths, and publishing checklist by target stack.
- `references/material3-responsive-flutter.md` — Material 3 mobile-first responsive/adaptive UI rules, Flutter patterns, navigation, canonical layouts, accessibility, and verification checklist.
- `references/material3-tablet-flutter.md` — tablet/foldable UI guidance: panes, nav rail, list/detail, touch ergonomics, rotation, and state preservation.
- `references/material3-desktop-flutter.md` — PC/desktop UI guidance: resizable windows, mouse/keyboard, shortcuts, focus, content width, data-dense screens, and desktop builds.
- `references/flutter-security-testing.md` — standard security, privacy, testing, QA, release warning, and store-readiness checklist for migrated Flutter apps.
