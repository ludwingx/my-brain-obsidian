---
tags:
  - C.R.E.Insight
  - scraping
  - queries
  - analisis
---
# Menciones Externas y Queries

## 🧠 Lógica de Menciones Externas
El dolor principal de una empresa de servicios básicos no está solo en lo que publican ellos, sino en lo que la gente publica *de ellos*.

### Ejemplos de Queries de Búsqueda Clave (Keywords)
Nuestras integraciones deben inyectar este array dinámico dentro de las barras de búsqueda automatizadas (Scrapers):
```json
{
  "queries": [
    "C.R.E. luz",
    "factura CRE",
    "corte de luz CRE",
    "CRE Santa Cruz cobro",
    "medidor CRE"
  ]
}
```

### Proceso de Scraping Social Listening
1. Se utiliza la red de `Apify` o Google Dorks para escanear resultados públicos recientes en Facebook y TikTok usando los keywords.
2. Cada publicación se procesa, guardando: URL Original, Autor (Anonimizado si es necesario), Fecha y el Texto Raw.
3. El analizador de Sentimiento entra en acción. Si es un reclamo legítimo, se arroja el Flag de `Riesgo Medio` o `Riesgo Alto`.

## 🔗 Relacionado
- [[ApifyActors|Revisar qué Actors hacen esto posible]]
