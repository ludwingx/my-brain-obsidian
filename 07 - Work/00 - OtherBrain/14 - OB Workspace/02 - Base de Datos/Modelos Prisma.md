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

**Modelo Subtask:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `title`: Título de la subtarea
  - `description`: Descripción detallada (opcional)
  - `status`: Estado (TODO, DOING, DONE, BLOCKED)
  - `estimatedTime`: Tiempo estimado en minutos
  - `realTime`: Tiempo real acumulado (default 0)
  - `order`: Orden para visualización (default 0)
- **Relaciones:**
  - `ticket`: Ticket padre (onDelete: Cascade)
  - `workSessions`: Sesiones de trabajo asociadas
- **Timestamps:** createdAt, updatedAt

**Regla Dorada:** La sumatoria de `realTime` del Ticket, nace de agrupar las iteraciones subyacentes.

**Modelo Ticket:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `title`: Título del ticket
  - `description`: Descripción detallada (opcional)
  - `status`: Estado (TODO, IN_PROGRESS, IN_REVIEW, DONE, BLOCKED, CANCELLED)
  - `priority`: Prioridad (LOW, MEDIUM, HIGH, CRITICAL)
  - `estimatedTime`: Suma de tiempos de subtareas
  - `realTime`: Suma calculada de subtareas (default 0)
- **Relaciones:**
  - `project`: Proyecto padre (onDelete: Cascade)
  - `module`: Módulo opcional (onDelete: SetNull)
  - `lead`: Desarrollador principal (opcional)
  - `creator`: Usuario que creó el ticket
  - `subtasks`: Array de subtareas
  - `collaborators`: Array de colaboradores
- **Timestamps:** createdAt, updatedAt

### 2. Máquina de Estado del "Punch Clock"

Para evitar fraude, las horas se miden en ventanas cerradas:

**Modelo WorkSession:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `startTime`: Momento de inicio (default now)
  - `endTime`: Momento de cierre (opcional)
  - `duration`: Duración en minutos (default 0, mutado al cerrarse)
- **Relaciones:**
  - `user`: Usuario que trabajó (onDelete: Cascade)
  - `subtask`: Subtarea trabajada (onDelete: Cascade)
- **Timestamps:** createdAt, updatedAt

**Flujo de Time Tracking:**

1. **Start:** Usuario inicia WorkSession en una Subtarea
2. **Doing:** Subtarea cambia a estado `DOING`
3. **Stop:** Usuario cierra sesión, se calcula `duration`
4. **Update:** `duration` se suma a `realTime` de Subtarea
5. **Auto-Stop (Futuro):** Si `endTime` falta después de 10 horas, auto-stop para prevenir errores

### 3. Proyectos y Módulos

**Modelo Project:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `slug`: Slug único para URLs
  - `name`: Nombre del proyecto
  - `description`: Descripción detallada (opcional)
  - `budget`: Presupuesto asignado (opcional)
  - `status`: Estado (PLANNING, ACTIVE, ON_HOLD, COMPLETED, CANCELLED)
- **Relaciones:**
  - `client`: Cliente externo (opcional)
  - `modules`: Array de módulos
  - `tickets`: Array de tickets
  - `expenses`: Array de gastos
- **Timestamps:** createdAt, updatedAt

**Modelo Module:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `name`: Nombre del módulo
  - `description`: Descripción detallada (opcional)
  - `order`: Orden para visualización (default 0)
- **Relaciones:**
  - `project`: Proyecto padre (onDelete: Cascade)
  - `tickets`: Array de tickets
- **Timestamps:** createdAt, updatedAt

### 4. Usuarios y Roles

**Modelo User:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `email`: Email único
  - `name`: Nombre del usuario
  - `role`: Rol (CEO, DEVELOPER, INTERN, EXTERNAL_CLIENT)
  - `image`: URL de avatar (opcional)
- **Relaciones:**
  - `ticketsLead`: Tickets donde es líder
  - `ticketsCreated`: Tickets que creó
  - `workSessions`: Sesiones de trabajo
  - `projectClient`: Proyectos donde es cliente
