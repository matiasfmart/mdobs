## Pre-análisis

  

### 1. Roles del equipo y cobertura

  

| Rol | Necesidades documentales principales |

|-----|--------------------------------------|

| Líder de Producto | Estado de proyectos, funcional, casos de uso, roadmap |

| Analista | Especificaciones, relevamientos, reglas de negocio, flujos |

| Dev Back End | Arquitectura, módulos API, DB, implementación, guías técnicas |

| Dev Front End | Componentes, guías de implementación, sistema de diseño, módulos UI |

| Dev Full Stack | Todo lo anterior combinado |

| Dev Back End y Mobile | Arquitectura backend + guía mobile (stack a definir) |

| QA | Planes de prueba, Xray, criterios de aceptación, ambientes |

| Diseño | Sistema de diseño, guía de estilo, activos, flujos UX |

| Arquitecto | Decisiones arquitectónicas, patrones globales, DECISIONS.md |

| Líder Técnico | Releases, Runbooks, trazabilidad PR↔tarea, infraestructura, coordinación con externos |

  

Todos los roles tienen representación explícita. El Líder Técnico tiene la mayor transversalidad y se refleja en múltiples secciones.

  

---

  

### 2. Mapa de duplicación — dónde vive cada dato y por qué

  

| Dato | Vive en | Motivo |

|------|---------|--------|

| Integrantes, roles, horarios | **Space General** | Es un dato del equipo, no de un proyecto. Sería ruido replicarlo en cada proyecto |

| Stack tecnológico global | **Space General** | Es una decisión de equipo; los proyectos lo aplican pero no lo definen |

| Versiones/libs específicas de un proyecto | **Space de Proyecto** | Puede diferir del stack global (ej: versión de EF Core, versión de Next.js) |

| Convenciones de código, branching, SemVer | **Space General** | Son transversales. Un dev nuevo las consulta una sola vez, no una por proyecto |

| Cómo aplicar una convención en un proyecto concreto | **Space de Proyecto** | Las excepciones o adaptaciones son propias del proyecto |

| Template de Runbook | **Space General** | Es el proceso; la plantilla es reutilizable |

| Runbooks de releases | **Space de Proyecto** | El contenido cambia por proyecto y release |

| Proceso general de release | **Space General** | Es el "cómo se hace", independiente del proyecto |

| Historial de releases de un proyecto | **Space de Proyecto** | Es un registro vivo del proyecto específico |

| Política de gobernanza Oracle | **Space General** | Es una política del equipo acordada con el sector DBA |

| Objetos de DB, stored procedures, permisos de un proyecto | **Space de Proyecto** | Son datos propios de cada sistema |

| Guía de uso de AI_CONTEXT.md y DECISIONS.md | **Space General** | Es metodología del equipo |

| AI_CONTEXT.md real / DECISIONS.md real | **Space de Proyecto** | El contenido es propio de cada proyecto |

| Descripción funcional y de negocio | **Space de Proyecto** | Cada proyecto tiene su dominio distinto |

| Integraciones con COTO retail (descripción del contrato global) | **Space General** | Es un vínculo institucional del equipo con Retail |

| Implementación técnica de una integración en un proyecto | **Space de Proyecto** | Endpoints, auth y flujos son propios de ese proyecto |

| Rutas de logs y ubicaciones físicas | **Space de Proyecto** | Son datos de infraestructura específicos de cada app |

| Proceso de coordinación con equipos externos | **Space General** | Es una práctica del equipo, no de un proyecto |

| Entornos (Desarrollo / QA / Producción): política | **Space General** | La política de entornos es del equipo |

| Entornos: URLs, servidores, paths concretos | **Space de Proyecto** | Cada proyecto tiene sus propias instancias |

| Sistema de diseño / componentes compartidos | **Space General** | Aplica a todos los proyectos que tienen UI |

| Guías UX específicas de un proyecto | **Space de Proyecto** | Los flujos de usuario son propios de cada producto |

  

---

  

## Estructura propuesta

  

---

  

### Space General: `Zona E Tech (ZET)`

  

