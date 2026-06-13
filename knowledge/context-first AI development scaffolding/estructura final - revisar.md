## Context-First AI Development — Estructura final

---

### FASE 0 — Definición externa

_Con un agente externo, fuera del método._

- Qué se va a construir — análisis funcional del producto.
- Cómo se va a construir — arquitectura de software correcta.

_El programador experto puede saltear esta fase y documentar directamente._

---

### FASE 1 — Ejecución

**PILAR 1 — Contexto completo** Documentar todo el proyecto y sus decisiones antes de que el agente empiece. La IA ejecuta, vos decidís.

**PILAR 2 — Interpretación explícita** Antes de ejecutar, el agente declara cómo interpretó la instrucción y qué no puede resolver con certeza. El dev aprueba o corrige. Solo después ejecuta.

**PILAR 3 — Control humano** El agente valida su output antes de entregarlo. Cada corrección se documenta como regla nueva.

**PILAR 4 — Jerarquía de contexto** Lo crítico va primero y se repite al final. Las reglas base nunca se diluyen.

**PILAR 5 — Mantenimiento del contexto** Cada cierta cantidad de tareas, el agente audita todo el contexto acumulado, detecta contradicciones y reglas obsoletas, y propone una versión limpia. El dev aprueba.