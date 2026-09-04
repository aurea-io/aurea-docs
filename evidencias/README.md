# 📸 Registro de Evidencias de Pruebas y Validación (QA)

Este directorio almacena el historial cronológico y versionado de las ejecuciones de prueba, validaciones visuales en navegador (Google Chrome), suites de integración y auditorías de código del ecosistema **Aurea**.

---

## 🗂️ Historial de Ejecuciones

| Sesión (Fecha/Hora) | BE Aurea (`backoffice-be`) | FE Aurea (`backoffice-fe`) | BE Cliente | FE Cliente (`pages-template`) | Estado | Reporte |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| [`26-09-03_01-30`](./26-09-03_01-30_modules-catalog/README.md) | `backoffice-be-aurea-internal @ main` | `backoffice-fe-aurea-internal @ main` | N/A | N/A | 🟢 APROBADO | Validación E2E del Catálogo de Módulos (PLT-07): Árbol jerárquico canónico (3 niveles), búsqueda/filtros, editor de dependencias/permisos y alta de nuevas features. |
| [`26-09-02_23-40`](./26-09-02_23-40/README.md) | `backoffice-be-aurea-internal @ main` | `backoffice-fe-aurea-internal @ main` | `backoffice-be-aurea @ codex/platform-audit` | `backoffice-fe-aurea @ codex/frontend-e2e-validation` | 🟢 APROBADO | Formalización de Jerarquía en 3 Niveles (Sección → Página → Módulo), inclusión de `orders` en `commerce`, e Isomorfismo FBAC+RBAC blindado en el Playbook de Evidencias con Tolerancia Cero. |
| [`26-09-02_21-28`](./26-09-02_21-28/README.md) | `backoffice-be-aurea-internal @ main` | `backoffice-fe-aurea-internal @ main` | `backoffice-be-aurea @ codex/platform-audit` | `backoffice-fe-aurea @ codex/frontend-e2e-validation` | 🟢 APROBADO | Formalización de Bounded Context en Carpetas (1 Carpeta = 1 Negocio), prohibición de carpetas paraguas y creación de issues #107 y #65 en Project 2. |
| [`26-09-02_20-18`](./26-09-02_20-18/README.md) | `backoffice-be-aurea-internal @ main @ 9959c18` | `backoffice-fe-aurea-internal @ codex/fix-internal-pwa @ 381494d` | `backoffice-be-aurea @ codex/platform-audit @ e1a442f` | `backoffice-fe-aurea @ codex/frontend-e2e-validation @ 240320a` | 🔴 DESVÍO | Auditoría de código integral: formalización de regla de isomorfismo Feature Key ↔ Package, verificación de builds/tests y creación de 7 issues en Project 2. |
| [`26-09-02_19-27_internal-platform-production`](./26-09-02_19-27_internal-platform-production/README.md) | `backoffice-be-aurea-internal @ main @ 9959c18` | `backoffice-fe-aurea-internal @ codex/fix-internal-pwa @ 381494d` | `backoffice-be-aurea @ codex/platform-audit @ e1a442f` | `backoffice-fe-aurea @ codex/frontend-e2e-validation @ 240320a` | 🟡 PARCIAL | Deploy Render live, login platform_owner en Chrome, tenants, planes y features con datos sintéticos; aprobación del backend cliente y rotación del usuario de evidencia pendientes. |
| [`26-09-02_20-32_internal-platform`](./26-09-02_20-32_internal-platform/README.md) | `main @ 0288b99` | `main @ 381494d` | `NO VERIFICADO` | `NO VERIFICADO` | 🟡 PARCIAL | Separación platform/internal: builds, tests, Docker, health y rutas protegidas; deploy backend y flujo autenticado pendientes. |
| [`26-09-02_14-08`](./26-09-02_14-08/README.md) | `main @ b4f3efb` | `main @ 428196e` | N/A | `NO DISPONIBLE` | 🔴 DESVÍO | Review integral: documentación, código, CI/CD, MCP, Chrome, capturas y issues. |
| [`26-09-02_01-25`](./26-09-02_01-25/README.md) | `v0.18.0` | `v0.1.2` | N/A | `v0.4.0` | 🟡 AUDITORÍA | Auditoría de arquitectura profunda: taxonomía `services.bookings`, scopes `platform` vs `tenant` y tareas de refactor. |
| [`26-09-02_01-03`](./26-09-02_01-03/README.md) | `v0.18.0` | `v0.1.2` | N/A | `v0.4.0` | 🟢 APROBADO | Pruebas E2E en Chrome, RBAC/FBAC, Theme Service y Gestión de Tenants. |

