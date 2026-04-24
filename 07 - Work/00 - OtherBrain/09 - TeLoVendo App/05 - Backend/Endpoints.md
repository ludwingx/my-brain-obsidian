---
tags:
  - TeLoVendo
  - API
  - RouteHandlers
---
# Construcción del Backend: Route Handlers

## 🧠 Filosofía de API

El modelo de TeLoVendo obliga a mantener sincronismo entre la App Next.js y los clientes externos (Scripts Bot o Móviles). Por tanto, la carpeta `app/api` no solo es un wrapper de BD, sino un conector B2B.

### Endpoint: `/api/genmarketplace`
- **Función:** Este endpoint es *Heartbeat* & *Data-Pump* para los dispositivos bot.
- **GET:** Un bot (Identificado por un token Header o su MAC Address) interroga este endpoint para extraer todas las instancias `GenMarketplace` cuyo `Status == PENDIENTE` y `deviceId == Mi_ID`.
- **POST/PATCH:** El bot reporta de vuelta que la iteración fue exitosa o fallida. Actualiza `OrderState`, guarda URLs capturadas (`postUrl`), y libera su propio status en la DB.

### Endpoint: `/api/admin`
- Funciones de mantenimiento pesadas restringidas al rol `ADMIN`.
- Forzado de cancelaciones masivas de Órdenes (`OrderStatus.CANCELADA`) o reseteo de estados de Dispositivos (`DeviceStatus.OCUPADO` a `LIBRE`) tras colapsos físicos del bot.

## 🔗 Relacionado
- [[../03 - Orquestacion Bots/Flujo de Ordenes|Revisa el Flujo de la Orquestación Visualmente]]
