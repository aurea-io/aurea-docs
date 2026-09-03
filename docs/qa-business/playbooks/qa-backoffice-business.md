# Playbook QA completo — Backoffice Business

## Objetivo

Ejecutar una auditoría funcional y de UX del backoffice Business en Chrome, producir evidencia por pantalla, identificar funciones implementadas, fallas, inconsistencias y funcionalidades ausentes, y actualizar los reportes en `aurea-docs/evidencias/<corrida>/reportes/`.

## Precondiciones

1. Abrir `https://backoffice-fe-aurea.vercel.app/` en Google Chrome.
2. Confirmar que la app supera el estado “Verificando sesión en Aurea…” y que la API responde.
3. Usar únicamente cuentas QA autorizadas del entorno. No usar cuentas reales ni datos personales.
4. Registrar fecha, hora, URL, navegador, build/commit si está visible y estado de la API.
5. Antes de toda operación destructiva, usar datos de prueba y confirmar el alcance.

## Matriz de roles

| Rol | Objetivo |
|---|---|
| SUPERADMIN | Tenants, features, planes y preview |
| OWNER | Operación completa del tenant |
| MANAGER | Operación permitida y gestión operativa |
| STAFF | Lectura y acciones de operación permitidas |
| CASHIER | Caja y permisos restringidos |

## Recorrido por pantalla

Para cada pantalla:

1. Abrirla desde la navegación visible; registrar URL y título.
2. Capturar estado inicial, estado vacío, carga, error y estado con datos.
3. Recorrer cada control: enlaces, botones, filtros, búsqueda, modales, formularios, paginación, acciones de fila y estados disabled.
4. Verificar validaciones: requerido, formato, límites, mensajes y recuperación ante error.
5. Verificar persistencia recargando la pantalla y cambiando de tenant si aplica.
6. Repetir los controles relevantes con el rol de menor privilegio.
7. Registrar resultado como PASS, FAIL, BLOCKED o NOT IMPLEMENTED.
8. Guardar captura en `aurea-docs/evidencias/<fecha>/capturas/` con nombre estable y enlazarla desde el reporte de pantalla.

## Criterios funcionales

- La ruta protegida redirige correctamente sin sesión.
- La navegación muestra solo capacidades y permisos efectivos.
- Las operaciones exitosas muestran confirmación y actualizan la vista.
- Los errores son accionables y no pierden datos ingresados.
- El tenant activo queda visible y todas las consultas respetan su contexto.
- Acciones de escritura están restringidas por rol y capability.
- No hay botones que parezcan activos y no ejecuten ninguna acción.

## Criterios UX

- Jerarquía clara, títulos y CTA comprensibles.
- Consistencia de idioma, formato monetario y fechas.
- Formularios con labels, ayuda contextual y errores junto al campo.
- Feedback de carga, éxito, error y estado vacío.
- Accesibilidad básica: foco visible, teclado, nombres accesibles, contraste y targets táctiles.
- Responsive en viewport desktop y mobile.

## Reporte de faltantes

Marcar `NOT IMPLEMENTED` cuando una función está descrita por el producto o expuesta por el dominio pero no existe una ruta, control o endpoint utilizable. Marcar `BLOCKED` cuando existe pero no se puede probar por el bloqueo del entorno. No confundir ambos estados.

## Entregables

- Un `.md` por pantalla en `evidencias/<corrida>/reportes/`.
- README de ejecución con incidencias y capturas.
- Links a cada evidencia.
- Resumen de severidad: crítico, alto, medio, bajo.
- Lista de retest con precondiciones claras.
