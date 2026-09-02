# 📊 Reporte de Evidencias de Review Integral: `26-09-02_20-18`

## Resumen ejecutivo

### Qué se buscaba
Ejecutar una revisión y análisis estricto de código bajo el **Playbook de Review Integral (Tolerancia Cero)** sobre todo el ecosistema Aurea disponible (`aurea-docs`, `backoffice-be-aurea`, `backoffice-fe-aurea`, `backoffice-be-aurea-internal`, `backoffice-fe-aurea-internal`, `aurea-ci` y `aurea-mongo-mcp`). Se buscaba contrastar la implementación real de controladores, servicios, rutas, modelos de datos, guards y flujos de CI/CD contra los contratos normativos de `aurea-docs`, verificando cumplimiento exacto de la arquitectura modular y aislamiento multi-tenant.

### Qué se encontró
- **Validaciones automatizadas (100% verde):**
  - `backoffice-be-aurea`: Prisma generate OK, oxlint OK (0 errores, 9 warnings de imports/vars), 13/13 suites de tests (43/43 tests) pasaron, build NestJS exitoso.
  - `backoffice-fe-aurea`: TypeScript y Vite build exitosos con generación de bundle y service worker PWA.
  - `backoffice-be-aurea-internal`: Prisma generate OK, 7/7 suites de tests (40/40 tests) pasaron, build NestJS exitoso.
  - `backoffice-fe-aurea-internal`: Oxlint sin errores ni warnings, TypeScript y Vite build exitosos (con resolución de `workbox-window`).
  - `aurea-ci`: Validaciones de cálculo de versiones y manifests aprobadas.
- **Desvíos normativos identificados (Tolerancia Cero):**
  1. **Principio de Isomorfismo Feature Key ↔ Package:** `PlatformController` y `PlatformService` en `backoffice-be-aurea-internal` agrupaban de forma monolítica tenants, planes y features bajo un mismo controlador, violando la regla de empaquetado modular por namespace de capability (`platform.tenants.*`, `platform.plans.*`, `platform.features.*`).
  2. **Taxonomía de Dominio de Negocio:** En `backoffice-be-aurea`, los módulos `appointments`, `clients`, `inventory`, `pos` y `restaurant` residen en un nivel plano sin sección agrupadora formal (`services/`, `operations/`, etc.), y `payments`, `notifications` y `coupons` están en la raíz `src/`. Además, solo `commerce/catalog` posee archivo de manifiesto tipado `<module>.manifest.ts`.
  3. **Páginas de Backoffice Cliente:** En `backoffice-fe-aurea`, 11 páginas operativas continúan en `src/tenant/pages/` en vez de su jerarquía `src/tenant/sections/<section>/<page>/` con `features.ts`.
  4. **Nomenclatura de Roles de Plataforma:** En `backoffice-be-aurea-internal`, el enum de Prisma define `platform_operator` con matching granular en vez de `platform_readonly`.
  5. **Workflows Consumidores:** Faltan los callers versionados de `aurea-ci` (`ci.yml`, `release.yml`, etc.) en `backoffice-be-aurea-internal` y `backoffice-fe-aurea-internal`.

---

## Alcance y versiones auditadas

| Repositorio | Rama / Referencia | Commit | Estado |
| :--- | :--- | :---: | :---: |
| **`aurea-docs`** | `codex/require-project-assignment` | `1654559` | Documentación actualizada con regla de isomorfismo |
| **`backoffice-be-aurea`** | `codex/platform-audit` | `e1a442f` | Limpio |
| **`backoffice-fe-aurea`** | `codex/frontend-e2e-validation` | `240320a` | Limpio |
| **`backoffice-be-aurea-internal`** | `main` | `9959c18` | Limpio |
| **`backoffice-fe-aurea-internal`** | `codex/fix-internal-pwa` | `381494d` | Limpio |
| **`aurea-ci`** | `codex/ci-gate-contract` | `dabde9b` | Limpio |
| **`aurea-mongo-mcp`** | N/A (Directorio local) | — | Sin control git |
| **`aurea-pages-template`** | N/A | — | `NO VERIFICABLE` (archivado según `docs/ci.md`) |

---

## Actualización normativa en `aurea-docs`

Se actualizó la documentación de arquitectura para formalizar la relación estricta entre capabilities y código:

1. **`docs/modules-dynamic/technical.md` (Regla obligatoria 6):**
   Establece que el namespace de la capability o feature requerida en guards o decoradores (descontando el calificador final `.read`/`.write`) define estrictamente la subcarpeta y el nombre de archivo donde debe residir la lógica (`src/platform/tenants/` para `platform.tenants.*`, etc.). Se prohíben explícitamente controladores paraguas ("god controllers") o servicios comodín que agrupen subdominios disjuntos.
2. **`docs/modules-dynamic/decisions.md`:**
   Consignada la decisión de **Correspondencia unívoca entre Feature Keys y Paquetes de Código**, estableciendo que cualquier concentración de múltiples dominios bajo un solo controlador/módulo califica como `DESVÍO`.

---

## Tareas e issues creados en GitHub

Todos los issues fueron creados y agregados al **[Project 2 — Aurea Backlog](https://github.com/orgs/aurea-io/projects/2)**:

| Repositorio | Issue | Título | Estado Project 2 |
| :--- | :--- | :--- | :---: |
| **`backoffice-be-aurea-internal`** | [#16](https://github.com/aurea-io/backoffice-be-aurea-internal/issues/16) | `refactor(platform): estructurar paquetes por subdominio de capability (platform.tenants, platform.plans, platform.features)` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPQQ`) |
| **`backoffice-fe-aurea-internal`** | [#17](https://github.com/aurea-io/backoffice-fe-aurea-internal/issues/17) | `refactor(platform): estructurar páginas y servicios según namespace de capability (platform.tenants, platform.plans, platform.features)` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPXs`) |
| **`backoffice-fe-aurea`** | [#63](https://github.com/aurea-io/backoffice-fe-aurea/issues/63) | `refactor(architecture): migrar páginas de tenant/ a estructura formal tenant/sections/<section>/<page>/ con features.ts` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPeI`) |
| **`backoffice-be-aurea`** | [#104](https://github.com/aurea-io/backoffice-be-aurea/issues/104) | `refactor(architecture): estructurar módulos restantes en tenant/sections/<section>/<page> y crear manifests` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPlg`) |
| **`aurea-docs`** | [#47](https://github.com/aurea-io/aurea-docs/issues/47) | `docs(architecture): formalizar principio de isomorfismo entre feature keys y estructura de paquetes` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPro`) |
| **`backoffice-be-aurea-internal`** | [#17](https://github.com/aurea-io/backoffice-be-aurea-internal/issues/17) | `ci(platform): incorporar workflows consumidores centralizados de aurea-ci` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LPwc`) |
| **`backoffice-fe-aurea-internal`** | [#18](https://github.com/aurea-io/backoffice-fe-aurea-internal/issues/18) | `ci(platform): incorporar workflows consumidores centralizados de aurea-ci en main` | ✅ Ítem en Project 2 (`PVTI_lADOEyMpcc4BiDWqzg5LP2U`) |

---

## Conclusión

🔴 **DESVÍO:** La suite de código es plenamente funcional y compila sin errores, pero la arquitectura documental estricta de tolerancia cero exige desacoplar los paquetes de código para mantener isomorfismo 1:1 con las capabilities, culminar la migración de taxonomía en el backoffice cliente, e integrar los workflows consumidores de CI/CD en los repositorios de plataforma interna.
