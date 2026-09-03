# QA — Invitaciones

- Ruta: `/invitations`
- Chrome: **FAIL reproducible**: `/invitations` redirige a `/dashboard` para OWNER.
- Implementación: **Implementado en código**: crear, listar y revocar invitaciones; backend también expone verificar/aceptar.

## UX y faltantes

No fue posible validar creación/listado/revocación. Revisar capability `tenant:employees:manage` y la condición de navegación del rol OWNER.

## Retest

El PR #83 incorpora el fallback de capabilities para permisos wildcard (`*`/`all`). La producción continúa redirigiendo `/invitations` a `/dashboard`; queda pendiente verificarlo después de promover el build.
