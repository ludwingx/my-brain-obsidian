---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - AI
  - Workflow
---
# Flujo IA: Del Chat a la Acción (Ticket Architect)

##  El Corazón Inteligente de OB-Workspace

A diferencia de un chat convencional, la IA en este workspace actúa como un **Arquitecto de Software**. Su objetivo no es solo responder dudas, sino estructurar el trabajo técnico.

###  Flujo de Generación Estructurada

1.  **Prompt de Sistema (System Prompt):** Ubicado en `lib/ia/openrouter.ts`, obliga al modelo (`gpt-4o-mini`) a comportarse como un Project Manager Senior.
2.  **Inferencia de Requerimientos:** Cuando el usuario describe una tarea ("Necesito arreglar el login"), el modelo analiza el texto y genera:
    - Una explicación amigable del plan de acción.
    - Un bloque **`JSON_PROPOSAL`** (invisible al usuario en su forma cruda, formateado visualmente en el frontend).
3.  **El Botón Mágico "Aprobar y Crear":**
    - El componente `ai-chat-interface.tsx` detecta el JSON estructurado.
    - Ofrece al usuario un botón de acción rápida.
4.  **Mutación Atómica (Server Action):**
    - Al aprobar, los datos viajan a `createTicketFromAI`.
    - Prisma crea el `Ticket` y desglosa automáticamente las `Subtasks` con los tiempos estimados por la IA.
5.  **Contextualización Dinámica (New):**
    - El asistente ahora hereda el `projectId` y `moduleId` mediante parámetros de URL.
    - La interfaz de chat muestra un badge del proyecto activo y personaliza el saludo inicial.
    - La tarjeta de propuesta incluye un selector de destino explícito, permitiendo confirmar o cambiar la asignación (Proyecto vs Módulo) antes de la creación final.

###  Ejemplo Completo de Flujo

#### Escenario: Usuario solicita implementación de autenticación

**Usuario:**
```
Necesito implementar autenticación con Google OAuth en el proyecto CRM
```

**IA Response (JSON Proposal):**
```json
{
  "summary": "Implementación completa de Google OAuth para autenticación de usuarios en el proyecto CRM",
  "estimatedTotalTime": 240,
  "subtasks": [
    {
      "title": "Configurar Google Cloud Console y obtener credenciales OAuth",
      "description": "Crear proyecto en Google Cloud, configurar OAuth 2.0, obtener Client ID y Secret",
      "estimatedTime": 30,
      "priority": "HIGH"
    },
    {
      "title": "Instalar y configurar NextAuth.js con Google Provider",
      "description": "Instalar next-auth, configurar Google provider en auth options",
      "estimatedTime": 45,
      "priority": "HIGH"
    },
    {
      "title": "Crear endpoints de API para autenticación",
      "description": "Implementar /api/auth/[...nextauth]/route.ts con callbacks",
      "estimatedTime": 60,
      "priority": "HIGH"
    },
    {
      "title": "Actualizar modelo User en Prisma para almacenar Google ID",
      "description": "Agregar campo googleId, emailVerified, image en modelo User",
      "estimatedTime": 30,
      "priority": "MEDIUM"
    },
    {
      "title": "Crear UI de login con botón de Google",
      "description": "Implementar componente LoginButton con icono de Google",
      "estimatedTime": 45,
      "priority": "MEDIUM"
    },
    {
      "title": "Implementar middleware de protección de rutas",
      "description": "Configurar middleware.ts para verificar sesión JWT",
      "estimatedTime": 30,
      "priority": "HIGH"
    }
  ],
  "suggestedPriority": "HIGH",
  "suggestedModule": "Authentication"
}
```

**UI Response:**
```
He analizado tu requerimiento de implementar Google OAuth. Aquí está mi propuesta estructurada:

## Resumen
Implementación completa de Google OAuth para autenticación de usuarios en el proyecto CRM

## Plan de Acción
He desglosado la implementación en 6 subtareas con un tiempo estimado total de 4 horas:

1. Configurar Google Cloud Console y obtener credenciales OAuth (30 min) - HIGH
2. Instalar y configurar NextAuth.js con Google Provider (45 min) - HIGH
3. Crear endpoints de API para autenticación (60 min) - HIGH
4. Actualizar modelo User en Prisma para almacenar Google ID (30 min) - MEDIUM
5. Crear UI de login con botón de Google (45 min) - MEDIUM
6. Implementar middleware de protección de rutas (30 min) - HIGH

[Botón: Aprobar y Crear Ticket] [Botón: Modificar Propuesta]
```

###  Casos de Uso Específicos

#### Caso 1: Refactorización de Código

**Usuario:**
```
El componente UserProfile.tsx tiene 500 líneas y es difícil de mantener. Necesito refactorizarlo.
```

