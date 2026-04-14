---
tags:
  - ScentDuo
  - Backend
  - Architecture
---
## 🏗️ Arquitectura de ScentDuo App Web

El sistema sigue un enfoque de **Monolito Híbrido** en Next.js 16 (App Router).
Todo vive en el mismo repositorio, pero está segmentado internamente mediante Route Groups para proteger la zona administrativa.

### Estructura de Directorios (Server Layouts)
- `(public)/` -> FrontStore. Renderizado estático (SSG) e Incremental (ISR) para el catálogo de productos por temas de SEO y ultrarapidez.
- `(auth)/` -> Rutas ocultas o login.
- `(admin)/` -> Dashboard privado protegido por Middleware. SSR (Server Side Rendering) obligatorio, para garantizar siempre la actualidad del inventario y no indexar.

### Core de Datos
- **Prisma** opera como puente directo hacia PostgreSQL.
- Se recomienda el uso de Server Actions para los formularios del Panel Analítico y Mantenimiento del catálogo, disminuyendo uso de JS en el cliente y evitando API paths innecesarios.
