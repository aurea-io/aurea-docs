# Decisiones y respuestas — módulos dinámicos

**Estado:** decisiones de producto y arquitectura para módulos dinámicos  
**Fuente:** respuestas del equipo a la revisión de seguridad, performance, personalización, mantenibilidad y escalamiento.

## Decisiones confirmadas

### Tenants y usuarios

- Un usuario puede pertenecer a uno o varios tenants.
- Un tenant puede tener varios empleados.
- Cada membership es independiente: el usuario puede tener un rol y permisos
  diferentes en cada tenant.
- El usuario opera sobre un único tenant activo por vez; cambiar de tenant
  debe volver a resolver contexto, capabilities y datos visibles.
- El cliente puede gestionar sus empleados.
- El `tenantId` no se acepta del body como fuente de autorización.
- Un acceso cruzado responde `403` sin revelar si el recurso de otro tenant existe.

### Roles

Los roles de plataforma y de tenant tienen scopes independientes:

Los roles se almacenan en la base de datos y son dinámicos; no se debe asumir una lista fija en el código. Como roles iniciales de plataforma se contemplan `platform_owner` y `platform_readonly`, pero el catálogo puede agregar o modificar roles sin una nueva versión del frontend.

Los roles están asociados a funciones/capabilities. Por ejemplo, una función puede ser `service.booking.photo_upload` y un permiso de solo lectura puede representarse como `service.booking.photo_upload.read`. El modelo debe permitir permisos de lectura, escritura, administración y futuras acciones sin hardcodear nombres de roles.

El scope platform queda reservado a usuarios de AUREA. `platform_owner` debe ser explícito y auditable, no un permiso universal llamado simplemente `owner`. El administrador de un tenant puede gestionar los empleados de su empresa y sus roles, sin poder conceder roles de plataforma.

MFA y restricciones geográficas son capas adicionales a evaluar; la primera regla geográfica será por país.

### Módulos, funciones y superficies

La unidad técnica es el módulo. Sus funciones pueden ser consumidas por distintas superficies:

```text
Módulo Reservas
├── Features: crear, reprogramar, fotos
├── Backoffice del cliente
├── Página pública
└── Permisos y datos del dominio
```

Una función no pertenece a una sola pantalla. `services.bookings.photo_upload` puede aparecer en la página pública y en el backoffice del cliente. Las páginas pueden tener una versión completa o reducida según las capabilities efectivas, y el cliente final no debe ver controles inactivos.

### Correspondencia unívoca entre Feature Keys y Paquetes de Código (Front y Back)

- El namespace de la capability o feature requerida (excluyendo únicamente el calificador de acción final como `.read`, `.write`, `.create`, etc.) define de manera obligatoria la estructura modular, paquete y nombre de archivo donde reside la lógica.
- Por ejemplo, la lógica y endpoints protegidos por `platform.tenants.read` o `platform.tenants.write` (como `createTenant` o `listTenants`) deben vivir en el paquete de dominio `platform/tenants/` (o archivo `platform.tenants.ts`), y no en un archivo genérico `platform.controller.ts` / `PlatformService` que mezcle tenants, planes y features.
- Queda estrictamente prohibido el uso de controladores o módulos comodín ("god controllers") que agrupen subdominios disjuntos con diferentes namespaces de capabilities.
- Toda revisión de código o auditoría de evidencias debe marcar como desvío cualquier controlador, servicio o módulo que concentre más de un dominio de capabilities. Esta regla aplica con idéntico rigor a Backend y Frontend.
- A nivel de implementación, los controladores declaran `@FeatureDomain('<scope>.<dominio>')` a nivel de clase y métodos anotados con `@RequireRead()` / `@RequireWrite()`; en el frontend se adopta el hook contextual simétrico `useDomainPermissions('<scope>.<dominio>')`.

### Jerarquía Canónica en 3 Niveles (Sección → Página → Módulo)

- **Sección:** Macro-área o departamento de la empresa (`services`, `commerce`, `gastronomy`, `crm`, `marketing`, `core`).
- **Página:** Pantalla y carpeta física en `src/tenant/sections/<sección>/<página>/` que navega el operador o cliente final (`bookings`, `catalog`, `orders`, `tables`, `inventory`, `pos`, etc.).
- **Módulo (Feature):** Es una parte, función o widget dentro de la página que se activa o desactiva dinámicamente según el plan y la configuración del tenant (ej. `photo_upload`, `add_to_cart`, `split_bill`, `qr_generator`).
- **`orders` como Núcleo de `commerce`:** La orden de venta no es exclusiva de gastronomía; representa la transacción de compraventa de cualquier comercio (mostrador, delivery o salón). Gastronomía queda enfocada en salón (`tables`) y cocina (`kitchen`).

### Principio de Isomorfismo Unificado (FBAC + RBAC)

- El mismo string de namespace `<sección>.<página>.<módulo>` define simultáneamente:
  1. La **feature comercial** contratada por la empresa en su plan (`module_catalog`).
  2. El **rol / permiso granular** asignable al empleado en su membresía (`:read` y `:write`).
  3. La **ubicación física del código** en Backend y Frontend (`src/tenant/sections/<sección>/<página>/`).
- **Doble filtro de seguridad:** Toda acción valida primero si el tenant tiene la feature activa (`FeatureGuard`), y luego si el colaborador particular tiene el rol/permiso para operarla (`PermissionsGuard`).
- Se establece una distinción inquebrantable entre `services.bookings` (turnos y citas de servicios para profesionales/estética) y las reservas de salón gastronómico (`gastronomy.tables.bookings`), impidiendo colisiones de capabilities entre diferentes rubros comerciales.




