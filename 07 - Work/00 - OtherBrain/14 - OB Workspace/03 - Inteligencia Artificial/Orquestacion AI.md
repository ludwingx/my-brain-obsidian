---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - AI
  - OpenRouter
---
# Integración de Ecosistemas Inteligentes

##  Vercel AI SDK y OpenRouter

En vez de limitar la plataforma a un CRUD ciego, los agentes de IA se vuelven "colegas" del repositorio para incrementar la eficiencia del Management.

###  Los Patrones de Uso

#### 1. El Asistente Arquitecto (Ticket Architect)
El equipo presiona "Ayuda con este Requerimiento". El string crudo viaja al Backend, donde la API de **OpenRouter** (`@ai-sdk/openai` adaptado) procesa e invoca el modo estructurado (`zod schema`). Retorna un objeto desglosado JSON llenando instantáneamente Múltiples Subtareas con Tiempos Predictivos (Minutos) y validando qué herramientas aplicar de las librerías `ai`.

#### 2. Asistente Conversacional (Tool Calling)
Un Chat lateral permite a los PMs decirle: "Levantame un ticket para cambiar el color del botón rojo". El modelo parsea la intención de lenguaje natural e invoca la Herramienta expuesta (`create_ticket`), escribiendo nativamente en Prisma a través de `AiConversation`.

###  Configuración Técnica Detallada

#### 1. Setup de OpenRouter

```typescript
// lib/ia/openrouter.ts
import { createOpenAI } from '@ai-sdk/openai'
import { generateText, streamText } from 'ai'

const openrouter = createOpenAI({
  baseURL: 'https://openrouter.ai/api/v1',
  apiKey: process.env.OPENROUTER_API_KEY,
})

// Modelos disponibles
export const MODELS = {
  GPT_4O_MINI: 'openai/gpt-4o-mini',
  GPT_4O: 'openai/gpt-4o',
  CLAUDE_3_HAIKU: 'anthropic/claude-3-haiku',
  CLAUDE_3_SONNET: 'anthropic/claude-3-sonnet',
  LLAMA_3_8B: 'meta-llama/llama-3-8b-instruct',
  LLAMA_3_70B: 'meta-llama/llama-3-70b-instruct',
} as const

export async function generateTicketProposal(
  userRequest: string,
  context: {
    projectId?: string
    moduleId?: string
    userRole: UserRole
    projectContext?: string
  }
) {
  const systemPrompt = buildSystemPrompt(context.userRole)

  const { text } = await generateText({
    model: openrouter(MODELS.GPT_4O_MINI),
    system: systemPrompt,
    prompt: userRequest,
    temperature: 0.7,
    maxTokens: 2000,
  })

  return parseAIResponse(text)
}
```

#### 2. System Prompt Dinámico

```typescript
// lib/ia/prompts.ts
export function buildSystemPrompt(userRole: UserRole): string {
  const basePrompt = `Eres un Arquitecto de Software Senior y Project Manager experimentado.
Tu objetivo es ayudar a estructurar requerimientos técnicos en tickets y subtareas accionables.

RESPONSIABILIDADES:
1. Analizar el requerimiento del usuario
2. Desglosar en subtareas específicas y medibles
3. Estimar tiempos realistas (en minutos)
4. Asignar prioridades apropiadas
5. Sugerir módulos o áreas del sistema

FORMATO DE RESPUESTA:
Siempre responde en formato JSON válido con la siguiente estructura:
{
  "summary": "Breve resumen del requerimiento",
  "estimatedTotalTime": tiempo total en minutos,
  "subtasks": [
    {
      "title": "Título de la subtarea",
      "description": "Descripción detallada",
      "estimatedTime": tiempo en minutos,
      "priority": "LOW|MEDIUM|HIGH|CRITICAL"
    }
  ],
  "suggestedPriority": "LOW|MEDIUM|HIGH|CRITICAL",
  "suggestedModule": "Nombre del módulo"
}

ESTIMACIONES DE TIEMPO:
- Configuración inicial: 15-30 minutos
- Implementación simple: 30-60 minutos
- Implementación compleja: 60-120 minutos
- Testing y validación: 15-45 minutos
- Documentación: 15-30 minutos
`

  const roleSpecificInstructions = getRoleInstructions(userRole)

  return `${basePrompt}\n\n${roleSpecificInstructions}`
}

function getRoleInstructions(userRole: UserRole): string {
  switch (userRole) {
    case 'CEO':
      return `
INSTRUCCIONES PARA CEO:
- Enfócate en métricas de negocio, rentabilidad y eficiencia
- Propone iniciativas estratégicas y optimizaciones operativas
- Considera impacto financiero y ROI
- Sugiere mejoras de procesos y gestión de equipo
- Evita detalles técnicos muy específicos
`

    case 'DEVELOPER':
      return `
