---
title: Sistema de Feature Flags
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - feature-flags
  - architecture
---
# Sistema de Feature Flags

Página corta. Comportamiento y contratos: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]].

## Decisiones

- Globales (toda la app).
- Viven en env vars del backend. Se cambian con deploy. No se editan desde UI.
- Backend las expone en runtime: `GET /config` (público, sin `/api`). Cuerpo: `{ "features": { ... } }` en snake_case.
- Frontend las pide al arrancar (store `config` en `core/client/src/lib/stores/config.ts`). Si falla: Core on, avanzadas off.

## Mapa pre-launch

Core on: `groups`, `expenses`, `balances`, `friends`, `notifications`.

Avanzadas off: `treasury`, `fund_rounds`, `investments`, `governance_advanced`, `blockchain`, `wallets_onchain`, `ai_chat`.

Env: `FEATURE_<NOMBRE_EN_MAYÚSCULAS>`. Defaults y parseo en la página de decisiones.

`FEATURE_WALLETS_ONCHAIN` apaga `/wallet` y las wallets de perfil. **No** apaga el login con wallet (Reown + `/auth/request-challenge`).

## Azure

Container App del backend. Mismas `FEATURE_*`. Cambio = nueva revisión. `core/.env.azure` replica el mapa Core on / avanzadas off. `LOG_FORMAT` no está seteado ahí (el proceso default es pretty).

## Por qué este approach

Cero tabla en DB. Auditable. Cabe en 8–10 h/semana. El frontend no hace falta redeployarlo para leer un flag nuevo (sí hace falta redeploy del backend).
