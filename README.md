# Migration WebApp → Flutter Skill

[![skills.sh](https://skills.sh/b/senasdev/migration-webapp-to-flutter)](https://skills.sh/senasdev/migration-webapp-to-flutter/migration-webapp-to-flutter)

Skill para agentes de IA que guía migraciones incrementales de una webapp existente a **Flutter nativo**, manteniendo la web operativa mientras se migra feature por feature.

## Para qué sirve

- Convertir React, Vue, Svelte, Angular u otra webapp a Flutter.
- Planificar una migración incremental sin romper producción.
- Decidir cuándo usar un shell con WebView y cuándo eliminarlo.
- Traducir patrones web a Flutter: types/interfaces, hooks/state, fetch/API, localStorage, geolocation, Tailwind y componentes UI.
- Mantener una arquitectura simple con `domain/`, `services/`, `features/` y `widgets/`.

## Qué hace el agente al usarla

La skill indica al agente que debe:

1. Inventariar features de usuario antes de tocar archivos.
2. Clasificar cada feature como migrable, web-only o híbrido.
3. Migrar modelos y lógica pura antes de widgets.
4. Reutilizar APIs y contratos de datos existentes.
5. Usar WebView shell solo cuando tenga sentido para una transición incremental.
6. Ocultar en la web los bloques ya migrados a Flutter para evitar duplicados.
7. Verificar con `flutter analyze`, tests y build debug.

## Instalación

Desde GitHub:

```bash
npx skills add SenasDev/migration-webapp-to-flutter
```

Para instalarla globalmente y apuntar a Codex:

```bash
npx skills add SenasDev/migration-webapp-to-flutter -g -a codex -y
```

Para comprobar que `skills.sh` detecta la skill:

```bash
npx skills add SenasDev/migration-webapp-to-flutter --list
```

También puedes revisar la ficha en:

```text
https://skills.sh/senasdev/migration-webapp-to-flutter/migration-webapp-to-flutter
```

## Uso

Pide al agente algo como:

```text
Usa la skill migration-webapp-to-flutter para planificar la migración de esta app React a Flutter.
```

```text
Migra el feature de búsqueda a Flutter siguiendo la metodología incremental.
```

```text
Analiza qué partes de esta webapp son migrables, web-only o híbridas.
```

## Estructura

```text
.
├── SKILL.md
├── references/
│   └── migration-guide.md
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
| `SKILL.md` | Instrucciones principales y compactas que seguirá el agente. |
| `references/migration-guide.md` | Guía larga con ejemplos, tablas de equivalencias, arquitectura y checklists. |
| `PUBLISHING.md` | Checklist de publicación y verificación en `skills.sh`. |

## Compatibilidad

La skill está preparada para repositorios compatibles con `skills.sh`: el archivo `SKILL.md` está en la raíz, contiene frontmatter YAML y usa referencias locales para el contenido largo.

## Licencia

Apache-2.0.
