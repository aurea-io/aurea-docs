# QA — Preview público del tenant

- Ruta: `/preview/:tenantId` y `/public/:slug`
- Chrome: **PASS parcial**: el Preview nuevo resuelve rutas profundas al login de la SPA; el flujo autenticado no pudo completarse con las credenciales QA disponibles.
- Implementación: **Implementado en código** como preview público.

## UX y faltantes

La ruta pública carga el branding “Aurea Digital Space” y el estado vacío de catálogo sin 404. Quedan pendientes branding completo, enlaces de reserva, datos de catálogo, responsive y separación de acciones públicas versus privadas.

Evidencia: [`preview-dashboard-retest-login.jpg`](../capturas/preview-dashboard-retest-login.jpg), [`preview-public-retest.jpg`](../capturas/preview-public-retest.jpg) y [`preview-public-final.jpg`](../capturas/preview-public-final.jpg).
