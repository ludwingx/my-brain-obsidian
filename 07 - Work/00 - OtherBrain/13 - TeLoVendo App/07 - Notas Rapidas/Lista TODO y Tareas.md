---
tags:
  - TeLoVendo
  - ideas
  - todo
---
# Muro de Planificación (TeLoVendo)

## 📌 TODOs Técnicos
*Estos ítems nacen a raíz del análisis profundo del esquema Prisma y los componentes detectados.*

- [ ] **Manejo de Errores Bot:** ¿Qué pasa si el `Device` se "crashea" a la mitad de una orden? Configurar un *Timeout Job* que marque el Device como libre automáticamente si pasa 1 hora atorado en `OCUPADO`.
- [ ] **Dashboard Effects (`dashboard-effects.tsx`):** Optimizar estos efectos de estado para evitar *re-renders* profundos que tiren los frames de la animación en dispositivos móviles de gama baja.
- [ ] **Multi-Divisas en UI:** El modelo soporta moneda `Currency` (BOLIVIANOS / DOLARES), pero hay que implementar un selector en el Multi-Step form de Marketplace para pre-calcular precios según la locación del proxy del Bot.
- [ ] **Auth Roles:** Reforzar que el middleware capture el role `ESPECTADOR` (creado en la DB por default) y lo empuje a una zona aislada / read-only limitando los server actions.

## 💡 Ideas de Negocio
- Si `ChatSession` maneja conversaciones, intentar inyectar en RAG (Retrieval) el listado activo del cliente para que un Bot pueda cerrar la venta usando OpenAI directamente sobre Messenger / WhatsApp.
