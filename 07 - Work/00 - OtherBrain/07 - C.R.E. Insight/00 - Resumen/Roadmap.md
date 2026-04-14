# Roadmap de C.R.E. Insights

## Fase 1: Preparación e Infraestructura
**Fecha estimada:** 02-10-2025 → 04-10-2025
- Configuración de n8n y nodos de Apify para scraping interno y externo.
- Diseño inicial del esquema de base de datos (Post, PostMetrics, MencionesExternas) con Prisma.
- Configuración del repositorio de Next.js 15 para el frontend.
- Creación de estructura de carpetas y documentación en Obsidian.

## Fase 2: Backend y Scraping
**Fecha estimada:** 05-10-2025 → 07-10-2025
- Desarrollo de API en n8n para gestionar datos de posts y métricas.
- Integración de Apify para scraping automático de menciones externas en Facebook, Instagram y TikTok.
- Validación de datos obtenidos y manejo de errores.
- Implementación de pipeline básico de análisis de sentimiento.

## Fase 3: Frontend y Dashboard
**Fecha estimada:** 08-10-2025 → 09-10-2025
- Desarrollo del dashboard en Next.js 15.
- Creación de componentes UI reutilizables (gráficas, tablas, alertas).
- Visualización de métricas de engagement y publicaciones negativas.
- Implementación de filtros por fecha, red social y tipo de publicación.

## Fase 4: Roles, Autenticación y Alertas
**Fecha estimada:** 09-10-2025 → 10-10-2025
- Configuración de roles de usuario y autenticación.
- Sistema de alertas para publicaciones negativas y menciones externas críticas.
- Pruebas de seguridad y permisos de acceso.
- Documentación final de uso y manual de procedimientos.

## Fase 5: Lanzamiento y Revisión
**Fecha estimada:** 10-10-2025
- Revisión completa del sistema.
- Pruebas de stress en scraping y dashboard.
- Ajustes finales según feedback.
- Entrega y despliegue de la plataforma interna.
