---
tags:
  - TeLoVendo
  - React
  - UI
---
# Componentes Críticos del Ecosistema

## 🧠 Abstracción de UI avanzada

El directorio `components/` refleja un sistema maduro, en el que no solo hay botones y textos, sino lógica de negocio embebida.

### 🛡️ `project-enforcer.tsx`
- **Función:** Se asegura que el usuario no pueda mezclar visualmente Órdenes de dos proyectos diferentes. Next JS mantiene contextos globales, y si un usuario switchea de Proyecto A a Proyecto B, este componente resetea el caché o Zustand y empuja al router hacia el nuevo ID base.

### 🎭 `bot-animation.tsx`
- **Uso:** Implementación de `framer-motion`. Cuando una orden pasa a `GENERANDO`, en lugar de un spinner estático, se lanza esta micro-animación en pantalla, dando feedback visual de que hay hardware operando del otro lado. Reduce la ansiedad del usuario frente a operaciones lentas (un click humano en web demora milisegundos, un bot automatizando en un teléfono puede tomar 30 segundos por post).

### 📍 `dynamic-breadcrumbs.tsx`
- **Routing:** Mapea el URL del navegador (`/dashboard/projects/[id]/orders`) interceptando cada hook de slug y traduciéndolo dinámicamente a Nombres Legibles ("Proyectos" -> "Mi Proyecto Auto" -> "Ordenes Activas").
