# Corrida de evidencias — separación platform/internal

## Resumen ejecutivo

Esta corrida es un snapshot histórico del avance de la separación entre el Backoffice interno de AUREA y el backoffice operativo de los tenants. Su fuente de verdad actualizada es la corrida de producción [`26-09-02_19-27_internal-platform-production`](../26-09-02_19-27_internal-platform-production/README.md), que incorpora Render, MongoDB remoto y la validación autenticada en Chrome.

La separación todavía no puede declararse completa: el backend internal aún no está desplegado con una conexión MongoDB de producción y, por ese motivo, no se pudo ejecutar el flujo autenticado end-to-end ni validar mutaciones contra datos reales.

## Alcance

- `backoffice-be-aurea-internal` en `main`.
- `backoffice-fe-aurea-internal` en `main`.
- API platform de lectura y mutación básica.
- Login y protección de rutas platform.
- Health check y contenedor de producción.
- Publicación estática del frontend internal.

## Versiones verificadas

| Componente | Commit | Estado |
| --- | --- | --- |
| Backend internal | `0288b99` | Publicado en `main` |
| Frontend internal | `381494d` | Publicado en `main` |

## Validaciones automatizadas

| Validación | Resultado |
| --- | --- |
| TypeScript backend internal | ✅ Sin errores |
| Tests backend internal | ✅ 40/40 |
| Build frontend internal | ✅ Exitoso |
| Build Docker backend internal | ✅ Exitoso |
| Health en contenedor local | ✅ `200`, `scope: platform` |
| Backend cliente desplegado `/api/health/live` | ✅ `200` |

## Validaciones funcionales en navegador

### 1. Login del Backoffice interno

Resultado: ✅ La aplicación muestra una superficie separada, identificada como `Backoffice interno`, con autenticación específica de plataforma.

![Login del Backoffice interno](./capturas/01_login_internal.png)

### 2. Protección de ruta platform

Resultado: ✅ El acceso no autenticado a `/platform/tenants` vuelve a la pantalla de login.

![Ruta platform protegida](./capturas/02_protected_route_login.png)

## Contratos implementados

Backend internal (contratos vigentes; autenticación JWT de plataforma):

- `GET /api/v1/platform/tenants`
- `GET /api/v1/platform/tenants/:id`
- `POST /api/v1/platform/tenants`
- `PATCH /api/v1/platform/tenants/:id`
- `GET /api/v1/platform/plans`
- `POST /api/v1/platform/plans`
- `PATCH /api/v1/platform/plans/:id`
- `GET /api/v1/platform/features`
- `GET /api/v1/health/live`

Para los endpoints `/platform/*`, una solicitud sin JWT devuelve `401`; un JWT válido sin el scope o capability requerido devuelve `403`. Las respuestas exitosas devuelven DTOs de plataforma y no exponen payload operativo de tenants. Las mutaciones reciben el DTO correspondiente y responden con la entidad de plataforma creada o actualizada. La corrida de producción verifica actualmente los `GET` autenticados de tenants, planes y features; las mutaciones quedan pendientes de una sesión específica de QA.

Frontend internal:

- `/platform/dashboard`
- `/platform/tenants`
- `/platform/catalog`

## Limitaciones y pendientes

- El snapshot no incluía todavía un servicio Render para `backoffice-be-aurea-internal`; ese punto quedó resuelto en la corrida de producción enlazada arriba.
- La publicación Vercel del frontend internal del snapshot no tenía aún la API productiva configurada; ese punto quedó resuelto en la corrida de producción enlazada arriba.
- No se verificaron alta/edición real de tenants y planes contra MongoDB remoto en este snapshot; siguen pendientes como prueba de mutación separada.
- Aún falta retirar los endpoints y pantallas Superadmin del backoffice cliente.
- Aún falta una corrida autenticada con `platform_owner` y `platform_operator`, incluyendo respuestas `200`, `401`, `403` y casos de mutación.

## Conclusión

La base de la separación estaba implementada y publicada en `main` al momento de este snapshot. Los pendientes de despliegue y lectura autenticada fueron resueltos por la corrida de producción enlazada; permanecen como trabajo separado la prueba de mutaciones y el cierre de las épicas con la aprobación del backend cliente.
