---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Backend
  - Server Actions
---
# Server Actions: Mutaciones Atómicas

##  Centralización de Mutaciones

Las Server Actions centralizan la mutación de datos para mantener los Server Components limpios y seguros.

### Acciones por Dominio

#### Gestión de Proyectos/Tickets (`tickets.ts`)

```typescript
// app/actions/tickets.ts
'use server'

#### 3. Gastos y Finanzas (`expenses.ts`)
- *Restringido:* Estas acciones verifican internamente el rol `CEO` antes de proceder.
- `registerExpense(data)`: Asocia gastos a proyectos específicos para calcular la rentabilidad real.

## 🔗 Relacionado
- [[../02 - Base de Datos/Modelos Prisma|Ver Modelos de Datos Relacionados]]
- [[../01 - Arquitectura/Arquitectura General|Arquitectura de Capas]]
