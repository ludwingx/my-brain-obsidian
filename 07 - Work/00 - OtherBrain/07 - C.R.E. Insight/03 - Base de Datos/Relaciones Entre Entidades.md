---
tags:
  - C.R.E.Insight
  - database
  - relations
---
# Relaciones y Cascada de Entidades

## 🧠 Arquitectura de Relacionamiento en PostgreSQL

### 📊 `Post` 1 <-----> N `PostMetrics`
Relación troncal en el sistema. Un Post único detectado en Facebook no debe duplicarse. Lo que se duplica son los puntos históricos a través de `PostMetrics`.
- **onDelete: Cascade:** Si la publicación desaparece y se purga desde el Administrador de Insight, se eliminará silenciosamente toda su cola de evoluciones en `post_metrics`.

```prisma
model PostMetrics {
  // ...
  post        Post     @relation(fields: [postId], references: [id], onDelete: Cascade)
  // ...
}
```

### Independencia de Menciones (`Mention`)
El modelo `Mention` no está ligado estrictamente al modelo `Post`. Existen libres en el vacío digital y se enlazan semánticamente de manera abstracta basándonos en la fecha (`publishedAt`), no a través de una Foreign Key. Esto previene cuellos de botella en la base de datos al realizar sub-queries complejas.
