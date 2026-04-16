---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Arquitectura
  - NextJS
---
# Estructura del Monolito Next.js

##  Arquitectura Basada en Contextos (Colocation)

Para lograr mantenibilidad en un SaaS tan extenso, la jerarquía en `src/app` (o `/app`) dicta cómo se protegen las rutas HTTP mediante middlewares.

### Route Groups Declarados

#### `(auth)` - Autenticación Pública
- **Propósito:** Login, registro y recuperación de contraseñas
- **Rutas:**
  - `/login` - Formulario de inicio de sesión
  - `/register` - Registro de nuevos usuarios
  - `/forgot-password` - Recuperación de credenciales
- **Protección:** Middleware permite acceso público
- **Componentes:** Client Components para interactividad de formularios

#### `(dashboard)` - Panel Principal Protegido
- **Propósito:** Panel protegido con Server Components (RSC) nativos
- **Rutas:**
  - `/dashboard` - Home del dashboard
  - `/projects` - Lista y gestión de proyectos
  - `/projects/[slug]` - Detalle de proyecto específico
  - `/tickets` - Gestión global de tickets
  - `/tracking` - Time tracking y reportes
  - `/analytics` - Métricas y dashboards
  - `/finances` - Módulo financiero (CEO only)
  - `/admin` - Configuración global (CEO only)
- **Protección:** Middleware verifica sesión JWT
- **Componentes:** Server Components para lectura, Client Components para interacción
- **Acciones:** Mutaciones vía `app/actions/*.ts`

#### `(portal)` - Portal de Clientes Externos
- **Propósito:** Rutas aisladas para clientes externos
- **Rutas:**
  - `/portal` - Home del portal cliente
  - `/portal/projects` - Proyectos visibles para el cliente
  - `/portal/tickets` - Tickets asignados al cliente
  - `/portal/releases` - Release notes y novedades
- **Protección:** Middleware + filtrado por `projectId` en queries
- **Seguridad:** Enum `External Client` solo visibiliza tickets vinculados a su ID
- **Diseño:** Minimalista, sin acceso a datos corporativos cruzados

### Estructura de Carpetas Detallada

```
app/
  (auth)/
    login/
      page.tsx (Server Component)
      _components/
        LoginForm.tsx (Client Component)
    register/
      page.tsx
      _components/
        RegisterForm.tsx
  (dashboard)/
    layout.tsx (Layout protegido)
    page.tsx (Dashboard home)
    projects/
      page.tsx (Lista de proyectos)
      [slug]/
        page.tsx (Detalle proyecto)
        _components/
          KanbanBoard.tsx
          CreateTicketDialog.tsx
          ProjectHeader.tsx
    tickets/
      page.tsx (Lista global)
      [id]/
        page.tsx (Detalle ticket)
    tracking/
      page.tsx (Time tracking)
    analytics/
      page.tsx (Dashboards)
    finances/
      page.tsx (Finanzas - CEO only)
    admin/
      page.tsx (Admin - CEO only)
  (portal)/
    layout.tsx (Layout cliente)
    page.tsx (Home portal)
    projects/
      page.tsx (Proyectos cliente)
    tickets/
      page.tsx (Tickets cliente)
  api/
    auth/
      [...nextauth]/
        route.ts (NextAuth config)
    webhooks/
      stripe/
        route.ts (Webhooks pagos)
components/
  ui/ (shadcn/ui components)
  ai/
    ai-chat-interface.tsx
  theme-provider.tsx
  providers.tsx
lib/
  prisma.ts (Singleton Prisma)
  permissions.ts (Lógica ABAC)
  ia/
    openrouter.ts (Configuración IA)
actions/
  tickets.ts (Server Actions tickets)
  tracking.ts (Server Actions time tracking)
  finances.ts (Server Actions finanzas)
  ai.ts (Server Actions IA)
middleware.ts (Protección de rutas)
```

### Patrones de Arquitectura

#### 1. Server Components + Client Components
**Server Components (RSC)**
- **Uso:** Lectura de datos, layouts, páginas estáticas
- **Ventajas:** Zero-JS payload, SEO friendly, acceso directo a BD
- **Ejemplo:** `app/(dashboard)/projects/page.tsx` carga proyectos desde Prisma

**Client Components**
- **Uso:** Interactividad, estado local, eventos del browser
- **Ventajas:** React hooks, interactividad inmediata
- **Ejemplo:** `components/ai/ai-chat-interface.tsx` maneja estado del chat

#### 2. Server Actions para Mutaciones
**Patrón:**
```typescript
// app/actions/tickets.ts
'use server'

import { revalidatePath } from 'next/cache'
import { can } from '@/lib/permissions'

export async function updateTicketStatus(id: string, status: string) {
  const user = await getCurrentUser()
  if (!can(user, 'update', 'ticket', id)) {
    throw new Error('Unauthorized')
  }

  await prisma.ticket.update({
    where: { id },
    data: { status }
  })

  revalidatePath('/dashboard/projects')
}
```

