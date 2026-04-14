---
tags:
  - C.R.E.Insight
  - frontend
  - roles
---
# Gestión de Roles de Usuario

## 🧠 Permisos Basados en Roles (RBAC)

De acuerdo al esquema original analizado (`schema.prisma`), el sistema cuenta con el modelo `User` que define un `rol` predeterminado:
```prisma
enum Rol {
  admin
  user
  analyst
}
```

### Tabla de Privilegios UI

| Vista / Acción | `admin` | `analyst` | `user` |
| --- | --- | --- | --- |
| **Dashboard** (Métricas Globales) | ✅ | ✅ | ✅ |
| **Ver Menciones Críticas** | ✅ | ✅ | ✅ |
| **Resolver / Gestionar Menciones** | ✅ | ✅ | ❌ |
| **Gestión de Keywords y Crawling** | ✅ | ❌ | ❌ |
| **Añadir / Eliminar Usuarios** | ✅ | ❌ | ❌ |

### Implementación Frontend
En Next.js App Router, ciertas rutas están protegidas encapsulándolas en componentes asíncronos que verifican el `Role` desde la sesión actual (`getServerSession` o JWT decrypt), ocultando condicionalmente accesos en el Sidebar.
