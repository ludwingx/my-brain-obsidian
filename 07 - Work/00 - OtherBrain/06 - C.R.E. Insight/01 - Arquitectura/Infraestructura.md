---
tags:
  - C.R.E.Insight
  - arquitectura
  - infraestructura
  - devops
---
# Infraestructura Técnica

## 🧠 Descripción de los Entornos y Despliegue
C.R.E. Insight debe manejar altos volúmenes de datos en texto plano y json. La infraestructura debe garantizar alta disponibilidad y separación de la carga pesada (scraping) y la carga cliente (frontend).

## 🚀 Despliegue de Componentes

### 1. Panel Web y API (Next.js 15)
- **Hosting Sugerido:** Vercel o VPS Propio con PM2.
- **Ventaja:** SSR/SSG nativo. Permite proteger los dashboards internos sin exponer el origen real.

### 2. Base de Datos (PostgreSQL)
- **Estrategia:** Supabase o un clúster RDS.
- **Razón:** Postgres maneja muy bien columnas `JSONB` que son ideales para guardar el raw payload del scraping sin perder estructura original en caso de mutaciones de las APIs de las redes sociales.

### 3. Motor de Automatización (n8n)
- **Despliegue:** Contenedor Docker en un servidor propio (VPS Dedicado). Se evita n8n Cloud para reducir los costos debido a la gran cantidad de ejecuciones (*Triggers*) que requiere el monitoreo constante de menciones en múltiples redes.

### 4. Proveedor de Scraping (Apify)
- **Por qué Apify:** Evita los baneos de IP de Meta y TikTok mediante rotación de Proxies residenciales y *Actors* mantenidos por expertos. 
- **Conexión:** n8n consume los Webhooks de Apify de forma asíncrona.

## 🔗 Relacionado
- [[ArquitecturaGeneral|Arquitectura General]]
- [[Seguridad|Políticas de Seguridad]]
