# Flutter Security And Testing Standard

Use this before declaring a migrated feature or release build ready.

## Output Severity

Report issues as:

```text
BLOCKER: must fix before release/store upload
WARNING: should fix or explicitly accept risk
INFO: useful note, not release-blocking
```

Always include:

- affected feature/file
- risk
- fix
- verification command or manual check

## Security Blockers

Treat as blockers:

- Hardcoded API keys, tokens, passwords, signing passwords, private URLs, or secrets.
- Committed `*.jks`, `android/key.properties`, provisioning profiles, `.env`, or private certificates.
- Missing backup plan for Android upload keystore or Apple certificates.
- Auth/session tokens stored in plain SharedPreferences when secure storage is required.
- HTTP endpoints for auth, payments, personal data, or production APIs.
- Disabled TLS/certificate validation.
- WebView loading untrusted remote content with unrestricted JavaScript bridge.
- Payment/auth flows migrated without targeted tests.
- Personal data collection without visible permission/data handling review.
- Release build created from dev/staging API config by mistake.

## Secrets And Config

Rules:

- Use `--dart-define-from-file=env/<env>.json` for build config.
- Do not put true secrets in Dart code; compiled client code can be inspected.
- Keep API base URLs and non-secret public config in env files.
- Keep server secrets on backend only.
- Add to `.gitignore`:

```text
*.jks
android/key.properties
.env
env/*.local.json
*.mobileprovision
*.p12
```

Check:

```bash
rg -n "api[_-]?key|secret|token|password|BEGIN PRIVATE|AIza|sk_" .
```

## Network

Rules:

- Use HTTPS for production.
- Add timeouts to all API calls.
- Handle non-2xx status codes.
- Avoid logging tokens, auth headers, personal data, payment data, or full request/response bodies in production.
- Preserve backend contracts unless explicitly changed.

Recommended pattern:

```dart
final response = await http
    .get(uri, headers: headers)
    .timeout(const Duration(seconds: 15));
```

Tests:

- success
- timeout
- 401/403
- 404
- 500
- malformed JSON
- offline/no connection

## Storage

Use:

```text
SharedPreferences -> non-sensitive preferences only
flutter_secure_storage -> tokens/secrets that must live on device
sqflite/drift/hive -> local app data, avoid sensitive data unless encrypted/justified
```

Rules:

- Do not store passwords.
- Clear auth/session data on logout.
- Consider app lock/biometric only when product requires it.
- Avoid local cache of unnecessary personal data.

## Permissions

Review every platform permission:

- camera
- photos/files
- location
- notifications
- contacts
- microphone
- Bluetooth/NFC

Rules:

- Ask permission at point of need, not app start.
- Explain user benefit in UI before system prompt when permission is sensitive.
- Handle denied and permanently denied states.
- Remove unused permissions from Android/iOS manifests.

## WebView Migration Risks

If using WebView shell:

- Restrict allowed origins.
- Avoid unrestricted JavaScript channels.
- Disable file access unless required.
- Hide migrated web sections only after native replacement works.
- Do not pass auth tokens into WebView URLs.
- Clear WebView cache/session on logout if auth state exists there.

## Auth

Test:

- login success/failure
- logout clears state
- expired token
- refresh token failure
- app restart with existing session
- deep link/open app while logged out
- role/permission denied states

Warnings:

- Client-side role checks are UX only; backend must enforce authz.
- Never trust hidden UI as security boundary.

## Payments And Purchases

Block release until:

- sandbox purchase tested
- failed/cancelled payment tested
- restore purchase tested when applicable
- backend receipt validation exists when required
- duplicate tap/double purchase guarded

Do not trust client-only purchase state for premium access.

## Deep Links

Test:

- valid link
- invalid/malformed link
- unauthenticated user
- already-authenticated user
- expired invite/token
- app cold start
- app foreground/background

Never put long-lived secrets in links.

## Testing Layers

Minimum:

```bash
flutter analyze
flutter test
```

Use test types:

- Unit tests: models, parsing, pure migrated business logic.
- Widget tests: forms, validation, responsive branching, empty/error/loading states.
- Integration tests: auth, checkout/payment, critical API flows, navigation, local persistence.
- Golden/screenshot tests: optional for stable visual components and responsive layouts.

## Feature Test Checklist

For each migrated feature:

- loading state
- empty state
- error state
- success state
- invalid input
- API timeout/offline
- auth denied/expired if applicable
- text scale >= 1.3
- compact/medium/expanded layout if UI changed
- back navigation
- state after app restart if persisted

## Release QA

Before release:

```bash
flutter clean
flutter pub get
flutter analyze
flutter test
flutter build apk --release --dart-define-from-file=env/prod.json
flutter build appbundle --release --dart-define-from-file=env/prod.json
```

For desktop/iOS, use stack-specific commands from `references/flutter-builds-by-stack.md`.

Manual:

- verify prod API config
- install release artifact on real device when possible
- fresh install
- upgrade install
- logout/login
- offline behavior
- push/deep links if present
- crash-free smoke test
- app version/build number
- app icon/name/splash
- privacy labels/store listing consistency

## Store Readiness Warnings

Android:

- signed AAB required for Play Store
- upload keystore backed up
- `version` build number incremented
- Play App Signing understood
- SHA1 exported for Firebase/Google APIs if used

iOS:

- Apple Developer account required
- bundle ID correct
- certificates/profiles valid
- privacy manifest/permission strings reviewed
- App Store screenshots/privacy answers consistent with actual data use

Desktop:

- release artifact tested on target OS
- installer/package strategy known
- app permissions/notarization/signing reviewed where relevant

## Final Security/Test Report Template

```text
Security:
- BLOCKER: ...
- WARNING: ...
- INFO: ...

Tests:
- flutter analyze: pass/fail/not run
- flutter test: pass/fail/not run
- build: pass/fail/not run
- manual checks: ...

Residual risk:
- ...
```