- **Timestamps:** createdAt, updatedAt

**Roles del Sistema:**
- **CEO**: Acceso total, finanzas, administración
- **DEVELOPER**: Desarrollo técnico, tickets asignados
- **INTERN**: Tareas simples, tickets asignados
- **EXTERNAL_CLIENT**: Solo sus proyectos, lectura limitada

### 5. Finanzas y Gastos

**Modelo Expense:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `title`: Título del gasto
  - `amount`: Monto del gasto
  - `type`: Tipo (FIXED, VARIABLE, ONE_TIME, RECURRING)
  - `description`: Descripción detallada (opcional)
  - `date`: Fecha del gasto (default now)
- **Relaciones:**
  - `project`: Proyecto asociado (opcional, onDelete: SetNull)
- **Timestamps:** createdAt, updatedAt

**Tipos de Gastos:**
- **FIXED**: Gastos fijos mensuales (servidores, licencias)
- **VARIABLE**: Pagos a colaboradores por hora
- **ONE_TIME**: Gastos únicos (hardware, setup)
- **RECURRING**: Suscripciones recurrentes (SaaS)

### 6. Colaboradores en Tickets

**Modelo TicketCollaborator:**
- **Campos principales:**
  - `id`: Identificador único (CUID)
  - `ticketId`: ID del ticket
  - `userId`: ID del usuario
- **Relaciones:**
  - `ticket`: Ticket asociado (onDelete: Cascade)
  - `user`: Usuario colaborador (onDelete: Cascade)
- **Constraints:** Unique constraint en [ticketId, userId]
- **Timestamps:** createdAt

### Diagrama de Relaciones de Base de Datos (ER)

```mermaid
erDiagram
    User ||--o{ Ticket : "crea"
    User ||--o{ Ticket : "lidera"
    User ||--o{ WorkSession : "registra"
    User ||--o{ Project : "es cliente de"
    User ||--o{ TicketCollaborator : "colabora en"

    Project ||--o{ Module : "contiene"
    Project ||--o{ Ticket : "tiene"
    Project ||--o{ Expense : "registra"
    Project ||o|| User : "cliente"

    Module ||--o{ Ticket : "contiene"

    Ticket ||--o{ Subtask : "desglosa en"
    Ticket ||--o{ TicketCollaborator : "tiene colaboradores"
    Ticket }o--|| Module : "pertenece a"
    Ticket }o--|| Project : "pertenece a"
    Ticket }o--|| User : "liderado por"
    Ticket }o--|| User : "creado por"

    Subtask ||--o{ WorkSession : "registra"
    Subtask }o--|| Ticket : "pertenece a"

    WorkSession }o--|| User : "registrado por"
    WorkSession }o--|| Subtask : "asociado a"

    TicketCollaborator }o--|| Ticket : "asociado a"
    TicketCollaborator }o--|| User : "usuario"

    Expense }o--|| Project : "asociado a"

    User {
        string id PK
        string email UK
        string name
        UserRole role
        string image
        datetime createdAt
        datetime updatedAt
    }

    Project {
        string id PK
        string slug UK
        string name
        string description
        float budget
        ProjectStatus status
        string clientId FK
        datetime createdAt
        datetime updatedAt
    }

    Module {
        string id PK
        string name
        string description
        int order
        string projectId FK
        datetime createdAt
        datetime updatedAt
    }

    Ticket {
        string id PK
        string title
        string description
        TicketStatus status
        TicketPriority priority
        int estimatedTime
        int realTime
        string projectId FK
        string moduleId FK
        string leadId FK
        string creatorId FK
        datetime createdAt
        datetime updatedAt
    }

    Subtask {
        string id PK
        string title
        string description
        SubtaskStatus status
        int estimatedTime
        int realTime
        int order
        string ticketId FK
        datetime createdAt
        datetime updatedAt
    }

    WorkSession {
        string id PK
        datetime startTime
        datetime endTime
        int duration
        string userId FK
        string subtaskId FK
        datetime createdAt
        datetime updatedAt
    }

    Expense {
        string id PK
        string title
        float amount
        ExpenseType type
        string description
        datetime date
        string projectId FK
        datetime createdAt
        datetime updatedAt
    }

    TicketCollaborator {
        string id PK
        string ticketId FK
        string userId FK
        datetime createdAt
    }

    enum UserRole {
        CEO
        DEVELOPER
        INTERN
        EXTERNAL_CLIENT
    }

    enum ProjectStatus {
        PLANNING
        ACTIVE
        ON_HOLD
        COMPLETED
        CANCELLED
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

    enum SubtaskStatus {
        TODO
        DOING
        DONE
        BLOCKED
    }

    enum ExpenseType {
        FIXED
        VARIABLE
        ONE_TIME
        RECURRING
    }
```

