# Plan de Implementación: TeloComento

Este documento detalla el paso a paso estructurado para la construcción de la aplicación **TeloComento**, abarcando desde la infraestructura de base de datos hasta la interfaz de usuario con roles y dashboards independientes, siguiendo el modelo de Next.js (App Router), Prisma y shadcn/ui.

## 1. Configuración de Base de Datos e Infraestructura (Prisma)
- **Problema actual**: Migración a configuración de Prisma sin `url` en el schema.
- **Solución**:
  - Instalar `prisma`, `@prisma/client`, `pg` y `@prisma/adapter-pg`.
  - Configurar `prisma.config.ts` para manejar la URL de migraciones.
  - Crear `lib/prisma.ts` donde se instancie el `PrismaClient` inyectando el adaptador `PrismaPg` que conectará directamente a la base de datos de PostgreSQL.
- **Modelo de Datos (Schema)**:
  - Se deben definir los modelos principales: `User` (con rol `ADMIN` | `USER`), `Session`, `Account` (para NextAuth).
  - Modelos de negocio: `ScrapingCard` (tarjeta de monitoreo), `Publication` (publicación extraída), `Order` (orden de comentarios), `Comment` (comentario generado por IA).

## 2. Layouts y Autenticación (Auth & Roles)
- **Autenticación (NextAuth v5)**:
  - Implementar login mediante credenciales o proveedores.
  - Extender la sesión para incluir el rol del usuario (Role-Based Access Control).
- **Estructura de Layouts**:
  - `(auth)`: Un *route group* independiente para las páginas de inicio de sesión (`/login`, `/register`). Layout sin menús laterales, centrado y atractivo.
  - `(dashboard)`: Un *route group* protegido, que validará la sesión y el rol. Contendrá un Sidebar moderno (usando componentes como NavigationMenu o un Sidebar de shadcn/ui), Header con perfil, y contenido principal responsivo.

## 3. Módulos del Sistema (Dashboard)
Desarrollo por módulos para garantizar mantenibilidad y completitud:
- **Módulo Dashboard Principal**: Resumen de actividad, métricas de publicaciones scrapeadas por sentimiento, y avance de órdenes.
- **Módulo Tarjetas (Monitoreo)**: 
  - CRUD (Crear, Leer, Actualizar, Eliminar) para tarjetas.
  - Campos: Palabra clave, causa/persona, fuentes y filtros de visualización.
- **Módulo Publicaciones (Ingesta y Revisión)**:
  - Vista para revisar publicaciones obtenidas por el scraper.
  - Acciones: Aprobar/Rechazar, cambiar sentimiento (Positivo, Neutro, Negativo), y acción para "Crear Orden".
- **Módulo Órdenes y Comentarios**:
  - Gestión de órdenes con sus instrucciones.
  - Panel para visualizar comentarios generados por IA, con opciones de edición antes de "Activar" para los bots.
- **Módulo Usuarios/Roles (Solo Admin)**:
  - Gestión de acceso y visualización de todos los usuarios registrados.

## 4. API e Integración Externa
- **Scraper Ingest API**: Endpoints protegidos por API Key para que el scraper externo registre las publicaciones.
- **Bots API**: Cola de trabajo expuesta vía API para que los bots externos consuman las órdenes activadas, publiquen en Facebook y reporten el estado (En progreso, Finalizado, Error).

## 5. UI/UX y Responsividad
- Componentes exclusivos de **shadcn/ui**.
- Diseño modular "True Black & White" (o adaptado a esquemas oscuros modernos y minimalistas).
- Formularios manejados con `react-hook-form` y validación con `zod`.
- Interfaz 100% responsiva (Mobile First) con animaciones sutiles.