INSTRUCCIONES PARA DESARROLLADOR:
- Enfócate en implementación técnica y mejores prácticas
- Sugiere refactorización, optimización y patrones de diseño
- Considera arquitectura, performance y mantenibilidad
- Propone mejoras de código y testing
- Usa terminología técnica apropiada
`

    case 'INTERN':
      return `
INSTRUCCIONES PARA INTERN:
- Enfócate en tareas de aprendizaje y contribución simple
- Sugiere tareas apropiadas para nivel junior
- Propone documentación, testing simple, bug fixes menores
- Considera mentoría y crecimiento profesional
- Evita tareas críticas o complejas
`

    case 'EXTERNAL_CLIENT':
      return `
INSTRUCCIONES PARA CLIENTE EXTERNO:
- Enfócate en funcionalidad visible y experiencia de usuario
- Propone features y mejoras desde perspectiva del cliente
- Considera valor de negocio y necesidades del usuario final
- Evita detalles técnicos internos
- Enfócate en resultados y entregables
`

    default:
      return ''
  }
}
```

#### 3. Tool Calling con Vercel AI SDK

```typescript
// lib/ia/tools.ts
import { z } from 'zod'

export const aiTools = {
  createTicket: {
    description: 'Crear un nuevo ticket con subtareas',
    parameters: z.object({
      title: z.string().describe('Título del ticket'),
      description: z.string().describe('Descripción detallada'),
      priority: z.enum(['LOW', 'MEDIUM', 'HIGH', 'CRITICAL']),
      estimatedTime: z.number().describe('Tiempo estimado en minutos'),
      projectId: z.string().optional().describe('ID del proyecto'),
      moduleId: z.string().optional().describe('ID del módulo'),
      subtasks: z.array(
        z.object({
          title: z.string(),
          description: z.string(),
          estimatedTime: z.number(),
          priority: z.enum(['LOW', 'MEDIUM', 'HIGH', 'CRITICAL']),
        })
      ),
    }),
    execute: async (params: any) => {
      // Lógica para crear ticket en Prisma
      const ticket = await prisma.ticket.create({
        data: {
          title: params.title,
          description: params.description,
          priority: params.priority,
          estimatedTime: params.estimatedTime,
          projectId: params.projectId,
          moduleId: params.moduleId,
          subtasks: {
            create: params.subtasks,
          },
        },
      })
      return ticket
    },
  },

  getProjectStatus: {
    description: 'Obtener estado actual de un proyecto',
    parameters: z.object({
      projectId: z.string(),
    }),
    execute: async (params: any) => {
      const project = await prisma.project.findUnique({
        where: { id: params.projectId },
        include: {
          tickets: {
            where: {
              status: {
                in: ['TODO', 'IN_PROGRESS', 'IN_REVIEW'],
              },
            },
          },
          _count: {
            select: {
              tickets: true,
            },
          },
        },
      })
      return project
    },
  },

  getDeveloperWorkload: {
    description: 'Obtener carga de trabajo actual de un desarrollador',
    parameters: z.object({
      developerId: z.string(),
    }),
    execute: async (params: any) => {
      const workload = await prisma.workSession.groupBy({
        by: ['subtaskId'],
        where: {
          userId: params.developerId,
          endTime: null,
        },
      })
      return workload
    },
  },
}
```

#### 4. Chat Interface con Streaming

```typescript
// components/ai/ai-chat-interface.tsx
'use client'

import { useChat } from 'ai/react'
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'

export function AIChatInterface({
  projectId,
  moduleId,
  userRole,
}: {
  projectId?: string
  moduleId?: string
  userRole: UserRole
}) {
  const { messages, input, handleInputChange, handleSubmit, isLoading } = useChat({
    api: '/api/ai/chat',
    body: {
      projectId,
      moduleId,
      userRole,
    },
  })

  return (
    <div className="flex flex-col h-full">
      <div className="flex-1 overflow-y-auto space-y-4 p-4">
        {messages.map((message) => (
          <div
            key={message.id}
            className={`flex ${
              message.role === 'user' ? 'justify-end' : 'justify-start'
            }`}
          >
            <div
              className={`max-w-[80%] rounded-lg p-3 ${
                message.role === 'user'
                  ? 'bg-primary text-primary-foreground'
                  : 'bg-muted'
              }`}
            >
              <p className="text-sm">{message.content}</p>
              {message.toolInvocations && (
                <AIProposalCard
                  proposal={parseToolInvocation(message.toolInvocations[0])}
                />
              )}
            </div>
          </div>
        ))}
        {isLoading && (
          <div className="flex justify-start">
            <div className="bg-muted rounded-lg p-3">
              <div className="flex space-x-2">
                <div className="w-2 h-2 bg-current rounded-full animate-bounce" />
                <div className="w-2 h-2 bg-current rounded-full animate-bounce delay-100" />
                <div className="w-2 h-2 bg-current rounded-full animate-bounce delay-200" />
              </div>
            </div>
          </div>
        )}
      </div>

      <form onSubmit={handleSubmit} className="p-4 border-t">
        <div className="flex gap-2">
          <Input
            value={input}
            onChange={handleInputChange}
            placeholder="Describe tu requerimiento..."
            disabled={isLoading}
          />
          <Button type="submit" disabled={isLoading}>
            Enviar
          </Button>
        </div>
      </form>
    </div>
  )
}
```

