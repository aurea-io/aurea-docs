# Aurea Docs

Repositorio central de documentación de Aurea.
Acá vive todo lo que necesitás saber sobre el producto, la arquitectura y las decisiones del equipo.

> El código está en los repos de ejecución. Este repo explica **por qué** se hizo así y **cómo** funciona todo junto.

---

## Mapa de documentación

### Estrategia y marca

| Documento | Qué encontrás |
| --- | --- |
| [Identidad empresarial](docs/identidad-empresarial.md) | Visión, misión, meta estratégica, propuesta de valor, principios de marca y speech comercial |
| [Orux](docs/orux/README.md) | Nueva identidad empresarial, estrategia comercial, crecimiento y redes sociales |

### Módulos dinámicos

| Documento | Qué encontrás |
| --- | --- |
| [Flujo completo](docs/modules-dynamic/flow.md) | Recorrido de punta a punta: desde que AUREA define un módulo hasta que el tenant lo usa |
| [Alcance y POC](docs/modules-dynamic/product-scope.md) | Qué valida la POC, criterios de aceptación, fases de implementación |
| [Decisiones](docs/modules-dynamic/decisions.md) | Respuestas confirmadas sobre tenants, roles, planes, personalización y lifecycle |
| [Especificación técnica](docs/modules-dynamic/technical.md) | Modelo MongoDB, evaluación de capabilities, API, frontend y theming |

### CI/CD

| Documento | Qué encontrás |
| --- | --- |
| [CI/CD de Aurea](docs/ci.md) | Validaciones, autotag, publicación, deploy, smoke tests y notificaciones |

### Convenciones

| Documento | Qué encontrás |
| --- | --- |
| [Guía de contribución](docs/contributing.md) | Cómo nombrar archivos, escribir ADRs, documentar evidencias y mantener enlaces |

Los assets visuales (diagramas, mockups) están en [`assets/`](assets/).

---

## Ecosistema de repositorios

| Repo | Para qué sirve | Owner |
| --- | --- | --- |
| **[aurea-docs](https://github.com/aurea-io/aurea-docs)** | Documentación central — este repo | Federico · Nahuel |
| **[backoffice-be-aurea](https://github.com/aurea-io/backoffice-be-aurea)** | API backend (NestJS + MongoDB) | Federico · Nahuel |
| **[backoffice-fe-aurea](https://github.com/aurea-io/backoffice-fe-aurea)** | Frontend backoffice (React) | Federico · Nahuel |
| **[aurea-pages-template](https://github.com/aurea-io/aurea-pages-template)** | Template de páginas públicas por tenant | Federico · Nahuel |
| **[aurea-ci](https://github.com/aurea-io/aurea-ci)** | Pipelines de CI/CD | Federico · Nahuel |

---

## Reglas centrales

Estas reglas aplican a **todos** los repositorios. No son sugerencias, son decisiones tomadas.

1. **El backend es la fuente de verdad para autorización.** El JWT identifica al usuario; la decisión de acceso se resuelve consultando el estado vigente en cada request.

2. **Todo es tenant-scoped.** Cada consulta y mutación filtra por `tenantId`. Si un usuario intenta acceder a un recurso de otro tenant, la respuesta es `403` sin revelar si el recurso existe.

3. **El frontend pregunta, el backend decide.** Ocultar un botón mejora la UX, pero nunca reemplaza el guard del endpoint.

4. **La documentación viaja con el cambio.** Cada PR funcional debe actualizar la documentación correspondiente y enlazar el issue que lo implementa.

---

## Cómo contribuir

Antes de agregar o modificar documentación, leé la [guía de contribución](docs/contributing.md). Es corta.
