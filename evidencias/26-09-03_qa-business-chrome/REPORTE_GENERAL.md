# Reporte general — QA funcional y UX

## Estado de la ejecución

La ejecución en Chrome se completó parcialmente con `QA Owner`. Luego se implementaron los seis fixes en el frontend. El chequeo TypeScript posterior finalizó sin diagnósticos y todos los checks del PR #83 pasaron. El nuevo Preview de Vercel fue verificado en Chrome; la URL productiva todavía no fue promovida.

## Retest de implementación

| Issue | Verificación local | Estado runtime |
|---|---|---|
| #77 | Fallback con timeout de 12 s y botón Reintentar | Implementado; runtime protegido pendiente de login en Preview |
| #78 | OWNER con `*`/`all` reconoce capabilities granulares | Implementado; runtime pendiente de promoción |
| #79 | Misma corrección de capabilities para Invitaciones | Implementado; runtime pendiente de promoción |
| #80 | URLs públicas resueltas con `/public/:slug` | Implementado; producción aún muestra URL localhost |
| #81 | Formatter centralizado desde centavos y moneda ARS explícita | Implementado; runtime pendiente de promoción |
| #82 | Catálogo sin dialogs nativos; usa Dialog y feedback inline | Implementado; runtime pendiente de promoción |

## Inventario

Se encontraron 25 superficies de pantalla entre autenticación, tenant, plataforma y preview. Cada una tiene un reporte individual en [`reportes/`](./reportes/).

## Hallazgos de implementación

Los hallazgos siguientes corresponden a la ejecución autenticada en producción. El retest del Preview confirmó el arreglo de deep-link, pero no pudo autenticarse con las credenciales QA disponibles.

- **NAV-001 / alto:** `/members` se muestra en sidebar pero redirige a `/dashboard` para OWNER.
- **NAV-002 / alto:** `/invitations` se muestra en sidebar pero redirige a `/dashboard` para OWNER.
- **UX-001 / medio:** la pantalla de verificación inicial no tiene timeout ni recuperación visible.
- **UX-003 / alto:** catálogo muestra `$ 15,00`, `$ 95,00`, `$ 180,00`, `$ 350,00`; revisar conversión esperada desde los centavos del seed.
- **QA-001 / medio:** faltan roles, mutaciones controladas, responsive y pantallas no visibles para OWNER.
- **UX-001 / medio:** varias acciones de catálogo usan `window.prompt`/`window.alert`; esto reduce accesibilidad, consistencia y calidad del feedback.
- **UX-002 / medio:** validar que todas las pantallas con mutaciones tengan estados de carga y error visibles, especialmente operaciones de inventario, caja, fidelización y reservas.
- **FUNC-001 / alto para retest:** POS expone apertura/cierre de caja, pero debe confirmarse si el flujo de venta/cobro está implementado en UI.
- **FUNC-002 / alto para retest:** salón expone dominios de ticket/recibo en backend; confirmar que existan controles equivalentes en UI.
- **FUNC-003 / medio para retest:** planes comerciales muestran alta; confirmar edición, precios, baja y versionado histórico.

## Criterio de “sin implementar”

No se marcó como definitivamente no implementada una función que solo quedó sin probar por AUTH-001. Las funciones señaladas como “Parcial” requieren retest en Chrome para separar ausencia de UI de un problema de datos/capability.

## Retest recomendado

1. Promover el PR #83 a producción y comprobar `/api/auth/me` y `/api/auth/me/capabilities`.
2. Repetir el playbook con credenciales válidas para OWNER y, luego, MANAGER, STAFF y CASHIER.
3. Completar capturas por estado y actualizar cada reporte con resultado observado.
4. Cerrar los issues #77–#82 solo después del runtime retest; mantener abiertos NAV-001/NAV-002 si las rutas siguen redirigiendo.
