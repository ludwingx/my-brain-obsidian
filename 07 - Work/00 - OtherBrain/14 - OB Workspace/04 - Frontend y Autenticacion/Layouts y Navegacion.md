---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Frontend
  - Navigation
---
# Layouts, Navegación y UX

##  Experiencia de Usuario Centralizada

El diseño de **OB Workspace** se basa en un layout "App-Shell" que maximiza el espacio de trabajo para el desarrollo y la gestión.

### Estructura de Navegación

#### 1. Sidebar Inteligente

El sidebar cambia o destaca elementos según el rol y el contexto actual:

```typescript
// components/layout/sidebar.tsx
'use client'

import { cn } from '@/lib/utils'
import { Button } from '@/components/ui/button'
import { ScrollArea } from '@/components/ui/scroll-area'
import { usePathname } from 'next/navigation'

const navigation = [
  { name: 'Dashboard', href: '/dashboard', icon: LayoutDashboard },
  { name: 'Proyectos', href: '/dashboard/projects', icon: Folder },
  { name: 'Tickets', href: '/dashboard/tickets', icon: Ticket },
  { name: 'Time Tracking', href: '/dashboard/tracking', icon: Clock },
  { name: 'Analytics', href: '/dashboard/analytics', icon: BarChart, roles: ['CEO'] },
  { name: 'Finanzas', href: '/dashboard/finances', icon: DollarSign, roles: ['CEO'] },
  { name: 'Chat IA', href: '/dashboard/ai', icon: Bot },
  { name: 'Admin', href: '/dashboard/admin', icon: Settings, roles: ['CEO'] },
]

export function Sidebar() {
  const pathname = usePathname()
  const user = useCurrentUser()

  return (
    <aside className="w-64 border-r bg-card">
      <div className="flex h-full flex-col">
        <div className="p-4 border-b">
          <h1 className="text-xl font-bold">OB Workspace</h1>
        </div>

        <ScrollArea className="flex-1">
          <nav className="space-y-1 p-2">
            {navigation
              .filter((item) => !item.roles || item.roles.includes(user.role))
              .map((item) => (
                <Button
                  key={item.href}
                  variant={pathname === item.href ? 'secondary' : 'ghost'}
                  className={cn('w-full justify-start', pathname === item.href && 'bg-secondary')}
                  asChild
                >
                  <Link href={item.href}>
                    <item.icon className="mr-2 h-4 w-4" />
                    {item.name}
                  </Link>
                </Button>
              ))}
          </nav>
        </ScrollArea>

        <div className="p-4 border-t">
          <ProjectSelector />
        </div>
      </div>
    </aside>
  )
}
```

**Características del Sidebar:**

- **Contextual:** El sidebar cambia o destaca elementos según el rol
- **Módulos Activos:** Acceso directo a `Kanban`, `Proyectos`, `Time Tracking` y el `Chat AI`
- **Selector de Proyecto:** Permite cambiar el contexto global de visualización
- **Responsive:** Se oculta en móviles y se muestra con un menú hamburguesa

#### 2. Header Global

```typescript
// components/layout/header.tsx
'use client'

import { Button } from '@/components/ui/button'
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { ModeToggle } from '@/components/mode-toggle'
import { TimerDisplay } from '@/components/timer/timer-display'

export function Header() {
  const user = useCurrentUser()

  return (
    <header className="h-16 border-b bg-background/95 backdrop-blur supports-[backdrop-filter]:bg-background/60">
      <div className="flex h-full items-center justify-between px-4">
        <div className="flex items-center gap-4">
          <MobileSidebarTrigger />
          <Breadcrumb />
        </div>

        <div className="flex items-center gap-4">
          <TimerDisplay />
          <CommandTrigger />
          <ModeToggle />
          <UserMenu />
        </div>
      </div>
    </header>
  )
}
```

#### 3. Componentes de UI (Radix + Shadcn)

**Kanban Board:**

Implementado con arrastrar y soltar (Dnd-kit o similar), interactuando directamente con las *Server Actions*:

