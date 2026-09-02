# 📊 Reporte de Evidencias de Auditoría Arquitectónica: `26-09-02_01-25`

**Fecha de ejecución:** 2 de Septiembre de 2026, 01:25 hs  
**Alcance de la Auditoría:** Arquitectura de Módulos Dinámicos, Taxonomía de Dominio, Separación de Scopes por Rol y Mantenibilidad.  
**Estado General:** 🟡 **AUDITORÍA COMPLETADA · PLAN DE REFACTORIZACIÓN DEFINIDO**

---

## 📦 Versiones de Productos Auditados

| Componente | Repositorio / Proyecto | Versión / Tag | Commit en `main` |
| :--- | :--- | :---: | :---: |
| **BE Aurea** | `backoffice-be-aurea` | `v0.18.0` | `d448787` |
| **FE Aurea** | `backoffice-fe-aurea` | `v0.1.2` | `e1800ea` |
| **BE Cliente** | N/A (Integrado en API central) | — | — |
| **FE Cliente** | `aurea-pages-template` | `v0.4.0` | `9c02589` |

---

## 🏗️ 1. Diagnóstico Arquitectónico

### A. Taxonomía de Dominio (`Section → Page → Feature → Module`)
* **Especificación:** Según `docs/modules-dynamic/technical.md`, toda capacidad debe respetar la jerarquía formal:
  * **Section** (ej. `services`, `commerce`, `gastronomy`)
  * **Page** (ej. `services.bookings`, `commerce.catalog`)
  * **Feature** (ej. `services.bookings.create`, `services.bookings.photo_upload`)
* **Situación Actual:** Los módulos están en una estructura plana en la raíz (`src/catalog/`, `src/tenant/`, `src/features/catalog/`), lo que dificulta la incorporación limpia de nuevas verticales sin sobrecargar la raíz del proyecto.

### B. Separación de Scopes por Rol (`platform/*` vs `tenant/*`)
* **Especificación:** 
  * **Platform Scope (`platform_owner`, `platform_readonly`):** Gestión global de plataforma (planes, precios, mantenimiento de módulos, altas/bajas de comercios). No requiere `x-tenant-id`.
  * **Tenant Scope (`tenant_owner`, `tenant_manager`, `tenant_staff`, `tenant_cashier`):** Gestión operativa del comercio activo. Requiere obligatoriamente `x-tenant-id`.
* **Situación Actual:** Existe mezcla de rutas y componentes entre el panel global de SuperAdmin y el panel de comercio dentro de las mismas carpetas.

---

## 📐 2. Estándar de Organización de Carpetas Propuesto

### Backend (`backoffice-be-aurea/src/`)
```text
src/
├── core/                               # Transversal (Guards, Decorators, Prisma, Evaluator)
├── platform/                           # Scope Plataforma (Superadmin Aurea: tenants, plans, audit)
└── tenant/                             # Scope Comercio
    ├── core/                           # Ajustes base (branding, members, invitations)
    └── sections/                       # Módulos por sección
        ├── services/                   # Sección: Servicios
        │   └── bookings/               # Módulo: Turnos (services.bookings)
        └── commerce/                   # Sección: Comercio
            └── catalog/                # Módulo: Catálogo (commerce.catalog)
```

### Frontend (`backoffice-fe-aurea/src/`)
```text
src/
├── core/                               # API client, Stores, Hooks, UI components
├── platform/                           # Backoffice AUREA (Superadmin Layout, Tenants, Features)
└── tenant/                             # Backoffice del Comercio
    ├── layout/                         # Tenant Layout (Sidebar dinámico, Switcher)
    ├── pages/                          # Dashboard, Members, Settings
    └── sections/                       # Páginas funcionales por sección
        ├── services/bookings/          # BookingsPage, features.ts
        └── commerce/catalog/           # CatalogPage, features.ts
```

---

## 📌 3. Tareas e Issues Creados en GitHub

Para llevar a cabo este refactor arquitectónico se abrieron las siguientes tareas:

| Repositorio | Issue | Título |
| :--- | :--- | :--- |
| **`backoffice-be-aurea`** | [#85](https://github.com/aurea-io/backoffice-be-aurea/issues/85) | `refactor(architecture): estructurar módulos por scope (platform vs tenant) y secciones de dominio (Section/Page/Feature)` |
| **`backoffice-fe-aurea`** | [#50](https://github.com/aurea-io/backoffice-fe-aurea/issues/50) | `refactor(architecture): desacoplar código en platform/ y tenant/sections/<section>/<page>` |
| **`aurea-docs`** | [#22](https://github.com/aurea-io/aurea-docs/issues/22) | `docs(architecture): formalizar estándar de carpetas y taxonomía de módulos para Backend y Frontend` |