**IA Response:**
```json
{
  "summary": "Refactorización de UserProfile.tsx para mejorar mantenibilidad y legibilidad",
  "estimatedTotalTime": 180,
  "subtasks": [
    {
      "title": "Analizar estructura actual del componente",
      "description": "Identificar responsabilidades, dependencias y puntos de mejora",
      "estimatedTime": 30,
      "priority": "MEDIUM"
    },
    {
      "title": "Extraer componentes reutilizables (UserAvatar, UserStats, UserSettings)",
      "description": "Crear componentes separados para cada sección del perfil",
      "estimatedTime": 60,
      "priority": "HIGH"
    },
    {
      "title": "Implementar custom hooks para lógica de negocio",
      "description": "Crear useUserProfile, useUserSettings hooks",
      "estimatedTime": 45,
      "priority": "MEDIUM"
    },
    {
      "title": "Optimizar rendimiento con React.memo y useMemo",
      "description": "Aplicar memoización donde sea apropiado",
      "estimatedTime": 30,
      "priority": "LOW"
    },
    {
      "title": "Actualizar tests unitarios para nuevos componentes",
      "description": "Escribir tests para componentes extraídos",
      "estimatedTime": 15,
      "priority": "MEDIUM"
    }
  ],
  "suggestedPriority": "MEDIUM",
  "suggestedModule": "Frontend"
}
```

#### Caso 2: Corrección de Bug

**Usuario:**
```
El formulario de contacto no envía emails cuando el usuario usa Gmail. Solo funciona con otros proveedores.
```

**IA Response:**
```json
{
  "summary": "Investigación y corrección de bug en envío de emails a Gmail",
  "estimatedTotalTime": 90,
  "subtasks": [
    {
      "title": "Revisar logs de errores en servidor de emails",
      "description": "Identificar mensajes de error específicos para Gmail",
      "estimatedTime": 20,
      "priority": "HIGH"
    },
    {
      "title": "Verificar configuración SPF/DKIM/DMARC",
      "description": "Comprobar registros DNS para evitar spam de Gmail",
      "estimatedTime": 30,
      "priority": "HIGH"
    },
    {
      "title": "Probar envío con diferentes proveedores de email",
      "description": "Test con Gmail, Outlook, Yahoo para aislar el problema",
      "estimatedTime": 20,
      "priority": "MEDIUM"
    },
    {
      "title": "Implementar corrección y validar con Gmail",
      "description": "Aplicar fix y confirmar funcionamiento con Gmail",
      "estimatedTime": 20,
      "priority": "HIGH"
    }
  ],
  "suggestedPriority": "CRITICAL",
  "suggestedModule": "Backend"
}
```

#### Caso 3: Nueva Feature

**Usuario:**
```
Quiero agregar un dashboard de analytics para el CEO con métricas financieras y de rendimiento del equipo.
```

**IA Response:**
```json
{
  "summary": "Desarrollo de dashboard de analytics para CEO con métricas financieras y de equipo",
  "estimatedTotalTime": 480,
  "subtasks": [
    {
      "title": "Definir KPIs y métricas a mostrar",
      "description": "Identificar métricas clave: ingresos, gastos, velocidad del equipo, productividad",
      "estimatedTime": 60,
      "priority": "HIGH"
    },
    {
      "title": "Crear endpoints de API para datos agregados",
      "description": "Implementar queries de Prisma para métricas financieras y de rendimiento",
      "estimatedTime": 90,
      "priority": "HIGH"
    },
    {
      "title": "Diseñar layout y componentes del dashboard",
      "description": "Crear estructura visual con cards, gráficos y tablas",
      "estimatedTime": 60,
      "priority": "MEDIUM"
    },
    {
      "title": "Implementar gráficos con Recharts o Tremor",
      "description": "Crear componentes para visualizar datos (líneas, barras, pies)",
      "estimatedTime": 90,
      "priority": "HIGH"
    },
    {
      "title": "Agregar filtros y selector de período",
      "description": "Permitir filtrar por mes, trimestre, año",
      "estimatedTime": 45,
      "priority": "MEDIUM"
    },
    {
      "title": "Implementar caching para performance",
      "description": "Usar React Cache para evitar queries repetitivas",
      "estimatedTime": 45,
      "priority": "MEDIUM"
    },
    {
      "title": "Protección de ruta (solo CEO)",
      "description": "Configurar middleware y permisos ABAC",
      "estimatedTime": 30,
      "priority": "HIGH"
    },
    {
      "title": "Testing y validación con CEO",
      "description": "Revisión final con stakeholder principal",
      "estimatedTime": 60,
      "priority": "MEDIUM"
    }
  ],
  "suggestedPriority": "HIGH",
  "suggestedModule": "Analytics"
}
```

###  Contexto de Rol (ABAC Injected)

Para que la IA no proponga cosas fuera de lugar, el sistema inyecta el rol del usuario (`CEO`, `DEV`, `CLIENT`) en cada mensaje de forma programática.

#### Ejemplo por Rol

**CEO:**
```
Usuario: ¿Cómo podemos mejorar la rentabilidad de los proyectos?

IA Response (CEO Context):
Para mejorar la rentabilidad de los proyectos, te sugiero:

1. **Análisis de Margen por Proyecto**: Implementar un dashboard que compare presupuesto vs gastos reales
2. **Alertas de Desviación**: Notificaciones automáticas cuando un proyecto excede el 80% del presupuesto
3. **Optimización de Recursos**: Reasignar desarrolladores basándose en carga de trabajo actual
4. **Revisión de Gastos Fijos**: Auditoría mensual de suscripciones y servicios recurrentes

¿Quieres que cree un ticket para implementar alguna de estas iniciativas?
```

