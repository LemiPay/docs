<div align="center">
  <img src="./images/lemipay-logo.png" width="40" height="40" alt="lemipay">
</div>

# Casos de Uso

Actores y capacidades **según `core/`**, con una excepción: el camino de invitar / unirse a grupo ya está decidido (2026-08-27) y **aún no está en código**. Ver [[02 - Core/MIEMBROS_E_INVITACIONES]].

Lo apagado con flags no está disponible en el día 1 aunque el código exista.

## Usuario no autenticado

- Ver landing y docs.
- Ver `/status` (ping de frontend + `GET /health`).
- Registrarse con nombre, email y contraseña (`POST /auth/register`) — hace falta **una** cuenta para **crear** un grupo.
- Iniciar sesión con email/contraseña (`POST /auth/login`).
- Iniciar sesión con wallet (Reown): `POST /auth/request-challenge` + firma + `POST /auth/verify-challenge`. Puede asociar email/nombre si la wallet no está linkeada.
- Unirse a un grupo **sin** registrarse, por link:
  - **Link abierto:** elige un `display_name` único en el grupo y se crea el asiento. Nombre tomado = error, no pisar.
  - **Invitación nominada:** entra ya sentado con el nombre que puso el admin.
- Sin el token del URL no es miembro ni ve el grupo.
- No entra al resto de LemiPay (amigos, perfil, crear grupos) hasta el claim.

Hoy en código el join todavía es invitación new-member a usuarios LemiPay.

El login con wallet es **Core**. No depende de `FEATURE_WALLETS_ONCHAIN`. Es del *usuario* dueño, no del asiento.

## Asiento de grupo (sin email)

Decisión 2026-08-27. No implementado todavía.

- Existe solo en ese grupo: `display_name` único, `user_id` null, sin password, con balance.
- Ve y usa **ese** grupo: gastos Splitwise (igual o custom), deudas, saldar. Quien ya está sentado puede cargar gastos.
- Sesión de asiento (cookie) tras el link. Otro dispositivo = hace falta el link. No hay login con password de miembro.
- No es admin del grupo.
- Claim: agrega email + password de cuenta. Email nuevo crea `user`; existente se vincula. Un user no puede estar dos veces en el mismo grupo. Recién ahí existe el resto de LemiPay.

## Usuario autenticado (Core, flags on)

- Ver y editar su perfil (`/profile/me`, `GET /user/me`). Ver otro usuario (`/users/[id]`, `GET /user/id/{id}`).
- Amigos: buscar usuarios, enviar solicitud, aceptar/rechazar, listar, bloquear, eliminar (`/friend/*`).
- Grupos: crear (hace falta una cuenta), listar los propios, ver detalle y miembros, editar nombre/descripción, salir, promover admin, borrar grupo, entrar en resolución de deudas.
- Invitar: el admin comparte **link abierto** o crea **invitación nominada**. Puede regenerar el abierto (el viejo deja de andar) y anular una nominada pendiente. Camino día 1 **no** es propuesta new-member a un user existente. Hoy en código sigue new-member; se reemplaza ([#224](https://github.com/LemiPay/core/issues/224) / [#218](https://github.com/LemiPay/core/issues/218)).
- Gastos: crear, listar, editar/eliminar los propios; el admin del grupo puede editar/eliminar cualquiera (`/expense/admin/...`). Participantes = asientos (con o sin `user`).
- Balances: ver quién debe a quién, settlements, pagar settlement, claim (`/core/balances`, `/core/get-settlements`, `/core/pay-settlement`, `/core/claim`).
- Notificaciones in-app: listar, marcar leídas, preferencias globales y por grupo. Emails básicos de auth/eventos — **solo si hay email**.
- Permisos del grupo: ver; el admin agrega/quita acciones por rol (`/permission/{group_id}`).

## Admin de grupo (además del usuario autenticado)

Permisos por defecto / configurables (`group_permission`):

- Actualizar o eliminar el grupo.
- Iniciar resolución de deudas.
- Invitar miembros (link abierto y/o nominada).
- Cancelar / anular invitaciones pendientes; regenerar el link abierto.
- Editar o eliminar cualquier gasto.
- (Apagado hoy) crear/cancelar rondas de fondeo y crear inversiones.

Asiento sin email **no** es admin.

No hay umbral de votos por grupo en DB. El quorum global de config es un entero de boot (`GovernanceConfig.quorum`), no una pantalla de settings.

## Usuario autenticado — features apagadas (código intacto)

Solo si el flag correspondiente está `true`. Hoy en pre-launch están en `false`: UI oculta y API 404.

| Flag | Casos de uso |
| --- | --- |
| `treasury` | Ver/crear wallets de grupo, fondear, tab Billetera, `/dashboard/treasury` |
| `fund_rounds` | Proponer ronda, aportar, cancelar, ver restantes |
| `investments` | Ver estrategias, proponer/ejecutar inversión, snapshots, withdraw, página `/groups/.../investments` |
| `governance_advanced` | Proponer y ejecutar retiros del fondo común |
| `blockchain` | Listar eventos on-chain; live sync del vault |
| `wallets_onchain` | Vincular/listar/fondear/retirar/transferir wallets de usuario en perfil (`/wallet`) |
| `ai_chat` | Chat assistant en el dashboard (`/ai`) |

No hay on-ramp (fiat → crypto) en el client ni en el server.

## Super-admin (operaciones)

- Abrir `/admin` y ver KPIs / flags / status.

Hecho. Epic [#209](https://github.com/LemiPay/core/issues/209). Ver [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]] (cerrado).

---

<p align="center"> ⚡ by <b>LemiPay</b> </p>
