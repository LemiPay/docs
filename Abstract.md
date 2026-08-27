<div align="center">
  <img src="./images/lemipay-logo.png" width="40" height="40" alt="lemipay">
</div>

# LemiPay: Abstract

LemiPay es una aplicación web de gastos compartidos (tipo Splitwise) con un camino hacia tesorerías grupales on-chain. El código vive en `core/`: frontend SvelteKit (`core/client`) y API Rust/Axum (`core/server`), con PostgreSQL.

El producto **día 1** no es una dApp completa. Es el Core de división de gastos, con registro/login, grupos, amigos, expenses, balances y notificaciones. Tesorería, rondas de fondeo, inversiones, gobernanza de retiros, wallets on-chain, sync blockchain y chat de IA **existen en el código** y están apagados con feature flags (`GET /config`).

App live: https://lemipay.app/

## Qué está encendido (Core)

- Registro y login con email/contraseña, y login con wallet (Reown AppKit + challenge/verify en `/auth`).
- Perfil, amigos, grupos. Invitaciones: hoy en código son propuestas new-member a usuarios LemiPay; el producto día 1 acordado es asientos + link abierto / nominada ([[02 - Core/MIEMBROS_E_INVITACIONES]], aún no implementado).
- Gastos (crear, dividir, editar/eliminar) y balances / settlements / claim.
- Notificaciones in-app y emails básicos.
- Permisos por rol en el grupo y resolución de deudas.

## Qué existe en código pero está apagado

- Tesorerías / wallets de grupo, transacciones de fondo común.
- Rondas de fondeo, propuestas de retiro, inversiones (estrategias, pulse, leverage).
- Eventos on-chain y live sync del vault (`LemiPayVault` vía alloy).
- Wallets on-chain en perfil (`/wallet`).
- Chat de IA (`/ai`).

Cambio de flags = deploy del backend. Detalle: [[02 - Core/SCOPE_DEL_CORE]] y [[02 - Core/FEATURE_FLAGS]].

## Qué no está implementado

- On-ramps (Mercado Pago u otros): roadmap, no hay código de pasarela.
- Panel `/admin` de super-admin: hecho (epic [#209](https://github.com/LemiPay/core/issues/209)).
- Account Abstraction como producto (ERC-4337 “full”): el login web3 verifica firmas EOA y ERC-6492; no hay smart accounts propias ni abstracción de gas como feature de usuario.

## Visión (no es el estado actual)

Tesorerías grupales on-chain, gobernanza de fondos, DeFi sobre el capital ocioso y entrada con moneda local. Eso es el roadmap largo, no el lanzamiento. Ver [[01 - Roadmap/PRE_LAUNCH_ROADMAP]].

---

<p align="center"> ⚡ by <b>LemiPay</b> </p>
