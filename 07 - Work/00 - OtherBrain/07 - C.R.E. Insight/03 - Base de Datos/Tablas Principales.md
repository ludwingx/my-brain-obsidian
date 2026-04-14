---
tags:
  - C.R.E.Insight
  - database
  - schema
  - prisma
---
# Tablas Principales y Esquema Físico

## 🧠 Estructura de Entidades
La persistencia del proyecto se confía a **PostgreSQL** a través del motor generador de tipos de **Prisma ORM**. Esta decisión obedece a la facilidad de almacenar y consultar arrays de strings (hashtags) o esquemas complejos en columnas `JSONB`.

## 📦 Modelo `Post` (Fanpage)
Almacena el registro estático de una publicación generada por C.R.E.
```prisma
model Post {
  id               Int       @id @default(autoincrement())
  id_publicacion   String    @unique
  plataforma       String    
  fecha            DateTime
  timestamp        BigInt?
  texto            String    @db.Text
  me_gusta         Int       @default(0)
  comentarios      Int       @default(0)
  compartidos      Int       @default(0)
  hashtags         String[]  
  url_image        String?   @db.Text
  url_publicacion  String    @db.Text
  seguimiento      Boolean   @default(true)
  tipoContenido    String    @default("imagen")
  metrics          PostMetrics[]
}
```

## 📊 Modelo `PostMetrics` (Evolución de Engagement)
Registra "fotografías" del rendimiento de un post en un instante específico.
```prisma
model PostMetrics {
  id                 Int      @id @default(autoincrement())
  postId             Int
  post               Post     @relation(fields: [postId], references: [id], onDelete: Cascade)
  likes              Int      @default(0)
  comments           Int      @default(0)
  shares             Int      @default(0)
  views              Int?     @default(0)
  collectedAt        DateTime @default(now())
  totalInteracciones Int?     @default(0)
  engagementRate     Float?
  tipoContenido      String?  @default("imagen")

  @@index([postId, collectedAt])
  @@map("post_metrics")
}
```

## 🚨 Modelo `Mention` (Radar de Reputación)
Posts generados por terceros en redes públicas detectados en los raspados.
```prisma
model Mention {
  id                   Int      @id @default(autoincrement())
  sourceName           String   // Nombre del perfil o grupo
  sourceUrl            String   
  url_image            String?  @db.Text
  platform             String   // Facebook, Instagram, TikTok
  content              String
  mentionUrl           String   @unique
  razon_especifica     String?  // Ej: "Crítica en grupo Facebook"
  comentario_principal String?
  publishedAt          DateTime
  collectedAt          DateTime @default(now())
}
```

## 🔑 Modelo `Keyword`
Gestión dinámica desde dashboard de palabras clave de tracking.
```prisma
model Keyword {
  id        Int      @id @default(autoincrement())
  palabra   String   @unique
  activa    Boolean  @default(true)
}
```

## 🔗 Relacionado
- [[../01 - Arquitectura/ArquitecturaGeneral|Revisar Arquitectura General]]
- [[Migraciones|Estrategias de Migración DB]]
