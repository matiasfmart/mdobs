#ia #coding #work
## 1. Introducción  

ACD (AI-Driven Contextual Development) es una metodología de desarrollo de software diseñada para trabajar con inteligencia artificial como actor principal en la generación y evolución del código.  
  
El objetivo no es solo “usar IA”, sino **controlar su comportamiento** mediante contexto explícito, estructurado y persistente.  
  
Este framework es:  
- Agnóstico de lenguaje  
- Agnóstico de arquitectura  
- Aplicable a frontend, backend, mobile o cualquier sistema  
  
---  
  
## 2. Problema que resuelve  
  
Cuando se usa IA sin estructura:  
  
- Pierde contexto entre prompts  
- Interpreta en lugar de ejecutar  
- Toma decisiones inconsistentes  
- Rompe código existente  
- No respeta arquitectura ni convenciones  
  
Esto ocurre porque la IA completa los “vacíos” con su propio criterio.  
  
ACD elimina esos vacíos.  
  
---  
  
## 3. Enfoque del framework  
  
La idea central es simple:  
  
> La IA nunca trabaja con contexto implícito.  
  
Cada interacción incluye todo lo necesario para que:  
- No tenga que asumir  
- No tenga que decidir fuera de reglas  
- No tenga que interpretar comportamiento  
  
---  
  
## 4. Principios fundamentales  
  
### 4.1 Contexto explícito y completo  
  
Cada ejecución incluye:  
- Arquitectura  
- Reglas  
- Convenciones  
- Ejemplos reales  
- Decisiones específicas del proyecto  
  
No depende de memoria conversacional.  
  
---  
  
### 4.2 La IA ejecuta, no diseña  
  
- No define arquitectura  
- No inventa patrones  
- No modifica estructuras sin indicación  
  
Opera dentro de límites definidos.  
  
---  
  
### 4.3 Documentación como sistema operativo  
  
La documentación no es descriptiva.    
Es **operativa**.  
  
Define:  
- Qué hacer  
- Cómo hacerlo  
- Qué no hacer  
  
---  
  
### 4.4 Desarrollo basado en intención  
  
El desarrollador:  
- Define una petición estructurada  
  
La IA:  
- Implementa dentro del marco definido  
  
---  
  
### 4.5 Eliminación de ambigüedad  
  
Si algo puede interpretarse de más de una forma    
→ debe documentarse  
  
---  
  
## 5. Estructura del framework  
  
El framework se basa en dos elementos:  
  
### 5.1 Documentación estructurada (/docs)  
  
### 5.2 Prompt base universal  
  
---  
  
## 6. Estructura de documentación  

/docs  
ARCHITECTURE.md  
HOW_TO_WORK.md  
HOW_TO_ADD.md  
HOW_TO_FIX.md  
HOW_TO_TEST.md  
AI_CONTEXT.md  
DECISIONS.md

  
---  
  
## 7. Descripción detallada de cada documento  
  
### 7.1 ARCHITECTURE.md  
  
Define cómo está construido el sistema.  
  
Incluye:  
- Tipo de arquitectura  
- Estructura de carpetas  
- Flujo de datos (ASCII)  
- Responsabilidad de cada capa  
  
Objetivo:  
Eliminar cualquier duda sobre la estructura del sistema.  
  
---  
  
### 7.2 HOW_TO_WORK.md  
  
Define cómo trabajar dentro del proyecto.  
  
Incluye:  
- Cómo interactuar con la IA  
- Qué está permitido modificar  
- Qué está prohibido  
- Reglas de alcance  
  
Objetivo:  
Controlar el comportamiento operativo del agente.  
  
---  
  
### 7.3 HOW_TO_ADD.md  
  
Define cómo agregar nuevas funcionalidades.  
  
Incluye:  
- Pasos exactos  
- Archivos a crear  
- Ejemplo real  
- Checklist de validación  
  
Objetivo:  
Estandarizar la creación de features.  
  
---  
  
### 7.4 HOW_TO_FIX.md  
  
Define cómo corregir errores.  
  
Incluye:  
- Cómo analizar bugs  
- Qué se puede modificar  
- Qué no se debe tocar  
- Cómo validar el fix  
  
Objetivo:  
Evitar efectos colaterales.  
  
---  
  
### 7.5 HOW_TO_TEST.md  
  
Define cómo escribir tests.  
  
Incluye:  
- Qué testear  
- Cómo estructurar tests (AAA)  
- Uso de mocks  
- Cobertura funcional  
  
Objetivo:  
Asegurar calidad y evitar tests incorrectos.  
  
