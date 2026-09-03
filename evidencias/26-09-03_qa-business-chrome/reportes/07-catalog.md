# QA — Catálogo

- Ruta: `/catalog`
- Chrome: **PASS parcial**: se observaron 4 ítems, búsqueda, filtros, importar CSV, alta, pausar, editar y eliminar.
- Implementación: **Implementado en código**: CRUD de ítems, búsqueda, categorías, modificadores, importación CSV.

## UX y faltantes

Riesgos: uso de `window.prompt`/`alert` para categorías, modificadores e importación. Los precios renderizados requieren revisar conversión de centavos a ARS.
