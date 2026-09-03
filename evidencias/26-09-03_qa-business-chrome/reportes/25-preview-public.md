# QA — Preview público del tenant

- Ruta: `/preview/:tenantId` y `/public/:slug`
- Chrome: **PASS parcial**: el Preview nuevo resuelve rutas profundas al login de la SPA; el flujo autenticado no pudo completarse con las credenciales QA disponibles.
- Implementación: **Implementado en código** como preview público.

## UX y faltantes

Validar branding, enlaces de reserva, catálogo visible, responsive y separación de acciones públicas versus privadas. No ejecutado por falta de sesión válida en el Preview.

Evidencia: [`preview-dashboard-retest-login.jpg`](../capturas/preview-dashboard-retest-login.jpg).
