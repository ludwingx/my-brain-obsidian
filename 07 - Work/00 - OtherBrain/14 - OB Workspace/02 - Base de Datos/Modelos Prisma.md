---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Prisma
  - SQL
---
# Prisma Schema y Tracking Avanzado

##  Modelos Clave del SaaS

La aplicación está diseñada sobre **PostgreSQL**, aprovechando relaciones para delegar cálculos a sub-queries.

### 1. Motor de Tickets y Subtareas

Los tiempos reales de finalización no son quemados manualmente en el ticket (`hardcoded`).

```prisma
model Subtask {
  id            String        @id @default(cuid())
  title         String
  description   String?
  status        SubtaskStatus @default(TODO)
  estimatedTime Int           // En Minutos
  realTime      Int           @default(0) // Agrupado de WorkSessions
  order         Int           @default(0) // Para ordenamiento visual

  ticketId      String
  ticket        Ticket        @relation(fields: [ticketId], references: [id], onDelete: Cascade)

  workSessions  WorkSession[]
  createdAt     DateTime      @default(now())
  updatedAt     DateTime      @updatedAt
}

enum SubtaskStatus {
  TODO
  DOING
  DONE
  BLOCKED
}
```

**Regla Dorada:** La sumatoria de `realTime` del Ticket, nace de agrupar las iteraciones subyacentes.

```prisma
model Ticket {
  id            String    @id @default(cuid())
  title         String
  description   String?
  status        TicketStatus @default(TODO)
  priority      TicketPriority @default(MEDIUM)
  estimatedTime Int       // Suma de subtareas
  realTime      Int       @default(0) // Suma calculada de subtareas

  projectId     String
  moduleId      String?
  leadId        String?   // Desarrollador principal
  creatorId     String   // Quién creó el ticket

  project       Project   @relation(fields: [projectId], references: [id], onDelete: Cascade)
  module        Module?   @relation(fields: [moduleId], references: [id], onDelete: SetNull)
  lead          User?     @relation("TicketLead", fields: [leadId], references: [id])
  creator       User      @relation("TicketCreator", fields: [creatorId], references: [id])

  subtasks      Subtask[]
  collaborators TicketCollaborator[]

  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

enum TicketStatus {
  TODO
  IN_PROGRESS
  IN_REVIEW
  DONE
  BLOCKED
  CANCELLED
}

enum TicketPriority {
  LOW
  MEDIUM
  HIGH
  CRITICAL
}
```

### 2. Máquina de Estado del "Punch Clock"

Para evitar fraude, las horas se miden en ventanas cerradas:

```prisma
model WorkSession {
  id        String    @id @default(cuid())
  startTime DateTime  @default(now())
  endTime   DateTime?
  duration  Int       @default(0) // Mutado al cerrarse (en minutos)

  userId    String
  subtaskId String

  user      User      @relation(fields: [userId], references: [id], onDelete: Cascade)
  subtask   Subtask   @relation(fields: [subtaskId], references: [id], onDelete: Cascade)

  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
}
```

**Flujo de Time Tracking:**

1. **Start:** Usuario inicia WorkSession en una Subtarea
2. **Doing:** Subtarea cambia a estado `DOING`
3. **Stop:** Usuario cierra sesión, se calcula `duration`
4. **Update:** `duration` se suma a `realTime` de Subtarea
5. **Auto-Stop (Futuro):** Si `endTime` falta después de 10 horas, auto-stop para prevenir errores

### 3. Proyectos y Módulos

```prisma
model Project {
  id          String    @id @default(cuid())
  slug        String    @unique
  name        String
  description String?
  budget      Float?    // Presupuesto asignado
  status      ProjectStatus @default(ACTIVE)

  clientId    String?   // Cliente externo

  client      User?     @relation("ProjectClient", fields: [clientId], references: [id])
  modules     Module[]
  tickets     Ticket[]
  expenses    Expense[]

  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}

enum ProjectStatus {
  PLANNING
  ACTIVE
  ON_HOLD
  COMPLETED
  CANCELLED
}

model Module {
  id          String    @id @default(cuid())
  name        String
  description String?
  order       Int       @default(0)

  projectId   String

  project     Project   @relation(fields: [projectId], references: [id], onDelete: Cascade)
  tickets     Ticket[]

  createdAt   DateTime  @default(now())
  updatedAt   DateTime  @updatedAt
}
```

### 4. Usuarios y Roles

