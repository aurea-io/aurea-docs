# QA — Dashboard

- Ruta: `/dashboard`
- Chrome: **PASS parcial**: renderizó autenticado como QA Owner tras esperar la hidratación.
- Implementación: **Implementado en código** con contexto, analytics y sesión actual.

## UX y faltantes

Se observaron tenant activo, rol, miembros, reservas, pedidos, stock y facturación. El link “Visitar Web” apunta a `http://localhost:5173/preview/de-santas` desde producción: riesgo alto de enlace roto.

## Retest

El Preview del PR #83 ya resuelve `/dashboard` al login de la SPA sin 404. En producción, la sesión autenticada todavía muestra la URL `localhost`, por lo que el fix requiere promoción.

Evidencia: [`dashboard-production-retest.jpg`](../capturas/dashboard-production-retest.jpg) y [`preview-dashboard-retest-login.jpg`](../capturas/preview-dashboard-retest-login.jpg).
