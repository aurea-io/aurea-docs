# QA — Dashboard

- Ruta: `/dashboard`
- Chrome: **PASS parcial**: renderizó autenticado como QA Owner tras esperar la hidratación.
- Implementación: **Implementado en código** con contexto, analytics y sesión actual.

## UX y faltantes

Se observaron tenant activo, rol, miembros, reservas, pedidos, stock y facturación. El link “Visitar Web” apunta a `http://localhost:5173/preview/de-santas` desde producción: riesgo alto de enlace roto.
