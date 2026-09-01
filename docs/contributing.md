# Guía de contribución

Convenciones para mantener la documentación útil, consistente y fácil de encontrar.

---

## Estructura de carpetas

```text
aurea-docs/
├── README.md                  ← mapa general, repos y owners
├── docs/
│   ├── contributing.md        ← este archivo
│   └── <área>/                ← una carpeta por área funcional
│       ├── flow.md            ← flujo de punta a punta
│       ├── product-scope.md   ← alcance, POC y criterios
│       ├── decisions.md       ← decisiones confirmadas (ADRs)
│       └── technical.md       ← especificación técnica
└── assets/
    └── <área>/                ← imágenes y diagramas de esa área
```

Cada área nueva sigue la misma estructura. No hace falta crear los cuatro archivos si no aplican — creá solo los que tengan contenido real.

---

## Nombres de archivos

- **Minúsculas y guiones:** `product-scope.md`, no `ProductScope.md`
- **En inglés:** los nombres de archivo van en inglés para consistencia con el código. El contenido puede estar en español.
- **Sin prefijos numéricos:** el orden se define en el README, no en el nombre del archivo.

---

## ADRs (Architecture Decision Records)

Las decisiones importantes van en `decisions.md` dentro de cada área. No usamos un sistema de ADRs numerados porque genera archivos que nadie revisa.

Cada decisión debe tener:

```markdown
### Título corto de la decisión

- Qué se decidió y por qué.
- Alternativas que se descartaron (si es relevante).
- Quién participó.
```

Si una decisión se revierte, no la borres — agregá un bloque debajo explicando qué cambió y por qué.

---

## Diagramas

Usamos **Mermaid** embebido en Markdown. No pegamos screenshots de draw.io ni Figma como única fuente.

```markdown
​```mermaid
flowchart LR
  A[Paso 1] --> B[Paso 2]
​```
```

Tips:
- Usá `flowchart` para flujos, `sequenceDiagram` para interacciones entre sistemas.
- Si un diagrama supera ~15 nodos, dividilo en dos.
- Las imágenes complementarias (mockups, capturas) van en `assets/<área>/`.

---

## Evidencia para cerrar un issue

Un issue no se cierra solo con código mergeado. La evidencia mínima es:

| Qué | Ejemplo |
| --- | --- |
| **Captura o video** | Screenshot del backoffice desplegado, no de localhost |
| **URL o entorno** | `https://staging.aurea.io/...` |
| **Pasos reproducibles** | "Ir a Módulos → activar Reservas → verificar en la página pública" |
| **Tests** | Link al test que lo cubre, o al pipeline verde |
| **PR/commit** | Link directo al PR |

> No alcanza con una captura de un mock ni con una prueba solo local.

---

## Enlaces relativos

- Los enlaces entre documentos usan **rutas relativas** desde el archivo que enlaza.
- Los assets se referencian con `../../assets/<área>/archivo.png`.
- Antes de commitear, verificá que los enlaces funcionen. Un enlace roto es peor que no tener enlace.

---

## Versionado y sincronización

- La documentación se mantiene en `main`. No usamos branches de docs.
- Cada PR funcional en un repo de ejecución debe referenciar el cambio de docs correspondiente (puede ser otro PR en `aurea-docs` o una actualización en el mismo PR si el repo tiene docs locales).
- Los cambios de decisiones de arquitectura se documentan **antes** de implementar, no después.
