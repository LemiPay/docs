---
title: Miembros e Invitaciones
created: 2026-08-27
updated: 2026-08-27
status: active
tags:
  - design-decision
  - core
  - ux
  - members
---
# Miembros e Invitaciones

## Objetivo

Un solo usuario LemiPay arma el grupo. El resto entra por un **link con secreto**, sin registrarse, con un nombre único en ese grupo. Recién si quieren el resto de la app, agregan email.

Acordado 2026-08-27. **Aún no está en `core/`.** El invite actual (propuesta new-member a usuarios LemiPay) se reemplaza.

Epic: [#215](https://github.com/LemiPay/core/issues/215). Backend primero: [#224](https://github.com/LemiPay/core/issues/224). Sprint: [[01 - Roadmap/SPRINT_3_UX_CORE]]. Scope: [[02 - Core/SCOPE_DEL_CORE]]. Principios: [[05 - UX-UI/PRINCIPIOS_DE_DISENO]].

## Decisiones cerradas (no reabrir)

- **Una cuenta LemiPay (email) crea el grupo.** El resto no necesita registrarse.
- **Miembro de grupo = asiento:**
  - `display_name` único por `group_id`
  - `user_id` nullable
  - **sin password**
  - tiene balance y participa en gastos tipo Splitwise
  - no existe fuera de ese grupo hasta que agrega email
- **Autorización = secreto en el URL** (token largo, no adivinable). Sin el link no sos miembro ni ves el grupo.
- El admin del grupo tiene **dos invitaciones**:
  1. **Link abierto:** quien tiene el URL elige un nombre único en el grupo y se crea el asiento. Nombre tomado = error, no pisar. Admin puede regenerar (el link viejo deja de andar).
  2. **Invitación nominada:** el admin indica el nombre; el URL deja a la persona ya sentada con ese nombre. Admin puede anular una nominada pendiente.
- **Sesión de asiento** (cookie) tras entrar por el link. Otro dispositivo = hace falta el link. No hay login con password de miembro.
- **Claim:** el asiento agrega email + password de cuenta.
  - Email nuevo → se crea `user` y se vincula.
  - Email existente → se vincula ese `user`.
  - Un `user` no puede estar dos veces en el mismo grupo.
  - Recién con email existe el resto de LemiPay (otros grupos, amigos, perfil, crear grupos).
- **Gastos = Splitwise entre asientos** (con o sin `user`): alguien pagó, split igual o custom. Quien ya es miembro sentado puede cargar gastos.
- Asiento **sin email no es admin** de grupo.
- **Copy:** invitás con un link; amigos no necesitan cuenta.
- **Cero jerga blockchain** en el camino Core (vault, on-chain, tesorería, ronda de fondeo, DeFi, “gobernanza” como label).
- Feature flag off = no existe.
- Admin Panel ([#209](https://github.com/LemiPay/core/issues/209)) ya está cerrado. No reabrir.

## Contratos HTTP previstos (alto nivel)

Sin prefijo `/api`. Las rutas exactas las propone [#224](https://github.com/LemiPay/core/issues/224); acá van las operaciones y campos conceptuales.

**Admin del grupo** (sesión del *usuario* LemiPay admin):

- Crear link abierto → URL con token.
- Regenerar link abierto → el token anterior deja de valer.
- Crear invitación nominada (`display_name`) → URL atado a ese asiento.
- Anular nominada pendiente.
- Listar invitaciones del grupo.

**Público con token** (el secreto del URL):

- Resolver el token: tipo (abierto vs nominada), grupo, si hay que elegir nombre o ya está sentado.
- Link abierto: claim de `display_name` (único; tomado = error, no pisar) → crea asiento + sesión de miembro.
- Nominada: entra ya sentado → sesión de miembro.

**Sesión de asiento:**

- Cookie de miembro de grupo, no de usuario app.
- Alcance: ese grupo (ver, balances, cargar gastos).
- Otro dispositivo = hace falta el link de nuevo.

**Claim** (asiento con sesión):

- Email + password de cuenta.
- Email nuevo → se crea `user` y se setea `user_id`.
- Email existente → se vincula ese `user` (mismo grupo, no duplicar miembro).

**Gastos / balances:** participantes = asientos (`user_id` puede ser null).

## Fuera de alcance

- Password de miembro. Usuario global solo con nombre. No crear filas en `user` sin email.
- Leftovers [#57](https://github.com/LemiPay/core/issues/57) y [#59](https://github.com/LemiPay/core/issues/59): no son este modelo (invite a users LemiPay / promover). No bloquear UX Core.
- Tablas Diesel concretas / migraciones: eso es [#224](https://github.com/LemiPay/core/issues/224). No hay ER inventado en este vault; el diagrama actual es el schema de hoy, con una nota.
- Admin Panel, on-ramp, blockchain, investments, AI, auditoría de seguridad, soft launch.

## Abierto (no inventar)

Anotar acá; no cerrar en el ticket.

1. **Expiración automática** del link (además de revocar / regenerar).
2. **PIN extra** además del token.
3. **Saldar:** confirmación a dos puntas vs un solo toque.
4. **Nombre al irse:** qué pasa con el `display_name` si el asiento se va y otro quiere el mismo.

## Estado

No implementado en `core/`. Hoy el invite es propuesta new-member a usuarios LemiPay.

Stories ya creadas (citar, no duplicar, no editar): epic [#215](https://github.com/LemiPay/core/issues/215), [#224](https://github.com/LemiPay/core/issues/224), [#216](https://github.com/LemiPay/core/issues/216)–[#223](https://github.com/LemiPay/core/issues/223).
