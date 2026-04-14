---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Arquitectura
  - NextJS
---
# Estructura del Monolito Next.js

## 🧠 Arquitectura Basada en Contextos (Colocation)

Para lograr mantenibilidad en un SaaS tan extenso, la jerarquía en `src/app` (o `/app`) dicta cómo se protegen las rutas HTTP mediante middlewares.

### Route Groups Declarados
- `(auth)`: Login y control de sesiones crudo.
- `(dashboard)`: Panel protegido con Server Components (RSC) nativos. Las acciones fluyen a través de `actions/*.ts`.
  - Rutas como `/projects/`, `/tickets/`, `/tracking/`, y sub-módulos restringidos al `CEO` como `/expenses/` y `/admin/`.
- `(portal)`: *Rutas aisladas por aire*. Diseño minimalista donde el Enum de `External Client` de Prisma solo visibilice los tickets vinculados a su ID exacto, evitando filtración corporativa cruzada.

### Rendimiento (DB vs SSR)
Para evitar que paneles como el KanbanBoard colapsen la app y BD por múltiples refetches simultáneos de desarrolladores revisando tickets, todo componente base carga mediante Server Components (Zero-JS payload en lectura) y las mutaciones en `Kanban` operan a través de *Server Actions* atómicas (como `updateTicketStatus`).
