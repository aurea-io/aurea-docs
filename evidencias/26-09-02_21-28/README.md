# 📋 Sesión de Evidencias: Formalización de Bounded Context en Carpetas (1 Carpeta = 1 Negocio)

**Fecha:** 2026-09-02 21:28  
**Autor:** Antigravity Agent & Tech Lead  
**Estado:** 🟢 APROBADO (Normativa formalizada en Docs y trazada en Backlog)

---

## 1. Contexto y Desvío Detectado

Durante la auditoría de controladores en `backoffice-be-aurea`, se identificaron carpetas paraguas (`src/tenant/sections/restaurant/`) que agrupaban múltiples negocios disjuntos (`tables`, `orders`, `kitchen`), generando:
1. Mezcla de subdominios comerciales con distintos ciclos de vida y perfiles de usuario.
2. Colisión semántica entre las reservas de mesa de salón gastronómico y el módulo de turnos/citas de servicios (`services.bookings`).
3. Servicios y DTOs monolíticos con funciones compactadas en una sola línea.

---

## 2. Decisiones y Reglas Formalizadas

1. **Regla 7 en `technical.md`:** Principio de Bounded Context en Carpetas: 1 Carpeta = 1 Negocio / Subdominio.
2. **Prohibición de carpetas paraguas:** Queda prohibido el uso de carpetas comodín como `restaurant/`.
3. **Jerarquía canónica unificada:**
   * `gastronomy/tables/` (`tables`)
   * `gastronomy/orders/` (`orders`)
   * `gastronomy/kitchen/` (`kitchen`)
   * `services/bookings/` (`bookings`)
   * `commerce/catalog/` (`catalog`)
   * `commerce/inventory/` (`inventory`)
   * `commerce/pos/` (`pos_cashier`)
   * `crm/clients/` (`clients`)
4. **Distinción obligatoria entre turnos de servicios y reservas de salón:**
   * `services.bookings` es exclusivo de turnos/citas de servicios (salud, estética, consultorios).
   * Las reservas de mesa son gestionadas bajo el subdominio de salón (`tables`).

---

## 3. Trazabilidad en GitHub y Project 2

* **Backend Issue:** [backoffice-be-aurea#107](https://github.com/aurea-io/backoffice-be-aurea/issues/107) - `refactor(architecture): reestructurar carpetas de tenant por seccion y negocio unico (gastronomy, services, commerce)`
* **Frontend Issue:** [backoffice-fe-aurea#65](https://github.com/aurea-io/backoffice-fe-aurea/issues/65) - `refactor(architecture): organizar paginas y secciones por seccion y negocio unico (gastronomy, services, commerce)`
* **Backlog:** Ambos issues vinculados a [Aurea Project 2](https://github.com/orgs/aurea-io/projects/2).
