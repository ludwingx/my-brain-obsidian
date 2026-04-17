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
El equipo presiona "Ayuda con este Requerimiento". El string crudo viaja al Backend, donde la API de **OpenRouter** procesa e invoca el modo estructurado. Retorna un objeto desglosado JSON llenando instantáneamente Múltiples Subtareas con Tiempos Predictivos (Minutos) y validando qué herramientas aplicar.

#### 2. Asistente Conversacional (Tool Calling)
Un Chat lateral permite a los PMs decirle: "Levantame un ticket para cambiar el color del botón rojo". El modelo parsea la intención de lenguaje natural e invoca la Herramienta expuesta (`create_ticket`), escribiendo nativamente en Prisma a través de `AiConversation`.

###  Configuración Técnica Detallada

#### 1. Setup de OpenRouter

**Configuración del Cliente:**
- Base URL: `https://openrouter.ai/api/v1`
- API Key: Variable de entorno `OPENROUTER_API_KEY`
- SDK: Vercel AI SDK con adaptador OpenRouter

**Catálogo de Modelos Disponibles:**
- **GPT-4o-mini**: Modelo principal para generación de tickets (balance costo/rendimiento)
- **GPT-4o**: Modelo premium para análisis complejos y decisiones estratégicas
- **Claude 3 Haiku**: Modelo económico para tareas simples y conversación
- **Claude 3 Sonnet**: Modelo balanceado para tareas de complejidad media
- **Llama 3 8B**: Modelo gratuito para clientes externos
- **Llama 3 70B**: Modelo potente para análisis profundos

**Función Principal:**
- `generateTicketProposal()`: Genera propuesta de ticket con subtareas estructuradas
- Entradas: requerimiento del usuario, contexto (proyecto, módulo, rol)
- Salidas: JSON con subtareas, tiempos estimados, prioridades

#### 2. System Prompt Dinámico

**Prompt Base del Arquitecto:**
- Rol: Arquitecto de Software Senior y Project Manager
- Objetivo: Estructurar requerimientos técnicos en tickets y subtareas accionables

**Responsabilidades del Prompt:**
1. Analizar el requerimiento del usuario
2. Desglosar en subtareas específicas y medibles
3. Estimar tiempos realistas (en minutos)
4. Asignar prioridades apropiadas
5. Sugerir módulos o áreas del sistema

**Formato de Respuesta JSON:**
- `summary`: Breve resumen del requerimiento
- `estimatedTotalTime`: Tiempo total en minutos
- `subtasks`: Array de subtareas con título, descripción, tiempo, prioridad
- `suggestedPriority`: Prioridad sugerida (LOW, MEDIUM, HIGH, CRITICAL)
- `suggestedModule`: Nombre del módulo recomendado

**Guías de Estimación de Tiempo:**
- Configuración inicial: 15-30 minutos
- Implementación simple: 30-60 minutos
- Implementación compleja: 60-120 minutos
- Testing y validación: 15-45 minutos
- Documentación: 15-30 minutos

**Instrucciones por Rol:**

**CEO:**
- Enfócate en métricas de negocio, rentabilidad y eficiencia
- Propone iniciativas estratégicas y optimizaciones operativas
- Considera impacto financiero y ROI
- Sugiere mejoras de procesos y gestión de equipo
- Evita detalles técnicos muy específicos

**DEVELOPER:**
- Enfócate en implementación técnica y mejores prácticas
- Sugiere refactorización, optimización y patrones de diseño
- Considera arquitectura, performance y mantenibilidad
- Propone mejoras de código y testing
- Usa terminología técnica apropiada

**INTERN:**
- Enfócate en tareas de aprendizaje y contribución simple
- Sugiere tareas apropiadas para nivel junior
- Propone documentación, testing simple, bug fixes menores
- Considera mentoría y crecimiento profesional
- Evita tareas críticas o complejas

**EXTERNAL_CLIENT:**
- Enfócate en funcionalidad visible y experiencia de usuario
- Propone features y mejoras desde perspectiva del cliente
- Considera valor de negocio y necesidades del usuario final
- Evita detalles técnicos internos
- Enfócate en resultados y entregables

#### 3. Tool Calling con Vercel AI SDK

**Herramientas Disponibles:**

**createTicket:**
- Descripción: Crear un nuevo ticket con subtareas
- Parámetros: título, descripción, prioridad, tiempo estimado, proyecto, módulo, subtareas
- Ejecución: Crea ticket en Prisma con todas las subtareas

**getProjectStatus:**
- Descripción: Obtener estado actual de un proyecto
- Parámetros: ID del proyecto
- Ejecución: Retorna proyecto con tickets activos y contadores

