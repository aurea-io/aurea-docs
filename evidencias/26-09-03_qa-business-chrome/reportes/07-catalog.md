# QA — Catálogo

- Ruta: `/catalog`
- Chrome: **PASS parcial**: se observaron 4 ítems, búsqueda, filtros, importar CSV, alta, pausar, editar y eliminar.
- Implementación: **Implementado en código**: CRUD de ítems, búsqueda, categorías, modificadores, importación CSV.

## UX y faltantes

Riesgos observados en producción: los precios renderizados requieren revisar conversión de centavos a ARS. El código del PR #83 reemplaza los dialogs nativos del Catálogo por dialogs accesibles y feedback inline, pero todavía requiere promoción y retest autenticado.

Evidencia repetida: [`catalog-production-retest.jpg`](../capturas/catalog-production-retest.jpg).
