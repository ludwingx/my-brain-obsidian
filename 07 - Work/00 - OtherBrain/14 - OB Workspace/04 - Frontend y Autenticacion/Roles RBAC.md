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

1. **CEO / ADMIN_DEV:** Panel Master. Poseen acceso global. `ADMIN_DEV` tiene control total sobre gestión técnica y personal (roles y configuración). CEO mantiene el control exclusivo financiero.
2. **Client:** Ruta paralela que lo aísla físicamente (`/portal`).
3. **Intern / Dev:** Ven el Kanban global, pero el Developer sólo mueve *Cards (Tickets)* en las que está atado como `ticket.leadId` o `ticket.collaborators`. El *Intern* se limita exclusivamente a lo que le asignaron expresamente, evitando mover tickets críticos que alteren los reportes productivos de la agencia.

### Implementación de Roles en Frontend

#### 1. Hook de Autenticación

**Funcionalidades:**
- Obtiene el usuario actual desde `/api/auth/user`
- Maneja estados: user, loading, isAuthenticated
- Se ejecuta automáticamente al montar el componente
- Retorna null si hay error o no hay sesión

**Uso típico:**
- Verificar si el usuario está autenticado
- Obtener información del usuario (rol, nombre, etc.)
- Mostrar estados de carga

#### 2. Componente Role-Based

**Props:**
- children: Contenido a renderizar si el usuario tiene permiso
- allowedRoles: Array de roles permitidos
- fallback: Contenido opcional si no tiene permiso

**Lógica:**
- Muestra estado de carga mientras obtiene el usuario
- Si el usuario no existe o su rol no está en allowedRoles, muestra fallback
- Si tiene permiso, renderiza children

**Uso típico:**
- Proteger secciones completas del dashboard
- Mostrar componentes solo a roles específicos
- Evitar que usuarios vean UI que no pueden usar

#### 3. Botones Condicionales por Rol

**Props:**
- ticket: Objeto del ticket
- onEdit, onDelete, onView: Callbacks para cada acción

**Lógica de permisos:**
- canView: Todos pueden ver tickets
- canEdit: CEO, ADMIN_DEV, o lead del ticket
- canDelete: CEO, ADMIN_DEV, o lead del ticket

**Características:**
- Solo renderiza botones que el usuario puede usar
- Usa íconos de Lucide (Eye, Edit, Trash2)
- Estilo ghost para no distraer
- Botón delete con color destructivo

**Uso típico:**
- Filas de tablas de tickets
- Cards de tickets en kanban
- Listas de proyectos

#### 4. Navegación Contextual

**Estructura de navegación:**
- Dashboard: Todos
- Proyectos: Todos
- Tickets: Todos
- Time Tracking: Todos
- Analytics: Solo CEO
- Finanzas: Solo CEO
- Admin: Solo CEO

**Lógica:**
- Filtra items de navegación según el rol del usuario
- Si item.roles está vacío, todos pueden verlo
- Si item.roles tiene valores, solo esos roles lo ven

**Características:**
- Usa íconos de Lucide
- Estilo hover con bg-accent
- Espaciado vertical entre items
- Responsive

#### 5. Dashboard por Rol

**Lógica de renderizado:**
- CEO / ADMIN_DEV: Muestra CEODashboard (con reportes avanzados, analytics). CEO también ve finanzas.
- DEVELOPER/INTERN: Muestra DeveloperDashboard (tickets asignados, time tracking)
- EXTERNAL_CLIENT: Muestra ClientDashboard (proyectos, tickets creados)

**Características:**
- Server Component (obtiene usuario en servidor)
- Usa RoleGate para proteger cada dashboard
- Fallback para CEO/ADMIN_DEV muestra DeveloperDashboard si los permisos no coinciden.
- Título común "Dashboard"

### Patrones de UI por Rol

#### CEO Dashboard

**KPIs principales:**
- Ingresos Totales: $45,231.89 (+20.1% vs mes anterior)
- Gastos Totales: $12,450.00 (+5.2% vs mes anterior)
- Proyectos Activos: 12 (3 en riesgo de presupuesto)
- Alertas: 5 (tickets estancados)

**Secciones principales:**
- Finanzas por Proyecto: Tabla con ingresos, gastos, rentabilidad
- Rendimiento del Equipo: Gráfico de productividad por desarrollador

**Layout:**
- Grid de 4 columnas en desktop
- Cards con íconos de Lucide
- Secciones amplias ocupan 2 columnas
- Estadísticas comparativas vs mes anterior

#### Developer Dashboard

**KPIs principales:**
- Mis Tickets: 24 (8 pendientes, 16 completados)
- Horas Trabajadas: 156h (esta semana)
- Tickets Estancados: 2 (más de 7 días sin movimiento)

**Secciones principales:**
- Mis Tickets Asignados: Lista de tickets donde es lead o colaborador
- Historial de Time Tracking: Registro de horas trabajadas por ticket

**Layout:**
- Grid de 3 columnas en desktop
- Foco en productividad personal
- Secciones amplias ocupan todo el ancho
- Íconos de Lucide para cada KPI

#### Client Dashboard

**KPIs principales:**
- Mis Proyectos: 3 (2 activos, 1 completado)
- Tickets Creados: 45 (38 completados, 7 pendientes)
- Progreso Global: 78% (promedio de todos los proyectos)
- Mensajes: 12 (5 sin leer)

**Secciones principales:**
- Mis Proyectos: Lista de proyectos del cliente
- Actividad Reciente: Historial de cambios en tickets y proyectos

**Layout:**
- Grid de 2 columnas en desktop
- Foco en visibilidad del progreso
- Secciones amplias ocupan todo el ancho
- Íconos de Lucide para cada KPI

### Diagrama de Matriz de Permisos por Rol

```mermaid
graph LR
    subgraph CEO["CEO/ADMIN_DEV - Master Control"]
        A1[Finanzas (Solo CEO)]
        A2[Analytics]
        A3[Admin/Gestión Usuarios]
        A4[Todos los Tickets]
        A5[Todos los Proyectos]
    end

    subgraph DEVELOPER["DEVELOPER - Desarrollo"]
        B1[Tickets Asignados]
        B2[Tickets Creados]
        B3[Proyectos Visibles]
        B4[Time Tracking]
        B5[Chat IA]
    end

    subgraph INTERN["INTERN - Tareas Asignadas"]
        C1[Tickets Asignados]
        C2[Time Tracking]
        C3[Chat IA]
        C4[Solo lectura]
    end

    subgraph CLIENT["CLIENT - Solo sus Proyectos"]
        D1[Mis Proyectos]
        D2[Tickets Creados]
        D3[Progreso Visible]
        D4[Chat IA]
    end

    style CEO fill:#e6f4ea
    style DEVELOPER fill:#e8f0fe
    style INTERN fill:#fce8e6
    style CLIENT fill:#fff3e0
```

##  Relacionado
- [[../05 - Backend/Seguridad y Permisos (ABAC)|Implementación Backend]]
- [[Layouts y Navegacion|Estructura de Navegación]]
