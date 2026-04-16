---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Backend
  - Security
---
# Seguridad, Permisos y ABAC (Attribute-Based Access Control)

##  Lógica de Autorización Contextual

La seguridad en **OB Workspace** no se limita a "estás logueado o no". Se basa en **quién** eres y **qué relación** tienes con el recurso que intentas manipular.

### La Función Maestra `can()`

Ubicada en `lib/permissions.ts`, esta función evalúa la terna `(usuario, acción, recurso)`.

```typescript
// lib/permissions.ts
import { UserRole } from '@prisma/client'

export type Action = 'create' | 'read' | 'update' | 'delete' | 'admin'
export type Resource = 'ticket' | 'project' | 'expense' | 'user' | 'settings'

export interface PermissionContext {
  user: User
  action: Action
  resource: Resource
  resourceId?: string
  additionalContext?: Record<string, any>
}

export async function can({
  user,
  action,
  resource,
  resourceId,
  additionalContext,
}: PermissionContext): Promise<boolean> {
  // CEO tiene bypass en casi todo
  if (user.role === 'CEO') {
    return canCEO(action, resource)
  }

  // External Client tiene acceso limitado
  if (user.role === 'EXTERNAL_CLIENT') {
    return canClient(user, action, resource, resourceId, additionalContext)
  }

  // Developer tiene acceso basado en asignación
  if (user.role === 'DEVELOPER') {
    return canDeveloper(user, action, resource, resourceId, additionalContext)
  }

  // Intern tiene acceso muy limitado
  if (user.role === 'INTERN') {
    return canIntern(user, action, resource, resourceId, additionalContext)
  }

  return false
}

function canCEO(action: Action, resource: Resource): boolean {
  const ceoPermissions: Record<Resource, Action[]> = {
    ticket: ['create', 'read', 'update', 'delete', 'admin'],
    project: ['create', 'read', 'update', 'delete', 'admin'],
    expense: ['create', 'read', 'update', 'delete', 'admin'],
    user: ['create', 'read', 'update', 'delete', 'admin'],
    settings: ['create', 'read', 'update', 'delete', 'admin'],
  }

  return ceoPermissions[resource]?.includes(action) ?? false
}

async function canClient(
  user: User,
  action: Action,
  resource: Resource,
  resourceId?: string,
  additionalContext?: Record<string, any>
): Promise<boolean> {
  // Clientes solo pueden leer y crear tickets en sus proyectos
  if (resource === 'ticket') {
    if (action === 'read') {
      if (!resourceId) return true
      const ticket = await prisma.ticket.findUnique({
        where: { id: resourceId },
        select: { projectId: true, creatorId: true },
      })
      return ticket?.projectId === user.projectId || ticket?.creatorId === user.id
    }
    if (action === 'create') {
      return true
    }
    return false
  }

  if (resource === 'project') {
    if (action === 'read') {
      if (!resourceId) return true
      const project = await prisma.project.findUnique({
        where: { id: resourceId },
        select: { clientId: true },
      })
      return project?.clientId === user.id
    }
    return false
  }

  return false
}

async function canDeveloper(
  user: User,
  action: Action,
  resource: Resource,
  resourceId?: string,
  additionalContext?: Record<string, any>
): Promise<boolean> {
  if (resource === 'ticket') {
    if (action === 'read' || action === 'create') return true

    if (action === 'update' || action === 'delete') {
      if (!resourceId) return false
      const ticket = await prisma.ticket.findUnique({
        where: { id: resourceId },
        select: { leadId: true, collaborators: { select: { userId: true } } },
      })
      if (!ticket) return false

      const isLead = ticket.leadId === user.id
      const isCollaborator = ticket.collaborators.some(c => c.userId === user.id)
      return isLead || isCollaborator
    }
  }

  if (resource === 'project') {
    if (action === 'read') return true
    return false
  }

  return false
}

async function canIntern(
  user: User,
  action: Action,
  resource: Resource,
  resourceId?: string,
  additionalContext?: Record<string, any>
): Promise<boolean> {
  // Interns solo pueden leer y actualizar tickets asignados
  if (resource === 'ticket') {
    if (action === 'read') return true

    if (action === 'update') {
      if (!resourceId) return false
      const ticket = await prisma.ticket.findUnique({
        where: { id: resourceId },
        select: { collaborators: { select: { userId: true } } },
      })
      if (!ticket) return false

      return ticket.collaborators.some(c => c.userId === user.id)
    }
  }

  return false
}
```