---

## 📂 Convención de Nombres de Subcarpetas

Cada sesión de validación y generación de evidencias se almacena en una subcarpeta identificada por la fecha y hora de inicio de la prueba:

```text
evidencias/<YY-MM-DD>_<HH-mm>/
```

### Formato:
* **`YY-MM-DD`**: Año (2 dígitos), Mes y Día de la prueba (ej: `26-09-02`).
* **`HH-mm`**: Hora y Minutos en formato de 24 horas (ej: `01-03`).

---

## 📑 Estructura Requerida en Cada Subcarpeta

Cada subcarpeta de evidencia contiene:
1. **`README.md`**: Reporte técnico de la sesión (versión de cada producto auditado, entorno, credenciales de prueba, matriz de tests, reporte de discrepancias detectadas y tareas/issues creados en GitHub).
2. **`capturas/`**: Subcarpeta dedicada a almacenar exclusivamente los archivos de imagen (`*.png` / `*.webp`) numerados y descriptivos para mantener el directorio ordenado.

```text
evidencias/<YY-MM-DD>_<HH-mm>/
├── README.md
└── capturas/
    ├── 01_login_page.png
    ├── 02_dashboard_superadmin.png
    └── ...
```

---

## 🤖 Playbook de Review Integral Asistida por IA — Tolerancia Cero

Este playbook define cómo solicitar y ejecutar una revisión completa del ecosistema Aurea. Su objetivo es que un pedido corto active una auditoría reproducible, estricta y exhaustiva a nivel de código y a nivel funcional en Google Chrome.

La revisión es de **tolerancia cero**: no existe un margen aceptable de diferencia entre la documentación, el código y el comportamiento observado. Una diferencia mínima —incluyendo un carácter, un punto y coma, un nombre, una ruta, un orden, un estilo, un mensaje, un código HTTP o una condición de borde— es un incumplimiento. No se debe redondear, relativizar, omitir ni marcar como “menor” para aprobar el resultado.

### Pedido corto recomendado

Usar este pedido cuando se quiera revisar todo el ecosistema:

```text
Ejecutá el playbook de review integral de aurea-docs sobre todo Aurea. El README de la sesión debe incluir un resumen de qué se buscaba y qué se encontró, las capturas embebidas, y la sesión debe agregarse al índice.
```

Para acotar el alcance, agregarlo explícitamente al final:

```text
Ejecutá el playbook de review integral de aurea-docs sobre backoffice-be-aurea y backoffice-fe-aurea, con foco en autenticación y tenants. Subí y embebé las capturas en el README, agregá un resumen de objetivo/hallazgos y actualizá el índice.
```

Si el pedido no especifica un alcance, el alcance por defecto es **todo el ecosistema documentado**: `aurea-docs`, `backoffice-be-aurea`, `backoffice-fe-aurea`, `aurea-pages-template` y `aurea-ci`, incluyendo sus integraciones y flujos de punta a punta.

### Principio rector: la documentación es la fuente de verdad

La IA debe leer primero este repositorio y usarlo como contrato normativo. En particular, debe consultar el mapa de documentación del [README principal](../README.md), la especificación técnica, las decisiones, el alcance del producto, los flujos, CI/CD y la guía de contribución.

La implementación y la interfaz deben coincidir exactamente con ese contrato. Cualquier diferencia se considera un defecto, incluso si es mínima: nombres, rutas, scopes, capabilities, validaciones, códigos HTTP, mensajes, estados, textos, estilos, orden, permisos, comportamiento responsive, un enlace roto o un carácter incorrecto. Toda diferencia confirmada debe producir un issue y quedar registrada en el README de la sesión dentro de [`evidencias/`](./).

La única excepción es una definición ausente, contradictoria o materialmente ambigua. En ese caso la IA debe:

1. detener la decisión sobre ese punto;
2. formular una consulta concreta con las alternativas observadas y su impacto;
3. esperar la definición acordada;
4. actualizar la documentación de `aurea-docs` con esa decisión;
5. continuar la revisión usando la documentación actualizada como contrato.

