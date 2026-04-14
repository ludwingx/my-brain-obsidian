---
created: 02-10-2025 13:00
tags:
  - n8n
  - scraping
  - fullstack
  - appweb
  - postgreSQL
  - NextJS
  - Project
  - Work
---
# C.R.E. Insights

## Descripción del Proyecto
Plataforma para administrar analíticas de publicaciones de la fanpage de C.R.E., métricas de engagement, seguimiento de cada publicación y detección de publicaciones negativas. Además, permite monitorear menciones de C.R.E. en perfiles, grupos y publicaciones de Facebook, Instagram y TikTok para análisis de reputación y detección de posibles riesgos.

## Información del Proyecto
Creación: 02-10-2025 13:00  
Fecha límite: 15-10-2025  
En pausa: No  
Fecha estimada de finalización: 10-10-2025  
Completado: No  
Tipo: Plataforma interna  
Plataforma: Web  

## Objetivos
1. Resultado ideal
   - Sistema completo de monitoreo de publicaciones de la fanpage y de menciones externas en redes sociales con métricas de engagement y detección de publicaciones negativas.
2. Resultado aceptable
   - Visualización de métricas y alertas de publicaciones negativas de la fanpage y registro básico de menciones externas.

## Expectativas
1. Útil para el proyecto
   - Monitoreo de métricas y evolución de cada publicación de la fanpage.
   - Seguimiento de menciones externas relevantes sobre C.R.E. en Facebook, Instagram y TikTok.
2. Obstáculos
   - Cambios en las APIs de las redes sociales que afecten el scraping.
   - Posibles limitaciones legales o de privacidad al scrapear perfiles y grupos.
3. Novatez
   - Dependencia inicial de scraping manual para validar datos.
4. Conocimientos/Insights
   - Uso de n8n + Apify para automatización de scraping interno y externo.
   - Integración de análisis de sentimiento para publicaciones externas.

## Tareas
- Configuración de n8n y nodos de Apify para scraping interno y externo.
- Diseño del esquema de base de datos (Post, PostMetrics, MencionesExternas).
- Desarrollo de API en n8n para gestionar datos y consultas.
- Creación del dashboard en Next.js 15.
- Implementación de análisis de sentimiento y categorización de publicaciones.
- Configuración de roles de usuario y autenticación.
- Documentación completa en Obsidian.

## Recursos
- n8n: https://n8n.io
- Apify: https://apify.com
- Next.js 15: https://nextjs.org
- Prisma: https://www.prisma.io

## Logs del Proyecto
### Secciones de la Documentación
- 00 - Resumen
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/00 - Resumen/Vision|Visión del Proyecto]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/00 - Resumen/DescripcionGeneral|Descripción General]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/00 - Resumen/Roadmap|Roadmap]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/00 - Resumen/Glosario|Glosario de Términos]]
- 01 - Arquitectura
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/01 - Arquitectura/ArquitecturaGeneral|Arquitectura General]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/01 - Arquitectura/DiagramaFlujo|Flujo General]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/01_Arquitectura/Infraestructura|Infraestructura Técnica]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/01 - Arquitectura/Seguridad|Seguridad]]
- 02 - Scraping
  - [[Flujo General en n8n|Flujo General en n8n]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/02 - Scraping/FlujoDiarioPorPostID|Scraping Diario por PostID]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/02 - Scraping/ConfiguracionN8N|Configuración de n8n]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/02 - Scraping/ApifyActors|Apify Actors]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/02 - Scraping/EjemplosQueries|Ejemplos de Queries]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/02 -Scraping/MencionesExternas|Scraping de Menciones Externas]]
- 03 - Base de Datos
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/03 - Base de Datos/Guardar Publicaciones|Guardar Publicaciones]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/03 - Base de Datos/Tablas Principales|Tablas Principales]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/03 - Base de Datos/Relaciones Entre Entidades|Relaciones Entre Entidades]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/03 - Base de Datos/Migraciones|Migraciones DB]]
- 04 - Backend
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/04 - Backend/Endpoints de la API|Endpoints de la API]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/04 - Backend/Servicios|Servicios Node/Next]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/04 - Backend/Autenticacion|Autenticación y Bearer]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/04 - Backend/Logs Errores|Tracking y Logs]]
- 05 - Frontend
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/05 - Frontend/Dashboard|Dashboard Structure]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/05 - Frontend/Componentes UI|Componentes Core]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/05 - Frontend/Graficas y Visualizaciones|Modelos Gráficos]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/05 - Frontend/Roles de Usuario|RBAC Roles de Acceso]]
- 06 - Notas Rapidas
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/06 - Notas Rapidas/TODO|Lista de Tareas (TODO)]]
  - [[07 - Work/00 - OtherBrain/07 - C.R.E. Insight/06 - Notas Rapidas/Ideas|Lluvia de Ideas]]

---
## 🔗 Navegación
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
