---
tags:
  - C.R.E.Insight
  - ux
  - metricas
---
# Gráficas y Visualizaciones

## 🧠 Renderizado Numérico

La ventaja competitiva de un dashboard de Insight es mostrar tendencias.

### Stack Elegido: `Tremor.so` / `Recharts`
- **Área Evolutiva (Eje temporal vs Reacciones):** Permite cruzar el volumen del engagement (`PostMetrics.totalInteracciones`) agrupado a través del tiempo `collectedAt`. 
- **Gráfico de Dono (Sentiment Balance):** División simple para visualizar visualmente el Ratio Menciones Altas frente a Bajas.

### Transición de la Base de Datos a la Gráfica
Para no transferir el procesamiento numérico y agrupaciones a librerías en JS que derretirían el navegador del cliente, el Backend ejecuta llamadas complejas y Agrupaciones en la Base de Datos nativa, entregando solo series ordenadas a las Librerías UI.
