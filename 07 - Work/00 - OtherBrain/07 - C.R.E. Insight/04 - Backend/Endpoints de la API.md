---
tags:
  - C.R.E.Insight
  - backend
  - endpoints
  - n8n
---
# Endpoints y API REST

## 🧠 Comunicación y Rutas

Dado que Next.js 15 actuará como Front, necesitamos exponer endpoints mediante sus `Route Handlers` (`app/api/...`) para que el cliente web consuma datos y **webhook receivers** para que n8n inyecte el scrapping.

### 📥 1. Endpoints de Ingesta (Consumidos por n8n)
Estos endpoints son protegidos por una variable de entorno `N8N_SECRET_TOKEN` y sólo pueden emitir peticiones POST.

- **`POST /api/scrape`**
  - **Función:** Recibir el hook o iniciar un scrape mandando a llamar a la función externa correspondiente. Utilizado posiblemente por n8n para sincronización.
  
- **`POST /api/posts`**
  - **Función:** Realiza la ingesta masiva y UPSERT de las publicaciones de la fanpage recopiladas hacia la tabla `Post`.

- **`POST /api/mentions`**
  - **Función:** Ingestar menciones externas con comentarios asociados.

### 📤 2. Endpoints Internos (Dashboard Web)

- **`GET /api/posts`** 
  - Usado por Server Components o `fetch` interno para mostrar la grilla de posts de C.R.E.
- **`GET /api/mentions`**
  - Extrae las menciones que levanten alertas o requieran acción operativa.
- **`GET/POST /api/keywords`**
  - CRUD para administrar la tabla `Keyword` en vivo para que el Scraper busque de forma dinámica.

## 🔗 Relacionado
- [[Autenticacion|Reglas de Seguridad API]]
- [[../05 - Frontend/Dashboard|Cómo se consume esto en el Front]]
