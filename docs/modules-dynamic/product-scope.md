# POC — Módulos dinámicos por empresa

Esta POC valida la experiencia de seleccionar módulos y funciones para una empresa, manteniendo la misma jerarquía en catálogo, API y UI.

También fija el límite entre los dos productos administrativos de Aurea: el backoffice AUREA, orientado a la operación de la plataforma, y el backoffice del cliente, orientado a la operación de cada negocio.

## Objetivo

Demostrar tres escenarios:

1. un owner publica el catálogo `Servicios → Reservas → Subir foto a la reserva`;
2. un admin de tenant activa o desactiva la función si su plan la incluye;
3. la página React muestra u oculta el control, mientras el backend sigue protegiendo el endpoint.

## Flujos

```mermaid
flowchart LR
  A[Owner define manifiesto] --> B[Sync a module_catalog]
  B --> C[Plan concede capability]
  C --> D[Admin tenant activa]
  D --> E[GET /me/capabilities]
  E --> F[React renderiza Reserva]
  F --> G[Backend vuelve a autorizar acción]
```

## Alcance administrativo

### Backoffice AUREA

El owner puede:

- crear y editar planes/membresías;
- definir precio, moneda, intervalo, prueba y grace period;
- asignar módulos y funciones a cada plan;
- gestionar tenants y su estado;
- hacer ABM del catálogo de módulos y asignar categorías;
- activar o desactivar mantenimiento global;
- consultar auditoría.

El usuario `Readonly` puede consultar esas pantallas, pero cualquier mutación debe devolver `403 INSUFFICIENT_PERMISSION`.

### Backoffice del cliente

El cliente administra su negocio completo dentro del tenant elegido. Solo ve páginas, módulos y funciones que resultan habilitados por plan, configuración de tenant, estado global y rol. Las páginas finales se especializan por rubro, por ejemplo reservas para servicios, menú/pedidos para restaurante e inventario para stock.

```mermaid
flowchart TD
  PLATFORM[Backoffice AUREA] --> PLAN[Plan + precio]
  PLATFORM --> TENANT[Tenant]
  PLATFORM --> CATALOG[Módulo + categoría + mantenimiento]
  PLAN --> EFFECTIVE[Capabilities efectivas]
  TENANT --> EFFECTIVE
  CATALOG --> EFFECTIVE
  EFFECTIVE --> CLIENT[Backoffice cliente]
  EFFECTIVE --> PUBLIC[Página final pública]
```

## Mockup visual

![Mockup de configuración de módulos dinámicos](../../assets/modules-dynamic/modulos-dinamicos-backoffice.png)

![Mockup de configuración de Reservas y preview mobile](../../assets/modules-dynamic/modulos-dinamicos-reservas.png)

Los mockups son conceptuales: sirven para conversar sobre jerarquía, estados, plan, roles, alcance del negocio y preview público desktop/mobile. Los textos y estados reales deben venir de la API.

## Criterio de aceptación

- La pantalla agrupa por sección y página, sin mezclar features de dominios distintos.
- El admin ve por qué una función no está disponible: plan, rol, dependencia o estado.
- Activar una función actualiza el preview y el mapa de capabilities.
- Cambiar `tenantId` invalida la configuración anterior.
- Una llamada directa al endpoint sin capability devuelve `403`.
- La documentación de dominio mantiene las keys estables.
- La página pública carga el CSS desde `Theme Service`, no desde un archivo específico del tenant.
- Repetir la consulta de la misma versión responde desde caché o con `304`.
- Cambiar el branding publica una nueva versión sin invalidar todos los tenants.
- MongoDB no se consulta en cada visita cuando existe una versión cacheada.
- Los usuarios de plataforma usan un scope separado de los usuarios del tenant.
- Un módulo puede consumir créditos del plan y activar un addon para ampliar disponibilidad.
- Un cambio visual se guarda como borrador, permite preview y conserva las últimas cuatro versiones publicadas.
- La POC documenta la política de 14 días para pagos vencidos.

## Datos mínimos de demo

```json
{
  "plan": "pro",
  "tenant": "acme-salon",
  "enabled": [
    "services",
    "services.bookings",
    "services.bookings.create",
    "services.bookings.reschedule"
  ],
  "disabled": ["services.bookings.photo_upload"]
}
```

## Implementación incremental

## Contrato de alta y onboarding de tenants

