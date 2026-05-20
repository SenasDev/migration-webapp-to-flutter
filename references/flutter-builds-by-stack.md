# Flutter App Creation And Builds By Stack

Use this reference when the user asks to create, configure, sign, build, or publish the migrated Flutter app.

## Baseline Checks

Run from Flutter project root:

```bash
flutter doctor
flutter --version
flutter devices
flutter pub get
flutter analyze
```

Use Java 17 for Android:

```bash
java -version
```

Check Android SDK:

```bash
sdkmanager --list
```

## Create New Flutter App

Recommended for migrations: create the Flutter app in a dedicated repo-root folder named `flutter/`.

```bash
flutter create flutter
cd flutter
flutter pub get
flutter run
```

If the Flutter app already exists and a platform folder is missing, run the platform-specific `flutter create --platforms=<platform> .` command from the existing Flutter project root.

## Project Layout

Recommended:

```text
project/
  android/
  ios/
  linux/
  windows/
  macos/
  env/
    dev.json
    staging.json
    prod.json
  lib/
  assets/
  pubspec.yaml
```

## Build-Time Config

Flutter uses compile-time variables with `--dart-define`.

In Dart:

```dart
const apiUrl = String.fromEnvironment(
  'API_URL',
  defaultValue: 'https://dev.api.com',
);

const isProd = bool.fromEnvironment(
  'IS_PROD',
  defaultValue: false,
);

const appName = String.fromEnvironment(
  'APP_NAME',
  defaultValue: 'MyApp DEV',
);
```

Prefer env files:

```json
{
  "API_URL": "https://api.midominio.com",
  "IS_PROD": "true",
  "APP_NAME": "Mi App"
}
```

Build with:

```bash
flutter build appbundle --release \
  --dart-define-from-file=env/prod.json
```

## Versioning

Set in `pubspec.yaml`:

```yaml
version: 1.0.0+1
```

Meaning:

```text
1.0.0 = visible version
+1 = build number
```

Or pass at build time:

```bash
flutter build appbundle \
  --build-name=1.0.0 \
  --build-number=1
```

## Android: Create Or Repair Platform Folder

From Flutter project root:

```bash
flutter create --platforms=android .
```

Important files:

```text
android/
android/app/build.gradle
android/key.properties
android/app/src/main/AndroidManifest.xml
```

## Android: Keystore

Linux/macOS:

```bash
keytool -genkey -v \
  -keystore ~/upload-keystore.jks \
  -storetype JKS \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000 \
  -alias upload
```

Windows PowerShell:

```powershell
keytool -genkey -v `
  -keystore $env:USERPROFILE\upload-keystore.jks `
  -storetype JKS `
  -keyalg RSA `
  -keysize 2048 `
  -validity 10000 `
  -alias upload
```

If this `.jks` is lost, app updates can be blocked. Back it up securely.

## Android: Signing Config

Create `android/key.properties`.

Linux/macOS:

```properties
storePassword=TU_PASSWORD
keyPassword=TU_PASSWORD
keyAlias=upload
storeFile=/home/usuario/upload-keystore.jks
```

Windows:

```properties
storePassword=TU_PASSWORD
keyPassword=TU_PASSWORD
keyAlias=upload
storeFile=C:\\Users\\usuario\\upload-keystore.jks
```

Configure `android/app/build.gradle`:

Use this when the Android project uses Groovy Gradle files. If the project uses Kotlin DSL (`build.gradle.kts`), adapt the same `signingConfigs` and `buildTypes.release` intent to Kotlin syntax instead of replacing the file blindly.

```gradle
import java.util.Properties

def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')

if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

android {
    signingConfigs {
        release {
            keyAlias keystoreProperties['keyAlias']
            keyPassword keystoreProperties['keyPassword']
            storeFile file(keystoreProperties['storeFile'])
            storePassword keystoreProperties['storePassword']
        }
    }

    buildTypes {
        release {
            signingConfig signingConfigs.release
            minifyEnabled true
            shrinkResources true
        }
    }
}
```

## Android APK

Use for direct install/testing outside Play Store.

```bash
flutter clean
flutter pub get
flutter build apk --release \
  --dart-define-from-file=env/prod.json
```

Split per ABI:

```bash
flutter build apk --release --split-per-abi
```

Artifact:

```text
build/app/outputs/flutter-apk/
```

## Android AAB

Use for Google Play.

```bash
flutter clean
flutter pub get
flutter build appbundle --release \
  --dart-define-from-file=env/prod.json
```

Artifact:

```text
build/app/outputs/bundle/release/app-release.aab
```

Google Play checklist:

- Upload `app-release.aab`.
- Enable Play App Signing.
- Enable Integrity API when required.
- Add Crashlytics when Firebase exists.
- Verify visible version and build number.

Export SHA1 for Firebase, Google Login, and Google APIs:

```bash
keytool -list -v \
  -keystore upload-keystore.jks
```

## Linux Desktop

Enable:

```bash
flutter create --platforms=linux .
```

Run:

```bash
flutter run -d linux
```

Build:

```bash
flutter clean
flutter pub get
flutter build linux --release \
  --dart-define-from-file=env/prod.json
```

Artifact:

```text
build/linux/x64/release/bundle/
```

Optional packaging: AppImage via `appimagetool`.

## Windows Desktop

Enable:

```bash
flutter create --platforms=windows .
```

Run:

```bash
flutter run -d windows
```

Build:

```powershell
flutter clean
flutter pub get
flutter build windows --release `
  --dart-define-from-file=env/prod.json
```

Artifact:

```text
build/windows/x64/runner/Release/
```

Installer tools: Inno Setup or NSIS.

## macOS Desktop

Requires macOS, Xcode, Apple Developer account for signing/distribution.

Enable:

```bash
flutter create --platforms=macos .
```

Run:

```bash
flutter run -d macos
```

Build:

```bash
flutter clean
flutter pub get
flutter build macos --release \
  --dart-define-from-file=env/prod.json
```

Artifact:

```text
build/macos/Build/Products/Release/
```

Signing requires Apple certificates and Xcode.

## iOS

Requires macOS, Xcode, Apple Developer account, Distribution Certificate, Provisioning Profile, and App ID.

Create/repair platform folder:

```bash
flutter create --platforms=ios .
```

Install pods:

```bash
cd ios
pod install
```

Run:

```bash
flutter run -d ios
```

Build:

```bash
flutter clean
flutter pub get
flutter build ipa --release \
  --dart-define-from-file=env/prod.json
```

Artifact:

```text
build/ios/ipa/
```

Publish with Xcode or Transporter.

## Security Rules

Never commit:

```text
*.jks
android/key.properties
.env
```

Back up:

- `upload-keystore.jks`
- keystore passwords
- key alias
- Apple certificates
- provisioning profiles

Keep separate DEV, STAGING, and PROD config for APIs, Firebase, variables, certificates, and builds.
