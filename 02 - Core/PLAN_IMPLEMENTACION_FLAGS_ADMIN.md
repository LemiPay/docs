---
title: Plan de Implementación - Feature Flags + Admin Panel
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - implementation
  - feature-flags
  - admin
---

# Plan de Implementación (histórico + restante)

Los flags ya cerraron (están en `core/`). El Admin Panel se mueve a [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]]. Esta página no es el plan vivo del sprint: es el recuento de lo que se hizo y lo que quedó.

## Objetivo de esta etapa (original)

Feature Flags funcionando + Admin Panel básico en producción.

## Flags (pasos 1–2) — hecho

Epic [#197](https://github.com/LemiPay/core/issues/197), PR [#208](https://github.com/LemiPay/core/pull/208).

- [x] Variables `FEATURE_*` en `.env` / `.env.example` y Azure
- [x] Flags en config del backend al arrancar
- [x] `GET /config` público
- [x] Endpoints avanzados → 404 si el flag está off
- [x] Store `config` en frontend
- [x] UI avanzada oculta
- [x] Mapa pre-launch: Core on / avanzadas off

Detalle: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]].

## Admin Panel (paso 3) — pendiente = Sprint 2

1. Guard super-admin + endpoints admin
2. Ruta `/admin` protegida
3. UI de KPIs + flags (lectura) + status
4. Verificar en producción

Decisiones: [[02 - Core/ADMIN_PANEL]]. Fuera de alcance y orden: [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]].

En `core/` hoy: **cero** rutas admin de producto.

## Limpieza extra (no es Sprint 2)

La landing todavía habla de fondos comunes / tesorería / DeFi (`hero-one.svelte`) con esas flags en false. Eso es UX Core, no Admin.
