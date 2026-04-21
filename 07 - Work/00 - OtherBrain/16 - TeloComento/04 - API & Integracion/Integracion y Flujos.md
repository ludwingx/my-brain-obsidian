# Documentación de API e Integración

## 1. API de Ingesta (Scraper Externo -> Web App)
**Endpoint**: `POST /api/ingest`
**Auth**: `X-API-KEY` en el header.

### Request Body
```json
{
  "searchFormatId": "string",
  "externalId": "string",
  "content": "string",
  "url": "string",
  "publishDate": "ISO8601"
}
```

## 2. API de Órdenes (Bots -> Web App)
**Endpoint**: `GET /api/orders/pending`
**Filtros**: `?type=SUPPORT|CRITICIZE`

### Response
```json
[
  {
    "orderId": "string",
    "publicationUrl": "string",
    "comments": [
      {
        "id": "string",
        "content": "string"
      }
    ]
  }
]
```

**Endpoint**: `PATCH /api/comments/:id`
**Body**: `{"status": "PUBLISHED", "botId": "bot_01"}`

## 3. Flujo de Integración IA (OpenRouter)
1. **Detección de Sentimiento**: Al recibir la publicación, se envía el `content` a OpenRouter con un prompt de clasificación (`positiva`, `neutra`, `negativa`).
2. **Generación de Comentarios**: Al activar la orden, se envía el `content` de la publicación + `notes` del usuario + `goal` (apoyar/criticar) para generar N respuestas únicas.
3. **Generación de Variantes de Búsqueda**: El backend utilizará la IA para generar variaciones de la "Palabra Clave + Causa" (ej. "Luis Fernando Camacho en El Deber", "Noticias Camacho", "Post sobre Camacho") para que el scraper las utilice como semillas de búsqueda.
