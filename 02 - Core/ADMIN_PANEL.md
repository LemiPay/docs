---
title: Admin Panel
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - admin
  - core
  - operations
---
# Admin Panel

## Objetivo

Pantallazo operativo el día 1: simple, estable, extensible. No es feature de usuario final. No se flaggea; se protege por super-admin.

Sprint de implementación: [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]]. Flags: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]].

## Decisiones cerradas (no reabrir)

- Ruta frontend: `/admin`, layout propio (`routes/admin/`).
- Super-admins **hardcodeados en backend** (lista, no tabla `admin_users` en v1).
- Flags: **solo lectura**. Se cambian con deploy.
- KPIs v1 (nada más):
  - Usuarios totales
  - Usuarios activos últimos 7 días / 30 días
  - Grupos totales / grupos activos
  - Gastos totales / gastos del mes actual
  - Invitaciones pendientes
- System status v1: backend + DB. Básico.
- Misma paleta que el resto (blanco/negro + verde y violeta).
- Mobile-friendly, aunque se use más en desktop.
- v1 mínima y extensible. No charts, no moderación, no edición de flags.
- El API no usa prefijo `/api`.
- Contratos previstos: `GET /admin/overview` (protegido) y `GET /admin/flags` (protegido; reusa el mapa de `GET /config`).
- Reutilizar UI existente.

## No-admin

Quien no es super-admin no usa el panel. Opciones ya nombradas, **todavía no elegida una**: 404, o redirect a dashboard.

## v4+ (futuro, no Sprint 2)

- Toggle de flags (implica pasar flags a DB o similar)
- Lista de usuarios + búsqueda + ban
- Moderación de grupos
- Logs de errores recientes
- Métricas avanzadas (retención, funnel)

## Abierto (bloquea o sesga la implementación)

Anotar acá; no inventar en el ticket.

1. **Emails (o ids) exactos** de la lista hardcodeada.
2. **No-admin en frontend**: 404 vs redirect a dashboard.
3. **No-admin en API**: 401 si no hay sesión; 403 vs 404 si hay sesión y no es super-admin.
4. **Usuarios activos 7/30 días**: la tabla `user` **sigue sin** `last_login` ni `created_at` (solo `id`, `email`, `password` nullable, `name`). Hay que definir el criterio (y si hace falta un campo nuevo).
5. **Grupos activos**: existe `group.status` (`Active` / `Ended` / `DebtResolution`). ¿Eso alcanza, o es “con actividad reciente”?
6. **Invitaciones pendientes**: lo natural es propuestas new-member en `Pending`. Confirmar.
7. **Status de DB**: `GET /health` hoy solo dice que el proceso HTTP está up; no pings DB. El panel v1 necesita un chequeo de DB (puede vivir en overview, no hace falta inflar `/health` público).
8. **Última migración / versión de deploy**: opcional (“si se puede”). No es KPI v1. Si no sale barato, se omite.

## Estado

No implementado en `core/`. Flags y logger sí.
