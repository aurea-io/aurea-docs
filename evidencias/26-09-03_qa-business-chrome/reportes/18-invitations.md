# QA — Invitaciones

- Ruta: `/invitations`
- Chrome: **FAIL reproducible**: `/invitations` redirige a `/dashboard` para OWNER.
- Implementación: **Implementado en código**: crear, listar y revocar invitaciones; backend también expone verificar/aceptar.

## UX y faltantes

No fue posible validar creación/listado/revocación. Revisar capability `tenant:employees:manage` y la condición de navegación del rol OWNER.
