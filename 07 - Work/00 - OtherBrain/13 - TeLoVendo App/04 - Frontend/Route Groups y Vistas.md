---
tags:
  - TeLoVendo
  - Frontend
  - RouteGroups
---
# Layouts y Route Groups (App Router)

## 🧠 Estructura de Navegación Segmentada

El proyecto utiliza al máximo las convenciones de Next.js (`(folder)`) para abstraer lógicas de visualización y control de acceso en tres mundos completamente aislados pero que co-existen bajo el mismo dominio.

### 1. `(auth)` - Zona de Ingreso
- Rutas: `/login`, `/register`.
- Renderiza componentes dedicados como `login-form.tsx` y `signup-form.tsx`.
- Estas vistas empujan al usuario directo al `(dashboard)` si ya poseen un token de JWT activo en sus cookies para evitar parpadeos de login.

### 2. `(public)` - Facing Público
- Páginas que se sirven con **SSG (Static Site Generation)** para posicionamiento SEO.
- Renderizado de Landing Pages o directorios de servicios sin requerir lectura a la base de datos de usuarios recurrentemente.
- Se apoya en el componente `public-user-menu.tsx`.

### 3. `(dashboard)` - Panel Transaccional
- Zona resguardada donde opera el núcleo de `TeLoVendo`.
- Contiene un Sidebar interactivo construido con `app-sidebar.tsx`, `nav-main.tsx`, y `nav-projects.tsx`.
- Aquí funciona el **Project Enforcer**, verificando a cada instante a qué proyecto pertenece el Data Mutation en curso.
