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

**app/(auth)/:**
- `login/page.tsx` - Server Component para login
- `login/_components/LoginForm.tsx` - Client Component con formulario
- `register/page.tsx` - Server Component para registro
- `register/_components/RegisterForm.tsx` - Client Component con formulario

**app/(dashboard)/:**
- `layout.tsx` - Layout protegido con sidebar y header
- `page.tsx` - Dashboard home con métricas principales
- `projects/page.tsx` - Lista de proyectos
- `projects/[slug]/page.tsx` - Detalle de proyecto
- `projects/[slug]/_components/` - Componentes específicos del proyecto
- `tickets/page.tsx` - Lista global de tickets
- `tickets/[id]/page.tsx` - Detalle de ticket
- `tracking/page.tsx` - Time tracking y reportes
- `analytics/page.tsx` - Dashboards de métricas
- `finances/page.tsx` - Módulo financiero (CEO only)
- `admin/page.tsx` - Configuración global (CEO only)

**app/(portal)/:**
- `layout.tsx` - Layout simplificado para clientes
- `page.tsx` - Home del portal
- `projects/page.tsx` - Proyectos del cliente
- `tickets/page.tsx` - Tickets del cliente

**app/api/:**
- `auth/[...nextauth]/route.ts` - Configuración NextAuth
- `webhooks/stripe/route.ts` - Webhooks de pagos

**components/:**
- `ui/` - shadcn/ui components reutilizables
- `ai/ai-chat-interface.tsx` - Chat con IA
- `theme-provider.tsx` - Provider de temas
- `providers.tsx` - Providers globales

**lib/:**
- `prisma.ts` - Singleton Prisma
- `permissions.ts` - Lógica ABAC
- `ia/openrouter.ts` - Configuración IA

**actions/:**
- `tickets.ts` - Server Actions para tickets
- `tracking.ts` - Server Actions para time tracking
- `finances.ts` - Server Actions para finanzas
- `ai.ts` - Server Actions para IA

**middleware.ts** - Protección de rutas

### Diagrama de Estructura de Carpetas

```mermaid
graph TD
    A[app/] --> B[(auth)/]
    A --> C[(dashboard)/]
    A --> D[(portal)/]
    A --> E[api/]
    A --> F[components/]
    A --> G[lib/]
    A --> H[actions/]
    A --> I[middleware.ts]

    B --> B1[login/]
    B --> B2[register/]
    B --> B3[forgot-password/]

    C --> C1[layout.tsx]
    C --> C2[page.tsx]
    C --> C3[projects/]
    C --> C4[tickets/]
    C --> C5[tracking/]
    C --> C6[analytics/]
    C --> C7[finances/]
    C --> C8[admin/]

    D --> D1[layout.tsx]
    D --> D2[page.tsx]
    D --> D3[projects/]
    D --> D4[tickets/]

    E --> E1[auth/]
    E --> E2[webhooks/]

    F --> F1[ui/]
    F --> F2[ai/]
    F --> F3[theme-provider.tsx]
    F --> F4[providers.tsx]

    G --> G1[prisma.ts]
    G --> G2[permissions.ts]
    G --> G3[ia/]

    H --> H1[tickets.ts]
    H --> H2[tracking.ts]
    H --> H3[finances.ts]
    H --> H4[ai.ts]
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
- Directiva `'use server'` al inicio del archivo
- Import de `revalidatePath` para invalidación de cache
- Import de `can` para verificación de permisos
- Funciones exportadas que ejecutan en servidor
- Validación de permisos antes de mutar datos
- Revalidación de rutas después de mutaciones

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
- Buscar proyecto por slug con Prisma
- Incluir tickets, subtareas y colaboradores
- Renderizar ProjectHeader
- Usar Suspense con KanbanSkeleton como fallback
- Cargar KanbanBoard de forma progresiva

#### Edge Runtime para Acciones Críticas
**Uso:** Server Actions que requieren baja latencia
**Beneficios:**
- Respuestas más rápidas
- Menos carga en servidor principal
- Mejor experiencia de usuario

**Implementación:**
- Directiva `export const runtime = 'edge'`
- Acciones de time tracking con baja latencia
- Ideal para operaciones que requieren respuesta inmediata

### Patrones de Seguridad

#### 1. Middleware Defense
**Ubicación:** `middleware.ts`
**Función:** Primera línea de defensa, verifica sesión antes de llegar a rutas

**Lógica de Verificación:**
- Extraer token de cookies
- Obtener path de la request
- Verificar si path requiere autenticación
- Redirigir a login si no hay token
- Permitir acceso si token existe

**Rutas Protegidas:**
- `/dashboard/*` - Requiere sesión JWT
- `/portal/*` - Requiere sesión JWT
- `/login`, `/register` - Acceso público

#### 2. Server Actions Defense
**Ubicación:** `app/actions/*.ts`
**Función:** Segunda línea de defensa, verifica permisos antes de mutar datos

**Lógica de Verificación:**
- Obtener usuario actual autenticado
- Buscar recurso en base de datos
- Verificar permisos con función `can(user, action, resource, resourceId)`
- Lanzar error si no tiene permisos
- Ejecutar mutación si tiene permisos

**Protección:**
- Verificación de permisos ABAC
- Validación de existencia de recursos
- Protección contra manipulación de IDs

#### 3. Database-Level Filtering
**Ubicación:** Queries de Prisma
**Función:** Tercera línea de defensa, filtra datos por usuario

**Lógica de Filtrado:**
- Cliente externo solo ve sus proyectos
- Developer ve tickets donde es lead o colaborador
- CEO ve todos los datos
- Filtrado por projectId, userId, etc.

**Ejemplo:**
- Cliente externo: `where: { projectId: user.projectId }`
- Developer: `where: { OR: [{ leadId: user.id }, { collaborators: { userId: user.id } }] }`
- CEO: Sin filtros, acceso total

### Patrones de Caching

#### 1. React Cache
**Uso:** Cache de queries repetitivas
**Implementación:**
- Importar `cache` de `react`
- Envolver función de query con `cache()`
- Cache persiste mientras el componente está montado
- Evita queries duplicadas

**Beneficios:**
- Reducción de queries a base de datos
- Mejor rendimiento
- Menos carga en servidor

#### 2. revalidatePath
**Uso:** Invalidación de cache después de mutaciones
**Implementación:**
- Importar `revalidatePath` de `next/cache`
- Llamar después de mutación exitosa
- Invalida cache de rutas específicas
- Actualiza datos en UI

**Ejemplo:**
- Después de actualizar ticket: `revalidatePath('/dashboard/projects')`
- Después de crear ticket: `revalidatePath('/dashboard/tickets')`

#### 3. revalidateTag
**Uso:** Invalidación granular por tags
**Implementación:**
- Asignar tag a queries específicas
- Invalidar por tag en lugar de por path
- Mayor granularidad de control
- Evita invalidación excesiva

**Ejemplo:**
- Query con tag: `revalidateTag('projects')`
- Invalidación específica después de crear proyecto
- Solo invalida queries con ese tag

##  Relacionado
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Datos]]
- [[../../04 - Frontend y Autenticacion/Layouts y Navegacion|UI y Navegación]]
- [[../../05 - Backend/Server Actions|Server Actions Detallados]]
