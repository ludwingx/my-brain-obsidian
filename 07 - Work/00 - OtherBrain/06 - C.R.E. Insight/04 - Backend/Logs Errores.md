---
tags:
  - C.R.E.Insight
  - backend
  - errores
---
# Gestión de Logs y Errores

## 🧠 Estándar de Reporte (Sentry & Logging)

En un entorno monolítico + orquestación (Next.js + n8n) los errores mudan y son difíciles de rastrear. Por ende se aplican dos estrategias:

### 1. Manejo de Errores en Scraping (n8n side)
Cualquier falla en los Webhooks hacia Next.js o caída de `Apify` desata el Workflow `Error Trigger` predeterminado en n8n.
- Envío automático de JSON de error (con `NodeName` e `IP`) hacia el canal interno de Slack / Discord del equipo técnico.

### 2. Next.js Route Handlers (Backend side)
Los errores internos en Prisma no pueden llegar crudos al cliente o al logs estándar. Todo endpoint expuesto en la carpeta `api/` debe estar envuelto en un bloque `try/catch`.
```typescript
try {
  // Lógica Prisma
} catch (error) {
  console.error('[API_ERROR] En /api/posts:', error);
  // Posible integración de Sentry aquí
  return NextResponse.json({ error: "Fallo en la sincronización" }, { status: 500 });
}
```
Esto asegura que las búsquedas futuras del error en Vercel Logs o PM2 Logs solo requieran un grep de `[API_ERROR]`.
