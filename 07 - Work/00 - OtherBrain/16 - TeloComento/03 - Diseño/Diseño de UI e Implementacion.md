# Diseño de la Interfaz de Usuario (UI/UX)

## 1. Mapa de Sitio / Navegación
- **Dashboard**: Resumen de campañas activas, últimas publicaciones y estado de bots.
- **Campañas**:
    - Listado de campañas (Cards).
    - Creación/Edición de campaña.
- **Publicaciones (Feed)**:
    - Filtros por campaña, sentimiento y estado de revisión.
    - Acción: Aprobar/Rechazar.
    - Acción: Crear Orden.
- **Órdenes**:
    - Listado de órdenes activas y su progreso.
    - Detalle de orden y edición de comentarios generados.
- **Configuración**: Perfil de usuario y API Keys (OpenRouter).

## 2. Componentes Clave (shadcn/ui)
- **Campaña Card**: Muestra nombre, palabra clave, causa y un badge de estado (Activo/Inactivo).
- **Publicación Row**: Muestra el texto de la publicación, fuente, sentimiento (Badge de color: Rojo/Gris/Verde) y botones de acción.
- **Progress Bar**: Para visualizar el avance de los bots en una orden.
- **Sonner Notifications**: Alertas para cambios de estado exitosos o errores de generación.

## 3. Guía de Colores (Sentimientos)
- **Positivo**: `text-green-600`, `bg-green-100`
- **Neutro**: `text-slate-600`, `bg-slate-100`
- **Negativo**: `text-red-600`, `bg-red-100`

---

# Guía de Implementación Técnica (Para Desarrolladores)

## Stack Tecnológico
- **Frontend**: Next.js 14+ (App Router).
- **Estilos**: Tailwind CSS + shadcn/ui.
- **Base de Datos**: PostgreSQL (Supabase o Local).
- **ORM**: Prisma.
- **IA**: OpenRouter SDK (Modelo: `gpt-3.5-turbo` o similar económico).
- **Estado**: TanStack Query (para fetching reactivo).

## Pasos Iniciales
1. Clonar estructura y configurar `.env` con `DATABASE_URL` y `OPENROUTER_API_KEY`.
2. Ejecutar `npx prisma db push` para sincronizar el esquema.
3. Implementar el Layout principal con la navegación lateral.
4. Crear el endpoint de API para recibir datos del Scraper externo (`POST /api/ingest`).
5. Implementar la lógica de generación de comentarios en un Server Action o Route Handler.
