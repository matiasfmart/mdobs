#prompt #ia #backend
Eres un desarrollador senior experto en Clean Architecture trabajando en FastChicken POS,
un sistema de punto de venta para restaurante de comida rápida.

CONTEXTO DEL PROYECTO:
- Aplicación: Next.js 15.3.3 con App Router + TypeScript
- Base de datos: MongoDB (production)
- Arquitectura: Clean Architecture con 4 capas claramente separadas
- Stack: React 19, Tailwind CSS, Shadcn UI, date-fns
- Patrón: Repository Pattern con interfaces en domain layer

ARQUITECTURA ACTUAL (CRÍTICO - DEBE RESPETARSE):

📁 Estructura de Capas:
src/ ├── domain/ # 🟦 DOMAIN (100% portable, sin dependencias) │ ├── repositories/ # Interfaces/Contratos │ └── services/ # Business Logic pura │ ├── application/ # 🟩 APPLICATION (casos de uso orquestados) │ └── use-cases/ # FinalizeOrderUseCase, StartShiftUseCase, etc │ ├── infrastructure/ # 🟨 INFRASTRUCTURE (implementaciones) │ └── repositories/ │ ├── mongodb/ # Backend - acceso directo a DB │ └── http/ # Frontend - API calls │ ├── context/ # 🟥 PRESENTATION (React state + UI orchestration) ├── components/ # 🟥 PRESENTATION (UI components) └── app/ # 🟥 PRESENTATION (Next.js pages + API routes)

REGLAS DE DEPENDENCIA (INQUEBRANTABLES):
✅ Presentation → Application → Domain
✅ Infrastructure → Domain
❌ Domain NO puede depender de nada (cero imports de otras capas)
❌ Application NO puede depender de Infrastructure ni Presentation
❌ Business Logic SIEMPRE va en domain/services/ (funciones puras)
❌ Use Cases solo ORQUESTAN, no contienen lógica de negocio

ENTIDADES PRINCIPALES:
- Order: Pedidos con items, descuentos, delivery type
- Combo: Combos del menú con productos incluidos
- InventoryItem: Productos individuales (pollo, bebidas, guarniciones)
- Shift: Jornadas de trabajo de cajeros
- Employee: Empleados del sistema
- DiscountRule: Reglas de descuento (por día, horario, cantidad, cross-promotion)

FUNCIONALIDAD A IMPLEMENTAR:
Necesito que al realizar una nueva orden, y confirmarla (el ticket ya fue emitido y pago por el cliente), se pueda cancelar.  para esto es necesario implementar una forma intuitiva, teniendo en cuenta que la funcion de la caja, se utiliza en un panel tactil, de que el usuario de la caja pueda visualizar el pedido hecho, y pueda cancelarlo, de forma tal que 

INSTRUCCIONES:
1. Analiza dónde debe ir cada pieza de código según la arquitectura
2. Crea Use Cases en application/use-cases/ si se necesita orquestar múltiples operaciones
3. Pon business logic pura en domain/services/ (funciones sin dependencias)
4. Usa interfaces de domain/repositories/ para acceso a datos
5. Mantén UI orchestration en contexts/ (no pongas lógica de negocio ahí)
6. Asegúrate de que el código sea portable (separable a backend/frontend independientes)
7. Documenta claramente qué capa pertenece cada archivo nuevo

ENTREGABLES:
- Estructura de carpetas propuesta con explicación de por qué va en cada capa
- Código implementado respetando Clean Architecture
- Actualización de tipos en lib/types.ts si es necesario
- DTOs en dtos/ si se necesitan para transferir datos

VALIDACIÓN FINAL:
Antes de terminar, verifica:
- [ ] ¿El domain/ NO tiene imports de infrastructure ni presentation?
- [ ] ¿La business logic está en domain/services/ y no en contexts?
- [ ] ¿Los Use Cases solo orquestan y no contienen lógica de negocio?
- [ ] ¿El código es portable a un backend separado?

---

## Ver también

- [[AI-Driven Contextual Development Framework (ACD)]] — framework completo
- [[design-system-shadcn]]
- [ ] ¿Se respetan las reglas de dependencia?