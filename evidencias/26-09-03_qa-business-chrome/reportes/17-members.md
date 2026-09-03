# QA — Miembros

- Ruta: `/members`
- Chrome: **FAIL reproducible**: el enlace se muestra, pero abrir `/members` redirige a `/dashboard`.
- Implementación: **Implementado en código**: listado, activar/suspender y quitar miembro; el servicio también expone actualización de rol/permisos.

## UX y faltantes

No fue posible validar listado ni controles. Revisar capability efectiva `tenant:employees:read` y consistencia entre sidebar y `CapabilityRoute`.
