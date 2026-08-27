---
title: Scope del Core - LemiPay
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - core
  - scope
  - pre-launch
---

# Scope del Core (Lanzamiento inicial)

## Objetivo

Experiencia tipo Splitwise, limpia, solo con lo esencial. El resto queda detrás de feature flags y se habilita después. Criterio: si agrega fricción o no es esencial para dividir gastos → apagado.

Flags: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]]. Admin operativo: [[02 - Core/ADMIN_PANEL]].

## Encendido (Core)

Estas capacidades están on. Los flags `FEATURE_GROUPS` / `EXPENSES` / `BALANCES` / `FRIENDS` / `NOTIFICATIONS` se exponen en `GET /config` en `true`. Hoy **no** 404-ean esas APIs ni ocultan la UI del Core.

El login con wallet (Reown) también es Core: **no** usa `FEATURE_WALLETS_ONCHAIN`.

### Usuarios
- [x] Registro / Login (email + contraseña)
- [x] Login con wallet (challenge / verify)
- [x] Perfil básico (`/profile/me`, `/users/[id]`)
- [x] Amigos (solicitar / aceptar / rechazar / listar / bloquear / eliminar / buscar)

### Grupos
- [x] Crear grupo
- [x] Editar nombre y descripción
- [x] Invitar miembros (propuestas new-member: son Core, no van con `governance_advanced`)
- [x] Ver miembros
- [x] Salir de un grupo
- [x] Roles básicos (admin / miembro) + promover admin
- [x] Permisos por rol (`group_permission`)
- [x] Eliminar grupo
- [x] Resolución de deudas (`DebtResolution`)
- [x] Status de grupo: `Active` / `Ended` / `DebtResolution`
- [x] Status de miembro: `Active` / `Banned` / `Left`

### Gastos
- [x] Crear gasto
- [x] Dividir entre miembros (igual o montos custom)
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
- [x] Emails básicos (registro / login)

### Otros Core
- [x] Página `/status` (health del API)
- [x] Dashboard: home, actividad, grupos, amigos, gobernanza (invitaciones), settings

## Apagado (flags avanzados en `false`)

Código intacto. UI oculta + endpoints 404. Cambio = deploy.

| Flag | Qué apaga |
| --- | --- |
| `FEATURE_TREASURY` | Fondo común / wallets de grupo / tab Billetera / `/dashboard/treasury` / `/group-wallet` / `/transaction` |
| `FEATURE_FUND_ROUNDS` | Rondas de fondeo (rutas `/governance/fund-round/...` y tab del grupo) |
| `FEATURE_INVESTMENTS` | Inversiones / DeFi (`/investment`, `/groups/.../investments`). El pulse en background **sigue** |
| `FEATURE_GOVERNANCE_ADVANCED` | Propuestas de retiro. Invitaciones a miembros **siguen** |
| `FEATURE_BLOCKCHAIN` | Eventos on-chain (`/blockchain-event`) + live sync del vault. No arranca el poller |
| `FEATURE_WALLETS_ONCHAIN` | Wallets on-chain del usuario en perfil (API `/wallet`). No el login web3 |
| `FEATURE_AI_CHAT` | Chat de IA (`/ai`, ChatAssistant) |

No hay fondo común en el día 1. No hay on-ramp.

## Admin Panel (operativo, no es Core de usuario)

No es una feature de usuario final. No tiene flag. Entra en el pre-lanzamiento para operar el producto.

- [ ] Ruta `/admin` (solo super-admins hardcodeados)
- [ ] Overview con KPIs v1
- [ ] Feature flags en solo lectura
- [ ] System status básico (backend + DB)

Sprint: [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]].
