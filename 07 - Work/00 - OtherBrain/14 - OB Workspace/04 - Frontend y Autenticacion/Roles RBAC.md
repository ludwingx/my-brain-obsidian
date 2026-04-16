---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - RBAC
  - Frontend
---
# RBAC (Role-Based Access Control) y Permisos

##  Muros Front y Back por Entidades

A diferencia de un acceso de tipo `{ isAdmin: true }`, se usa un modelo conceptual ABAC.
El Enum define contornos fuertes: `CEO`, `DEVELOPER`, `INTERN`, `EXTERNAL_CLIENT`.

### El método `can(user, action, resource)`

La interfaz no renderiza botones "Delete" condicionales solo ocultándolos mediante CSS. Se emplea una verificación del lado del server (Server Action / RSC) que corrobora si ese *UserId* tiene permisos contra el ID del Row.

#### Comportamiento del UI según Rol:

1. **CEO:** Panel Master, posee acceso global y único al submódulo `expenses` y facturación de la agencia.
2. **Client:** Ruta paralela que lo aísla físicamente (`/portal`).
3. **Intern / Dev:** Ven el Kanban global, pero el Developer sólo mueve *Cards (Tickets)* en las que está atado como `ticket.leadId` o `ticket.collaborators`. El *Intern* se limita exclusivamente a lo que le asignaron expresamente, evitando mover tickets críticos que alteren los reportes productivos de la agencia.

### Implementación de Roles en Frontend

#### 1. Hook de Autenticación

```typescript
// hooks/use-auth.ts
'use client'

import { useState, useEffect } from 'react'
import { User } from '@prisma/client'

export function useAuth() {
  const [user, setUser] = useState<User | null>(null)
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    async function fetchUser() {
      try {
        const response = await fetch('/api/auth/user')
        if (response.ok) {
          const userData = await response.json()
          setUser(userData)
        }
      } catch (error) {
        setUser(null)
      } finally {
        setLoading(false)
      }
    }

    fetchUser()
  }, [])

  return { user, loading, isAuthenticated: !!user }
}
```

#### 2. Componente Role-Based

```typescript
// components/role-based/role-gate.tsx
'use client'

import { useAuth } from '@/hooks/use-auth'
import { UserRole } from '@prisma/client'

interface RoleGateProps {
  children: React.ReactNode
  allowedRoles: UserRole[]
  fallback?: React.ReactNode
}

export function RoleGate({ children, allowedRoles, fallback }: RoleGateProps) {
  const { user, loading } = useAuth()

  if (loading) {
    return <div className="animate-pulse">Cargando...</div>
  }

  if (!user || !allowedRoles.includes(user.role)) {
    return fallback || (
      <div className="p-4 border border-destructive bg-destructive/10 rounded-lg">
        <p className="text-sm text-destructive">No tienes permiso para ver esta sección.</p>
      </div>
    )
  }

  return <>{children}</>
}
```

#### 3. Botones Condicionales por Rol

```typescript
// components/role-based/action-buttons.tsx
'use client'

import { useAuth } from '@/hooks/use-auth'
import { Button } from '@/components/ui/button'
import { Trash2, Edit, Eye } from 'lucide-react'

interface ActionButtonsProps {
  ticket: Ticket
  onEdit?: () => void
  onDelete?: () => void
  onView?: () => void
}

export function ActionButtons({ ticket, onEdit, onDelete, onView }: ActionButtonsProps) {
  const { user } = useAuth()

  if (!user) return null

  const canEdit = user.role === 'CEO' || ticket.leadId === user.id
  const canDelete = user.role === 'CEO' || ticket.leadId === user.id
  const canView = true // Todos pueden ver

  return (
    <div className="flex gap-2">
      {canView && (
        <Button size="icon" variant="ghost" onClick={onView}>
          <Eye className="h-4 w-4" />
        </Button>
      )}
      {canEdit && (
        <Button size="icon" variant="ghost" onClick={onEdit}>
          <Edit className="h-4 w-4" />
        </Button>
      )}
      {canDelete && (
        <Button size="icon" variant="ghost" onClick={onDelete}>
          <Trash2 className="h-4 w-4 text-destructive" />
        </Button>
      )}
    </div>
  )
}
```

#### 4. Navegación Contextual

```typescript
// components/role-based/role-navigation.tsx
'use client'

import { useAuth } from '@/hooks/use-auth'
import { UserRole } from '@prisma/client'

interface NavItem {
  title: string
  href: string
  icon: React.ReactNode
  roles?: UserRole[]
}

const navigation: NavItem[] = [
  { title: 'Dashboard', href: '/dashboard', icon: LayoutDashboard },
  { title: 'Proyectos', href: '/dashboard/projects', icon: Folder },
  { title: 'Tickets', href: '/dashboard/tickets', icon: Ticket },
  { title: 'Time Tracking', href: '/dashboard/tracking', icon: Clock },
  { title: 'Analytics', href: '/dashboard/analytics', icon: BarChart, roles: ['CEO'] },
  { title: 'Finanzas', href: '/dashboard/finances', icon: DollarSign, roles: ['CEO'] },
  { title: 'Admin', href: '/dashboard/admin', icon: Settings, roles: ['CEO'] },
]

export function RoleNavigation() {
  const { user } = useAuth()

  if (!user) return null

  const filteredNav = navigation.filter(
    item => !item.roles || item.roles.includes(user.role)
  )

  return (
    <nav className="space-y-1">
      {filteredNav.map(item => (
        <Link
          key={item.href}
          href={item.href}
          className="flex items-center gap-2 px-3 py-2 rounded-md hover:bg-accent"
        >
          <item.icon className="h-4 w-4" />
          {item.title}
        </Link>
      ))}
    </nav>
  )
}
```