---  
  
### 7.6 AI_CONTEXT.md (núcleo del sistema)  
  
Es el documento más importante.  
  
Contiene:  
- Stack tecnológico  
- Arquitectura resumida  
- Reglas inviolables  
- Convenciones de nombres  
- Ejemplos reales  
- Anti-patrones  
  
Es el contrato de ejecución de la IA.  
  
---  
  
### 7.7 DECISIONS.md (contexto operativo implícito)  
  
Este documento captura lo que no entra en ningún otro.  
  
Define:  
- Estructuras no evidentes  
- Flujos reales del sistema  
- Restricciones prácticas  
- Convenciones emergentes  
- Casos especiales  
  
No es un historial.  
  
Es un **mapa de cómo funciona realmente el sistema**.  
  
Objetivo:  
Evitar que la IA complete vacíos con decisiones propias.  
  
---  
  
## 8. Flujo de trabajo  
  
### Fase 1 — Definición inicial  
  
Se define:  
- Arquitectura  
- Stack  
- Reglas base  
  
---  
  
### Fase 2 — Scaffolding  
  
La IA genera:  
- Proyecto base  
- Estructura  
- Documentación  
  
---  
  
### Fase 3 — Desarrollo evolutivo  
  
Cada tarea sigue el mismo proceso:  
  
1. Se carga el contexto completo  
2. Se define la petición  
3. La IA ejecuta dentro de reglas  
  
---  
  
### Fase 4 — Persistencia de contexto  
  
Después de cada ejecución:  
  
La IA debe:  
  
- Detectar decisiones implícitas  
- Identificar comportamientos no documentados  
- Actualizar `DECISIONS.md` si corresponde  
  
---  
  
## 9. Prompt base universal  
  
Este prompt se utiliza para todas las tareas.  

## CONTEXTO DEL PROYECTO

[ARCHITECTURE.md]

## FORMA DE TRABAJO

[HOW_TO_WORK.md]

## AGREGAR

[HOW_TO_ADD.md]

## CORREGIR

[HOW_TO_FIX.md]

## TESTING

[HOW_TO_TEST.md]

## DECISIONES

[DECISIONS.md]

## CONTEXTO IA

[AI_CONTEXT.md]

## PETICIÓN

[DESCRIPCIÓN DE LA TAREA]

  
---  
  
## 10. Reglas operativas para la IA  
  
### Antes de ejecutar  
  
- Validar arquitectura  
- Detectar conflictos  
- Definir alcance  
- Listar archivos a modificar o crear  
  
---  
  
### Durante ejecución  
  
- No salir del scope  
- No modificar código no solicitado  
- No asumir comportamiento  
  
---  
  
### Después  
  
- Explicar cambios realizados  
- Actualizar `DECISIONS.md` si aplica  
  
---  
  
## 11. Rol del desarrollador  
  
El desarrollador:  
  
- Define la intención (prompt)  
- Valida resultados  
- Aprueba cambios  
- Mantiene calidad del contexto  
  
No delega decisiones críticas sin revisión.  
  
---  
  
## 12. Beneficios  
  
- Consistencia en todo el código  
- Reducción de errores  
- Escalabilidad del equipo  
- Onboarding rápido  
- Independencia del contexto conversacional  
  
---  
  
## 13. Riesgos  
  
### Documentación incompleta  
→ La IA vuelve a improvisar  
  
### Uso incorrecto del prompt base  
→ Se pierde consistencia  
  
### Sobrecarga de información  
→ Reduce claridad  
  
---  
  
## 14. Cuándo aplicar  
  
Recomendado en:  
  
- Proyectos medianos o grandes  
- Equipos que usan IA activamente  
- Sistemas con evolución constante  
  
No necesario en:  
  
- Scripts simples  
- Prototipos descartables  
  
---  
  
## 15. Resultado esperado  
  
Un sistema donde:  
  
- La IA no improvisa  
- El contexto es completo  
- Las decisiones son explícitas  
- El código es consistente  
  
---  
  
## 16. Resumen ejecutivo  
  
ACD es un marco donde:  
  
1. Se define arquitectura y reglas  
2. Se documentan de forma estructurada  
3. Se usa siempre el mismo prompt con contexto completo  
4. La IA desarrolla dentro de ese marco  
5. Las decisiones implícitas se documentan en `DECISIONS.md`  
  
Resultado:  

---

## Ver también

- [[backend ACD fwk]] — implementación backend del framework
- [[design-system-shadcn]]
Desarrollo predecible, controlado y escalable con IA.