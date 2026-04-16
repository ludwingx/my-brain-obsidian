---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Resumen
  - SaaS
---
# Brief Técnico y Core de Módulos

##  Solución Propuesta (OB-Workspace)

Un sistema monolítico basado en Next.js App Router para centralizar la gestión de la agencia de desarrollo.

### Módulos Vitales:
1. **Core Relacional:** Cascada de `Proyectos` > `Módulos` > `Tickets` > `Subtareas`.
2. **Time Tracking (Máquina de Estado):** Contabilidad al segundo de `WorkSessions`. El usuario dispara "Start" y "Stop", permitiendo trazar cuánto toma exactamente una tarea.
3. **Roles Granulares (RBAC):** Ecosistema diseñado para permitir entrada de colaboradores (`Intern`), Desarrolladores (`Dev`), Clientes observadores (`Client`) y Control Absoluto (`CEO`).
4. **Módulo de Finanzas y Hub Cognitivo de IA:** Soporte completo en la plataforma.

## Stack Tecnológico

### Frontend
- **Framework:** Next.js 16 con App Router
- **Estilos:** Tailwind CSS v4 + shadcn/ui
- **Componentes:** Radix UI (accesibilidad nativa)
- **Tipografía:** Inter (Google Fonts variable)
- **Temas:** Sistema híbrido cookies + localStorage para evitar FOUC

### Backend
- **Runtime:** Node.js con Edge Runtime para acciones críticas
- **ORM:** Prisma con PostgreSQL
- **Autenticación:** NextAuth.js con JWT
- **Server Actions:** Mutaciones atómicas con validación Zod
- **Caching:** React Cache + revalidatePath

### Inteligencia Artificial
- **Provider:** OpenRouter (multi-model)
- **SDK:** Vercel AI SDK
- **Modelos:** GPT-4o-mini (estructurado), Llama 3 8b (conversacional)
- **Tool Calling:** Integración nativa con Prisma

## Casos de Uso Principales

### Para el CEO
- **Visión 360°:** Dashboard con métricas financieras y técnicas
- **Control de Gastos:** Asignación de presupuestos por proyecto
- **Rentabilidad:** Comparación realTime vs estimatedTime
- **Gestión de Equipo:** Asignación de desarrolladores y carga de trabajo

### Para Desarrolladores
- **Kanban Operativo:** Vista de tickets asignados y pendientes
- **Time Tracking:** Timer flotante para registrar trabajo en tiempo real
- **IA Assistant:** Ayuda para estructurar subtareas y estimar tiempos
- **Colaboración:** Asignación de tickets y notificaciones

### Para Clientes Externos
- **Portal Aislado:** Vista exclusiva de sus proyectos
- **Progreso:** Seguimiento visual del desarrollo
- **Comunicación:** Chat con el equipo de desarrollo
- **Release Notes:** Boletines automáticos de novedades

## Métricas de Negocio

### KPIs Clave
- **Velocidad del Equipo:** Tickets completados por semana
- **Precisión de Estimaciones:** (estimatedTime - realTime) / estimatedTime
- **Rentabilidad por Proyecto:** (Ingresos - Gastos) / Ingresos
- **Productividad por Desarrollador:** Horas productivas / Horas totales

### Alertas Automáticas
- **Desviación de Presupuesto:** Cuando realTime > estimatedTime * 1.2
- **Tickets Estancados:** Más de 7 días sin movimiento
- **Carga Excesiva:** Más de 40 horas/semana por desarrollador
- **Gastos Anómalos:** Variación > 20% vs mes anterior

## Ventajas Competitivas

1. **IA Integrada:** No es un chatbot separado, es parte del flujo de trabajo
2. **Time Tracking Real:** No es estimación, es medición exacta
3. **Seguridad ABAC:** Permisos contextuales, no solo por rol
4. **Monolito Optimizado:** Server Components + Edge Runtime para máximo rendimiento
5. **Portal Cliente:** Experiencia aislada y profesional para stakeholders externos

## Roadmap de Implementación

### Fase 1: Cimientos (Completado)
- [x] Estructura base Next.js 16 + Tailwind v4
- [x] Modelado de base de datos en Prisma
- [x] Sistema de autenticación base
- [x] Gestión de tema con cookies

### Fase 2: Ejecución Core (En Progreso)
- [ ] Implementación de Kanban por módulos
- [ ] Motor de Time Tracking completo
- [ ] Dashboard de métricas para el CEO

### Fase 3: Inteligencia Artificial
- [ ] Integración con OpenRouter para "Ticket Architect"
- [ ] Sistema de notificaciones en tiempo real
- [ ] Portal para clientes externos
- [ ] Generador automático de Release Notes

## Relacionado
- [[Diario de Desarrollo|Ver progreso técnico actual]]
- [[Modulos de Negocio|Detalle de módulos financieros y analíticos]]
- [[../01 - Arquitectura/Arquitectura General|Estructura técnica del sistema]]
