---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - AI
  - Workflow
---
# Flujo IA: Del Chat a la Acción (Ticket Architect)

## 🧠 El Corazón Inteligente de OB-Workspace

A diferencia de un chat convencional, la IA en este workspace actúa como un **Arquitecto de Software**. Su objetivo no es solo responder dudas, sino estructurar el trabajo técnico.

### 🔄 Flujo de Generación Estructurada

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

### 🛡️ Contexto de Rol (ABAC Injected)
Para que la IA no proponga cosas fuera de lugar, el sistema inyecta el rol del usuario (`CEO`, `DEV`, `CLIENT`) en cada mensaje de forma programática. Esto garantiza que un Cliente Externo no reciba propuestas de refactorización de base de datos, sino explicaciones de negocio.

## 🔗 Relacionado
- [[Orquestacion AI|Ver Configuración Técnica de IA]]
- [[../../05 - Backend/Server Actions|Acciones de Servidor Relacionadas]]