**getDeveloperWorkload:**
- Descripción: Obtener carga de trabajo actual de un desarrollador
- Parámetros: ID del desarrollador
- Ejecución: Retorna sesiones de trabajo activas

**Validación de Parámetros:**
- Uso de Zod schemas para validación de tipos
- Campos opcionales para flexibilidad
- Tipos estrictos para prioridades y tiempos

#### 4. Chat Interface con Streaming

**Componente de Chat:**
- Tipo: Client Component para interactividad
- Hook: `useChat` de Vercel AI SDK
- API Endpoint: `/api/ai/chat`
- Contexto: projectId, moduleId, userRole

**Características de la UI:**
- Mensajes del usuario: alineados a la derecha, estilo primary
- Mensajes de IA: alineados a la izquierda, estilo muted
- Indicador de carga: animación de bouncing dots
- Tarjeta de propuesta: muestra estructura JSON visualmente

**Flujo de Interacción:**
1. Usuario escribe requerimiento en input
2. Mensaje se envía al endpoint de chat
3. IA responde con texto y tool invocations
4. Si hay tool invocation, se muestra tarjeta de propuesta
5. Usuario puede aprobar o modificar propuesta

**Estado de Carga:**
- Indicador visual con animación
- Input deshabilitado durante carga
- Botón de envío deshabilitado

###  Seguridad Anti Abuso

#### Rate Limits y Cost Control

**Sistema de Rate Limiting:**
- Backend: Redis para almacenamiento distribuido
- Estrategia: Sliding window (ventana deslizante)
- Límite por usuario: 10 requests por minuto
- Límite diario: 100 requests por día
- Analytics: Habilitados para monitoreo

**Funciones de Verificación:**
- `checkRateLimit(userId)`: Verifica límite por minuto
- `checkDailyLimit(userId)`: Verifica límite por día
- Retorna: Boolean indicando si puede proceder

**Prevención de Abuso:**
- Bloqueo temporal al exceder límites
- Logs de intentos de abuso
- Alertas para patrones sospechosos

#### Estrategia de Modelos por Costo

**Selección Dinámica de Modelos:**

**Por Rol:**
- **EXTERNAL_CLIENT**: Llama 3 8B (más económico)
- **INTERN**: Claude 3 Haiku (económico pero capaz)
- **DEVELOPER**: Varía por complejidad
- **CEO**: GPT-4o (premium para decisiones estratégicas)

**Por Complejidad (Developers):**
- **Simple**: Claude 3 Haiku
- **Medium**: GPT-4o-mini
- **Complex**: GPT-4o

**Lógica de Selección:**
- Prioriza costo para clientes externos
- Balancea rendimiento para desarrolladores
- Usa modelos premium para decisiones críticas
- Fallback a modelo base si no hay match

###  Casos de Uso Avanzados

#### Caso 1: Análisis de Proyecto Completo

**Propósito:** Análisis integral de salud del proyecto

**Datos Analizados:**
- Tickets con subtareas y sesiones de trabajo
- Gastos del proyecto
- Progreso técnico vs presupuesto

**Análisis Generado:**
1. Estado de salud general del proyecto
2. Riesgos identificados
3. Recomendaciones para mejorar eficiencia
4. Predicción de fecha de finalización
5. Análisis de rentabilidad

**Modelo Utilizado:** GPT-4o (análisis complejo)

#### Caso 2: Generación de Release Notes

**Propósito:** Generar boletines de novedades para clientes

**Datos Analizados:**
- Tickets completados en período específico
- Subtareas resueltas
- Cambios implementados

**Estilo de Generación:**
- Lenguaje simple y amigable
- Enfoque en beneficios para usuario final
- Sin detalles técnicos internos
- Formato profesional

**Modelo Utilizado:** GPT-4o-mini (costo eficiente)

#### Caso 3: Optimización de Carga de Trabajo

**Propósito:** Balancear carga de trabajo del equipo

**Datos Analizados:**
- Tickets asignados por desarrollador
- Sesiones de trabajo activas
- Carga actual vs capacidad

**Recomendaciones Generadas:**
1. Reasignaciones de tickets para balancear carga
2. Tickets que deberían ser priorizados
3. Desarrolladores con sobrecarga o subcarga
4. Recomendaciones para mejorar eficiencia del equipo

**Modelo Utilizado:** GPT-4o (análisis estratégico)

##  Relacionado
- [[IA Flow - Del Chat al Ticket|Flujo de Trabajo Detallado]]
- [[../../05 - Backend/Server Actions|Server Actions para IA]]
- [[../../02 - Base de Datos/Modelos Prisma|Modelos de Datos]]
