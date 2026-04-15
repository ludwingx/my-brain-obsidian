# Tech Stack - OB Universe

El repositorio fuente (`office-management`) que impulsa **OB Universe** está construido sobre las siguientes tecnologías que respaldan un alto rendimiento en interfaces y experiencia de usuario.

## Frontend
- **Framework**: Next.js 16.2 (App Router). Configurado bajo los nuevos estándares de rutas app.
- **Librería UI**: React 19.
- **Estilos**: Tailwind CSS v4 acoplado de forma nativa.
- **Componentes Base**: `shadcn/ui` y `@base-ui/react`.
- **Animaciones e Interacciones**: 
  - `framer-motion` (v12) para animaciones complejas (como el fondo Vortex).
  - `tailwindcss-animate` para microinteracciones fluidas.
- **Iconografía**: `lucide-react`.

## Estructura de Código y Diseño
- Arquitectura fuertemente basada en el diseño de interfaz: 
  - Incorporación de un Sidebar robusto (`app-sidebar.tsx`) que usa el contexto de equipos y proyectos.
  - Componentes estéticos retro: `retro-window.tsx`, animaciones tipo Neural Pattern y Terminal scripts visuales.
- **Enfoque de Diseño**: Utiliza fuertemente el contraste de variables `foreground` y `background` para soportar de manera nativa temas oscuros/claros minimalistas o interfaces invertidas.

El proyecto está diseñado fundamentalmente para ser una Single Page Application (SPA) / Portal Next.js sin recargas con transiciones inmediatas.

---
🔙 **Principal:** [[OB Universe]]
