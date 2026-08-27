---
title: Roadmap Pre-Launch LemiPay
created: 2026-08-24
updated: 2026-08-27
status: active
tags:
  - roadmap
  - pre-launch
  - core
---
# Roadmap Pre-Launch LemiPay

Cola inmediata (corte 2026-08-27): [[01 - Roadmap/ESTADO_ACTUAL]].

## Objetivo general

Llevar la app de "proyecto de Lab 1 usable" a un producto simple, confiable y listo para primeros usuarios reales, enfocado 100% en el core de división de gastos compartidos.

## Principios

- **Core primero**: grupos + gastos + balances + amigos + notificaciones.
- **Feature flags**: nada se borra. Lo complejo se apaga por configuración y se cambia con deploy.
- **UX extrema**: más cómoda y rápida que Splitwise. Mobile-first.
- **Documentación**: este vault es contexto para humanos y agentes.
- **Ritmo**: ~8–10 h/semana + agentes. Lento pero seguro; techo ~4 meses.

## Cola inmediata (no reabre el largo plazo)

1. UX Core — [[01 - Roadmap/SPRINT_3_UX_CORE]] / epic [#215](https://github.com/LemiPay/core/issues/215)
2. Seguridad
3. Usuarios reales

Admin Panel ya cerró: [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]].

## Fases

### Fase 0 – Fundación (Semana 1-2)

- [x] Definir scope exacto del Core
- [x] Structured logging backend ([#203](https://github.com/LemiPay/core/issues/203))
- [x] Sistema de Feature Flags (backend + frontend) ([#197](https://github.com/LemiPay/core/issues/197))
- [x] Desactivar features de fricción (avanzadas en `false`; `core/.env.example` y `core/.env.azure`)
- [x] Crear estructura de documentación Obsidian (vault en `docs/` de este repo)
- [x] Admin Panel v1 — Sprint 2 ([#209](https://github.com/LemiPay/core/issues/209))
- [ ] Auditoría inicial de seguridad (se hace **después** de UX Core; ver cola inmediata)

### Fase 1 – Core limpio + UX (Semana 3-7)

- [ ] Modelo de miembros: asientos sin user, link abierto + nominada, claim con email. **No** es “invitar amigos que ya tienen cuenta”. Ver [[02 - Core/MIEMBROS_E_INVITACIONES]]
- [ ] Simplificar flujos principales (crear grupo, cargar gasto, saldar deudas, invitar con un link)
- [ ] Rediseño UX/UI mobile-first + desktop
- [ ] Pulir notificaciones y emails (solo si hay email)
- [ ] Testing intensivo del core
- [ ] Soft launch interno + feedback de 5-10 personas cercanas

### Fase 2 – Primeros usuarios + Seguridad (Semana 8-11)

- [ ] Mejoras basadas en feedback real
- [ ] Auditoría de seguridad profunda (agentes AI + revisión manual)
- [ ] Onboarding ultra simple
- [ ] Empezar a buscar primeros usuarios reales (amigos, comunidades, etc.)

### Fase 3 – On-ramp custodial (Semana 12-14)

- [ ] Integración Mercado Pago / Mercado Libre (depósito de fondos)
- [ ] Modelo custodial simple y transparente
- [ ] Flujos de depósito y retiro básicos

Hoy **no hay** código de on-ramp en `core/`.

### Fase 4 – Preparación Blockchain (Semana 15-16)

- [ ] Reactivar y pulir entrada blockchain con la mejor UX posible
- [ ] Asegurar que todo el core funcione perfectamente con y sin blockchain
- [ ] Documentar el camino hacia non-custodial (smart contracts)

El largo plazo no se reabre: custodial Mercado Pago → blockchain UX → non-custodial.

## Estado actual

Ver [[01 - Roadmap/ESTADO_ACTUAL]]. App deployada. Logging, flags y Admin Panel mergeados. Core activo, avanzadas apagadas. Siguiente: UX Core (asientos + links; el modelo **no** está en código todavía).

## Documentos

- [x] Scope del Core
- [x] Feature Flags (decisiones + estado post-#208)
- [x] Logger (decisiones post-#207)
- [x] Admin Panel (decisiones v1; implementación cerrada)
- [x] Miembros e invitaciones (decisiones 2026-08-27; implementación = Sprint 3)
- [x] Principios de UX
- [ ] Checklist de Seguridad
- [ ] User Flows del Core