```

ZET / Inicio

├── Quiénes somos

├── Cómo usar esta documentación

└── Onboarding — lectura de inicio para nuevos integrantes

├── Onboarding Líder de Producto

├── Onboarding Analista

├── Onboarding Dev Back End

├── Onboarding Dev Front End

├── Onboarding Dev Full Stack

├── Onboarding Dev Back End y Mobile

├── Onboarding QA

├── Onboarding Diseño

├── Onboarding Arquitecto

└── Onboarding Líder Técnico

  

ZET / Equipo

├── Integrantes, roles y horarios

├── Organigrama y estructura del área

└── Equipos externos con los que coordinamos

├── Administración Tecnologías Microsoft

├── Administración Tecnologías Linux

├── Sector DBA Oracle

├── Retail / Sistemas COTO

└── Cuántica (E-commerce China)

  

ZET / Proyectos

├── Mapa de proyectos (estado, space Confluence, Jira board, responsables)

└── Relación entre proyectos (Ze Portal como hub centralizador)

  

ZET / Estándares y Convenciones

├── Convenciones de código

│ ├── .NET — nombrado, estructura, patrones

│ ├── React y TypeScript — nombrado, estructura

│ └── Mobile — [completar cuando se defina el stack]

├── Gestión de versiones

│ ├── Git Flow — estrategia de branching

│ ├── SemVer — versionado semántico

│ └── Nomenclatura de ramas, commits y PRs

└── Metodología de desarrollo asistido por IA

├── Scaffolding estructurado — flujo de prompts

├── AI_CONTEXT.md — qué es, cómo se crea, cómo se mantiene

└── DECISIONS.md — qué es, cómo se usa, ejemplos

  

ZET / Arquitectura Global

├── Stack tecnológico del equipo

├── Patrones arquitectónicos adoptados

│ ├── Vertical Slice Architecture

│ ├── MediatR — uso y convenciones

│ └── EF Core — convenciones de acceso a datos

├── Decisiones arquitectónicas transversales (DECISIONS.md global)

└── Integración con sistemas COTO retail

└── Descripción institucional del vínculo (no datos de implementación)

  

ZET / Base de Datos

├── Política de gobernanza Oracle

├── Gestión de permisos (por usuario, tabla, stored procedure)

├── Convenciones de scripts y trazabilidad en CI/CD

├── DBLinks — convenciones de uso

└── Proceso de coordinación con el sector DBA

  

ZET / Infraestructura

├── Política de entornos (Desarrollo / QA / Producción)

├── IIS — guía general de despliegue

├── Docker — guía general de despliegue

├── Windows Task Scheduler — convenciones de uso

└── Proceso de coordinación con equipos de infraestructura

  

ZET / Proceso de Releases

├── Flujo general de un release (paso a paso)

├── Template de Runbook

├── Trazabilidad PR ↔ tarea Jira (metodología manual)

├── Coordinación con equipos externos para pases a producción

└── Gestión de tickets Jira para cambios en producción

  

ZET / QA y Testing

├── Proceso general de QA del equipo

├── Xray — guía de uso y convenciones

└── Criterios de aceptación — plantilla estándar

  

ZET / Diseño

├── Sistema de diseño — componentes compartidos

├── Guía de estilo (tipografía, colores, iconografía)

├── Activos compartidos (logos, recursos)

└── Proceso de handoff Diseño → Desarrollo

```

  

**Justificación del Space General:** Concentra todo lo que es del equipo como unidad, no de un proyecto en particular. Un dev que entre nuevo puede leer este space y entender cómo opera el equipo antes de entrar a cualquier proyecto.

  

---

  

### Template de Space de Proyecto

  

Todos los proyectos activos siguen esta estructura. Las variaciones por proyecto se detallan abajo.

  

```

[PROYECTO] / Inicio

├── Descripción del producto (qué es, para quién, valor que genera)

├── Estado actual

└── Links rápidos: Jira board, repositorio Bitbucket, ambientes

  

[PROYECTO] / Negocio y Funcional

├── Visión del producto y objetivos

├── Actores / usuarios del sistema

├── Glosario del dominio

├── Casos de uso y flujos principales

├── Reglas de negocio

└── Integraciones externas — descripción funcional (qué hace el sistema externo,

no cómo se conecta; eso va en Técnico)

  

[PROYECTO] / Análisis

├── Especificaciones funcionales por feature / módulo

├── Historial de relevamientos

└── Decisiones de producto (qué se priorizó y por qué)

  

[PROYECTO] / Arquitectura y Decisiones

├── AI_CONTEXT.md del proyecto (o equivalente en Confluence)

├── DECISIONS.md del proyecto

├── Diagrama de arquitectura

├── Módulos y estructura del repositorio

├── Modelo de datos

└── Diagramas de flujo de datos / secuencia

  

[PROYECTO] / Módulos

│ (Una subpágina por módulo relevante)

└── [Módulo X]

├── Descripción y responsabilidad

├── Endpoints / contratos de API

├── Lógica de negocio relevante

└── Modelo de datos del módulo

  

[PROYECTO] / Guías de Implementación

├── Setup del entorno local

├── Cómo correr el proyecto

├── Guías técnicas específicas del proyecto

└── Troubleshooting frecuente

  

[PROYECTO] / Infraestructura del Proyecto

├── Entornos

│ ├── Desarrollo — URL, servidor, path físico

│ ├── QA — URL, servidor, path físico

│ └── Producción — URL, servidor, path físico, IIS site / contenedor Docker

├── Rutas de logs por entorno

├── Servicios en IIS / Docker / Task Scheduler

├── Variables de entorno y configuración (sin secretos)

└── BATs / servicios batch (descripción, cuándo ejecutar, cómo)

  

[PROYECTO] / Base de Datos del Proyecto

├── Esquema y objetos de DB (tablas, vistas, stored procedures)

├── Permisos específicos del proyecto

├── Scripts de migración / historial

└── DBLinks utilizados

  

[PROYECTO] / Integraciones Técnicas

│ (Una subpágina por integración)

└── [Integración X]

├── Descripción técnica

├── Endpoints, contratos y autenticación

├── Diagrama de flujo de la integración

└── Consideraciones de error y retry

  

[PROYECTO] / QA del Proyecto

├── Plan de pruebas

├── Casos de prueba (Xray — link o embebido)

├── Ambiente de testing

└── Criterios de aceptación por módulo

  

[PROYECTO] / Diseño del Proyecto

├── Flujos UX (Figma embebido o links)

├── Decisiones de UX propias del proyecto

└── Componentes específicos no cubiertos por el sistema de diseño global

  

[PROYECTO] / Releases

├── Historial de releases (tabla: versión, fecha, cambios)

└── Runbooks

└── [Runbook vX.Y.Z]

├── Cambios incluidos (con link a Jira)

├── PRs asociados

├── Scripts de DB a ejecutar

├── Pasos de despliegue

├── Verificación post-deploy

└── Procedimiento de rollback

```

  

