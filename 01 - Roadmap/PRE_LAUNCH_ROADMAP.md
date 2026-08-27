---
title: Roadmap Pre-Launch LemiPay
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - roadmap
  - pre-launch
  - core
---
# Roadmap Pre-Launch LemiPay

Cola inmediata (corte 2026-08-26): [[01 - Roadmap/ESTADO_ACTUAL]].

## Objetivo general

Llevar la app de "proyecto de Lab 1 usable" a un producto simple, confiable y listo para primeros usuarios reales, enfocado 100% en el core de división de gastos compartidos.

## Principios

- **Core primero**: grupos + gastos + balances + amigos + notificaciones.
- **Feature flags**: nada se borra. Lo complejo se apaga por configuración y se cambia con deploy.
- **UX extrema**: más cómoda y rápida que Splitwise. Mobile-first.
- **Documentación**: este vault es contexto para humanos y agentes.
- **Ritmo**: ~8–10 h/semana + agentes. Lento pero seguro; techo ~4 meses.

## Cola inmediata (no reabre el largo plazo)

1. Admin Panel — [[01 - Roadmap/SPRINT_2_ADMIN_PANEL]]
2. UX Core
3. Seguridad
4. Usuarios reales

## Fases

### Fase 0 – Fundación (Semana 1-2)

- [x] Definir scope exacto del Core
- [x] Structured logging backend ([#203](https://github.com/LemiPay/core/issues/203))
- [x] Sistema de Feature Flags (backend + frontend) ([#197](https://github.com/LemiPay/core/issues/197))
- [x] Desactivar features de fricción (avanzadas en `false`; `core/.env.example` y `core/.env.azure`)
- [x] Crear estructura de documentación Obsidian (vault en `docs/` de este repo)
- [ ] Admin Panel v1 — Sprint 2
- [ ] Auditoría inicial de seguridad (se hace **después** de UX Core; ver cola inmediata)

### Fase 1 – Core limpio + UX (Semana 3-7)

- [ ] Simplificar flujos principales (crear grupo, agregar gasto, saldar deudas, invitar)
- [ ] Rediseño UX/UI mobile-first + desktop
- [ ] Pulir notificaciones y emails
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

Ver [[01 - Roadmap/ESTADO_ACTUAL]]. App deployada. Logging y flags mergeados. Core activo, avanzadas apagadas. Siguiente: Admin Panel.

## Documentos

- [x] Scope del Core
- [x] Feature Flags (decisiones + estado post-#208)
- [x] Logger (decisiones post-#207)
- [x] Admin Panel (decisiones v1; implementación = Sprint 2)
- [x] Principios de UX
- [ ] Checklist de Seguridad
- [ ] User Flows del Core
