# Migration WebApp -> Flutter Skill

[![skills.sh](https://skills.sh/b/senasdev/migration-webapp-to-flutter)](https://skills.sh/senasdev/migration-webapp-to-flutter/migration-webapp-to-flutter)

Skill para agentes de IA que guía migraciones incrementales de una webapp existente a **Flutter nativo**, manteniendo la web operativa mientras se migra feature por feature.

También cubre creación de app Flutter, diseño responsive Material 3, adaptación móvil/tablet/PC, builds por plataforma, seguridad, tests y checks de release.

## Qué hace

- Migra React, Vue, Svelte, Angular u otra webapp a Flutter.
- Mantiene una estrategia incremental: feature por feature, sin romper la web existente.
- Decide cuándo usar WebView shell como transición y cuándo retirarlo.
- Porta modelos, lógica pura, servicios/API, estado y UI en orden seguro.
- Reutiliza contratos backend existentes salvo petición explícita.
- Define arquitectura simple: `domain/`, `services/`, `features/`, `widgets/`.
- Convierte patrones web a Flutter: types/interfaces, hooks/state, fetch/API, localStorage, geolocation, Tailwind y componentes UI.
- Guía diseño mobile-first con Material 3.
- Adapta UI a móvil, tablet/foldable y PC/desktop.
- Genera comandos y checklist para Android APK/AAB, iOS IPA, Linux, Windows y macOS.
- Añade prácticas estándar de seguridad, privacidad, QA, tests y release readiness.

## Qué puede hacer

### Migración

- Inventariar features de usuario.
- Clasificar cada feature como migrable, web-only o híbrido.
- Ordenar la migración por riesgo y dependencias.
- Mantener la web funcionando durante la transición.
- Ocultar secciones web ya migradas para evitar duplicados con Flutter nativo.
- Detectar APIs, browser APIs, estado, formularios, navegación y componentes que requieren adaptación.

### Diseño Flutter / Material 3

- Diseñar UI mobile-first y touch-first.
- Usar Material 3: `ThemeData(useMaterial3: true)`, `ColorScheme`, componentes y tipografía semántica.
- Aplicar breakpoints:
  - compact: `<600`
  - medium: `600-839`
  - expanded: `>=840`
  - large desktop: `>=1200`
- Cambiar navegación según tamaño:
  - móvil: `NavigationBar`
  - tablet: `NavigationRail`
  - desktop: `NavigationRail(extended: true)` o navegación lateral persistente cuando proceda.
- Diseñar layouts canónicos: feed, list/detail, supporting pane.
- Verificar safe areas, text scaling, targets de 48 dp, foco, hover, teclado y overflow.

### Tablet y PC

- Tablet/foldable:
  - panes
  - list/detail
  - rotación
  - split-screen
  - estado preservado al redimensionar

- PC/desktop:
  - ventanas redimensionables
  - mouse, hover, scroll wheel
  - shortcuts
  - tab traversal/focus
  - tablas y pantallas densas
  - constraints de ancho para texto/forms

### Builds y publicación

- Crear o reparar proyecto Flutter.
- Configurar envs con `--dart-define-from-file=env/prod.json`.
- Crear APK release.
- Crear AAB release para Google Play.
- Configurar firma Android con keystore y `android/key.properties`.
- Exportar SHA1 para Firebase/Google APIs.
- Crear builds Linux, Windows, macOS.
- Guiar build iOS IPA con Xcode, certificados y provisioning profiles.
- Reportar artifact paths y estado de firma.

### Seguridad y tests

- Detectar blockers: secrets hardcodeados, keystores commiteados, HTTP en producción, TLS deshabilitado, storage inseguro, WebView riesgoso.
- Revisar permisos: cámara, ubicación, archivos, notificaciones, micrófono, contactos, Bluetooth/NFC.
- Revisar auth, pagos, deep links, storage local, logout y expiración de sesión.
- Definir tests unitarios, widget tests e integration tests.
- Ejecutar o pedir:

```bash
flutter analyze
flutter test
flutter build apk --release
flutter build appbundle --release
```

- Emitir reporte con `BLOCKER`, `WARNING`, `INFO`, tests ejecutados y riesgo residual.

## Instalación

```bash
npx skills add SenasDev/migration-webapp-to-flutter
```

Global para Claude:

```bash
npx skills add SenasDev/migration-webapp-to-flutter -g -a claude -y
```

Global para Codex:

```bash
npx skills add SenasDev/migration-webapp-to-flutter -g -a codex -y
```

Ficha:

```text
https://skills.sh/senasdev/migration-webapp-to-flutter/migration-webapp-to-flutter
```

## Uso

```text
Usa la skill migration-webapp-to-flutter para planificar la migración de esta app React a Flutter.
```

```text
Migra el feature de búsqueda a Flutter siguiendo la metodología incremental.
```

```text
Crea la app Flutter y prepara APK/AAB release con env de producción.
```

```text
Diseña esta pantalla con Material 3 responsive para móvil, tablet y PC.
```

```text
Haz revisión de seguridad, tests y release readiness antes de publicar.
```

## Estructura

```text
.
├── SKILL.md
├── references/
│   ├── flutter-builds-by-stack.md
│   ├── flutter-security-testing.md
│   ├── material3-responsive-flutter.md
│   ├── material3-tablet-flutter.md
│   └── material3-desktop-flutter.md
├── LICENSE
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── PUBLISHING.md
└── SECURITY.md
```

## Archivos principales

| Archivo | Función |
|---|---|
| `SKILL.md` | Contrato principal de activación, reglas, decision gates y output esperado. |
| `references/flutter-builds-by-stack.md` | Creación de app, env vars, firma, APK/AAB, iOS, Linux, Windows, macOS y publicación. |
| `references/material3-responsive-flutter.md` | Guía base Material 3 responsive/adaptive para Flutter. |
| `references/material3-tablet-flutter.md` | Guía específica para tablet/foldable. |
| `references/material3-desktop-flutter.md` | Guía específica para PC/desktop. |
| `references/flutter-security-testing.md` | Seguridad, privacidad, tests, QA, warnings y release readiness. |
| `PUBLISHING.md` | Checklist de publicación y verificación en `skills.sh`. |

## Output esperado del agente

La skill fuerza al agente a devolver:

- inventario de features y orden de migración
- archivos creados/modificados
- cambios WebView/feature flags
- decisiones responsive: breakpoints, navegación, layout, accesibilidad
- comandos de verificación y resultado
- blockers/warnings de seguridad
- tests ejecutados o no ejecutados
- comandos build, artifact paths y estado de firma
- features pendientes, híbridas o web-only

## Compatibilidad

La skill está preparada para repositorios compatibles con `skills.sh`: `SKILL.md` vive en raíz, usa frontmatter YAML y delega detalles largos a referencias locales.

## Licencia

Apache-2.0.
