# QA — Facturación y suscripción

- Ruta: `/settings/billing`
- Chrome: **PASS parcial**: se observó el plan `QA Backoffice`, estado `active` y módulos incluidos.
- Implementación: **Parcial**: consulta billing y botón de pago Mercado Pago.

## Faltantes / riesgos

No se ejecutó el pago. Validar precios, retorno de checkout, errores del proveedor, idempotencia y comprobantes.
