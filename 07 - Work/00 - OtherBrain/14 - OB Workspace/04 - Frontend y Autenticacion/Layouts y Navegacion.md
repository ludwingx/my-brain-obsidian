---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Frontend
  - Navigation
---
# Layouts, Navegación y UX

## 🧠 Experiencia de Usuario Centralizada

El diseño de **OB Workspace** se basa en un layout "App-Shell" que maximiza el espacio de trabajo para el desarrollo y la gestión.

### Estructura de Navegación

#### 1. Sidebar Inteligente
- **Contextual:** El sidebar cambia o destaca elementos según el rol.
- **Módulos Activos:** Acceso directo a `Kanban`, `Proyectos`, `Time Tracking` y el `Chat AI`.
- **Selector de Proyecto:** Permite cambiar el contexto global de visualización.

#### 2. Componentes de UI (Radix + Shadcn)
- **Kanban Board:** Implementado con arrastrar y soltar (Dnd-kit o similar), interactuando directamente con las *Server Actions*.
- **Timer Flotante:** Un componente global (Client Component) que persiste mientras el usuario navega, mostrando el tiempo de la `WorkSession` activa.
- **Comandos Rápidos (CMDK):** Una barra de búsqueda tipo Raycast (`Cmd+K`) para saltar rápidamente entre tickets o proyectos.

### Visualización de Datos
- Uso de componentes de **Tremor** o **Recharts** en el dashboard del CEO para visualizar la quema de presupuesto (`Expenses`) versus el progreso técnico (`Tickets Done`).

## 🔗 Relacionado
- [[../04 - Frontend y Autenticacion/Roles RBAC|Control de Acceso (RBAC)]]
- [[../03 - Inteligencia Artificial/Orquestacion AI|Asistente IA Integrado]]