```prisma
model User {
  id            String    @id @default(cuid())
  email         String    @unique
  name          String
  role          UserRole  @default(DEVELOPER)
  image         String?

  // Relaciones
  ticketsLead   Ticket[]  @relation("TicketLead")
  ticketsCreated Ticket[] @relation("TicketCreator")
  workSessions  WorkSession[]
  projectClient Project[] @relation("ProjectClient")

  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt
}

enum UserRole {
  CEO
  DEVELOPER
  INTERN
  EXTERNAL_CLIENT
}
```

### 5. Finanzas y Gastos

```prisma
model Expense {
  id          String        @id @default(cuid())
  title       String
  amount      Float
  type        ExpenseType
  description String?
  date        DateTime      @default(now())

  projectId   String?

  project     Project?      @relation(fields: [projectId], references: [id], onDelete: SetNull)

  createdAt   DateTime      @default(now())
  updatedAt   DateTime      @updatedAt
}

enum ExpenseType {
  FIXED        // Gastos fijos mensuales
  VARIABLE     // Pagos a colaboradores
  ONE_TIME     // Gastos únicos
  RECURRING    // Suscripciones
}
```

### 6. Colaboradores en Tickets

```prisma
model TicketCollaborator {
  id        String   @id @default(cuid())
  ticketId  String
  userId    String

  ticket    Ticket   @relation(fields: [ticketId], references: [id], onDelete: Cascade)
  user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)

  createdAt DateTime @default(now())

  @@unique([ticketId, userId])
}
```

##  Queries Avanzadas y Cálculos

### 1. Calcular Tiempo Real de Ticket

```typescript
// Sumar realTime de todas las subtareas
const ticketWithRealTime = await prisma.ticket.findUnique({
  where: { id: ticketId },
  include: {
    subtasks: {
      select: {
        realTime: true
      }
    }
  }
})

const totalRealTime = ticketWithRealTime.subtasks.reduce(
  (sum, subtask) => sum + subtask.realTime,
  0
)
```

### 2. Calcular Rentabilidad de Proyecto

```typescript
// Ingresos - Gastos
const projectProfitability = await prisma.project.findUnique({
  where: { id: projectId },
  include: {
    expenses: true
  }
})

const totalExpenses = projectProfitability.expenses.reduce(
  (sum, expense) => sum + expense.amount,
  0
)

const profit = projectProfitability.budget - totalExpenses
const margin = (profit / projectProfitability.budget) * 100
```

### 3. Métricas de Desarrollador

```typescript
// Horas trabajadas en el último mes
const developerMetrics = await prisma.workSession.groupBy({
  by: ['userId'],
  where: {
    userId: developerId,
    startTime: {
      gte: new Date(Date.now() - 30 * 24 * 60 * 60 * 1000)
    }
  },
  _sum: {
    duration: true
  }
})

const totalHours = developerMetrics[0]?._sum.duration / 60 || 0
```

### 4. Tickets Estancados

```typescript
// Tickets sin movimiento por más de 7 días
const staleTickets = await prisma.ticket.findMany({
  where: {
    status: {
      in: ['TODO', 'IN_PROGRESS']
    },
    updatedAt: {
      lt: new Date(Date.now() - 7 * 24 * 60 * 60 * 1000)
    }
  },
  include: {
    project: true,
    lead: true
  }
})
```

### 5. Desviación de Tiempo Estimado

```typescript
// Tickets donde realTime > estimatedTime * 1.2
const overBudgetTickets = await prisma.ticket.findMany({
  where: {
    realTime: {
      gt: 0
    }
  },
  include: {
    subtasks: true
  }
}).then(tickets => {
  return tickets.filter(ticket => {
    const totalRealTime = ticket.subtasks.reduce(
      (sum, subtask) => sum + subtask.realTime,
      0
    )
    return totalRealTime > ticket.estimatedTime * 1.2
  })
})
```

##  Índices y Optimización

```prisma
model Ticket {
  // ... campos existentes

  @@index([projectId])
  @@index([status])
  @@index([leadId])
  @@index([createdAt])
}

model WorkSession {
  // ... campos existentes

  @@index([userId])
  @@index([subtaskId])
  @@index([startTime])
}

model Expense {
  // ... campos existentes

  @@index([projectId])
  @@index([type])
  @@index([date])
}
```

##  Relacionado
- [[../../01 - Arquitectura/Arquitectura General|Estructura del Sistema]]
- [[../../05 - Backend/Server Actions|Server Actions para Mutaciones]]
- [[../../00 - Resumen/Modulos de Negocio|Módulos de Finanzas]]
