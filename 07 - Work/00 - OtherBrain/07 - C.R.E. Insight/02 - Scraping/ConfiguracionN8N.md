---
tags:
  - C.R.E.Insight
  - n8n
  - configuracion
---
# Configuración y Entorno de n8n

## 🧠 Estructura del Servidor
Para evitar costes insostenibles en n8n Cloud y restricciones de tiempo de ejecución, la orquestación se despliega en un entorno hosteado de manera independiente.

### Variables de Entorno (Environment) Clave
- `WEBHOOK_URL_CRE`: Ruta base utilizada por los nodos para realizar callbacks hacia nuestra Next.js API.
- `APIFY_API_TOKEN`: Guardado en la bóveda de credenciales (`Credentials`). Nunca se debe quemar (hardcodear) en los nodos HTTP.
- `POSTGRESDB_URI`: Cadena de conexión hacia Supabase / DB de Next.js, en caso de querer saltarse la API de Next.js y escribir directamente (no recomendado a menos que la carga sea abismal).

### Mejores Prácticas de Workflow
- **Naming Constraints:** Cada nodo debe llevar una descripción explícita de lo que hace, no solo "HTTP Request" sino -> `"Fetch Apify Actor - Facebook"`.
- **Manejo de Errores (Error Trigger):** Existe un flujo especial de n8n configurado globalmente para atrapar nodos fallidos y lanzar alertas (Slack / Discord / Email al desarrollador) cuando un proceso Scraping rompe por cambios en el CSS de Meta.
