# Modelo de Datos (Prisma Schema)

## Entidades Principales

### User
- `id`: String (PK)
- `email`: String (Unique)
- `name`: String
- `role`: Enum (`ADMIN`, `USER`)
- `campaigns`: Campaign[]

### Campaign (Campaña)
- `id`: String (PK)
- `name`: String
- `keyword`: String (ej. "El Deber")
- `target`: String (ej. "Luis Fernando Camacho")
- `status`: Enum (`ACTIVE`, `INACTIVE`)
- `userId`: String (FK)
- `sources`: Source[]
- `publications`: Publication[]

### Source (Fuente)
- `id`: String (PK)
- `name`: String (ej. "Página Facebook Red Uno")
- `url`: String
- `campaignId`: String (FK)

### Publication (Publicación Scrapeada)
- `id`: String (PK)
- `externalId`: String (ID de la red social)
- `content`: Text
- `url`: String
- `publishDate`: DateTime
- `sentiment`: Enum (`POSITIVE`, `NEUTRAL`, `NEGATIVE`)
- `reviewStatus`: Enum (`PENDING`, `APPROVED`, `REJECTED`)
- `campaignId`: String (FK)
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
