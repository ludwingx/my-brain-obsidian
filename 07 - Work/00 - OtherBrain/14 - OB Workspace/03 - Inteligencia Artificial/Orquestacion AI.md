---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - AI
  - OpenRouter
---
# Integración de Ecosistemas Inteligentes

## 🧠 Vercel AI SDK y OpenRouter

En vez de limitar la plataforma a un CRUD ciego, los agentes de IA se vuelven "colegas" del repositorio para incrementar la eficiencia del Management.

### Los Patrones de Uso
1. **El Asistente Arquitecto (Ticket Architect):** 
   - El equipo presiona "Ayuda con este Requerimiento". El string crudo viaja al Backend, donde la API de **OpenRouter** (`@ai-sdk/openai` adaptado) procesa e invoca el modo estructurado (`zod schema`). Retorna un objeto desglosado JSON llenando instantáneamente Múltiples Subtareas con Tiempos Predictivos (Minutos) y validando qué herramientas aplicar de las librerías `ai`.
2. **Asistente Conversacional (Tool Calling):**
   - Un Chat lateral permite a los PMs decirle: "Levantame un ticket para cambiar el color del botón rojo". El modelo parsea la intención de lenguaje natural e invoca la Herramienta expuesta (`create_ticket`), escribiendo nativamente en Prisma a través de `AiConversation`.

### Seguridad Anti Abuso
Se forzaría a Rate Limits por IP, usando LLMs económicos (Llama 3 8b / Claude 3 Haiku) configurados en OpenRouter para consultas menores, y delegando a GPT-4o sólo para estructuras críticas.