```typescript
// components/kanban/kanban-board.tsx
'use client'

import { DndContext, DragEndEvent } from '@dnd-kit/core'
import { SortableContext, verticalListSortingStrategy } from '@dnd-kit/sortable'
import { updateTicketStatus } from '@/app/actions/tickets'

export function KanbanBoard({ tickets }: { tickets: Ticket[] }) {
  const columns = ['TODO', 'IN_PROGRESS', 'IN_REVIEW', 'DONE']

  const handleDragEnd = async (event: DragEndEvent) => {
    const { active, over } = event
    if (!over) return

    const ticketId = active.id as string
    const newStatus = over.id as string

    await updateTicketStatus(ticketId, newStatus)
  }

  return (
    <DndContext onDragEnd={handleDragEnd}>
      <div className="grid grid-cols-4 gap-4">
        {columns.map((column) => (
          <SortableContext
            key={column}
            id={column}
            items={tickets.filter(t => t.status === column)}
            strategy={verticalListSortingStrategy}
          >
            <KanbanColumn
              title={column}
              tickets={tickets.filter(t => t.status === column)}
            />
          </SortableContext>
        ))}
      </div>
    </DndContext>
  )
}
```

**Timer Flotante:**

Un componente global (Client Component) que persiste mientras el usuario navega, mostrando el tiempo de la `WorkSession` activa:

```typescript
// components/timer/timer-display.tsx
'use client'

import { useState, useEffect } from 'react'
import { Button } from '@/components/ui/button'
import { Play, Pause, Square } from 'lucide-react'

export function TimerDisplay() {
  const [isActive, setIsActive] = useState(false)
  const [elapsed, setElapsed] = useState(0)

  useEffect(() => {
    if (!isActive) return

    const interval = setInterval(() => {
      setElapsed(prev => prev + 1)
    }, 1000)

    return () => clearInterval(interval)
  }, [isActive])

  const formatTime = (seconds: number) => {
    const hours = Math.floor(seconds / 3600)
    const minutes = Math.floor((seconds % 3600) / 60)
    const secs = seconds % 60
    return `${hours}:${String(minutes).padStart(2, '0')}:${String(secs).padStart(2, '0')}`
  }

  return (
    <div className="flex items-center gap-2">
      {isActive ? (
        <>
          <span className="font-mono text-sm">{formatTime(elapsed)}</span>
          <Button size="icon" variant="ghost" onClick={() => setIsActive(false)}>
            <Pause className="h-4 w-4" />
          </Button>
        </>
      ) : (
        <Button size="icon" variant="ghost" onClick={() => setIsActive(true)}>
          <Play className="h-4 w-4" />
        </Button>
      )}
    </div>
  )
}
```

**Comandos Rápidos (CMDK):**

Una barra de búsqueda tipo Raycast (`Cmd+K`) para saltar rápidamente entre tickets o proyectos:

```typescript
// components/command/command-palette.tsx
'use client'

import { Command } from 'cmdk'
import { useEffect, useState } from 'react'
import { searchTickets, searchProjects } from '@/app/actions/search'

export function CommandPalette() {
  const [open, setOpen] = useState(false)
  const [query, setQuery] = useState('')
  const [results, setResults] = useState({ tickets: [], projects: [] })

  useEffect(() => {
    const down = (e: KeyboardEvent) => {
      if (e.key === 'k' && (e.metaKey || e.ctrlKey)) {
        e.preventDefault()
        setOpen(open => !open)
      }
    }

    document.addEventListener('keydown', down)
    return () => document.removeEventListener('keydown', down)
  }, [])

  useEffect(() => {
    if (query.length < 2) return

    Promise.all([
      searchTickets(query),
      searchProjects(query)
    ]).then(([tickets, projects]) => {
      setResults({ tickets, projects })
    })
  }, [query])

  return (
    <Command.Dialog open={open} onOpenChange={setOpen}>
      <Command.Input
        placeholder="Buscar tickets, proyectos..."
        value={query}
        onValueChange={setQuery}
      />
      <Command.List>
        {results.tickets.length > 0 && (
          <Command.Group heading="Tickets">
            {results.tickets.map(ticket => (
              <Command.Item key={ticket.id} value={ticket.id}>
                {ticket.title}
              </Command.Item>
            ))}
          </Command.Group>
        )}
        {results.projects.length > 0 && (
          <Command.Group heading="Proyectos">
            {results.projects.map(project => (
              <Command.Item key={project.id} value={project.id}>
                {project.name}
              </Command.Item>
            ))}
          </Command.Group>
        )}
      </Command.List>
    </Command.Dialog>
  )
}
```

