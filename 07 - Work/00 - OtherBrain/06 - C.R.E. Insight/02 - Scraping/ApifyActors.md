---
tags:
  - C.R.E.Insight
  - apify
  - scraping
  - facebook
  - tiktok
---
# Configuración y Selección de Apify Actors

## 🧠 Selección de Actores
Para no reinventar la rueda lidianco con las estrictas protecciones anti-scraping de Meta y TikTok, se delega esta tarea a Apify.

### Actores Core a Utilizar:
1. **Facebook Pages Scraper (`apify/facebook-pages-scraper`)**
   - **Uso:** Scrapear la página oficial de C.R.E para extraer los posts originales puros.
   - **Input:** URL de la fanpage. Limitamos los resultados a `maxPosts: 10` u `opción recent` para reducir consumos.

2. **Facebook Search Scraper o Groups Scraper**
   - **Uso:** Obtener menciones externas (cuando alguien nombra a C.R.E en un grupo ciudadano).
   - **Input:** Keywords: "CRE Bolivia", "CRE Santa Cruz", "corte de luz CRE".

3. **TikTok Scraper (`apify/tiktok-scraper`)**
   - **Uso:** Buscar videos virales o quejas mencionando a CRE.
   - **Limitante:** Buscar por hashtag (`#crebolivia`) o buscar en los trends locales.

## ⚙️ Mapeo en n8n
- Enlazamos n8n con el nodo predeterminado de **Apify**.
- Se lanza el task con `Set Task` pasándole el JSON del Payload.
- Se debe manejar el estado asíncrono configurándolo en `wait for finish`. Si tarda más de 5 minutos, evitar el bloqueo de n8n usando Webhooks de respuesta desde Apify hacia un nodo `Webhook` del lado de n8n.

## 🔗 Relacionado
- [[Flujo General en n8n|Volver al Flujo Principal]]
- [[MencionesExternas|Estrategia de Menciones]]