### Planes, créditos y addons

- Un tenant puede tener un plan y addons.
- Planes, addons, créditos, precios y sus reglas se almacenan en la base de datos y son configurables desde el backoffice AUREA.
- El código consume el catálogo dinámico; no debe depender de que existan exactamente `Basic`, `Pro` o `Enterprise`.
- Los límites operativos se gestionan dentro de cada módulo.
- Planes, precios, membresías y addons se administran desde el backoffice AUREA.
- Los precios conservan historial.

La unidad de cobro inicial es el peso argentino (ARS), con período mensual. Se admite el pago adelantado de varios meses, por ejemplo seis meses. Mercado Pago será el primer proveedor, encapsulado detrás de un adapter para poder reemplazarlo.

La cantidad de créditos y las funcionalidades que consumen créditos se definirán en el catálogo de datos cuando avance el desarrollo. No deben quedar valores comerciales fijos en el código. Al desactivar un módulo, los créditos se liberan durante el período actual. Los addons tienen vencimiento y renovación según la suscripción, configurable desde la plataforma.

Los ejemplos de planes, módulos y créditos son datos de seed o configuración, no reglas compiladas. La POC puede utilizar valores de demostración siempre que sean editables desde la base de datos.

### Suscripciones vencidas

```text
Pago vencido:
- el backoffice Aurea y el backoffice del tenant quedan inhabilitados inmediatamente;
- se muestra una notificación;
- la página pública del cliente final sigue disponible durante 14 días;
- no se borran datos.

Después de 14 días:
- el tenant pasa a política suspendida;
- la página pública puede bloquear nuevas operaciones;
- los datos quedan conservados.
```

La regla se implementa en una política central, no endpoint por endpoint.

### Personalización

- La aplicación React es la misma para todos los tenants.
- Se permiten colores, imágenes, fuentes soportadas, variantes de componentes y cambios acotados de layout.
- No se permite un layout completamente libre ni CSS arbitrario inicialmente.
- Se recomienda preview antes de publicar.
- Se conservan las últimas cuatro versiones publicadas y un borrador.
- Los templates y sus límites son configurables por `platform_owner`.
- Los campos adicionales pueden ser dinámicos, pero deben validarse contra un catálogo de tipos y keys.
- La resolución pública inicial utiliza `publicId`. Los dominios personalizados quedan como evolución posterior.
- Las imágenes se almacenan inicialmente mediante Cloudinary, detrás de un adapter reemplazable.

### Theme Service

- MongoDB guarda tokens estructurados.
- `Theme Service` genera y sirve CSS por HTTP.
- El endpoint es público y usa el `publicId` del tenant con versionado: `/style/{publicId}.css?v=4`.
- Redis es opcional y no crítico.
- MongoDB es crítico: si no está disponible, el servicio responde `503`.
- El servicio puede escalar en servidores separados para tenants con mucho tráfico.
- Un CDN es opcional: cachea la respuesta HTTP, pero el CSS sigue siendo generado por el servicio.

### Mantenimiento y lifecycle

- Los módulos pueden ponerse en mantenimiento global sin alterar la selección del tenant.
- Una función retirada pasa por `toBeDeprecated` y luego `deprecated`.
- No se borra automáticamente una capability del catálogo.
- Los datos se conservan por defecto; algunas funciones pueden declarar TTL.
- Se contemplan ambientes `development` y `production`; la POC visual no necesita implementarlos todavía.

## Decisiones técnicas derivadas

### Tenancy y autorización

La relación entre usuarios y comercios es M:N mediante `TenantUser`. Un usuario puede tener membresías independientes en varios tenants, con rol, permisos y estado propios por comercio. Los roles de tenant conservan un enum base para compatibilidad, pero pueden resolverse mediante `RoleDefinition.roleKey`; las membresías de plataforma se administran por separado.

Los endpoints vigentes usan el prefijo `/api/*` (sin `/v1`). El endpoint público de estilos es `/api/style/:publicId.css?v=:version` y devuelve una versión publicada con ETag.

### Dependencias

Una feature puede requerir otras. Si un padre queda inactivo, los hijos quedan inactivos. La activación valida dependencias antes de confirmar.

### Desactivación

Desactivar bloquea nuevas operaciones, pero no elimina datos automáticamente. Cada módulo puede declarar una política `forever` o `ttl`; el borrado requiere auditoría, aviso y una tarea controlada.

### Sesiones

Revocar sesiones significa invalidar las sesiones activas cuando se elimina un empleado o cambia un permiso sensible. No es imprescindible para la POC visual, pero debe estar previsto para producción mediante `sessionVersion` o refresh tokens revocables.

### CDN

Un CDN no implica administrar archivos manualmente. Puede cachear la respuesta de `Browser → CDN → Theme Service → MongoDB`. Se puede omitir inicialmente y agregar cuando un tenant necesite más capacidad.

## Decisiones que quedan configurables, no bloqueantes

Estas definiciones no necesitan resolverse antes de implementar el modelo y los adapters. Deben persistirse como configuración o quedar explícitamente parametrizadas:

1. Qué funcionalidades consumen créditos y si el descuento ocurre al activar o durante el consumo operativo.
2. Renovación automática de addons y estados específicos de suscripción de Mercado Pago.
3. Política pública posterior a los 14 días de vencimiento; por defecto se bloquean nuevas operaciones y se conservan los datos.
4. Países permitidos, reglas de TTL por función y obligatoriedad futura de MFA.
5. Qué información puede consultar un usuario de plataforma al inspeccionar un tenant.
6. Qué módulos y créditos se muestran en la POC; se pueden usar seeds de demostración editables.
