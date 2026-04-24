---
tags:
  - C.R.E.Insight
  - scraping
  - keywords
---
# Ejemplos de Queries en Tiempo Real

## 🧠 Integración desde el Código
El sistema nutre a Apify no con un hardcoding, sino extrayendo las palabras registradas en vivo en el modelo `Keyword` originadas en el proyecto Next.js.

### Formato de Transmisión al Webhook de Scrapeo
```json
{
  "startUrls": [
    { "url": "https://www.facebook.com/search/posts?q=CRE%20corte" },
    { "url": "https://www.facebook.com/search/posts?q=CRE%20factura" }
  ],
  "resultsLimit": 15
}
```

### Palabras (Keywords) Críticas del Negocio
- *corte de luz CRE*
- *medidor tarifario CRE*
- *robo CRE*
- *mala atención CRE*

Todas estas palabras generan combinaciones (Search URLs) que nutren continuamente el radar.
