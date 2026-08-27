---
title: Proceso de Trabajo con Agentes
created: 2026-08-24
updated: 2026-08-26
status: active
tags:
  - meta
  - process
  - agents
  - agile
---

# Proceso de Trabajo con Agentes

## Modelo

Un humano (Mateo) + varios agentes. Agile corto: epics y stories en GitHub, un humano aprueba planes.

## Jerarquía

- **Epic**: objetivo grande de una fase (Feature Flags, Admin Panel, UX Core).
- **Story**: unidad resoluble por un agente en un ticket.
- **Task**: desglose interno de una story, si hace falta.

## Flujo

1. Se cierran decisiones de diseño en este vault.
2. Se crea el Epic + Stories como issues en `LemiPay/core`.
3. Se asigna un agente a **una** story.
4. El agente lee el issue, lee `docs/` y `core/`, y **propone un plan**.
5. El humano aprueba el plan (sí explícito).
6. El agente implementa exactamente lo aprobado, abre/actualiza el PR si se pidió, y actualiza las docs que el cambio dejó viejas.
7. Se revisa, se mergea, se actualiza el estado del Epic.

Sin aprobación no se toca código.

## Skill de tickets

Skill: `lemipay-ticket` (`core/.grok/skills/lemipay-ticket/SKILL.md`).

Invocación: `/lemipay-ticket <ticket_id> [comentarios]` — también `ticket 204`, `issue #197`.

- `ticket_id` = número de issue en `LemiPay/core`.
- `comentarios` = instrucciones extra. No amplían el alcance del issue.
- Un epic no se implementa entero: hay que elegir la story hija.
- Si faltan criterios de aceptación o hay una decisión de diseño abierta, el agente pregunta y para.

## Reglas

- Stories chicas, con criterios de aceptación.
- Las docs de decisión de este vault no llevan código de implementación.
- El código de features apagadas no se borra: se flaggea.
- No abrir issues salvo que el humano lo pida.
- No inventar features ni KPIs que no estén decididos.

## Labels estándar

- `epic` / `story`
- `backend` / `frontend` / `docs` / `devops`
- `pre-launch` / `feature-flags` / `logging` / `admin`
- `priority: high` / `priority: medium`
