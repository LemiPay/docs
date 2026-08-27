# Stack Tecnológico

Corte contra `core/` (client + server). Versión de los manifiestos: `3.0.0`.

---

## Arquitectura

```
SvelteKit (CSR, adapter Azure SWA)
        |
        |  HTTP JSON, sin prefijo /api
        v
Axum (Rust) + Diesel + r2d2
        |
        +---- PostgreSQL 15
        |
        +---- (si FEATURE_BLOCKCHAIN) RPC + contrato vault
```

| Capa | Tecnología | Rol |
| :--- | :--- | :--- |
| **Frontend** | SvelteKit 2 + Svelte 5 | SPA (`ssr = false`), UI en `core/client` |
| **Estilos** | Tailwind 4, bits-ui, shadcn-svelte | Paleta blanco/negro + verde/violeta |
| **Wallets UI** | Reown AppKit, wagmi, viem | Login web3 (no es la flag `wallets_onchain`) |
| **Backend** | Rust, Axum 0.8, Tokio | API REST en `core/server` |
| **ORM** | Diesel 2 (Postgres, uuid, chrono, numeric) | Schema generado en `infrastructure/db/schema.rs` |
| **Pool** | r2d2 | Conexiones a Postgres |
| **Auth** | JWT (`jsonwebtoken`) + Argon2 + challenge web3 | `/auth/register`, `/login`, `/request-challenge`, `/verify-challenge` |
| **Logging** | `tracing` + `tracing-subscriber` | stdout; `LOG_FORMAT=pretty\|json`, `RUST_LOG` |
| **Chain** | alloy + erc6492 | Vault `LemiPayVault`, sync si `FEATURE_BLOCKCHAIN` |
| **IA** | Gemini (env `GEMINI_*`) | `/ai`, flag `FEATURE_AI_CHAT` |
| **Market data** | CoinGecko (oracle mock/live) | Pulse de inversiones (corre aunque la flag esté off) |
| **Mail** | Templates Askama + API de mail del client | Registro / alertas |
| **DB** | PostgreSQL 15 (Docker) | `core/docker-compose.yml` |
| **Pkg** | pnpm (client), Cargo (server) | Workspaces en la raíz de `core/` |

El API **no** usa prefijo `/api`. Orígenes públicos: `GET /health`, `GET /config`.

---

## Backend

- Framework elegido: **Axum** (no Actix).
- Capas: `domain` / `application` / `infrastructure` / `interfaces/http` / `setup`.
- Flags: struct `FeatureFlags` leída de `FEATURE_*` al boot, expuesta en `GET /config` (JSON snake_case).
- Guards avanzados: middleware → **404** (no 403) sobre `/wallet`, `/group-wallet`, `/transaction`, `/investment`, `/blockchain-event`, `/ai`, y sobre withdraw / fund-rounds dentro de `/governance`.
- New-member (invitaciones) **no** se 404-ea: es Core. El camino día 1 deja de ser invite a usuarios LemiPay; pasa a asientos + links ([[02 - Core/MIEMBROS_E_INVITACIONES]], no implementado todavía).
- `GET /health` responde `{ "status": "ok" }`. No pings a la DB.
- CORS en el router: origen de desarrollo `http://localhost:5173`.
- Bind: `0.0.0.0:${PORT}` (default 3000).
- Blockchain: env requerida `RPC_URL` + `VAULT_ADDRESS` al construir el servicio Ethereum. Live sync (poll RPC) **no** arranca si `FEATURE_BLOCKCHAIN=false`.
- Pulse de inversiones: cada 10s, **siempre**, aunque `FEATURE_INVESTMENTS=false`.

Detalle de flags: [[02 - Core/FEATURE_FLAGS_DECISIONES_DE_DISENO]]. Logger: [[03 - Architecture/LOGGER_DECISIONES_DE_DISENO]].

---

## Frontend

- SvelteKit con `adapter` **Azure SWA** (`svelte-adapter-azure-swa`).
- CSR puro: `export const ssr = false` en el layout raíz.
- Bootstrap en el layout: `authStore.init()` y `config.init()` en paralelo. Rutas públicas (`/`, `/login`, `/register`, `/status`) no esperan al backend para renderizar.
- Store `config` (`core/client/src/lib/stores/config.ts`): pide `GET /config`. Si falla: Core on, avanzadas off, `error: true`.
- Base URL: `PUBLIC_API_URL` (inlined en el bundle).
- Rutas de app: landing, login, register, status, dashboard (activity, groups, “Amigos” en `/dashboard/expenses`, governance, settings, treasury), grupo `/groups/[group_id]` (+ `/investments`), perfil `/profile/me`, usuario `/users/[user_id]`.
- **No hay** `routes/admin/`.
- Feature apagada = no existe en UI (tabs Billetera / Rondas de Fondeo, ChatAssistant, wallets de perfil, card de inversiones). Inversiones y treasury redirigen si la flag está off.

---

## Infra local y producción

- Docker Compose: `db` (postgres:15, puerto 5432), `server` (3000), `client` (5173).
- Producción: frontend https://lemipay.app/ ; API en Azure Container App (el client usa `PUBLIC_API_URL`).
- Flags de producción (mismos nombres `FEATURE_*`): Core `true`, avanzadas `false`.

---

## Modelo de datos

El ER académico viejo (UserConfig, GroupConfig, `isActive`, `authToken`, etc.) **no** es el schema. El diagrama vivo es `diagrams/EntityRelationship/diagram.puml`, generado contra `core/server/src/infrastructure/db/schema.rs`.