No se debe interpretar silenciosamente una ambigüedad ni adaptar la documentación al código existente para hacer pasar la revisión.

### Procedimiento obligatorio

1. **Determinar alcance y versión.** Identificar los repositorios, ramas o commits, dependencias, variables de entorno necesarias y servicios involucrados. Registrar fecha, hora, navegador, versiones y commit de cada componente.

2. **Construir la matriz de verdad.** Extraer de la documentación todos los requisitos verificables: arquitectura, contratos API, modelo de datos, autenticación, autorización RBAC/FBAC, scopes `platform` y `tenant`, aislamiento multi-tenant, lifecycle, módulos, planes, branding, estados, errores, CI/CD y flujos de usuario. Cada requisito debe tener una referencia documental.

3. **Auditar el código.** Revisar backend, frontend, template público, CI/CD y configuración asociada. Cubrir rutas, DTOs, guards, servicios, repositorios, queries, filtros `tenantId`, validaciones, serialización, errores, logs, secretos, migraciones, tests, build, lint, tipado, dependencias, rutas de frontend, stores, hooks, componentes, permisos visuales, formularios, estados de carga/vacío/error, accesibilidad y responsive. Comparar cada hallazgo contra la matriz de verdad, no sólo contra la intención aparente del código.

4. **Validar funcionalmente en Chrome.** Levantar el sistema con el procedimiento documentado y recorrer los flujos completos de cada rol, tenant, módulo y estado relevante. Verificar login, sesión, logout, navegación, guards de ruta, permisos, aislamiento entre tenants, CRUD, invitaciones, configuración, catálogo, módulos dinámicos, páginas públicas, errores y casos negativos. Inspeccionar también consola, red, respuestas HTTP, payloads, redirects, almacenamiento, imágenes, enlaces, responsive y consistencia visual con lo documentado. Tomar capturas numeradas de cada flujo o defecto relevante, guardarlas en la carpeta de la sesión y subirlas al repositorio.

5. **Ejecutar las verificaciones automatizadas disponibles.** Correr tests unitarios, integración y E2E, builds, typecheck, lint, validadores de manifests y scripts de CI/CD indicados por la documentación. Un test inexistente no se considera evidencia de cumplimiento.

6. **Clasificar cada resultado con criterio estricto.** Separar explícitamente:
   - `CUMPLE`: código y comportamiento coinciden con la documentación, con evidencia verificable;
   - `DESVÍO`: existe una diferencia respecto de la documentación; siempre requiere issue y registro en evidencias;
   - `NO DEFINIDO`: falta una decisión o hay ambigüedad; requiere consulta y actualización documental antes de cerrar la revisión;
   - `NO VERIFICABLE`: falta entorno, credencial o dependencia; indicar exactamente el bloqueo y no convertirlo en cumplimiento.

   `CUMPLE` sólo puede utilizarse cuando la coincidencia es exacta y está respaldada por evidencia. `NO VERIFICABLE` tampoco equivale a aprobado: deja la revisión abierta hasta poder comprobarlo.

