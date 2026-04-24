---
tags:
  - C.R.E.Insight
  - n8n
  - scraping
  - cron
---
# Scraping Diario por PostID

## 🧠 Lógica del Proceso Mensual y Diario

Dado que monitorear cada uno de los posts históricos de la Fanpage de C.R.E consumiría demasiados recursos (CU de Apify/n8n), se establece una ventana de scraping dinámico enfocado en los IDs recientes.

### 1. Extracción de Nuevos Posts
- **Frecuencia:** Cada 12 horas.
- **Actor Apify:** `Facebook Pages Scraper`.
- **Acción:** Escanear la cuenta principal y extraer los `PostID` de las publicaciones de los últimos 7 días. Si el `PostID` no existe en nuestra Base de Datos, se inserta con estado "Monitoreando".

### 2. Scraping Recursivo de Engagement
- **Frecuencia:** Diaria.
- **Flujo en n8n:**
  1. Nodo `Postgres`: `SELECT id, platform_post_id FROM "Post" WHERE createdAt >= NOW() - INTERVAL '30 days'`.
  2. Bucle (`Item Lists` / `Loop`): Para cada `platform_post_id`, lanza una llamada a la API u otro Actor ligero para capturar estrictamente el número de Likes, Comentarios, y Shares ACTUALIZADOS.
  3. Inserta un nuevo registro en la tabla `PostMetrics` apuntando al `PostID` con la nueva métrica. Esto permite graficar una curva de evolución del post.

## 🔗 Relacionado
- [[Flujo General en n8n|Volver al Flujo Principal]]
- [[../03 - Base De Datos/Guardar Publicaciones|Modelo de Base de Datos para Posts]]
