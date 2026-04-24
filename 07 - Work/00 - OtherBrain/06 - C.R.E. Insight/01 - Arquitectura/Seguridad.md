---
tags:
  - C.R.E.Insight
  - arquitectura
  - seguridad
---
# Seguridad del Proyecto

## 🧠 Directrices de Seguridad
Dado que manejaremos información reputacional de una entidad clave (C.R.E.), es imperativo mitigar cualquier exposición de datos y proteger tanto el Frontend como los flujos de automatización.

## 🛡️ Vectores de Protección

### 1. Nivel Frontend / Endpoints
- **Rate Limiting:** Toda la API expuesta por Next.js estará limitada por IP para evitar ataques de DoS o extracción masiva de nuestra propia data analizada.
- **Authorization:** Middleware en Next.js para rutas `/dashboard/*`. Uso de librerías modernas como NextAuth / Auth.js.

### 2. Nivel Scraping y Ejecución (n8n)
- **Env Variables:** Total prohibición de quemar tokens de Apify (API Keys) dentro del texto de los Workflows JSON. Utilizar Vaults nativos de n8n.
- **Webhooks Seguros:** Los webhooks que n8n expone para recibir la respuesta de Apify deben tener URL con hashes criptográficos imposibles de adivinar y validar si el request proviene de IPs conocidas (whitelist) u ofrecer un Token propio en el header `Authorization`.

### 3. Base de Datos (Prisma)
- **Sanitización Automática:** Todo string extraído de redes sociales pasará por Prisma ORM, garantizando que el Input inyectado en consultas SQL evite vulnerabilidades *SQL Injections*.

## 🔗 Relacionado
- [[Infraestructura|Infraestructura del Proyecto]]
