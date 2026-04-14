---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Backend
  - Security
---
# Seguridad, Permisos y ABAC (Attribute-Based Access Control)

## 🧠 Lógica de Autorización Contextual

La seguridad en **OB Workspace** no se limita a "estás logueado o no". Se basa en **quién** eres y **qué relación** tienes con el recurso que intentas manipular.

### La Función Maestra `can()`
Ubicada en `lib/permissions.ts`, esta función evalúa la terna `(usuario, acción, recurso)`.

- **CEO:** Tiene un `bypass` total en la mayoría de los módulos (Finanzas, Admin, Proyectos).
- **DEVELOPER / INTERN:** Solo pueden editar tickets o subtareas donde figuren como `leadId` o estén en la lista de `collaborators`.
- **EXTERNAL_CLIENT:** Su acceso está filtrado por el campo `projectId` o `creatorId`. Solo ven lo que ellos mismos crearon o lo que se les ha compartido explícitamente.

### Protección en múltiples capas

1.  **Middleware de Next.js:** Protege las rutas de la aplicación `/dashboard/*` y `/portal/*` verificando la sesión JWT.
2.  **Server Actions (Defensa Final):** Cada acción en `app/actions/` vuelve a llamar a la lógica de `permissions.ts` antes de ejecutar cualquier comando de Prisma. Esto evita que un usuario manipule IDs de tickets de otros proyectos vía consola o herramientas externas.

## 📅 Matriz de Responsabilidades
| Recurso | CEO | Dev | Intern | Client |
| :--- | :---: | :---: | :---: | :---: |
| **Finanzas (Gastos)** | 👑 | ❌ | ❌ | ❌ |
| **Configuración Global** | 👑 | ❌ | ❌ | ❌ |
| **Crear Tickets** | ✅ | ✅ | ❌ | ✅ |
| **Mover Kanban** | ✅ | ✅ (Propios) | ✅ (Propios) | ❌ |

## 🔗 Relacionado
- [[../../04 - Frontend y Autenticacion/Roles RBAC|Definición de Roles]]
- [[Server Actions|Acciones Protegidas]]