El alta se ejecuta desde el backoffice AUREA con scope `platform_owner` y es una
operación auditable. El formulario requiere `name`, `slug`, `vertical` y
`ownerEmail`; `slug` se normaliza a minúsculas, se valida como único y no se
permite reutilizar uno ya asignado. El plan inicial y las features se resuelven
desde el catálogo persistido; si no se selecciona un paquete explícito se usa
la configuración por vertical.

La creación del tenant y sus features base es atómica. Si el dueño ya existe,
se crea su membership `TenantUser` con rol `OWNER`. Si todavía no existe, se
genera una invitación única de rol `OWNER`, válida por 14 días. No se guardan
claves temporales ni secretos en el frontend. El usuario completa el onboarding
mediante `GET /api/invitations/verify/:code` y, después de autenticarse,
`POST /api/invitations/accept` con `{ "code": "..." }`. La aceptación vincula
la cuenta existente, marca la invitación como usada y es idempotente frente a
reintentos.

Una invitación expirada, usada o inexistente no revela datos del tenant y
devuelve un error de validación. Los cambios de estado y la aceptación quedan
registrados en auditoría.

## Manifiestos y sincronización del catálogo

Cada feature se declara en un manifiesto versionado dentro del repositorio que
la implementa. La key canónica usa segmentos separados por punto y conserva
el mismo valor en manifiesto, catálogo, capability, ruta y permiso. Por ejemplo:

```yaml
key: services.bookings.photo_upload
module: services.bookings
surface: [backoffice, public]
requires: [services.bookings]
permissions: [service.booking.photo_upload.read, service.booking.photo_upload.write]
```

El manifiesto es la fuente de identidad; la base de datos es la fuente de
estado operativo (planes, addons, mantenimiento y activación por tenant). La
sincronización oficial es un proceso idempotente de CI/deploy que valida keys,
dependencias, superficies y duplicados antes de aplicar cambios. La UI puede
administrar el estado del catálogo, pero no inventar keys ni modificar el
contrato. Un cambio incompatible requiere una nueva key o una migración
explícita.

La validación se ejecuta con `aurea-ci/scripts/validate-manifests.sh` y el
workflow compartido de manifests. Un manifiesto inválido bloquea el pipeline y
no modifica el catálogo persistido.

### Fase 1 — contrato y datos

- Crear manifiestos de `services.bookings`.
- Seed de `module_catalog`, `plans`, `subscriptions`, `memberships` y `tenant_entitlements`.
- Implementar `resolveCapabilities(tenantId, userId, surface)`.

### Fase 2 — backoffice

- Árbol de sección/página/feature.
- Toggle con validación de plan, dependencias y rol.
- Audit log y preview público.

### Fase 3 — frontend y backend

- `useCapability` y `CapabilityRoute`.
- `requireCapability` en comandos sensibles.
- Tests de tenant cruzado y manipulación de JWT.

### Fase 4 — automatización

- Generación del snapshot desde manifests.
- Validación CI de keys, dependencias y rutas.
- Sincronización idempotente en deploy.

### Fase 5 — separación de backoffices

- Crear layouts y rutas de platform y tenant.
- Agregar guard de scope: `platform` versus `tenant`.
- Seed de roles `owner` y `readonly` para AUREA.
- Agregar mantenimiento global y pantalla de estado.
- Validar que un usuario de cliente no pueda consultar planes, precios ni otros tenants.

### Fase 6 — Theme Service

- Crear `themes.controller`, `themes.service`, `theme-cache` y `css-generator`.
- Guardar tokens versionados en MongoDB.
- Servir `/api/v1/themes/:tenantKey.css?v=:version` con `ETag`.
- Implementar caché local para la POC y dejar Redis como adaptador de producción.
- Validar tokens y rechazar CSS libre.
- Probar que 100 requests simultáneos para el mismo tenant no produzcan 100 consultas a MongoDB.

### Fase 7 — decisiones comerciales y de operación

- Seed de `platform_memberships`, `plan_prices`, `tenant_addons` y créditos mensuales.
- Implementar `accessPolicy` para pago vencido: backoffice bloqueado y página pública activa durante 14 días.
- Implementar borrador, preview y conservación de cuatro versiones de branding.
- Agregar roles múltiples por empleado y permisos requeridos por módulo.
- Documentar y probar retención/TTL por función.

## Qué no debe resolver esta POC

- Billing real o integración con un proveedor de suscripciones.
- Editor genérico de workflows.
- Permisos por campo.
- Reglas de autorización offline.
- Borrado físico de features o datos históricos.
