# Flujo General

![[Pasted image 20251003095244.png]]
---
## Objetivo

Automatizar la extracción, procedimiento y almacenamiento de datos de al fanpage de C.R.E. y de menciones externas de redes sociales, para análisis de métricas y reputación.

---
## Descripción
El flujo general de n8n permite: 
- Extraer publicaciones de la fanpage de C.R.E.
- Realizar scraping de menciones externas de Facebook, Instagram y Tiktok
- Analizar sentimiento de publicaciones
- Almacenar y actualizar datos de la base de datos para visualización en el dashboard.
---
## Pasos del Flujo:
### 1. Inicio
- Disparo automático con Cron Shedule a las 06:00 am
- Registro de ejecución en logs del flujo.
## 2. Scraping
### 2.1. Nodo Apify Actor:

- Scrapea menciones de C.R.E. en perfiles públicos, grupos y publicaciones.
- Plataformas: Facebook, Instagram, Tiktok.
### 2.2. Datos recibidos de Apify
El actor de Apify devuelve un JSON con la siguiente información clave:

|Campo|Descripción|
|---|---|
|`facebookUrl`|URL principal de la fanpage|
|`postId`|ID único de la publicación|
|`pageName`|Nombre de la página|
|`url`|URL del post|
|`time`|Fecha y hora de publicación|
|`timestamp`|Marca de tiempo UNIX|
|`user`|Información del autor/propietario del post (`id`, `name`, `profileUrl`, `profilePic`)|
|`text`|Contenido textual de la publicación|
|`textReferences`|Links y hashtags mencionados en el post|
|`link`|Link principal dentro del contenido|
|`likes`|Número de “me gusta”|
|`shares`|Número de compartidos|
|`topReactionsCount`|Número total de reacciones|
|`media`|Imágenes o videos adjuntos con detalles (URL, tamaño, OCR de texto)|
|`feedbackId`|ID interno de seguimiento de Apify|
|`topLevelUrl`|URL del post en Facebook|
|`facebookId`|ID del usuario/página de Facebook|
|`pageAdLibrary`|Información de publicidad de la página (opcional)|
|`inputUrl`|URL que se le dio al actor para el scraping|

### 3. Procesamiento de Datos

- Nodo **Validación**:
    - Evita duplicados.
    - Verifica consistencia con los esquemas (`Post`, `PostMetrics`, `MencionesExternas`).
- Nodo **Enriquecimiento**:
    - Calcula métricas adicionales (engagement rate, promedio de comentarios, score de sentimiento).
### 4. Almacenamiento

- Nodo **Base de Datos (Prisma)**:
    - Inserta o actualiza registros en `Post`, `PostMetrics` y `MencionesExternas`.

### 5. Alertas y Notificaciones

- Nodo **Notificaciones** (opcional):
    - Email, Slack o Discord para publicaciones negativas relevantes.
- Nodo **Dashboard Update**:
    - Actualiza visualizaciones en Next.js.

### 6. Fin del Flujo

- Registro final en logs:
    - Fecha de ejecución
    - Cantidad de publicaciones procesadas
    - Cantidad de menciones externas
    - Errores o alertas generadas