##  Queries Avanzadas y Cálculos

### 1. Calcular Tiempo Real de Ticket

**Lógica:**
- Buscar ticket por ID con subtareas incluidas
- Seleccionar solo campo `realTime` de cada subtarea
- Sumar todos los tiempos reales de subtareas
- Resultado: Tiempo total real del ticket

**Uso:**
- Comparación con tiempo estimado
- Cálculo de eficiencia
- Reportes de productividad

### 2. Calcular Rentabilidad de Proyecto

**Lógica:**
- Buscar proyecto por ID con gastos incluidos
- Sumar todos los montos de gastos
- Calcular profit = budget - totalExpenses
- Calcular margin = (profit / budget) * 100

**Uso:**
- Análisis de rentabilidad por proyecto
- Identificación de proyectos no rentables
- Toma de decisiones de precios

### 3. Métricas de Desarrollador

**Lógica:**
- Agrupar WorkSessions por userId
- Filtrar por desarrollador específico
- Filtrar por último mes (30 días)
- Sumar duraciones de todas las sesiones
- Convertir a horas (dividir por 60)

**Uso:**
- Reportes de productividad
- Cálculo de horas trabajadas
- Identificación de desarrolladores subutilizados o sobrecargados

### 4. Tickets Estancados

**Lógica:**
- Buscar tickets con estado TODO o IN_PROGRESS
- Filtrar por updatedAt hace más de 7 días
- Incluir proyecto y líder para contexto
- Resultado: Tickets que necesitan atención

**Uso:**
- Alertas de tickets olvidados
- Identificación de bloqueos
- Priorización de trabajo pendiente

### 5. Desviación de Tiempo Estimado

**Lógica:**
- Buscar tickets con realTime > 0
- Incluir subtareas
- Para cada ticket:
  - Sumar realTime de todas las subtareas
  - Comparar con estimatedTime * 1.2
  - Retornar si excede el 20% del estimado
- Resultado: Tickets que excedieron presupuesto de tiempo

**Uso:**
- Análisis de precisión de estimaciones
- Identificación de tickets problemáticos
- Mejora continua de estimaciones

##  Índices y Optimización

**Índices en Ticket:**
- `projectId`: Búsqueda rápida por proyecto
- `status`: Filtrado por estado
- `leadId`: Búsqueda por desarrollador líder
- `createdAt`: Ordenamiento temporal

**Índices en WorkSession:**
- `userId`: Búsqueda rápida por usuario
- `subtaskId`: Filtrado por subtarea
- `startTime`: Ordenamiento temporal

**Índices en Expense:**
- `projectId`: Búsqueda por proyecto
- `type`: Filtrado por tipo de gasto
- `date`: Ordenamiento temporal y filtrado por período

**Beneficios de Índices:**
- Consultas más rápidas
- Menos carga en base de datos
- Mejor rendimiento de dashboards
- Escalabilidad del sistema

##  Relacionado
- [[../../01 - Arquitectura/Arquitectura General|Estructura del Sistema]]
- [[../../05 - Backend/Server Actions|Server Actions para Mutaciones]]
- [[../../00 - Resumen/Modulos de Negocio|Módulos de Finanzas]]
