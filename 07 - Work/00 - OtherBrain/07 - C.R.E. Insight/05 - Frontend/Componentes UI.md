---
tags:
  - C.R.E.Insight
  - ui
  - react
  - shadcn
---
# Componentes UI (Design System)

## 🧠 Filosofía Front-End

Basado en Radix Primitives y Shadcn/ui. Esto permite copiar el código fuente local (`components.json` detectado en origen del repo), brindando control granular sin depender de pesadas liberías de DataTables y modales.

### Componentes Clave:
1. **DataTables (Tanstack Table)**: Componente vitalicio para desplegar el `Radar de Reputación`. Ofrece virtualización de Filas, permitiendo ver listados con miles de menciones (Mentions) sin colapsar el DOM.
2. **Badges (Píldoras semánticas)**:
   - `rojo opaco / bold text`: Riesgo Crítico (Mención Externa).
   - `verde pastel`: Neutralizado / Resuelto.
3. **Sentiment Card**: Cajas renderizadas con base matemática en Server Components, inmutables desde que la página hace `load`, reduciendo severamente los kilobyte de JS que llega al cliente.
