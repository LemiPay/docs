---
title: Estado Actual
created: 2026-08-26
updated: 2026-08-26
status: active
tags:
  - roadmap
  - status
  - pre-launch
---

# Estado Actual

Fecha de este corte: 2026-08-26. Contrastado con `core/`.

## Producto

LemiPay es una app de gastos compartidos (tipo Splitwise). App live: https://lemipay.app/ (Azure). Todavía no hay usuarios reales.

Stack: SvelteKit CSR (`core/client`) + Rust/Axum/Diesel/PostgreSQL (`core/server`). El API **no** usa prefijo `/api`. Manifiestos en `3.0.0`.

## Done (cerrado y mergeado)

### Structured Logging

- Epic [#203](https://github.com/LemiPay/core/issues/203) cerrado.
- Stories [#204](https://github.com/LemiPay/core/issues/204) (infra de tracing) y [#205](https://github.com/LemiPay/core/issues/205) (eventos de negocio).
- PRs: [#206](https://github.com/LemiPay/core/pull/206) (`#204`), [#207](https://github.com/LemiPay/core/pull/207) (epic).
- En código: `core/server/src/setup/logging.rs`, `init_tracing()` en `main`. Output por tracing, no `println!`.
- Decisiones: [[03 - Architecture/LOGGER_DECISIONES_DE_DISENO]].

### Feature Flags

- Epic [#197](https://github.com/LemiPay/core/issues/197) cerrado.
- Stories [#198](https://github.com/LemiPay/core/issues/198)–[#202](https://github.com/LemiPay/core/issues/202).
- PR [#208](https://github.com/LemiPay/core/pull/208) mergeada a `master`.
- En código: `FeatureFlags` en `setup/config.rs`, `GET /config`, store `config` en el client, middleware `feature_guard` → 404.
- Decisiones: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]].

## Core activo

Día 1 = Splitwise. Encendido: registro/login (email y wallet Reown), perfil, amigos (request/accept/block/unfriend/search), grupos, invitaciones new-member, expenses, balances/settlements/claim, permisos por rol, resolución de deudas, notificaciones in-app y emails básicos, `/status`.

Apagado con flags (código intacto): treasury / fondo común, fund rounds, investments, governance avanzada (retiros), blockchain, wallets on-chain de perfil, AI chat.

El pulse de inversiones **sigue corriendo** en background aunque la flag esté off. El live sync del vault **no** arranca si blockchain está off.

Detalle: [[02 - Core/SCOPE_DEL_CORE]].

## Qué sigue (orden)

1. **Admin Panel** — Sprint 2. Operativo, no es feature de usuario final. Ver [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]] y [[02 - Core/ADMIN_PANEL]].
2. **UX Core** — mobile-first, flujos cortos, copy sin jerga. Principios en [[05 - UX-UI/PRINCIPIOS_DE_DISENO]].
3. **Seguridad** — auditoría (hoy no hay checklist en el vault).
4. **Usuarios reales** — soft launch cercano, después primeros usuarios.

El roadmap largo no cambia: on-ramp custodial Mercado Pago → pulir entrada blockchain → camino non-custodial. Ver [[01 - Roadmap/PRE_LAUNCH_ROADMAP]].

## No implementado todavía

- Ruta `/admin`, guard de super-admin, KPIs, lectura de flags en UI de admin (no hay `routes/admin/` ni `/admin/*` en Axum).
- Rediseño UX del Core. La landing todavía habla de tesorería / DeFi con esas flags en false.
- Auditoría de seguridad.
- On-ramps.
