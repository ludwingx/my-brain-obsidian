---
title: Diseño de UI e Implementación
nav_order: 3
parent: TeloComento
---

# Diseño de la Interfaz de Usuario (UI/UX)

[⬅ Modelo de Datos](../02%20-%20Arquitectura/Modelo%20de%20Datos.md) | [Siguiente: Integración API ➡](../04%20-%20API%20&%20Integracion/Integracion y Flujos.md)

## 1. Mapa de Sitio / Navegación
- **Dashboard**: Estado general de los 3 formatos de búsqueda y resumen de bots.
- **Búsquedas (Setup)**: Configuración obligatoria inicial de los 3 formatos (Palabra Clave + Causa).
- **Aprobación (Tinder View)**: Interfaz de decisión rápida para nuevas publicaciones scrapeadas.
- **Publicaciones (Feed)**:
    - **Aprobadas**: Listado para generar órdenes.
    - **Rechazadas**: Historial con opción de recuperación.
- **Órdenes**: Seguimiento de ejecución de bots.
- **Configuración**: Perfil y API Keys.

## 2. Componentes Clave (shadcn/ui)
- **Tinder Card**: Componente con el texto de la publicación, sentimiento detectado por IA y botones gigantes (✅ Aprobar / ❌ Rechazar). Soporte para gestos de Swipe.
- **Create Order Modal**: Diálogo que se abre desde una publicación aprobada. Incluye:
    - Selector de objetivo: `Apoyo (+)` o `Crítica (-)`.
    - Input de texto para notas/instrucciones IA.
    - Indicador de bots que se usarán (ej. "Se generarán 40 comentarios").
- **Generated Comments View**: Lista de comentarios creados por la IA para una orden, con estados individuales.
- **Search Format Card**: Muestra la combinación (Palabra Clave + Causa) y el estado del scraper.

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