7. **Crear issues para todos los desvíos.** Crear un issue por defecto por cada desvío accionable, sin agrupar diferencias no relacionadas. El issue debe incluir repositorio afectado, referencia documental, comportamiento esperado, comportamiento observado, pasos para reproducir, impacto, evidencia, commit, severidad y criterio de aceptación. Incluir enlaces a las capturas subidas que demuestren el desvío; si el issue no admite una referencia persistente, dejar el enlace relativo en el README y explicar la limitación. Los casos de seguridad, pérdida de datos, aislamiento multi-tenant o bloqueo de flujo deben marcarse como prioritarios. **Agregar cada issue nuevo al [Project 2 — Aurea Backlog](https://github.com/orgs/aurea-io/projects/2), conservando el repositorio y el estado correspondiente.**

8. **Generar el reporte de evidencias.** Crear `evidencias/<YY-MM-DD>_<HH-mm>/README.md` siguiendo los reportes existentes. El README debe comenzar con un resumen ejecutivo que indique qué se buscaba verificar y qué se encontró. Debe incluir alcance, commits, entorno, matriz de cobertura, comandos ejecutados, usuarios/roles utilizados sin exponer secretos reales, resultados de código y Chrome, tabla completa de desvíos, enlaces a issues, preguntas abiertas, limitaciones, capturas embebidas con Markdown (`![...](./capturas/...)`) y conclusión. Subir el README y todas las capturas al repositorio. Agregar la nueva sesión al índice/historial de este README principal con enlace, estado y componentes revisados. No declarar aprobación si existe al menos un desvío o una verificación crítica pendiente.

9. **Verificar trazabilidad final.** Cada requisito debe terminar en una de estas salidas: evidencia de cumplimiento, issue con evidencia del desvío, o consulta documentada por definición faltante. Revisar que no haya enlaces rotos, rutas antiguas, capturas sin referencia, capturas sólo enlazadas pero no embebidas, README sin resumen ejecutivo, sesión ausente del índice, hallazgos sin issue o issues no agregados al Project 2. **Comprobar explícitamente en el proyecto que cada issue creado aparece como ítem y registrar el estado del ítem en el reporte.**

### Regla Inquebrantable de Isomorfismo y Jerarquía (Sección → Página → Módulo)

Toda auditoría ejecutada bajo este Playbook debe verificar de manera obligatoria y con tolerancia cero el cumplimiento de la estructura canónica:

1. **Jerarquía en 3 Niveles:**
   * **SECCIÓN:** Macro-área funcional (`services`, `commerce`, `gastronomy`, `crm`, `marketing`, `core`).
   * **PÁGINA:** Pantalla navegable y carpeta física obligatoria en `src/tenant/sections/<sección>/<página>/` tanto en Backend como en Frontend.
   * **MÓDULO:** Partes, funciones o widgets específicos dentro de esa página que se activan o desactivan dinámicamente según plan y configuración.
2. **Prohibición de Carpetas Paraguas y Páginas Planas:**
   Queda estrictamente prohibido amontonar páginas o subdominios bajo carpetas comodín (como `restaurant/` o una carpeta `pages/` plana sin sección ni dominio asignado).
3. **Principio de Isomorfismo Unificado (FBAC + RBAC):**
   El namespace `<sección>.<página>.<módulo>` debe ser unívoco y coincidir 1:1 entre:
   * La **feature comercial** contratada por la empresa en el catálogo de planes (`module_catalog`).
   * El **rol / permiso granular** asignable al empleado (`:read` y `:write`).
   * La **ubicación física del código y contratos** (`src/tenant/sections/<sección>/<página>/`).
4. **Criterio de Auditoría:**
   Cualquier controlador, servicio, componente visual, decorador o permiso que viole este namespace unívoco debe ser calificado automáticamente como **🔴 DESVÍO CRÍTICO**, generar un issue prioritario en el [Project 2 — Aurea Backlog](https://github.com/orgs/aurea-io/projects/2) y bloquear la aprobación de la sesión.

### Criterio de completitud

La revisión sólo está completa y puede declararse aprobada cuando se cubrieron todos los repositorios y flujos dentro del alcance, se contrastaron código y Chrome contra la documentación con coincidencia exacta, se ejecutaron las validaciones disponibles, y no existe ningún desvío ni verificación crítica pendiente. Cada diferencia debe quedar trazada en un issue y en el reporte de [`evidencias/`](./). Si el pedido fue “todo”, no se puede cerrar la revisión por haber validado únicamente el flujo principal o una sola aplicación.

### Capturas obligatorias

El playbook debe tomar y **subir capturas numeradas** de cada flujo validado y de cada desvío relevante. Las imágenes deben guardarse en `evidencias/<YY-MM-DD>_<HH-mm>/capturas/`, enlazarse y **embeberse dentro del README de la sesión** con Markdown (`![...](./capturas/...)`), y adjuntarse o enlazarse desde los issues correspondientes. No alcanza con describir una pantalla ni con dejar únicamente un enlace a la captura.

El README de cada sesión debe comenzar con un **resumen ejecutivo** que explique claramente qué se buscaba verificar y qué se encontró, incluyendo los principales cumplimientos, desvíos y bloqueos. También debe agregar la sesión al índice/historial de este README principal, con enlace al reporte, alcance, estado y componentes revisados.

Los issues y su asignación al Project 2 forman parte de la evidencia de trazabilidad: el reporte debe incluir el enlace de cada issue, confirmar que aparece en el proyecto y anotar cualquier fallo de permisos o limitación que impida asignarlo.

---
