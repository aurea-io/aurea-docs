# Flujo obligatorio de trabajo

Esta política aplica a todos los repositorios de Aurea y se controla mediante
templates, workflows compartidos y protección de la rama `main`.

## Organización del backlog

El Project oficial es [Aurea Backlog](https://github.com/orgs/aurea-io/projects/2).
El campo `Status` del Project es la única fuente de verdad para el estado:

`Backlog` → `Ready` → `In progress` → `Review` → `Blocked` → `Done`

No se usan labels `status:*`. Las labels se reservan para clasificar el tipo y
el área funcional o técnica.

## Issues

Toda tarea debe usar un template y tener:

- exactamente una label `area:*`;
- una label `type:*`;
- responsable y prioridad;
- criterios de aceptación;
- repositorio afectado;
- evidencia requerida para cerrar, cuando corresponda.

Áreas iniciales:

`area:ci`, `area:reservas`, `area:auth`, `area:login`, `area:catalogo`,
`area:tenants`, `area:backoffice`, `area:frontend`, `area:backend`, `area:docs`,
`area:infra`, `area:billing`, `area:usuarios` y `area:branding`.

### Épicas

Una épica representa un resultado grande y coordinado. Debe describir el
objetivo, alcance, fuera de alcance, criterios de éxito y dependencias. Las
implementaciones se dividen en issues hijas para frontend, backend, CI,
documentación u otras áreas. Cada issue hija enlaza a la épica y a su PR.

## Branches y commits

Las branches usan el formato:

```text
feat/123-gestion-tenants
fix/456-error-login
chore/789-actualizar-ci
docs/321-documentar-reservas
```

Los commits siguen Conventional Commits:

```text
feat(reservas): agregar disponibilidad por servicio
fix(auth): corregir expiración del token
docs(login): documentar recuperación de contraseña
```

Los commits que no cumplan la convención bloquean el PR.

## Pull requests

Todo PR debe:

- tener un único objetivo revisable;
- tener exactamente una label `area:*`;
- incluir `Closes #123` o una referencia cross-repo ejecutable;
- cumplir los criterios de aceptación de la issue;
- incluir tests, build y evidencia cuando aplique;
- pasar CI, seguridad y validación de governance;
- recibir al menos una aprobación antes del merge.

La política recomendada de merge es **Squash and merge**. Las branches se
eliminan automáticamente después del merge.

## Automatización y límites de GitHub

`aurea-ci` contiene el workflow reutilizable `governance.yml`. Los repositorios
consumidores solo mantienen un caller pequeño. El workflow rechaza issues y
PRs sin exactamente un área y rechaza PRs sin referencia ejecutable.

La protección de `main` exige CI, Governance, revisión y ausencia de pushes
directos. GitHub limita branch protection en repositorios privados según el
plan; para aplicar el mismo enforcement a los backoffices privados se necesita
GitHub Pro/Team/Enterprise o habilitar reglasets de organización con permisos
de administrador de la organización.

## Cierre y evidencia

Una issue se cierra solo cuando el PR está mergeado y existe evidencia real del
flujo: captura o video desplegado, URL y entorno, pasos reproducibles y tests.
