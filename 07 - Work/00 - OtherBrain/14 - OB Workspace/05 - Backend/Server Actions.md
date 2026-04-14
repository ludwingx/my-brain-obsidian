---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Backend
  - ServerActions
---
# Server Actions y Lógica de Negocio

## 🧠 Desacoplamiento de Lógica Core

En **OB Workspace**, para mantener los componentes de servidor (RSC) limpios y seguros, toda la mutación de datos se centraliza en la carpeta `actions/`. Esto permite que Next.js maneje automáticamente la seguridad de los endpoints y proporcione tipos estrictos de TypeScript entre el front y el back.

### Acciones por Dominio

#### 1. Gestión de Proyectos y Tickets (`tickets.ts`)
- `createTicket(data)`: Valida el esquema con **Zod**, inserta en Prisma y dispara una `revalidatePath('/dashboard/projects')`.
- `updateTicketStatus(id, status)`: Cambia el estado en el Kanban. Implementa lógica de **Optimistic Updates** en el cliente para una respuesta instantánea.
- `assignCollaborator(ticketId, userId)`: Maneja la relación N:N de colaboradores en Prisma.

#### 2. Time Tracking (`tracking.ts`)
- `startWorkSession(subtaskId)`: Crea la `WorkSession` y cambia el estado de la subtarea a `DOING` en una única transacción de Prisma (`$transaction`).
- `stopWorkSession(sessionId)`: Calcula la duración final, cierra la sesión y actualiza el `realTime` acumulado de la subtarea.

#### 3. Gastos y Finanzas (`expenses.ts`)
- *Restringido:* Estas acciones verifican internamente el rol `CEO` antes de proceder.
- `registerExpense(data)`: Asocia gastos a proyectos específicos para calcular la rentabilidad real.

## 🔗 Relacionado
- [[../02 - Base de Datos/Modelos Prisma|Ver Modelos de Datos Relacionados]]
- [[../01 - Arquitectura/Arquitectura General|Arquitectura de Capas]]
