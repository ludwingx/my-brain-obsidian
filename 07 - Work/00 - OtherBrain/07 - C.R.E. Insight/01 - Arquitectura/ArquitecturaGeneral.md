# Arquitectura General - C.R.E. Insights

## Visión General
C.R.E. Insights es una plataforma web para monitoreo de publicaciones de la fanpage de C.R.E. y menciones externas en redes sociales (Facebook, Instagram y TikTok).  
La arquitectura está diseñada para ser modular, escalable y segura, integrando scraping, análisis de métricas y visualización de datos.

## Componentes Principales

### 1. Frontend
- **Framework**: Next.js 15
- **Responsabilidades**:
  - Dashboard interactivo para métricas y alertas.
  - Visualización de análisis de sentimiento y evolución de publicaciones.
  - Gestión de roles de usuario y autenticación.
- **Integración**:
  - Consume APIs provistas por n8n.
  - Actualización en tiempo real de métricas mediante polling o WebSockets (opcional).

### 2. Backend / Workflow
- **Herramienta**: n8n
- **Responsabilidades**:
  - Orquestación de flujos de scraping y procesamiento.
  - Gestión de API para lectura y escritura de datos.
  - Integración de Apify para scraping automatizado de menciones externas.
  - Transformación y almacenamiento de datos en PostgreSQL a través de Prisma.

### 3. Scraping y Automatización
- **Plataforma**: Apify (integrado en n8n)
- **Responsabilidades**:
  - Extracción de publicaciones de la fanpage de C.R.E.
  - Scraping de perfiles y grupos en Facebook, Instagram y TikTok que mencionen a C.R.E.
  - Automatización diaria de flujos de scraping.
  - Enriquecimiento de datos con análisis de sentimiento y categorización de contenido.

### 4. Base de Datos
- **Motor**: PostgreSQL
- **ORM**: Prisma
- **Esquema principal**:
  - `Post`: Datos de publicaciones de la fanpage.
  - `PostMetrics`: Métricas de engagement por publicación.
  - `MencionesExternas`: Datos de menciones externas de C.R.E.
- **Objetivos**:
  - Mantener historial completo de publicaciones y métricas.
  - Facilitar consultas rápidas para el dashboard y análisis.

### 5. Seguridad y Control de Acceso
- Roles de usuario con permisos diferenciados (Admin, Analista, Viewer).
- Autenticación integrada en el frontend y controlada por n8n.
- Manejo seguro de tokens y credenciales para APIs de redes sociales.

## Diagrama Conceptual (opcional)