**Developer:**
```
Usuario: ¿Cómo puedo optimizar el rendimiento del KanbanBoard?

IA Response (Developer Context):
Para optimizar el rendimiento del KanbanBoard, te recomiendo:

1. **Server Components**: Cargar datos en el servidor para reducir payload de JavaScript
2. **React Cache**: Implementar caching de queries de Prisma
3. **Virtual Scrolling**: Si tienes muchos tickets, considera react-window o react-virtualized
4. **Suspense Boundaries**: Cargar columnas del Kanban de forma progresiva
5. **Optimistic Updates**: Usar useOptimistic para actualizaciones instantáneas visuales

¿Quieres que cree un ticket para implementar alguna de estas optimizaciones?
```

**External Client:**
```
Usuario: ¿Cuándo estará listo mi proyecto?

IA Response (Client Context):
Basado en el progreso actual de tu proyecto "CRM System":

**Progreso General**: 65% completado
**Tickets Pendientes**: 12 tickets restantes
**Tiempo Estimado**: 3-4 semanas para finalización

**Próximos Hitos**:
- Semana 1: Finalizar módulo de autenticación
- Semana 2: Implementar dashboard de analytics
- Semana 3: Testing y correcciones
- Semana 4: Deploy y release

¿Te gustaría que agende una reunión de seguimiento para discutir el progreso?
```

###  Integración con el Sistema

#### Server Action: createTicketFromAI

```typescript
// app/actions/ai.ts
'use server'

import { prisma } from '@/lib/prisma'
import { can } from '@/lib/permissions'
import { revalidatePath } from 'next/cache'

export async function createTicketFromAI(
  proposal: AIProposal,
  projectId: string,
  moduleId?: string
) {
  const user = await getCurrentUser()

  if (!can(user, 'create', 'ticket')) {
    throw new Error('Unauthorized')
  }

  // Crear ticket principal
  const ticket = await prisma.ticket.create({
    data: {
      title: proposal.summary,
      description: `AI-generated ticket based on user request\n\n${proposal.subtasks.map(s => `- ${s.title}: ${s.description}`).join('\n')}`,
      estimatedTime: proposal.estimatedTotalTime,
      priority: proposal.suggestedPriority,
      projectId,
      moduleId,
      creatorId: user.id,
      subtasks: {
        create: proposal.subtasks.map((subtask, index) => ({
          title: subtask.title,
          description: subtask.description,
          estimatedTime: subtask.estimatedTime,
          priority: subtask.priority,
          order: index
        }))
      }
    }
  })

  revalidatePath('/dashboard/projects')
  revalidatePath('/dashboard/tickets')

  return ticket
}
```

#### Componente: AIProposalCard

```typescript
// components/ai/ai-proposal-card.tsx
'use client'

import { Button } from '@/components/ui/button'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'

interface AIProposalCardProps {
  proposal: AIProposal
  onApprove: () => void
  onModify: () => void
}

export function AIProposalCard({ proposal, onApprove, onModify }: AIProposalCardProps) {
  return (
    <Card className="border-2 border-primary">
      <CardHeader>
        <CardTitle className="flex items-center justify-between">
          <span>Propuesta de IA</span>
          <span className="text-sm text-muted-foreground">
            {Math.floor(proposal.estimatedTotalTime / 60)}h {proposal.estimatedTotalTime % 60}m
          </span>
        </CardTitle>
      </CardHeader>
      <CardContent className="space-y-4">
        <div>
          <h4 className="font-semibold mb-2">Resumen</h4>
          <p className="text-sm text-muted-foreground">{proposal.summary}</p>
        </div>

        <div>
          <h4 className="font-semibold mb-2">Subtareas ({proposal.subtasks.length})</h4>
          <ul className="space-y-2">
            {proposal.subtasks.map((subtask, index) => (
              <li key={index} className="text-sm">
                <span className="font-medium">{subtask.title}</span>
                <span className="text-muted-foreground ml-2">
                  ({Math.floor(subtask.estimatedTime / 60)}h {subtask.estimatedTime % 60}m)
                </span>
                <span className="ml-2 px-2 py-0.5 rounded-full text-xs bg-secondary">
                  {subtask.priority}
                </span>
              </li>
            ))}
          </ul>
        </div>

        <div className="flex gap-2">
          <Button onClick={onApprove} className="flex-1">
            Aprobar y Crear
          </Button>
          <Button onClick={onModify} variant="outline" className="flex-1">
            Modificar
          </Button>
        </div>
      </CardContent>
    </Card>
  )
}
```

##  Relacionado
- [[Orquestacion AI|Ver Configuración Técnica de IA]]
- [[../../05 - Backend/Server Actions|Acciones de Servidor Relacionadas]]
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Tickets y Subtareas]]
