---
title: Feature Flags - Decisiones de Diseño
created: 2026-08-24
updated: 2026-08-27
status: active
tags:
  - design-decision
  - feature-flags
  - core
---
# Feature Flags – Decisiones de Diseño

Estado post-PR [#208](https://github.com/LemiPay/core/pull/208) / epic [#197](https://github.com/LemiPay/core/issues/197). Inventario corto: [[02 - Core/FEATURE_FLAGS]].

Código: `core/server/src/setup/config.rs` (`FeatureFlags`), `interfaces/http/config`, `interfaces/http/middlewares/feature_guard.rs`, `setup/router.rs`. Client: `core/client/src/lib/stores/config.ts` y `lib/types/endpoints/config.types.ts`.

## Objetivo

Activar y desactivar features avanzadas sin borrar código. Core usable para usuarios no técnicos. Flags globales, no por grupo.

## Dónde viven

- Variables de entorno `FEATURE_*` en el proceso del backend.
- Se leen **al arrancar**. Cambiar un flag = nuevo deploy / nueva revisión.
- **No se editan desde UI.** El Admin Panel v1 solo las muestra.
- Defaults si la var falta: Core `true`, avanzadas `false`.
- Parseo: `true` / `1` / `yes` (case-insensitive, con trim) = on. Cualquier otro valor = off. `on` no cuenta.
- Documentados en `core/.env.example`. En Azure: mismas vars en `core/.env.azure`.

## Nombres reales

Env var → clave JSON en `GET /config` (snake_case; serde default, sin rename):

| Env | Clave | Default pre-launch |
| --- | --- | --- |
| `FEATURE_GROUPS` | `groups` | true |
| `FEATURE_EXPENSES` | `expenses` | true |
| `FEATURE_BALANCES` | `balances` | true |
| `FEATURE_FRIENDS` | `friends` | true |
| `FEATURE_NOTIFICATIONS` | `notifications` | true |
| `FEATURE_TREASURY` | `treasury` | false |
| `FEATURE_FUND_ROUNDS` | `fund_rounds` | false |
| `FEATURE_INVESTMENTS` | `investments` | false |
| `FEATURE_GOVERNANCE_ADVANCED` | `governance_advanced` | false |
| `FEATURE_BLOCKCHAIN` | `blockchain` | false |
| `FEATURE_WALLETS_ONCHAIN` | `wallets_onchain` | false |
| `FEATURE_AI_CHAT` | `ai_chat` | false |

## Cómo se leen

1. Backend carga el mapa al boot (`AppConfig.features`).
2. Lo expone en **`GET /config`**: público, sin auth, sin prefijo `/api`.
3. Cuerpo: un objeto `features` con las claves de la tabla.
4. Frontend pide `/config` al bootstrap (layout principal, en paralelo con auth) y lo guarda en el store `config`.
5. Si `/config` falla: Core on, avanzadas off, y el store marca error. El login no se bloquea.
6. Rutas públicas (landing, login, register, status) no esperan al backend para renderizar.

## Qué hace cada lado cuando un flag está off

### Frontend

- Feature apagada = no existe: se ocultan menús, tabs, FABs, cards y bloques.
- Tabs de grupo: `wallets` (Billetera) solo si `treasury`; `fund_rounds` solo si `fund_rounds`.
- `/dashboard/treasury` redirige si `treasury` está off.
- Inversiones en dashboard y rutas de investments se ocultan/redirigen si `investments` está off.
- ChatAssistant solo si `ai_chat`.
- Wallets de perfil solo si `wallets_onchain`.
- El sidebar sigue mostrando Gobernanza: leftover del invite new-member. Se corrige en UX Core; “gobernanza” no es label del camino Core.
- Los flags de Core (`groups`, `expenses`, etc.) **hoy no ocultan** UI. Solo se publican.

### Backend

- Features avanzadas: middleware de guard → **404**. No 403.
- Rutas 404-eadas:
  - `/wallet` ← `wallets_onchain`
  - `/group-wallet`, `/transaction` ← `treasury`
  - `/investment` ← `investments`
  - `/blockchain-event` ← `blockchain`
  - `/ai` ← `ai_chat`
  - bajo `/governance`: withdraw ← `governance_advanced`; fund-round ← `fund_rounds`
- Invitaciones new-member **no** se protegen (`/governance/my`, `/received`, `/new-member/...`, etc.). Ese camino se reemplaza por asientos + links ([[02 - Core/MIEMBROS_E_INVITACIONES]]); no está en código todavía.
- `FEATURE_BLOCKCHAIN=false` además **no arranca** el live sync del vault (poll RPC).
- Flags de Core **no** 404-ean sus APIs.

## Hueco conocido (no es decisión nueva)

Con `FEATURE_INVESTMENTS=false` los endpoints de inversión dan 404, pero el pulse de inversiones en background **sigue corriendo** (intervalo 10s en `app_builder`). El live sync de blockchain sí se corta. No se “arregla” en estas docs: queda asentado.

## Login wallet vs `wallets_onchain`

El login web3 (`/auth/request-challenge`, `/auth/verify-challenge`, Reown en el client) es **Core**. `FEATURE_WALLETS_ONCHAIN` solo cubre el CRUD de wallets de usuario en `/wallet` y la UI de perfil.

## Admin Panel

- Solo lectura del mapa actual.
- Fuente: el mismo sistema (reusa `GET /config` / `GET /admin/flags`).
- Panel implementado: [[02 - Core/ADMIN_PANEL]] / [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]] (cerrado). No reabrir.

## Principios

- Simple > complejo.
- Cambio de flags = deploy consciente.
- Código apagado se mantiene.
- Más adelante se puede pasar a DB; no es v1.

## Stories cerradas

- [x] Backend: flags + `GET /config` ([#198](https://github.com/LemiPay/core/issues/198))
- [x] Frontend: store `config` ([#199](https://github.com/LemiPay/core/issues/199))
- [x] Frontend: UI avanzada oculta ([#200](https://github.com/LemiPay/core/issues/200))
- [x] Backend: guard 404 ([#201](https://github.com/LemiPay/core/issues/201))
- [x] `FEATURE_*` en `.env.example` + Azure ([#202](https://github.com/LemiPay/core/issues/202))
