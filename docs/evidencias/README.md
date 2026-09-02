# 📸 Registro de Evidencias de Pruebas y Validación (QA)

Este directorio almacena el historial cronológico y versionado de las ejecuciones de prueba, validaciones visuales en navegador (Google Chrome), suites de integración y auditorías de código del ecosistema **Aurea**.

---

## 📂 Convención de Nombres de Subcarpetas

Cada sesión de validación y generación de evidencias se almacena en una subcarpeta identificada por la fecha y hora de inicio de la prueba:

```text
evidencias/<YY-MM-DD>_<HH-mm>/
```

### Formato:
* **`YY-MM-DD`**: Año (2 dígitos), Mes y Día de la prueba (ej: `26-09-02`).
* **`HH-mm`**: Hora y Minutos en formato de 24 horas (ej: `01-03`).

---

## 📑 Estructura Requerida en Cada Subcarpeta

Cada subcarpeta de evidencia contiene:
1. **`README.md`**: Reporte técnico de la sesión (versión de cada producto auditado, entorno, credenciales de prueba, matriz de tests, reporte de discrepancias detectadas y tareas/issues creados en GitHub).
2. **`capturas/`**: Subcarpeta dedicada a almacenar exclusivamente los archivos de imagen (`*.png` / `*.webp`) numerados y descriptivos para mantener el directorio ordenado.

```text
evidencias/<YY-MM-DD>_<HH-mm>/
├── README.md
└── capturas/
    ├── 01_login_page.png
    ├── 02_dashboard_superadmin.png
    └── ...
```

---

## 🗂️ Historial de Ejecuciones

| Sesión (Fecha/Hora) | BE Aurea (`backoffice-be`) | FE Aurea (`backoffice-fe`) | BE Cliente | FE Cliente (`pages-template`) | Estado | Reporte |
| :--- | :---: | :---: | :---: | :---: | :---: | :--- |
| [`26-09-02_01-25`](./26-09-02_01-25/README.md) | `v0.18.0` | `v0.1.2` | N/A | `v0.4.0` | 🟡 AUDITORÍA | Auditoría de arquitectura profunda: taxonomía `services.bookings`, scopes `platform` vs `tenant` y tareas de refactor. |
| [`26-09-02_01-03`](./26-09-02_01-03/README.md) | `v0.18.0` | `v0.1.2` | N/A | `v0.4.0` | 🟢 APROBADO | Pruebas E2E en Chrome, RBAC/FBAC, Theme Service y Gestión de Tenants. |
