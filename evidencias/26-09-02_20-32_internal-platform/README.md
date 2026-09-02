# Corrida de evidencias — separación platform/internal

## Resumen ejecutivo

Esta corrida verifica el avance de la separación entre el Backoffice interno de AUREA y el backoffice operativo de los tenants. Se validaron los artefactos publicados en `main`, los builds locales, la imagen Docker del backend internal y las rutas públicas/protegidas del frontend internal.

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

Backend internal:

- `GET /api/v1/platform/tenants`
- `GET /api/v1/platform/tenants/:id`
- `POST /api/v1/platform/tenants`
- `PATCH /api/v1/platform/tenants/:id`
- `GET /api/v1/platform/plans`
- `POST /api/v1/platform/plans`
- `PATCH /api/v1/platform/plans/:id`
- `GET /api/v1/platform/features`
- `GET /api/v1/health/live`

Frontend internal:

- `/platform/dashboard`
- `/platform/tenants`
- `/platform/catalog`

## Limitaciones y pendientes

- No existe todavía un servicio Render para `backoffice-be-aurea-internal`.
- Falta configurar `DATABASE_URL`, `JWT_ACCESS_SECRET`, `GOOGLE_CLIENT_ID` y `FRONTEND_URL` en el servicio internal.
- La publicación Vercel del frontend internal es accesible, pero hasta configurar `VITE_API_URL` apunta al valor local por defecto.
- No se verificaron alta/edición real de tenants y planes contra MongoDB remoto.
- Aún falta retirar los endpoints y pantallas Superadmin del backoffice cliente.
- Aún falta una corrida autenticada con `platform_owner` y `platform_operator`, incluyendo respuestas `200`, `401`, `403` y casos de mutación.

## Conclusión

La base de la separación está implementada y publicada en `main`, con validaciones locales exitosas. La entrega completa queda pendiente de configurar el servicio backend internal y sus secretos de entorno; después debe repetirse esta corrida con autenticación y datos remotos antes de cerrar las épicas.