### Protección en múltiples capas

#### 1. Middleware de Next.js

Protege las rutas de la aplicación `/dashboard/*` y `/portal/*` verificando la sesión JWT:

```typescript
// middleware.ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { getToken } from 'next-auth/jwt'

export async function middleware(request: NextRequest) {
  const token = await getToken({ req: request })
  const { pathname } = request.nextUrl

  // Rutas públicas
  if (pathname.startsWith('/login') || pathname.startsWith('/register')) {
    if (token) {
      return NextResponse.redirect(new URL('/dashboard', request.url))
    }
    return NextResponse.next()
  }

  // Rutas protegidas
  if (pathname.startsWith('/dashboard') || pathname.startsWith('/portal')) {
    if (!token) {
      return NextResponse.redirect(new URL('/login', request.url))
    }

    // Verificar rol para rutas específicas
    if (pathname.startsWith('/dashboard/finances') || pathname.startsWith('/dashboard/admin')) {
      const user = await prisma.user.findUnique({
        where: { email: token.email as string },
        select: { role: true },
      })

      if (user?.role !== 'CEO') {
        return NextResponse.redirect(new URL('/dashboard', request.url))
      }
    }
  }

  return NextResponse.next()
}

export const config = {
  matcher: ['/((?!api|_next/static|_next/image|favicon.ico).*)'],
}
```

#### 2. Server Actions (Defensa Final)

Cada acción en `app/actions/` vuelve a llamar a la lógica de `permissions.ts` antes de ejecutar cualquier comando de Prisma. Esto evita que un usuario manipule IDs de tickets de otros proyectos vía consola o herramientas externas:

```typescript
// app/actions/tickets.ts
'use server'

import { prisma } from '@/lib/prisma'
import { can } from '@/lib/permissions'
import { revalidatePath } from 'next/cache'

export async function updateTicketStatus(
  ticketId: string,
  status: string
) {
  const user = await getCurrentUser()

  // Verificar permisos
  const hasPermission = await can({
    user,
    action: 'update',
    resource: 'ticket',
    resourceId: ticketId,
  })

  if (!hasPermission) {
    throw new Error('Unauthorized: You do not have permission to update this ticket')
  }

  // Verificar que el ticket existe
  const ticket = await prisma.ticket.findUnique({
    where: { id: ticketId },
  })

  if (!ticket) {
    throw new Error('Ticket not found')
  }

  // Actualizar ticket
  const updated = await prisma.ticket.update({
    where: { id: ticketId },
    data: { status },
  })

  revalidatePath('/dashboard/projects')
  revalidatePath('/dashboard/tickets')

  return updated
}

export async function deleteTicket(ticketId: string) {
  const user = await getCurrentUser()

  // Verificar permisos de eliminación
  const hasPermission = await can({
    user,
    action: 'delete',
    resource: 'ticket',
    resourceId: ticketId,
  })

  if (!hasPermission) {
    throw new Error('Unauthorized: You do not have permission to delete this ticket')
  }

  await prisma.ticket.delete({
    where: { id: ticketId },
  })

  revalidatePath('/dashboard/projects')
  revalidatePath('/dashboard/tickets')
}

export async function createTicket(data: CreateTicketInput) {
  const user = await getCurrentUser()

  // Verificar permisos de creación
  const hasPermission = await can({
    user,
    action: 'create',
    resource: 'ticket',
  })

  if (!hasPermission) {
    throw new Error('Unauthorized: You do not have permission to create tickets')
  }

  const ticket = await prisma.ticket.create({
    data: {
      ...data,
      creatorId: user.id,
    },
  })

  revalidatePath('/dashboard/projects')
  revalidatePath('/dashboard/tickets')

  return ticket
}
```

#### 3. Database-Level Filtering

Filtrado de datos por usuario en queries de Prisma:

```typescript
// app/actions/projects.ts
export async function getProjects(user: User) {
  // CEO ve todos los proyectos
  if (user.role === 'CEO') {
    return prisma.project.findMany({
      include: {
        tickets: true,
        expenses: true,
      },
    })
  }

  // Clientes solo ven sus proyectos
  if (user.role === 'EXTERNAL_CLIENT') {
    return prisma.project.findMany({
      where: { clientId: user.id },
      include: {
        tickets: true,
      },
    })
  }

  // Developers e Interns ven proyectos donde son colaboradores
  const tickets = await prisma.ticket.findMany({
    where: {
      OR: [
        { leadId: user.id },
        { collaborators: { some: { userId: user.id } } },
      ],
    },
    select: { projectId: true },
    distinct: ['projectId'],
  })

  const projectIds = tickets.map(t => t.projectId)

  return prisma.project.findMany({
    where: { id: { in: projectIds } },
    include: {
      tickets: true,
    },
  })
}
```

