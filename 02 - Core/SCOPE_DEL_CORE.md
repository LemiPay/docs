---
title: Scope del Core - LemiPay
created: 2026-08-24
updated: 2026-08-27
status: active
tags:
  - core
  - scope
  - pre-launch
---

# Scope del Core (Lanzamiento inicial)

## Objetivo

Experiencia tipo Splitwise, limpia, solo con lo esencial. El resto queda detrás de feature flags y se habilita después. Criterio: si agrega fricción o no es esencial para dividir gastos → apagado.

Flags: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]]. Admin operativo: [[02 - Core/ADMIN_PANEL]]. Miembros: [[02 - Core/MIEMBROS_E_INVITACIONES]].

Esta página es el **producto día 1** (decisiones). Lo que todavía no está en código se marca sin check y se implementa en UX Core ([[01 - Roadmap/SPRINT_3_UX_CORE]], epic [#215](https://github.com/LemiPay/core/issues/215)).

## Encendido (Core)

Los flags `FEATURE_GROUPS` / `EXPENSES` / `BALANCES` / `FRIENDS` / `NOTIFICATIONS` se exponen en `GET /config` en `true`. Hoy **no** 404-ean esas APIs ni ocultan la UI del Core.

El login con email/contraseña y con wallet (Reown) es del **usuario** dueño. También es Core: **no** usa `FEATURE_WALLETS_ONCHAIN`.

### Usuarios
- [x] Registro / Login (email + contraseña) — del usuario dueño del grupo
- [x] Login con wallet (challenge / verify) — del usuario dueño
- [x] Perfil básico (`/profile/me`, `/users/[id]`) — recién con email
- [x] Amigos (solicitar / aceptar / rechazar / listar / bloquear / eliminar / buscar) — recién con email

### Grupos
- [x] Crear grupo (hace falta **una** cuenta LemiPay)
- [x] Editar nombre y descripción
- Camino día 1 de invitación: **no** es propuesta new-member a usuarios que ya tienen cuenta. Es asientos + dos tipos de link. Hoy en código sigue el invite a users; se reemplaza. Detalle: [[02 - Core/MIEMBROS_E_INVITACIONES]].
  - [ ] Asientos sin `user_id` (`display_name` único por `group_id`, sin password, con balance)
  - [ ] Link abierto (token en URL; eligen nombre único; tomado = error, no pisar; admin puede regenerar)
  - [ ] Invitación nominada (el admin pone el nombre; el URL deja a la persona ya sentada; admin puede anular pendiente)
  - [ ] Claim: el asiento agrega email + password de cuenta (email nuevo crea `user`; existente se vincula; no dos veces en el mismo grupo)
- [x] Ver miembros
- [x] Salir de un grupo
- [x] Roles básicos (admin / miembro) + promover admin. Asiento sin email **no** es admin.
- [x] Permisos por rol (`group_permission`)
- [x] Eliminar grupo
- [x] Resolución de deudas (`DebtResolution`)
- [x] Status de grupo: `Active` / `Ended` / `DebtResolution`
- [x] Status de miembro: `Active` / `Banned` / `Left`

### Gastos
- [x] Crear gasto
- [x] Dividir entre miembros (igual o montos custom)
- [ ] Gastos Splitwise entre asientos (con o sin user; quien ya está sentado puede cargar)
- [x] Editar / eliminar gasto (propios; admin puede cualquiera)
- [x] Listado e historial de gastos
- [x] Status: `Created` / `Verified` / `Updated` / `Deleted`

### Balances
- [x] Cálculo automático de deudas (`/core/balances/{group_id}`)
- [x] Vista “quién le debe a quién”
- [x] Settlements: listar, pagar, claim (`/core/get-settlements`, `/core/pay-settlement`, `/core/claim`)

### Notificaciones
- [x] Notificaciones in-app
- [x] Preferencias globales y por grupo
- [x] Emails básicos (registro / login). A asientos: **solo si hay email**.

### Otros Core
- [x] Página `/status` (health del API)
- [x] Dashboard: home, actividad, grupos, amigos, settings. Asiento sin email solo ve **ese** grupo. Cero jerga (“gobernanza” no es label del camino Core).

## Apagado (flags avanzados en `false`)

Código intacto. UI oculta + endpoints 404. Cambio = deploy.

| Flag | Qué apaga |
| --- | --- |
| `FEATURE_TREASURY` | Fondo común / wallets de grupo / tab Billetera / `/dashboard/treasury` / `/group-wallet` / `/transaction` |
| `FEATURE_FUND_ROUNDS` | Rondas de fondeo (rutas `/governance/fund-round/...` y tab del grupo) |
| `FEATURE_INVESTMENTS` | Inversiones / DeFi (`/investment`, `/groups/.../investments`). El pulse en background **sigue** |
| `FEATURE_GOVERNANCE_ADVANCED` | Propuestas de retiro. Invitaciones (asientos + links) son Core, **no** van con esta flag |
| `FEATURE_BLOCKCHAIN` | Eventos on-chain (`/blockchain-event`) + live sync del vault. No arranca el poller |
| `FEATURE_WALLETS_ONCHAIN` | Wallets on-chain del usuario en perfil (API `/wallet`). No el login web3 |
| `FEATURE_AI_CHAT` | Chat de IA (`/ai`, ChatAssistant) |

No hay fondo común en el día 1. No hay on-ramp.

## Admin Panel (operativo, no es Core de usuario)

No es una feature de usuario final. No tiene flag. Cerrado: epic [#209](https://github.com/LemiPay/core/issues/209). No reabrir.

- [x] Ruta `/admin` (solo super-admins hardcodeados)
- [x] Overview con KPIs v1
- [x] Feature flags en solo lectura
- [x] System status básico (backend + DB)

Sprint: [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]] (cerrado).