### Visualización de Datos

Uso de componentes de **Tremor** o **Recharts** en el dashboard del CEO para visualizar la quema de presupuesto (`Expenses`) versus el progreso técnico (`Tickets Done`):

```typescript
// components/analytics/charts.tsx
'use client'

import { BarChart, Bar, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from 'recharts'

export function BudgetVsProgressChart({ projects }: { projects: Project[] }) {
  const data = projects.map(project => ({
    name: project.name,
    budget: project.budget,
    spent: project.expenses.reduce((sum, e) => sum + e.amount, 0),
    progress: (project.tickets.filter(t => t.status === 'DONE').length / project.tickets.length) * 100
  }))

  return (
    <ResponsiveContainer width="100%" height={300}>
      <BarChart data={data}>
        <CartesianGrid strokeDasharray="3 3" />
        <XAxis dataKey="name" />
        <YAxis />
        <Tooltip />
        <Bar dataKey="budget" fill="#8884d8" name="Presupuesto" />
        <Bar dataKey="spent" fill="#82ca9d" name="Gastado" />
      </BarChart>
    </ResponsiveContainer>
  )
}
```

### Layouts por Contexto

#### Dashboard Layout

```typescript
// app/(dashboard)/layout.tsx
import { Sidebar } from '@/components/layout/sidebar'
import { Header } from '@/components/layout/header'

export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <div className="flex h-screen">
      <Sidebar />
      <div className="flex-1 flex flex-col overflow-hidden">
        <Header />
        <main className="flex-1 overflow-y-auto p-6">
          {children}
        </main>
      </div>
    </div>
  )
}
```

#### Portal Layout (Clientes Externos)

```typescript
// app/(portal)/layout.tsx
import { PortalHeader } from '@/components/layout/portal-header'

export default function PortalLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <div className="min-h-screen bg-background">
      <PortalHeader />
      <main className="container mx-auto py-8">
        {children}
      </main>
    </div>
  )
}
```

### Patrones de UX

#### 1. Optimistic Updates

```typescript
// components/kanban/ticket-card.tsx
'use client'

import { useOptimistic } from 'react'

export function TicketCard({ ticket }: { ticket: Ticket }) {
  const [optimisticTicket, addOptimisticTicket] = useOptimistic(
    ticket,
    (state, newStatus) => ({ ...state, status: newStatus as TicketStatus })
  )

  const handleDragEnd = async (newStatus: TicketStatus) => {
    addOptimisticTicket(newStatus)
    await updateTicketStatus(ticket.id, newStatus)
  }

  return (
    <Card
      draggable
      onDragEnd={() => handleDragEnd('IN_PROGRESS')}
      className={cn(
        'cursor-grab active:cursor-grabbing',
        optimisticTicket.status === 'IN_PROGRESS' && 'ring-2 ring-primary'
      )}
    >
      <CardContent>
        <h3 className="font-semibold">{optimisticTicket.title}</h3>
      </CardContent>
    </Card>
  )
}
```

#### 2. Skeleton Loading

```typescript
// components/ui/skeleton.tsx
import { cn } from '@/lib/utils'

function Skeleton({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      className={cn('animate-pulse rounded-md bg-muted', className)}
      {...props}
    />
  )
}

// Uso en KanbanBoard
<Suspense fallback={<KanbanSkeleton />}>
  <KanbanBoard projectId={projectId} />
</Suspense>
```

#### 3. Error Boundaries

```typescript
// components/error-boundary.tsx
'use client'

import { Component, ReactNode } from 'react'

interface Props {
  children: ReactNode
  fallback?: ReactNode
}

interface State {
  hasError: boolean
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError() {
    return { hasError: true }
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="p-4 border border-destructive bg-destructive/10 rounded-lg">
          <h2 className="text-lg font-semibold text-destructive">Error</h2>
          <p className="text-sm text-muted-foreground">Algo salió mal. Por favor recarga la página.</p>
        </div>
      )
    }

    return this.props.children
  }
}
```

##  Relacionado
- [[Roles RBAC|Control de Acceso (RBAC)]]
- [[../03 - Inteligencia Artificial/Orquestacion AI|Asistente IA Integrado]]
- [[../01 - Arquitectura/Arquitectura General|Arquitectura del Sistema]]
