---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - RBAC
  - Frontend
---
# RBAC (Role-Based Access Control) y Permisos

## 🧠 Muros Front y Back por Entidades

A diferencia de un acceso de tipo `{ isAdmin: true }`, se usa un modelo conceptual ABAC.
El Enum define contornos fuertes: `CEO`, `DEVELOPER`, `INTERN`, `EXTERNAL_CLIENT`.

### El método `can(user, action, resource)`
La interfaz no renderiza botones "Delete" condicionales solo ocultándolos mediante CSS. Se emplea una verificación del lado del server (Server Action / RSC) que corrobora si ese *UserId* tiene permisos contra el ID del Row.

#### Comportamiento del UI según Rol:
1. **CEO:** Panel Master, posee acceso global y único al submódulo `expenses` y facturación de la agencia.
2. **Client:** Ruta paralela que lo aísla físicamente (`/portal`). 
3. **Intern / Dev:** Ven el Kanban global, pero el Developer sólo mueve *Cards (Tickets)* en las que está atado como `ticket.leadId` o `ticket.collaborators`. El *Intern* se limita exclusivamente a lo que le asignaron expresamente, evitando mover tickets críticos que alteren los reportes productivos de la agencia.