### Matriz de Responsabilidades

| Recurso | CEO | Dev | Intern | Client |
| :--- | :---: | :---: | :---: | :---: |
| **Finanzas (Gastos)** |  |  |  |  |
| **Configuración Global** |  |  |  |  |
| **Crear Tickets** |  |  |  |  |
| **Mover Kanban** |  | (Propios) | (Propios) |  |

### Casos de Uso de Seguridad

#### Caso 1: Prevención de Manipulación de IDs

```typescript
// Intento malicioso: Usuario intenta eliminar ticket de otro proyecto
await deleteTicket('other-project-ticket-id')

// Resultado: Error "Unauthorized" porque can() verifica que el usuario
// no es lead ni colaborador de ese ticket
```

#### Caso 2: Filtro de Datos por Rol

```typescript
// CEO ve todos los proyectos
const ceoProjects = await getProjects(ceoUser) // Retorna todos los proyectos

// Client solo ve sus proyectos
const clientProjects = await getProjects(clientUser) // Retorna solo sus proyectos

// Developer ve proyectos donde trabaja
const devProjects = await getProjects(devUser) // Retorna proyectos asignados
```

#### Caso 3: Protección de Rutas Sensibles

```typescript
// Intento de acceso a /dashboard/finances sin ser CEO
// Middleware redirige a /dashboard con error 403

// Intento de acceso a /dashboard/admin sin ser CEO
// Middleware redirige a /dashboard con error 403
```

### Patrones de Seguridad Avanzados

#### 1. Rate Limiting por Acción

```typescript
// lib/rate-limit.ts
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, '1 m'),
})

export async function checkRateLimit(
  userId: string,
  action: string
): Promise<boolean> {
  const { success } = await ratelimit.limit(`${userId}:${action}`)
  return success
}

// Uso en Server Actions
export async function createTicket(data: CreateTicketInput) {
  const user = await getCurrentUser()

  const canProceed = await checkRateLimit(user.id, 'create_ticket')
  if (!canProceed) {
    throw new Error('Rate limit exceeded')
  }

  // ... resto de la lógica
}
```

#### 2. Auditoría de Acciones

```typescript
// lib/audit.ts
export async function logAction({
  userId,
  action,
  resource,
  resourceId,
  metadata,
}: AuditLogInput) {
  await prisma.auditLog.create({
    data: {
      userId,
      action,
      resource,
      resourceId,
      metadata,
      timestamp: new Date(),
      ipAddress: getClientIP(),
    },
  })
}

// Uso en Server Actions
export async function deleteTicket(ticketId: string) {
  const user = await getCurrentUser()

  await logAction({
    userId: user.id,
    action: 'delete',
    resource: 'ticket',
    resourceId: ticketId,
    metadata: { role: user.role },
  })

  // ... resto de la lógica
}
```

#### 3. Validación de Entrada

```typescript
// lib/validation.ts
import { z } from 'zod'

export const createTicketSchema = z.object({
  title: z.string().min(1).max(200),
  description: z.string().optional(),
  priority: z.enum(['LOW', 'MEDIUM', 'HIGH', 'CRITICAL']),
  projectId: z.string().uuid(),
  moduleId: z.string().uuid().optional(),
})

export type CreateTicketInput = z.infer<typeof createTicketSchema>

// Uso en Server Actions
export async function createTicket(data: CreateTicketInput) {
  // Validar entrada
  const validatedData = createTicketSchema.parse(data)

  const user = await getCurrentUser()

  // Verificar permisos
  const hasPermission = await can({
    user,
    action: 'create',
    resource: 'ticket',
  })

  if (!hasPermission) {
    throw new Error('Unauthorized')
  }

  // Crear ticket con datos validados
  const ticket = await prisma.ticket.create({
    data: {
      ...validatedData,
      creatorId: user.id,
    },
  })

  return ticket
}
```

##  Relacionado
- [[../../04 - Frontend y Autenticacion/Roles RBAC|Definición de Roles]]
- [[Server Actions|Acciones Protegidas]]
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Datos]]
