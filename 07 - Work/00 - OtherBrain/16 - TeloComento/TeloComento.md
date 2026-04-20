# Planificación del Proyecto en base a los requerimientos

## 1) Idea / Objetivo

Construir una aplicación web que:

- **Analice publicaciones** en redes sociales en base a una **palabra clave** y una **persona/causa/tema**.
- **Clasifique** cada publicación como `positiva`, `neutra` o `negativa` usando IA.
- Permita **crear órdenes** para generar comentarios con IA.
- Permita que **bots externos (Facebook)** consuman esas órdenes y publiquen los comentarios.

La app está pensada para **clientes en general** (multiusuario) y con arquitectura escalable.

![BCO.0dfc9abf-39bb-4819-8db1-2ad377691468.png](https://copilot.microsoft.com/th/id/BCO.0dfc9abf-39bb-4819-8db1-2ad377691468.png)

---

## 2) Fuentes a analizar (MVP)

- Páginas/perfiles donde aparezca la palabra clave.
- Ejemplos de palabra clave:
  - El Deber
  - Red Uno
- Ejemplos de persona/causa/tema:
  - Luis Fernando Camacho
- (Extensible a otras páginas/medios/temas)

---

## 3) Alcance general (componentes)

- **A) Scraper / Collector (externo)**
  - Servicio independiente que realiza scraping/búsqueda por palabra clave y persiste resultados.

- **B) Web App (este proyecto)**
  - Administra usuarios, campañas, publicaciones, revisión y órdenes.

- **C) Bots (externo)**
  - Sistema que consume órdenes y ejecuta comentarios en Facebook, reportando progreso.

---

## 4) Conceptos y entidades principales

- **Usuario**
  - Cuentas, permisos, configuración.

- **Campaña**
  - `palabra_clave`
  - `persona/causa/tema`
  - fuentes (páginas)
  - estado (`activa | inactiva`)
  - filtros (`positivo | neutro | negativo`)

- **Publicación**
  - fuente, URL/id, texto, fecha
  - sentimiento: `positivo | neutro | negativo`
  - estado de revisión: `pendiente | aprobada | rechazada`

- **Orden de comentarios**
  - referencia a una publicación
  - objetivo: `apoyar (positivo)` o `criticar (negativo)`
  - nota/instrucciones del usuario
  - estado:
    - `borrador`
    - `generado`
    - `activado`
    - `en_progreso`
    - `finalizado`
    - `error`

- **Comentario generado**
  - texto
  - timestamps (`creación`, `actualización`)
  - estado (`pendiente | enviado | publicado | error`)

- **Asignación de bots**
  - Separación explícita:
    - bots para **órdenes positivas**
    - bots para **órdenes negativas**

---

## 5) Flujo principal

### 1. Configuración de campaña
- El usuario crea una o varias **campañas**.
- Define:
  - palabra clave
  - persona/causa/tema
  - fuentes
  - filtros
- Debe poder editarse en tiempo real.

### 2. Ingesta de publicaciones
- El scraper externo guarda publicaciones en la base de datos.
- La IA clasifica el sentimiento automáticamente.

### 3. Revisión humana
- Listado de publicaciones con opción de:
  - aprobar
  - rechazar
  - editar sentimiento manualmente

### 4. Creación de orden
- Desde una publicación aprobada se puede:
  - crear una **orden de comentarios**
  - agregar instrucciones personalizadas

### 5. Generación de comentarios con IA
- Generación de múltiples comentarios únicos.
- Posibilidad de:
  - editar
  - eliminar antes de activar

### 6. Activación para bots
- La orden se activa y queda disponible para bots externos:
  - publicación objetivo
  - comentarios
  - tipo de orden (positivo/negativo)

### 7. Seguimiento
- Visualización del progreso en tiempo casi real:
  - por orden
  - por comentario

---

## 6) Requisitos funcionales

- **Multiusuario**
  - Gestión completa de usuarios.

- **Campañas**
  - CRUD completo.
  - Múltiples por usuario.
  - Filtros por sentimiento.
  - Selección de fuentes.

- **Publicaciones**
  - Listado por campaña/fuente/fecha/sentimiento.
  - Aprobar/rechazar.
  - Edición de sentimiento.

- **Órdenes**
  - Crear desde una publicación.
  - Instrucciones personalizadas.
  - Separación estricta por tipo (positivo vs negativo).

- **Comentarios**
  - Generación con IA.
  - Sin duplicados.
  - Edición/eliminación.
  - Trazabilidad de fechas.

- **Integración con bots**
  - Endpoint o cola de consumo.
  - Registro de progreso y resultados.

---

## 7) Requisitos no funcionales

- **Escalabilidad**
  - Preparado para nuevas acciones (like, share, etc.).

- **Seguridad**
  - Manejo de llaves vía `.env`.

- **Observabilidad**
  - Estados claros en todo el flujo.

---

## 8) Stack propuesto (Web App)

- Next.js
- Prisma ORM
- PostgreSQL
- shadcn/ui
- Sonner

---

## 9) IA (generación)

- Integración con OpenRouter.
- Uso de modelos económicos para generación masiva.
- Objetivo:
  - comentarios variados
  - evitar duplicación

---

## 10) Extensiones futuras

- Acciones adicionales:
  - Like
  - Compartir