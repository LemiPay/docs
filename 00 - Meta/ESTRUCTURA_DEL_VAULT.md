---
title: Estructura del Vault LemiPay
created: 2026-08-24
updated: 2026-08-27
status: active
tags:
  - meta
  - obsidian
---

# Estructura del Vault LemiPay

El vault vive en `docs/` de este repo. Es la fuente de verdad para **decisiones** (agentes y humanos).

La fuente de verdad del **código** es `core/` (`client/` + `server/`). Si una página contradice lo mergeado en `core/`, gana el código. Actualizar la doc.

El dump académico (abstract, stack, casos de uso, ER) también vive acá, no en otro repo. Tiene que ser congruente con `core/`.

El índice vivo es [[INDEX]].

## Carpetas actuales

- `00 - Meta/` — cómo está armado el vault y cómo se trabaja
- `01 - Roadmap/` — estado, sprints, roadmap pre-launch
- `02 - Core/` — alcance del producto día 1, flags, admin, miembros e invitaciones
- `03 - Architecture/` — decisiones de arquitectura (logger)
- `05 - UX-UI/` — principios de interfaz
- raíz — `ABSTRACT`, `STACK_DESCRIPTION`, `USE_CASES`, `INDEX`, `README`
- `diagrams/` — ER (PlantUML) alineado a Diesel

## Carpetas previstas (aún no creadas)

- `04 - Security/`
- `06 - Agents/`
- `99 - Archive/`

## Convenciones

- Archivos `.md` en **MAYUSCULAS**, espacios y `-` → `_`, sin tildes (`FEATURE_FLAGS_DECISIONES_DE_DISENO.md`).
- Frontmatter YAML: `title`, `created` y/o `updated`, `status`, `tags`.
- Checkboxes `- [ ]` / `- [x]` para tracking de estado.
- Wikilinks de Obsidian entre páginas.
- En páginas de decisión: **cero bloques de código de implementación**.
- Nombres de env vars, rutas y contratos HTTP sí pueden aparecer. El detalle de defaults vive en `core/.env.example`.
- Al crear una página nueva, linkearla en [[INDEX]].