###  Seguridad Anti Abuso

#### Rate Limits y Cost Control

```typescript
// lib/ia/rate-limit.ts
import { Ratelimit } from '@upstash/ratelimit'
import { Redis } from '@upstash/redis'

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(10, '1 m'), // 10 requests por minuto
  analytics: true,
})

export async function checkRateLimit(userId: string): Promise<boolean> {
  const { success } = await ratelimit.limit(userId)
  return success
}

export async function checkDailyLimit(userId: string): Promise<boolean> {
  const { success } = await ratelimit.limit(`daily:${userId}`, {
    limiter: Ratelimit.slidingWindow(100, '1 d'), // 100 requests por día
  })
  return success
}
```

#### Estrategia de Modelos por Costo

```typescript
// lib/ia/model-selector.ts
export function selectModel(
  complexity: 'simple' | 'medium' | 'complex',
  userRole: UserRole
): string {
  // Clientes usan modelos más baratos
  if (userRole === 'EXTERNAL_CLIENT') {
    return MODELS.LLAMA_3_8B
  }

  // Interns usan modelos económicos
  if (userRole === 'INTERN') {
    return MODELS.CLAUDE_3_HAIKU
  }

  // Developers usan modelos balanceados
  if (userRole === 'DEVELOPER') {
    switch (complexity) {
      case 'simple':
        return MODELS.CLAUDE_3_HAIKU
      case 'medium':
        return MODELS.GPT_4O_MINI
      case 'complex':
        return MODELS.GPT_4O
    }
  }

  // CEO usa modelos premium para decisiones estratégicas
  if (userRole === 'CEO') {
    return MODELS.GPT_4O
  }

  return MODELS.GPT_4O_MINI
}
```

###  Casos de Uso Avanzados

#### Caso 1: Análisis de Proyecto Completo

```typescript
// app/actions/ai.ts
export async function analyzeProject(projectId: string) {
  const project = await prisma.project.findUnique({
    where: { id: projectId },
    include: {
      tickets: {
        include: {
          subtasks: true,
          workSessions: true,
        },
      },
      expenses: true,
    },
  })

  const analysis = await generateText({
    model: openrouter(MODELS.GPT_4O),
    system: `Eres un analista de proyectos experto. Analiza el siguiente proyecto y proporciona:
1. Estado de salud general del proyecto
2. Riesgos identificados
3. Recomendaciones para mejorar eficiencia
4. Predicción de fecha de finalización
5. Análisis de rentabilidad`,
    prompt: JSON.stringify(project, null, 2),
  })

  return analysis.text
}
```

#### Caso 2: Generación de Release Notes

```typescript
export async function generateReleaseNotes(projectId: string, period: string) {
  const tickets = await prisma.ticket.findMany({
    where: {
      projectId,
      status: 'DONE',
      updatedAt: {
        gte: new Date(period),
      },
    },
    include: {
      subtasks: true,
    },
  })

  const releaseNotes = await generateText({
    model: openrouter(MODELS.GPT_4O_MINI),
    system: `Genera release notes amigables para clientes externos basados en los tickets completados.
Usa lenguaje simple y enfócate en beneficios para el usuario final.`,
    prompt: JSON.stringify(tickets, null, 2),
  })

  return releaseNotes.text
}
```

#### Caso 3: Optimización de Carga de Trabajo

```typescript
export async function optimizeWorkload() {
  const developers = await prisma.user.findMany({
    where: {
      role: {
        in: ['DEVELOPER', 'INTERN'],
      },
    },
    include: {
      ticketsLead: {
        where: {
          status: {
            in: ['TODO', 'IN_PROGRESS'],
          },
        },
      },
      workSessions: {
        where: {
          endTime: null,
        },
      },
    },
  })

  const optimization = await generateText({
    model: openrouter(MODELS.GPT_4O),
    system: `Eres un gestor de recursos experto. Analiza la carga de trabajo actual del equipo y sugiere:
1. Reasignaciones de tickets para balancear carga
2. Tickets que deberían ser priorizados
3. Desarrolladores con sobrecarga o subcarga
4. Recomendaciones para mejorar eficiencia del equipo`,
    prompt: JSON.stringify(developers, null, 2),
  })

  return optimization.text
}
```

##  Relacionado
- [[IA Flow - Del Chat al Ticket|Flujo de Trabajo Detallado]]
- [[../../05 - Backend/Server Actions|Server Actions para IA]]
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Datos]]
