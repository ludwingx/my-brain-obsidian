---
tags:
  - C.R.E.Insight
  - seguridad
  - auth
---
# Sistema de Autenticación 

## 🧠 Doble Capa de Autenticación

Existen 2 ecosistemas en este proyecto, y cada uno cuenta con su propia seguridad separada por diseño.

### 1. Autenticación Operador - Interfaz Web Humano
- **Modelo:** `User` configurado en base de datos.
- Utilización de estrategias como Session Cookies de HTTP Only (NextAuth).
- Asegura vistas bajo el protector Middleware (`middleware.ts`), devolviendo `redirect('/login')` a accesos anónimos.
- Los Operadores ("analysts") solo pueden marcar comentarios como resueltos; no gestionan configuración alta de Keywords.

### 2. Autenticación Máquina - Interfaz API a n8n
- **Modelo:** Bearer Authorization Headers (Hardcoded Secret Token Server2Server).
- Se prohíbe el uso de OAuth u otros métodos stateful. Se comparan variables en el `.env` (Next) contra los Headers inyectados en los nodos HTTP Request de (n8n).
- Si hay desfazaje, 401 Automático.
