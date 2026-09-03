# 📋 Sesión de Evidencias: Formalización de Jerarquía en 3 Niveles (Sección → Página → Módulo) e Isomorfismo FBAC + RBAC en el Playbook de Tolerancia Cero

**Fecha:** 2026-09-02 23:40  
**Autor:** Antigravity Agent & Tech Lead  
**Estado:** 🟢 APROBADO (Normativa formalizada en Docs y blindada en el Playbook de Evidencias)

---

## 1. Contexto y Objetivos de la Sesión

Formalizar de manera taxativa e inquebrantable en la documentación normativa (`aurea-docs`) y en el **Playbook de Review Integral Asistida por IA (Tolerancia Cero)** los tres pilares de arquitectura de producto y acceso:

1. **Jerarquía en 3 Niveles:**
   * `SECCIÓN` (Área departamental macro: `services`, `commerce`, `gastronomy`, `crm`, `marketing`, `core`).
   * `PÁGINA` (Pantalla de la UI y carpeta física en `src/tenant/sections/<sección>/<página>/`).
   * `MÓDULO` (Funciones, widgets y capacidades granulares dentro de la página que se activan o desactivan dinámicamente).
2. **Reubicación de `orders` en `commerce`:**
   * Las órdenes representan la transacción universal de compraventa de cualquier comercio (mostrador, delivery o salón presencial), mientras que `gastronomy` se enfoca en salón (`tables`) y cocina (`kitchen`).
3. **Principio de Isomorfismo Unificado (FBAC + RBAC):**
   * El namespace `<sección>.<página>.<módulo>` (ej. `commerce.orders.takeaway`) define simultáneamente la feature comercial del plan para la empresa, el rol/permiso del empleado (`:read`/`:write`) y la carpeta física en el proyecto.
4. **Blindaje en el Playbook de Evidencias:**
   * Establecer como regla obligatoria de auditoría que cualquier desvío de jerarquía, carpeta paraguas o permiso desalineado sea calificado automáticamente como **🔴 DESVÍO CRÍTICO**, impidiendo la aprobación de cualquier sesión de QA.

---

## 2. Modificaciones Documentales Realizadas

1. **`docs/modules-dynamic/technical.md`:**
   * Actualizada la Regla 7 con la definición formal de los 3 niveles y el principio de isomorfismo unificado.
   * Incorporada la tabla canónica con ejemplos de módulos dinámicos (`create`, `add_to_cart`, `split_bill`, `qr_generator`, etc.).
   * Documentada la asignación de `orders` a `commerce/orders/`.
2. **`docs/modules-dynamic/decisions.md`:**
   * Registrada la decisión arquitectónica formal de la jerarquía Sección → Página → Módulo y el doble filtro de autorización (`FeatureGuard` + `PermissionsGuard`).
3. **`evidencias/README.md`:**
   * Incorporada la subsección obligatoria **"Regla Inquebrantable de Isomorfismo y Jerarquía (Sección → Página → Módulo)"** dentro del Playbook de Tolerancia Cero.

---

## 3. Trazabilidad en GitHub y Project 2

* **Pull Request:** Integrado y validado con checks de Governance (`area:docs`, `type:docs`) y CI.
* **Aurea Project 2:** Vinculado al backlog institucional de la organización.
