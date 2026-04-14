---
tags:
  - TeLoVendo
  - NextJS
  - Arquitectura
---
# Arquitectura General y Stack

## 🧠 Ecosistema Moderno
Construido utilizando el borde de la tecnología (Next.js 16.2.1) enfocándose rígidamente en *App Router* y server actions.

### Dependencias Críticas
- **Framework Core**: `Next.js 16.2.1` operando con el compilador nativo local (SWC).
- **Capa Base de Datos**: `@prisma/client 7.6.0` con adaptador `pg` interactuando contra Supabase / PostgreSQL.
- **Frontend / Estado**:
  - `zustand 5.0`: Gestión de estados de formularios complejos.
  - `tailwind-merge` + `shadcn` + `class-variance-authority`: Abstracción de UI sin sacrificar estilos atómicos.
  - `framer-motion`: Para micro-animaciones (Feedback de botones de "Generando orden").
- **Auth**: Gestión mediante `bcryptjs` y `jose` (JWT).

## Estructura de Directorios (SRC)
- `/app`: Rutas del App Router. Protegidas por Middleware JWT.
- `/hooks`: Hooks perzonalizados de React ligados a Zustand y SWR.
- `/lib`: Funciones de encriptación (`proxy.ts`, generación de strings) y adaptadores Prisma.
- `/components`: Colección vasta de primitivas UI (Inputs, DataTables, Badges de Status).
