---
title: Principios de Diseño
created: 2026-08-26
updated: 2026-08-27
status: active
tags:
  - ux
  - ui
  - core
---
# Principios de Diseño

Válidos para el Core y para el Admin Panel. El rediseño de flujos es el Sprint 3 (UX Core). Admin Panel ya cerró. Miembros: [[02 - Core/MIEMBROS_E_INVITACIONES]].

## Sensación

Simple, profesional, estable, cómoda, limpia, seria, moderna. No juguetona. No “crypto app”.

## Paleta

Blanco / negro + acentos verde y violeta (la paleta actual del client: lime + violeta en el dashboard). No inventar una marca nueva en v1.

## Mobile-first

Se diseña primero el teléfono. Desktop es el mismo producto, más aire, no otro producto. El admin también tiene que ser usable en mobile, aunque se opere más en desktop.

## Un usuario LemiPay alcanza para un grupo

Una cuenta (email) crea el grupo. El resto no se registra. Invitás con un link; amigos no necesitan cuenta.

## El link es la puerta

La autorización del miembro es el secreto en el URL. Sin el link no sos miembro ni ves el grupo. No hay password de asiento.

## Asiento ≠ cuenta

Miembro de grupo = asiento (`display_name` único en el grupo, `user_id` nullable, sin password, con balance). Recién con email existe el resto de LemiPay (otros grupos, amigos, perfil, crear grupos). Asiento sin email no es admin.

## Una acción principal por pantalla

Un CTA obvio. El resto es secundario o no está. No competir entre “crear gasto”, “fondear” e “invertir” en la misma vista.

## Pocos taps en el Core

Los tres flujos que tienen que ser cortos:

1. Crear grupo
2. Cargar un gasto
3. Ver deudas / saldar

Si un flujo pide blockchain, wallet o tesorería para completar eso, está mal para el día 1.

## Sin jerga blockchain

Ni en UI, ni en emails, ni en landing, mientras esas flags estén off. No “vault”, “on-chain”, “wallet”, “tesorería”, “ronda de fondeo”, DeFi, ni “gobernanza” como label en el camino del usuario Core.

## Feature apagada = no existe

No se muestra deshabilitada. No queda un tab gris. No hay “coming soon” si el flag está off. No se puede llegar por URL a una pantalla de feature apagada (redirect o como si no existiera).

El copy de marketing de la landing **todavía** habla de tesorería compartida, gobernanza y DeFi (`hero-one.svelte`): viola este principio. Se corrige en UX Core ([#216](https://github.com/LemiPay/core/issues/216)). El copy de landing debe decir **“invitar con un link”**.

El sidebar del dashboard sigue mostrando “Gobernanza”: esa label no queda. Invitaciones = links (abierto / nominada), no una pantalla de gobernanza.

## Admin

Misma paleta y misma seriedad. Información primero, cero edición de flags en v1. Ver [[02 - Core/ADMIN_PANEL]].
