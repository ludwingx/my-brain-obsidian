# Modelo de Datos (Prisma Schema)

## Entidades Principales

### User
- `id`: String (PK)
- `email`: String (Unique)
- `name`: String
- `role`: Enum (`ADMIN`, `USER`)
- `searchFormats`: SearchFormat[] (Max 3)

### SearchFormat (Formato de Búsqueda)
- `id`: String (PK)
- `keyword`: String (ej. "El Deber" o "Violador")
- `target`: String (ej. "Luis Fernando Camacho")
- `status`: Enum (`ACTIVE`, `INACTIVE`)
- `userId`: String (FK)
- `publications`: Publication[]

### Publication (Publicación Scrapeada)
- `id`: String (PK)
- `externalId`: String (ID de la red social)
- `content`: Text
- `url`: String
- `publishDate`: DateTime
- `sentiment`: Enum (`POSITIVE`, `NEUTRAL`, `NEGATIVE`)
- `reviewStatus`: Enum (`PENDING`, `APPROVED`, `REJECTED`)
- `searchFormatId`: String (FK)
- `orders`: Order[]

### Order (Orden de Comentarios)
- `id`: String (PK)
- `goal`: Enum (`SUPPORT`, `CRITICIZE`)
- `notes`: Text (Instrucciones extra)
- `status`: Enum (`DRAFT`, `GENERATED`, `ACTIVATED`, `IN_PROGRESS`, `COMPLETED`, `ERROR`)
- `publicationId`: String (FK)
- `comments`: Comment[]

### Comment (Comentario Generado)
- `id`: String (PK)
- `content`: Text
- `status`: Enum (`PENDING`, `SENT`, `PUBLISHED`, `ERROR`)
- `createdAt`: DateTime
- `updatedAt`: DateTime
- `publishedAt`: DateTime?
- `orderId`: String (FK)
- `botId`: String? (ID del bot que lo ejecutó)