#### 5. Dashboard por Rol

```typescript
// app/(dashboard)/page.tsx
import { RoleGate } from '@/components/role-based/role-gate'
import { CEODashboard } from '@/components/dashboard/ceo-dashboard'
import { DeveloperDashboard } from '@/components/dashboard/developer-dashboard'
import { ClientDashboard } from '@/components/dashboard/client-dashboard'

export default async function DashboardPage() {
  const user = await getCurrentUser()

  return (
    <div>
      <h1 className="text-2xl font-bold mb-6">Dashboard</h1>

      <RoleGate allowedRoles={['CEO']} fallback={<DeveloperDashboard />}>
        <CEODashboard />
      </RoleGate>

      <RoleGate allowedRoles={['DEVELOPER', 'INTERN']}>
        <DeveloperDashboard />
      </RoleGate>

      <RoleGate allowedRoles={['EXTERNAL_CLIENT']}>
        <ClientDashboard />
      </RoleGate>
    </div>
  )
}
```

### Patrones de UI por Rol

#### CEO Dashboard

```typescript
// components/dashboard/ceo-dashboard.tsx
'use client'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { DollarSign, Users, TrendingUp, AlertTriangle } from 'lucide-react'

export function CEODashboard() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Ingresos Totales</CardTitle>
          <DollarSign className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">$45,231.89</div>
          <p className="text-xs text-muted-foreground">+20.1% vs mes anterior</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Gastos Totales</CardTitle>
          <TrendingUp className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">$12,450.00</div>
          <p className="text-xs text-muted-foreground">+5.2% vs mes anterior</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Proyectos Activos</CardTitle>
          <Users className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">12</div>
          <p className="text-xs text-muted-foreground">3 en riesgo de presupuesto</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Alertas</CardTitle>
          <AlertTriangle className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">5</div>
          <p className="text-xs text-muted-foreground">Tickets estancados</p>
        </CardContent>
      </Card>

      {/* Finanzas Section */}
      <Card className="col-span-2">
        <CardHeader>
          <CardTitle>Finanzas por Proyecto</CardTitle>
        </CardHeader>
        <CardContent>
          <FinancialTable />
        </CardContent>
      </Card>

      {/* Team Performance */}
      <Card className="col-span-2">
        <CardHeader>
          <CardTitle>Rendimiento del Equipo</CardTitle>
        </CardHeader>
        <CardContent>
          <TeamPerformanceChart />
        </CardContent>
      </Card>
    </div>
  )
}
```

#### Developer Dashboard

```typescript
// components/dashboard/developer-dashboard.tsx
'use client'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Clock, CheckCircle, AlertCircle, Play } from 'lucide-react'

export function DeveloperDashboard() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Mis Tickets</CardTitle>
          <CheckCircle className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">24</div>
          <p className="text-xs text-muted-foreground">8 pendientes, 16 completados</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Horas Trabajadas</CardTitle>
          <Clock className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">156h</div>
          <p className="text-xs text-muted-foreground">Esta semana</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Tickets Estancados</CardTitle>
          <AlertCircle className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">2</div>
          <p className="text-xs text-muted-foreground">Más de 7 días sin movimiento</p>
        </CardContent>
      </Card>

      {/* My Tickets */}
      <Card className="col-span-3">
        <CardHeader>
          <CardTitle>Mis Tickets Asignados</CardTitle>
        </CardHeader>
        <CardContent>
          <MyTicketsList />
        </CardContent>
      </Card>

      {/* Time Tracking */}
      <Card className="col-span-3">
        <CardHeader>
          <CardTitle>Historial de Time Tracking</CardTitle>
        </CardHeader>
        <CardContent>
          <TimeTrackingHistory />
        </CardContent>
      </Card>
    </div>
  )
}
```

#### Client Dashboard

```typescript
// components/dashboard/client-dashboard.tsx
'use client'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { FolderOpen, CheckCircle2, Clock, MessageSquare } from 'lucide-react'

export function ClientDashboard() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Mis Proyectos</CardTitle>
          <FolderOpen className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">3</div>
          <p className="text-xs text-muted-foreground">2 activos, 1 completado</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Tickets Creados</CardTitle>
          <CheckCircle2 className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">45</div>
          <p className="text-xs text-muted-foreground">38 completados, 7 pendientes</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Progreso Global</CardTitle>
          <Clock className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">78%</div>
          <p className="text-xs text-muted-foreground">Promedio de todos los proyectos</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
          <CardTitle className="text-sm font-medium">Mensajes</CardTitle>
          <MessageSquare className="h-4 w-4 text-muted-foreground" />
        </CardHeader>
        <CardContent>
          <div className="text-2xl font-bold">12</div>
          <p className="text-xs text-muted-foreground">5 sin leer</p>
        </CardContent>
      </Card>

      {/* My Projects */}
      <Card className="col-span-2">
        <CardHeader>
          <CardTitle>Mis Proyectos</CardTitle>
        </CardHeader>
        <CardContent>
          <ClientProjectsList />
        </CardContent>
      </Card>

      {/* Recent Activity */}
      <Card className="col-span-2">
        <CardHeader>
          <CardTitle>Actividad Reciente</CardTitle>
        </CardHeader>
        <CardContent>
          <RecentActivity />
        </CardContent>
      </Card>
    </div>
  )
}
```

##  Relacionado
- [[../05 - Backend/Seguridad y Permisos (ABAC)|Implementación Backend]]
- [[Layouts y Navegacion|Estructura de Navegación]]
