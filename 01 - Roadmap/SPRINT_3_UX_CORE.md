---
title: Sprint 3 - UX Core
created: 2026-08-27
updated: 2026-08-27
status: active
tags:
  - sprint
  - ux
  - pre-launch
  - core
---

# Sprint 3 — UX Core

## Objetivo

Core tipo Splitwise: una sola cuenta LemiPay arma el grupo, el resto entra por un **link con secreto** (sin password), con un nombre único en ese grupo. Recién si quieren el resto de la app, agregan email. Mobile-first, una CTA, copy sin jerga.

Decisiones de producto: [[02 - Core/MIEMBROS_E_INVITACIONES]]. Principios: [[05 - UX-UI/PRINCIPIOS_DE_DISENO]]. Scope: [[02 - Core/SCOPE_DEL_CORE]]. Proceso: [[00 - Meta/PROCESO_DE_TRABAJO_CON_AGENTES]].

Epic: [#215](https://github.com/LemiPay/core/issues/215).

## Estado en `core/` (hoy)

El invite actual es a **usuarios LemiPay** (propuestas new-member). No hay asiento sin `user_id`. No hay link abierto ni invitación nominada como autorización. Quien no tiene cuenta no entra al grupo.

La landing todavía habla de tesorería / DeFi con esas flags en false. El sidebar sigue mostrando “Gobernanza”.

Admin Panel ya cerró ([#209](https://github.com/LemiPay/core/issues/209) / PR [#214](https://github.com/LemiPay/core/pull/214)). No reabrir.

Este modelo de miembros **no está en código**.

## Fuera de alcance

- Admin Panel (cerrado).
- On-ramp, blockchain, investments, AI.
- Auditoría de seguridad.
- Soft launch / usuarios reales.
- Password de miembro. Usuario global solo con nombre.
- Expiración automática del link, PIN extra, confirmación a dos puntas al saldar, reuso de `display_name` al irse (abierto en [[02 - Core/MIEMBROS_E_INVITACIONES]]).
- Leftovers [#57](https://github.com/LemiPay/core/issues/57) y [#59](https://github.com/LemiPay/core/issues/59): no son este modelo. No bloquear UX Core.
- Abrir issues: **ya existen, no crear más.**

## Orden de trabajo sugerido

Sin código en este vault. Una story a la vez, con plan aprobado. Orden:

1. **[#224](https://github.com/LemiPay/core/issues/224) Backend — asientos + dos tipos de link** (primero)
   - Asiento: `display_name` único por `group_id`, `user_id` nullable, sin password, con balance.
   - Link abierto (regenerable) e invitación nominada (anulable).
   - Resolver token, sesión de miembro, claim email.
   - Gastos/balances aceptan asientos sin `user_id`.
2. **[#216](https://github.com/LemiPay/core/issues/216) Copy y landing** y **[#217](https://github.com/LemiPay/core/issues/217) Nav: asiento sin email solo ve ese grupo**
   - Landing: “invitar con un link”; amigos no necesitan cuenta. Cero jerga blockchain.
   - Sin email: solo ese grupo. El resto de LemiPay (amigos, crear grupos, perfil) pide claim.
3. **[#218](https://github.com/LemiPay/core/issues/218) UI link abierto vs nominada**
   - Admin crea/regenera abierto; crea/anula nominada.
   - Quien abre el URL: elige nombre (abierto) o entra sentado (nominada). Nombre tomado = error, no pisar.
4. **[#219](https://github.com/LemiPay/core/issues/219) Cargar gasto entre asientos**
   - Splitwise: alguien pagó, split igual o custom. Miembro sentado puede cargar.
5. **[#220](https://github.com/LemiPay/core/issues/220) Deudas / saldar**
6. **[#221](https://github.com/LemiPay/core/issues/221) Notif / emails solo si hay email**
7. **[#222](https://github.com/LemiPay/core/issues/222) Claim email + pass mobile**
8. **[#223](https://github.com/LemiPay/core/issues/223) Verificar en lemipay.app**

## Dependencias ya hechas

- Flags: epic [#197](https://github.com/LemiPay/core/issues/197) / PR [#208](https://github.com/LemiPay/core/pull/208). Feature apagada = no existe.
- Logger: epic [#203](https://github.com/LemiPay/core/issues/203) / PR [#207](https://github.com/LemiPay/core/pull/207).
- Admin Panel: epic [#209](https://github.com/LemiPay/core/issues/209) / PR [#214](https://github.com/LemiPay/core/pull/214). Cerrado.
- Issues de este sprint: ya creados en `LemiPay/core` (#215, #224, #216–#223).

## Criterio de cierre del sprint

Un user crea grupo, comparte link abierto y/o nominada, y hay N asientos sin email con balance. Sin el token no se entra. Gastos y deudas funcionan con nombres del grupo. Sin email no están el resto de LemiPay. Landing/UI/emails Core sin jerga blockchain. Feature apagada no existe. Verificado en https://lemipay.app/.