---

  

### Variaciones por proyecto

  

**Brick**

- En "Integraciones Técnicas": pagos, firma digital (completar)

- En "Negocio y Funcional": documentar el modelo de participaciones y rendimientos — es el corazón del negocio

- Módulos previstos: API, backoffice web, app mobile

  

**Ze Portal**

- En "Módulos": un módulo por cada sistema integrado (Brick backoffice, Web Zona E backoffice, Reporterías gerenciales, Reporterías operativas, etc.)

- En "Arquitectura": documentar el modelo hub — cómo se incorporan nuevos proyectos

- En "Negocio y Funcional": describir qué sectores de Zona E reportan acá y qué reportes existen

  

**Web Zona E**

- En "Integraciones Técnicas": integración con sistema de caja COTO (con referencia al documento institucional del Space General), servicios de facturación propios, orquestador

- En "Infraestructura": documentar Manager de Caja y BATs con detalle operativo

  

**Zona E Pizzería**

- Estado: Finalizado → agregar una sección "Mejoras planificadas" en Negocio y Funcional

- Módulos: tablet frontend, panel cocina, panel barra

- En "Integraciones Técnicas": sistema de pago externo (completar cuál)

  

**Comandas**

- Estado: En planificación → el space se crea pero con una portada que indique el estado

- La sección "Análisis" será la más activa en esta etapa; el resto tendrá páginas vacías con estado "Por completar"

- No tiene Runbooks todavía

  

**E-commerce China**

- **No se crea un space propio** — el equipo no tiene visibilidad técnica ni responsabilidad de desarrollo

- En el Space General (`ZET / Proyectos / Mapa de proyectos`) aparece como entrada de referencia con: estado, responsable (Cuántica), punto de contacto, y nota explícita de que la documentación técnica es responsabilidad del proveedor

  

---

  

## Revisión de consistencia

  

**Inconsistencias detectadas y resueltas:**

  

1. **Ze Portal describe backoffices de otros proyectos.** La funcionalidad de un backoffice (ej: backoffice de Brick) se documenta en el space de Brick. Ze Portal documenta cómo integra esos backoffices, no qué hace cada uno — sin duplicación.

  

2. **Integración COTO retail aparece en dos lugares.** El vínculo institucional (qué es, quién es el contacto, la naturaleza del acuerdo) vive en el Space General. El contrato técnico concreto (endpoints, autenticación, flujo de error) vive en Web Zona E / Integraciones Técnicas. Son datos distintos.

  

3. **Onboarding por rol en el Space General.** Cada página de onboarding enlaza a los spaces de proyectos que le corresponden al rol, sin replicar contenido. Es una puerta de entrada, no un duplicado.

  

4. **Rutas de logs y paths físicos.** Están en "Infraestructura del Proyecto" de cada space, no en el Space General — son datos que cambian por proyecto y por entorno. El Space General solo documenta la política (cómo deben nombrarse los logs, dónde deben almacenarse), no los valores concretos.

  

5. **DECISIONS.md tiene dos niveles.** El Space General tiene un DECISIONS.md para decisiones transversales al equipo (ej: "adoptamos Vertical Slice como patrón global"). Cada proyecto tiene su propio DECISIONS.md para decisiones de ese dominio. No se superponen si se mantiene la regla: si aplica a más de un proyecto, es decisión del equipo.

  

6. **Comandas no tiene Jira board ni Confluence space creados aún.** La estructura los contempla; si no existen todavía, las páginas del space se crean con estado de placeholder. El Mapa de proyectos en el Space General indica el estado real.

  

7. **Mobile sin stack definido.** La sección de Mobile en Estándares y Convenciones se crea con estado "Pendiente — definir stack". El Onboarding de Dev Back End y Mobile referencia esa sección con nota de que está en construcción. No se documenta antes de decidir.