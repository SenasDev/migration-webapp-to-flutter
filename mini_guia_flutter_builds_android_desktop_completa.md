# MINI GUÍA TÉCNICA FLUTTER
## Builds Android, APK, AAB, Claves, Certificados y Desktop

Documentación oficial Flutter:
https://docs.flutter.dev

---

# ÍNDICE

1. Preparación del entorno
2. Comandos básicos Flutter
3. Variables de entorno
4. Configuración Android
5. Crear claves y certificados Android
6. Configuración de firma Android
7. Crear APK Release
8. Crear AAB Release
9. Versionado
10. Google Play Console
11. Linux Desktop
12. Windows Desktop
13. macOS Desktop
14. iOS / Apple
15. Estructura recomendada del proyecto
16. Comandos rápidos finales
17. Recomendaciones de seguridad

---

# 1. PREPARACIÓN DEL ENTORNO

## Verificar instalación

```bash
flutter doctor
```

Qué hace:

- Verifica Flutter SDK
- Verifica Android SDK
- Verifica Java
- Verifica emuladores
- Verifica herramientas desktop
- Detecta errores de configuración

---

## Ver versión Flutter

```bash
flutter --version
```

---

## Actualizar Flutter

```bash
flutter upgrade
```

---

## Ver dispositivos disponibles

```bash
flutter devices
```

---

# 2. COMANDOS BÁSICOS FLUTTER

## Limpiar proyecto

```bash
flutter clean
```

Elimina:

- builds anteriores
- caché
- temporales

---

## Instalar dependencias

```bash
flutter pub get
```

Instala paquetes definidos en:

```text
pubspec.yaml
```

---

## Analizar errores

```bash
flutter analyze
```

Detecta:

- errores
- warnings
- problemas de tipado

---

## Ejecutar aplicación

```bash
flutter run
```

---

## Ejecutar en release

```bash
flutter run --release
```

---

# 3. VARIABLES DE ENTORNO

# Uso recomendado

Flutter NO tiene variables de entorno reales como Node.js.

Se usan:

```bash
--dart-define
```

---

## Variables en Dart

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

---

## Compilar pasando variables

### APK

```bash
flutter build apk --release \
  --dart-define=API_URL=https://api.midominio.com \
  --dart-define=IS_PROD=true \
  --dart-define=APP_NAME=MiApp
```

---

### AAB

```bash
flutter build appbundle --release \
  --dart-define=API_URL=https://api.midominio.com \
  --dart-define=IS_PROD=true
```

---

# VARIABLES DESDE ARCHIVO JSON

## Estructura recomendada

```text
env/
  dev.json
  prod.json
  staging.json
```

---

## Ejemplo prod.json

```json
{
  "API_URL": "https://api.midominio.com",
  "IS_PROD": "true",
  "APP_NAME": "Mi App"
}
```

---

## Build usando archivo JSON

```bash
flutter build appbundle --release \
  --dart-define-from-file=env/prod.json
```

---

# 4. CONFIGURACIÓN ANDROID

## Carpeta importante

```text
android/
```

---

## Archivos importantes

```text
android/app/build.gradle
android/key.properties
android/app/src/main/AndroidManifest.xml
```

---

## Verificar Java

```bash
java -version
```

Recomendado:

- Java 17

---

## Verificar Android SDK

```bash
sdkmanager --list
```

---

# 5. CREAR CLAVES Y CERTIFICADOS ANDROID

# ¿Qué es el .jks?

Archivo privado usado para:

- firmar APK
- firmar AAB
- publicar en Google Play
- actualizar apps

IMPORTANTE:

Si pierdes este archivo:

- NO podrás actualizar la app.

---

# CREAR KEYSTORE EN LINUX

```bash
keytool -genkey -v \
  -keystore ~/upload-keystore.jks \
  -storetype JKS \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000 \
  -alias upload
```

---

# CREAR KEYSTORE EN WINDOWS

```powershell
keytool -genkey -v `
  -keystore $env:USERPROFILE\upload-keystore.jks `
  -storetype JKS `
  -keyalg RSA `
  -keysize 2048 `
  -validity 10000 `
  -alias upload
```

---

## Parámetros importantes

### keystore

Archivo generado:

```text
upload-keystore.jks
```

---

### keyalg RSA

Algoritmo de cifrado.

---

### keysize 2048

Tamaño de la clave.

---

### validity 10000

Duración en días.

---

### alias

Nombre interno de la clave.

---

# 6. CONFIGURAR FIRMA ANDROID

## Crear key.properties

Archivo:

```text
android/key.properties
```

---

## Linux

```properties
storePassword=TU_PASSWORD
keyPassword=TU_PASSWORD
keyAlias=upload
storeFile=/home/usuario/upload-keystore.jks
```

---

## Windows

```properties
storePassword=TU_PASSWORD
keyPassword=TU_PASSWORD
keyAlias=upload
storeFile=C:\\Users\\usuario\\upload-keystore.jks
```

---

# Configurar build.gradle

Archivo:

```text
android/app/build.gradle
```

---

## Configuración ejemplo

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

---

# 7. CREAR APK RELEASE

## APK normal

```bash
flutter build apk --release
```

---

## APK por arquitectura

