# QA — Login

- Ruta: `/login`
- Chrome: **BLOCKED** — la app permaneció en “Verificando sesión en Aurea…” y no mostró el formulario.
- Implementación: **Implementado en código** (`LoginPage`, login, Google, magic link y recuperación).

## UX

La captura previa del repo sugiere una pantalla visualmente cuidada; falta retest del formulario real, foco, errores y accesibilidad.

## Faltantes / riesgos

- AUTH-001 bloquea la validación del login en el deployment.
- Verificar timeout y mensaje accionable cuando la API de sesión no responde.

## Evidencia

[`26-09-03_qa-business-chrome`](../../../../evidencias/26-09-03_qa-business-chrome/README.md)
