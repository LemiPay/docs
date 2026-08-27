---
title: Logger - Decisiones de Diseño
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - architecture
  - logging
  - design-decision
---
# Logger – Decisiones de Diseño

Estado post-PR [#207](https://github.com/LemiPay/core/pull/207) / epic [#203](https://github.com/LemiPay/core/issues/203). Infra en [#204](https://github.com/LemiPay/core/issues/204) (PR [#206](https://github.com/LemiPay/core/pull/206)); eventos de negocio en [#205](https://github.com/LemiPay/core/issues/205).

Código: `core/server/src/setup/logging.rs`, `init_tracing()` en `main.rs`.

## Objetivo

Logging estructurado en backend: saber qué módulo hizo qué, registrar eventos de negocio y errores. Sin ruido.

## Decisiones (y cómo quedó)

- **Alcance**: solo backend.
- **Crates**: `tracing` + `tracing-subscriber` (env-filter, fmt, json, ansi) — están en `Cargo.toml`.
- **Destino**: stdout. Writer de consola, con target/módulo visible.
- **No se loguea**: cada request HTTP. No hay layer de access log.
- **Formato** via `LOG_FORMAT`:
  - `pretty` → legible con color. **Default** si falta la var o el valor no es `json`.
  - `json` → JSON estructurado (case-insensitive).
  - Otro valor → pretty, y un warn de logging (`target: logging`).
- **Nivel** via `RUST_LOG` (filtro env). Default: `info`.
- Intención de entorno: pretty en desarrollo, json en producción. El default del proceso es pretty: en Azure hay que setear `LOG_FORMAT=json` si se quiere JSON. `core/.env.azure` **no** define `LOG_FORMAT`.
- **Output del proceso**: todo por tracing. No `println!` / `eprintln!` en el servidor.
- **Arranque**: si falta env requerida, no hay pool de DB, o falla bind/serve → se loguea el error (`target: startup`) y el proceso sale 1. No panic de boot.
- **Panics restantes**: hook que los registra estructurado (`target: panic`).

## Qué se loguea (negocio + errores)

Éxitos en `info`, fallos de negocio en `warn`, fallos internos en `error`. Targets por módulo (`auth`, `group`, `expense`, `startup`, `email`, `logging`, `panic`, `investment`, …).

Instrumentado en use cases / services (mínimo de #205):

- Auth: registro; login ok / inválido / error interno.
- Grupos: creación; salir; invitar miembro; aceptar / rechazar invitación.
- Expenses: creación y fallos de ese flujo.

También hay logs de arranque, email, market data e investments (incl. el pulse), y del live sync si blockchain está on. Si blockchain está off, un info de startup lo dice.

## Qué no se loguea

- Passwords, JWT, montos ni descripciones de expenses.
- Cada request HTTP.

## Estado

- [x] Infra de logging (#204 / #206)
- [x] Eventos de negocio del Core (#205 / #207)
