---
created: 02-10-2025 13:45
tags:
  - proyecto
  - descripcion
  - resumen
aliases:
  - Descripción General
---

# Descripción General del Proyecto

## Resumen
**C.R.E. Insights** es una plataforma web interna destinada a **monitorear, analizar y reportar métricas de publicaciones** de la fanpage de C.R.E., así como a **scrapear menciones negativas externas** de la marca en redes sociales (Facebook, Instagram y TikTok).  

La plataforma combina **automatización de scraping, procesamiento de datos y visualización de métricas** en un dashboard accesible para el equipo de C.R.E.

## Componentes Principales
1. **Scraping y Automatización (n8n + Apify)**  
   - Scraping diario de publicaciones de la fanpage.  
   - Scraping de menciones negativas externas en perfiles, grupos y hashtags relevantes.  
   - Gestión de flujos y nodos en n8n para automatización completa.

2. **Backend y Procesamiento**  
   - Base de datos PostgreSQL gestionada con Prisma.  
   - API en n8n para exponer endpoints y servir datos al frontend.  
   - Análisis de sentimiento y clasificación de menciones negativas.

3. **Frontend (Next.js 15)**  
   - Dashboard interactivo con métricas de engagement, gráficos y alertas.  
   - Visualización de evolución histórica de cada publicación.  
   - Gestión de roles de usuario y autenticación.

## Flujo de Datos
1. n8n ejecuta scraping programado usando Apify.  
2. Datos de publicaciones y menciones se almacenan en la base de datos.  
3. Procesamiento y análisis de sentimiento se aplican sobre los datos.  
4. El frontend consulta la API para mostrar métricas, gráficos y alertas en tiempo real.

## Alcance
- Monitoreo completo de la fanpage de C.R.E.  
- Scraping de menciones externas en redes sociales seleccionadas.  
- Visualización de métricas y alertas de publicaciones negativas.  
- Dashboard responsivo y seguro con roles de usuario.

## Conexiones Relevantes
- [[Vision|Visión del Proyecto]]  
- [[00_Resumen/Roadmap|Roadmap del Proyecto]]  
- [[02_Scraping/FlujoGeneral|Flujo General de Scraping]]  
- [[03_BaseDeDatos/EsquemaPrisma|Esquema de Base de Datos]]  
- [[05_Frontend/Dashboard|Dashboard]]  
- [[06_Procesamiento/AnalisisSentimiento|Análisis de Sentimiento]]  
