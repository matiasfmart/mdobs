#work #db
# DB Utilities - Gestión de Scripts de Base de Datos

Este repositorio centraliza todos los scripts de base de datos para deploys a producción. Cada proyecto mantiene su historial independiente de releases.

## 📁 Estructura del Repositorio

```
db_utilities/
├── README.md (este archivo)
├── proyecto_A/
│   ├── releases/
│   │   ├── v1.0.0_sprint50_20250108/
│   │   │   ├── ZWR-2928_01_crear_tabla_usuarios.sql
│   │   │   ├── ZWR-2928_02_agregar_indices.sql
│   │   └── v1.1.0_sprint52_20250122/
│   │       └── ...
│   └── rollbacks/
│       └── v1.0.0_sprint50/
│           └── 001_rollback_completo.sql
│
├── proyecto_B/
│   ├── releases/
│   └── rollbacks/
│
└── proyecto_C/
    ├── releases/
    └── rollbacks/
```

---

## 🔄 Flow de Trabajo para Desarrolladores

### 1️⃣ Desarrollo de Feature

Cuando desarrollas una feature que requiere cambios en base de datos:

1. **Crear branch** en el repositorio principal del código
2. **Desarrollar código + scripts** de BD localmente
3. **Probar scripts** en tu ambiente de desarrollo (dev/local)
4. **Crear Pull Request** del código en el repositorio principal
5. **Si la feature requiere cambios de BD:**
    - La carpeta de release (`proyecto_X/releases/vX.X.X_sprintXX_YYYYMMDD/`) será creada previamente
    - Subir tus scripts a la carpeta de release correspondiente
    - **IMPORTANTE:** Nombrar scripts con el **número de ticket de JIRA** y el **orden de ejecución**
    - Crear un **PR en este repositorio** (db_utilities)
    - **En el PR del código**, mencionar:

```
     Este PR requiere cambios en BD
     Scripts en: [URL del PR de db_utilities]
```

---

## 📋 Convenciones y Estándares

### Nomenclatura de Scripts

**FORMATO OBLIGATORIO:**

```
[TICKET-JIRA]_[NN]_[descripcion].sql

Donde:
- TICKET-JIRA = Número de ticket (ej: ZWR-2928)
- NN          = Número de orden de ejecución dentro de tu feature (01, 02, 03...)
- descripcion = Lo que hace el script en snake_case

✅ Ejemplos correctos:
ZWR-2928_01_crear_tabla_usuarios.sql
ZWR-2928_02_agregar_columna_email.sql
ZWR-2928_03_crear_indice_usuarios_email.sql
ZWR-3041_01_crear_tabla_pedidos.sql
ZWR-3041_02_migrar_datos_pedidos_legacy.sql

❌ Ejemplos incorrectos:
crear_tabla_usuarios_01.sql           (falta ticket JIRA)
ZWR-2928_crear_tabla.sql              (falta orden de ejecución)
script_usuarios.sql                   (falta ticket y orden)
001_crear_tabla.sql                   (falta ticket JIRA)
```

---

**Última actualización:** 2026-08-01  
---

## Ver también

- [[ARQUITECTURA_TECNICA_ZCO]]
- [[ESTRUCTURA_PROYECTO_ZCO]]**Versión de este README:** 1.0