```bash
flutter build apk --release --split-per-abi
```

Genera:

- arm64-v8a
- armeabi-v7a
- x86_64

APK más pequeños.

---

## APK con variables

```bash
flutter build apk --release \
  --dart-define-from-file=env/prod.json
```

---

## Ubicación APK

```text
build/app/outputs/flutter-apk/
```

---

# 8. CREAR AAB RELEASE

# ¿Qué es un AAB?

Android App Bundle.

Formato oficial para Google Play.

Google genera APK optimizados automáticamente.

---

## Build AAB

```bash
flutter build appbundle --release
```

---

## Build AAB con variables

```bash
flutter build appbundle --release \
  --dart-define-from-file=env/prod.json
```

---

## Ubicación AAB

```text
build/app/outputs/bundle/release/app-release.aab
```

---

# 9. VERSIONADO

# pubspec.yaml

```yaml
version: 1.0.0+1
```

---

## Formato

```text
1.0.0 = versión visible
+1 = build number
```

---

## Build por comandos

```bash
flutter build appbundle \
  --build-name=1.0.0 \
  --build-number=1
```

---

# 10. GOOGLE PLAY CONSOLE

## Subir aplicación

Subir:

```text
app-release.aab
```

---

## Recomendaciones

Activar:

- Play App Signing
- Integrity API
- Crashlytics

---

## Certificados importantes

Google genera:

- Upload certificate
- App signing certificate

---

## Exportar SHA1

```bash
keytool -list -v \
  -keystore upload-keystore.jks
```

Usado para:

- Firebase
- Google Login
- APIs Google

---

# 11. LINUX DESKTOP

## Activar soporte Linux

```bash
flutter create --platforms=linux .
```

---

## Ejecutar Linux

```bash
flutter run -d linux
```

---

## Build release Linux

```bash
flutter build linux --release
```

---

## Salida

```text
build/linux/x64/release/bundle/
```

---

# Crear AppImage Linux (opcional)

Herramienta:

```text
appimagetool
```

---

# 12. WINDOWS DESKTOP

## Activar soporte Windows

```bash
flutter create --platforms=windows .
```

---

## Ejecutar Windows

```bash
flutter run -d windows
```

---

## Build Windows release

```powershell
flutter build windows --release
```

---

## Salida EXE

```text
build/windows/x64/runner/Release/
```

---

## Crear instalador Windows

Herramientas:

- Inno Setup
- NSIS

---

# 13. MACOS DESKTOP

## Activar soporte macOS

```bash
flutter create --platforms=macos .
```

---

## Ejecutar macOS

```bash
flutter run -d macos
```

---

## Build release macOS

```bash
flutter build macos --release
```

---

## Salida

```text
build/macos/Build/Products/Release/
```

---

# Firmar app macOS

Requiere:

- Apple Developer Account
- certificados Apple
- Xcode

---

# 14. IOS / APPLE

# Requisitos

- macOS
- Xcode
- Apple Developer Account

---

## Instalar pods

```bash
cd ios
pod install
```

---

## Ejecutar iOS

```bash
flutter run -d ios
```

---

## Build release iOS

```bash
flutter build ipa --release
```

---

## Salida

```text
build/ios/ipa/
```

---

# Certificados Apple

Necesarios:

- Distribution Certificate
- Provisioning Profile
- App ID

---

# Publicar en App Store

Usar:

- Xcode
- Transporter

---

# 15. ESTRUCTURA RECOMENDADA

```text
project/
│
├── android/
├── ios/
├── linux/
├── windows/
├── macos/
│
├── env/
│   ├── dev.json
│   ├── prod.json
│   └── staging.json
│
├── lib/
├── assets/
├── pubspec.yaml
│
└── .gitignore
```

---

# 16. COMANDOS RÁPIDOS

# Android APK producción

```bash
flutter clean
flutter pub get
flutter build apk --release \
  --dart-define-from-file=env/prod.json
```

---

# Android AAB producción

```bash
flutter clean
flutter pub get
flutter build appbundle --release \
  --dart-define-from-file=env/prod.json
```

---

# Linux release

```bash
flutter clean
flutter pub get
flutter build linux --release \
  --dart-define-from-file=env/prod.json
```

---

# Windows release

```powershell
flutter clean
flutter pub get
flutter build windows --release \
  --dart-define-from-file=env/prod.json
```

---

# macOS release

```bash
flutter clean
flutter pub get
flutter build macos --release \
  --dart-define-from-file=env/prod.json
```

---

# iOS release

```bash
flutter clean
flutter pub get
flutter build ipa --release \
  --dart-define-from-file=env/prod.json
```

---

# 17. RECOMENDACIONES DE SEGURIDAD

# NO subir a Git

```text
*.jks
android/key.properties
.env
```

---

# Guardar backup de:

- upload-keystore.jks
- passwords
- alias
- certificados Apple
- perfiles provisioning

---

# Sin esto NO podrás:

- actualizar apps
- firmar builds
- publicar nuevas versiones

---

# RECOMENDACIÓN FINAL

Mantener:

- entorno DEV
- entorno STAGING
- entorno PROD

Separando:

- APIs
- Firebase
- variables
- certificados
- builds

