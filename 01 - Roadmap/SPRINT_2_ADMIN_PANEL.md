---
title: Sprint 2 - Admin Panel
created: 2026-08-26
updated: 2026-08-26
status: active
tags:
  - sprint
  - admin
  - pre-launch
---

# Sprint 2 — Admin Panel

## Objetivo

Tener un panel mínimo, protegido, usable en producción, para ver si el Core está vivo: KPIs v1, flags en solo lectura y status básico de backend + DB.

Es parte operativa del pre-lanzamiento. No es una feature de usuario final. No se flaggea: se protege por super-admin.

Decisiones de producto: [[02 - Core/ADMIN_PANEL]]. Proceso: [[00 - Meta/PROCESO_DE_TRABAJO_CON_AGENTES]].

## Estado en `core/` (hoy)

No hay `core/client/src/routes/admin/`. No hay nest `/admin` en el router de Axum. Lo que existe con “admin” en el nombre es **admin de grupo** (editar gastos, `is_group_admin_middleware`), no super-admin de producto.

## Fuera de alcance

- Editar flags desde la UI (siguen cambiándose con deploy).
- Tabla `admin_users`, roles de admin en DB, o lista editable.
- Lista de usuarios, búsqueda, ban, moderación de grupos.
- Logs de errores en el panel.
- KPIs extra (retención, funnel, charts).
- Rediseño UX del Core.
- Auditoría de seguridad.
- Fondo común / blockchain / investments / AI.
- Abrir issues: el humano los crea si hace falta.

## Orden de trabajo sugerido

Sin código. Una story a la vez, con plan aprobado.

1. **Guard super-admin + endpoints admin**
   - Lista hardcodeada de super-admins en backend.
   - `GET /admin/overview` (KPIs v1) y `GET /admin/flags` (el mapa de `GET /config`), ambos protegidos.
2. **Ruta `/admin` protegida**
   - Layout propio bajo `routes/admin/`.
   - Quien no es super-admin no usa el panel (404 o redirect a dashboard: decisión abierta, ver [[02 - Core/ADMIN_PANEL]]).
3. **UI de KPIs + flags + status**
   - Cards de KPIs v1, lista de flags solo lectura, status backend + DB.
   - Misma paleta. Mobile-friendly. Mínima y extensible.
4. **Verificación en producción**
   - Super-admin ve el panel en https://lemipay.app/admin.
   - Usuario normal no entra.
   - Flags coinciden con `GET /config`.
   - KPIs coherentes con la DB real (hoy casi vacía).

## Dependencias ya hechas

- Flags: epic [#197](https://github.com/LemiPay/core/issues/197) / PR [#208](https://github.com/LemiPay/core/pull/208). `GET /config` existe.
- Logger: epic [#203](https://github.com/LemiPay/core/issues/203) / PR [#207](https://github.com/LemiPay/core/pull/207).
- `GET /health` existe y solo dice que el proceso HTTP está up. No prueba la DB.
- Tabla `user` no tiene `last_login` ni `created_at` (bloquea el KPI “usuarios activos” hasta definir criterio).

## Criterio de cierre del sprint

Un super-admin puede abrir `/admin` en producción y ver KPIs v1 + flags + status. Un no-admin no. Flags no se editan desde ahí.