**Ventajas:**
- Validación en servidor
- Type safety entre front y back
- Automatic revalidation de cache
- Protección contra manipulación de IDs

#### 3. Colocation Strategy
**Principio:** Mantener código relacionado cerca de donde se usa

**Ejemplo:**
```
app/(dashboard)/projects/[slug]/
  page.tsx (Server Component principal)
  _components/
    KanbanBoard.tsx (Componente específico del proyecto)
    CreateTicketDialog.tsx (Diálogo específico del proyecto)
    ProjectHeader.tsx (Header específico del proyecto)
```

**Beneficios:**
- Fácil localización de código
- Menos imports complejos
- Mejor mantenibilidad

### Rendimiento (DB vs SSR)

#### Optimización de Server Components
**Problema:** KanbanBoard puede colapsar la app por múltiples refetches simultáneos

**Solución:**
1. **Server Components para lectura:** Zero-JS payload
2. **Server Actions para mutaciones:** Atómicas y optimizadas
3. **React Cache:** Cache inteligente de queries
4. **Streaming:** Progressive rendering de componentes pesados

**Ejemplo de Optimización:**
```typescript
// app/(dashboard)/projects/[slug]/page.tsx
export default async function ProjectPage({ params }: { params: { slug: string } }) {
  const project = await prisma.project.findUnique({
    where: { slug: params.slug },
    include: {
      tickets: {
        include: {
          subtasks: true,
          collaborators: true
        }
      }
    }
  })

  return (
    <div>
      <ProjectHeader project={project} />
      <Suspense fallback={<KanbanSkeleton />}>
        <KanbanBoard projectId={project.id} />
      </Suspense>
    </div>
  )
}
```

#### Edge Runtime para Acciones Críticas
**Uso:** Server Actions que requieren baja latencia
**Beneficios:**
- Respuestas más rápidas
- Menos carga en servidor principal
- Mejor experiencia de usuario

**Ejemplo:**
```typescript
// app/actions/tracking.ts
export const runtime = 'edge'

export async function startWorkSession(subtaskId: string) {
  // Lógica de time tracking con baja latencia
}
```

### Patrones de Seguridad

#### 1. Middleware Defense
**Ubicación:** `middleware.ts`
**Función:** Primera línea de defensa, verifica sesión antes de llegar a rutas

```typescript
export function middleware(request: NextRequest) {
  const token = request.cookies.get('authjs.session-token')
  const path = request.nextUrl.pathname

  if (path.startsWith('/dashboard') && !token) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  if (path.startsWith('/portal') && !token) {
    return NextResponse.redirect(new URL('/login', request.url))
  }

  return NextResponse.next()
}
```

#### 2. Server Actions Defense
**Ubicación:** `app/actions/*.ts`
**Función:** Segunda línea de defensa, verifica permisos antes de mutar datos

```typescript
export async function deleteTicket(id: string) {
  const user = await getCurrentUser()
  const ticket = await prisma.ticket.findUnique({ where: { id } })

  if (!can(user, 'delete', 'ticket', ticket)) {
    throw new Error('Unauthorized')
  }

  await prisma.ticket.delete({ where: { id } })
}
```

#### 3. Database-Level Filtering
**Ubicación:** Queries de Prisma
**Función:** Tercera línea de defensa, filtra datos por usuario

```typescript
// Cliente externo solo ve sus tickets
const tickets = await prisma.ticket.findMany({
  where: {
    projectId: user.projectId // Filtrado por proyecto del cliente
  }
})
```

### Patrones de Caching

#### 1. React Cache
**Uso:** Cache de queries repetitivas
**Ejemplo:**
```typescript
import { cache } from 'react'

export const getProject = cache(async (slug: string) => {
  return prisma.project.findUnique({ where: { slug } })
})
```

#### 2. revalidatePath
**Uso:** Invalidación de cache después de mutaciones
**Ejemplo:**
```typescript
await prisma.ticket.update({ where: { id }, data: { status } })
revalidatePath('/dashboard/projects')
revalidatePath('/dashboard/tickets')
```

#### 3. revalidateTag
**Uso:** Invalidación granular por tags
**Ejemplo:**
```typescript
// Fetch con tag
const projects = await prisma.project.findMany()
revalidateTag('projects')

// Invalidación específica
await prisma.project.create({ data: {...} })
revalidateTag('projects')
```

##  Relacionado
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Datos]]
- [[../../04 - Frontend y Autenticacion/Layouts y Navegacion|UI y Navegación]]
- [[../../05 - Backend/Server Actions|Server Actions Detallados]]
