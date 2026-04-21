# Te lo Comento

[BCO.0dfc9abf-39bb-4819-8db1-2ad377691468.png (1536×1024)](https://copilot.microsoft.com/th/id/BCO.0dfc9abf-39bb-4819-8db1-2ad377691468.png)

## 1) Idea / Objetivo

Construir una aplicación web que:

- **Monitoree publicaciones** en redes sociales en base a una **palabra clave** y una **causa/persona/tema**.
- **Clasifique** cada publicación como `positiva`, `neutra` o `negativa` usando IA.
- Permita **crear órdenes** para generar comentarios con IA.
- Permita que **bots externos (Facebook)** consuman esas órdenes y publiquen los comentarios.

La app está pensada para **clientes en general** (multiusuario) y con arquitectura escalable.

## 2) Fuentes a monitorear (MVP)

- Páginas/perfiles donde aparezca la palabra clave.
- Ejemplos de palabra clave:
  - El Deber
  - Red Uno
- Ejemplos de causa/persona/tema:
  - Luis Fernando Camacho
- (Extensible a otras páginas/medios/temas)

## 3) Alcance general (componentes)

- **A) Scraper/Collector (externo)**
  - App separada que hace el scraping/búsqueda por palabra clave y guarda resultados.
- **B) Web App (este proyecto)**
  - Administra usuarios, solicitudes de scraping, publicaciones, aprobaciones y órdenes.
- **C) Bots (externo)**
  - Sistema externo que toma órdenes y ejecuta comentarios en Facebook, reportando progreso.

## 4) Conceptos y entidades principales

- **Usuario**
  - Cuentas, permisos, configuración.
- **Tarjeta / Solicitud de scraping**
  - `palabra_clave`
  - `causa/persona`
  - fuentes (páginas)
  - estado (activa/inactiva)
  - filtros (mostrar positivo/neutro/negativo)
- **Publicación scrapeada**
  - fuente, URL/id, texto, fecha
  - sentimiento: `positivo | neutro | negativo`
  - estado de revisión: `pendiente | aprobada | rechazada`
- **Orden de comentarios**
  - apunta a una publicación
  - objetivo: `apoyar (positivo)` o `atacar (negativo)` (según caso)
  - nota/instrucciones extra del usuario
  - estado (borrador/generado/activado/en_progreso/finalizado/error)
- **Comentarios generados**
  - texto, timestamps (creación/actualización)
  - estado (pendiente/enviado/publicado/error)
- **Asignación de bots**
  - separar bots para **órdenes positivas** y **órdenes negativas** para evitar mezclas.

## 5) Flujo principal

- **1. Configuración de monitoreo**
  - El usuario crea una o varias tarjetas (solicitudes) con palabra clave + causa/persona.
  - Debe poder **editarse en tiempo real** (palabra clave, causa/persona, fuentes, filtros).
- **2. Ingesta de publicaciones**
  - El scraper externo guarda publicaciones en la base de datos del sistema.
  - IA clasifica sentimiento y se almacena junto a la publicación.
- **3. Revisión humana**
  - Vista de publicaciones scrapeadas con opción de **aprobar / rechazar**.
  - Opción de **cambiar manualmente** el sentimiento (positivo/neutro/negativo).
- **4. Generación de orden**
  - En cada publicación, botón para **crear orden**.
  - El usuario agrega una **nota/instrucciones** para guiar la generación de comentarios.
- **5. Generación de comentarios con IA**
  - Se generan N comentarios (sin repetirse entre sí) según la orden.
  - Se permite **editar/eliminar** comentarios antes de activar.
- **6. Activación para bots**
  - Activar orden para que el sistema de bots externo vea:
    - qué publicación comentar
    - qué comentarios usar
    - qué pool de bots corresponde (positivo vs negativo)
- **7. Seguimiento**
  - Ver progreso de bots en “tiempo real” (o cercano a tiempo real) por orden/comentario.

## 6) Requisitos funcionales

- **Multiusuario**
  - Gestión de usuarios completa.
- **Tarjetas de scraping**
  - Crear/editar/eliminar, múltiples por usuario.
  - Filtros por sentimiento (positivo/neutro/negativo).
  - Selección de fuente/página específica.
- **Publicaciones scrapeadas**
  - Listado por tarjeta/fuente/fecha/sentimiento.
  - Aprobar/rechazar.
  - Editar sentimiento.
- **Órdenes**
  - Crear desde una publicación.
  - Nota/instrucciones adicionales.
  - Separación estricta: órdenes/bots para positivo vs negativo.
- **Comentarios**
  - Generación con IA.
  - No repetición entre comentarios.
  - Editar/eliminar.
  - Ver fechas: creación, actualización y cuándo se comentó.
- **Integración con bots externos**
  - Endpoint/cola para que bots consulten órdenes activas.
  - Registro de avances y resultados.

## 7) Requisitos no funcionales

- **Arquitectura escalable**
  - Preparada para sumar acciones futuras (like, share, etc.).
- **Seguridad**
  - Uso de `.env` para llaves (OpenRouter).
- **Observabilidad**
  - Estados claros por publicación/orden/comentario (para saber qué falla y dónde).

## 8) Stack propuesto (Web App)

- Next.js
- Prisma ORM
- PostgreSQL
- shadcn/ui
- Sonner
- Todo en última versión

## 9) IA (generación)

- Conexión a OpenRouter vía `.env`.
- Usar un modelo barato/simple para generación masiva.
- Objetivo: comentarios variados (evitar duplicados entre sí).

## 10) Extensiones futuras

- Reacciones adicionales sobre publicaciones scrapeadas:
  - Like
  - Compartir
