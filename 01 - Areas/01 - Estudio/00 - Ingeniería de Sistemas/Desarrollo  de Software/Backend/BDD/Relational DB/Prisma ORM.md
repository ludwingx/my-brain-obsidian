---
tags:
  - DesarrolloDeSoftware
  - RelationalDB
  - RelationalDB
  - Prisma
  - SQL
  - ORM
---
# ðŸ“Š Prisma ORM: Modelado de Datos Moderno

La herramienta que unificÃ³ mi Backend y mi Frontend mediante el poder del tipado dinÃ¡mico.

## ðŸ—ï¸ La Capa de Datos
AprendÃ­ que el esquema `schema.prisma` es la base de cualquier aplicaciÃ³n robusta.

### 1. Modelado de Relaciones
- **1:1, 1:n y m:n:** Entender cÃ³mo Prisma maneja las tablas intermedias de forma transparente.
- **Referential Actions:** Uso de `onDelete: Cascade` para mantener la base de datos limpia.

### 2. El Poder del Prisma Client
- **Type Safety:** El cliente se genera a partir del esquema, dÃ¡ndome autocompletado total en VS Code.
- **Filtros Avanzados:** Uso de `include` y `select` para optimizar el rendimiento y evitar "Over-fetching" (traer mÃ¡s datos de los necesarios).

## ðŸš€ IntegraciÃ³n con PostgreSQL & Vercel
Mi configuraciÃ³n preferida para producciÃ³n:
- **Connection Pooling:** Uso de herramientas para no agotar las conexiones de la base de datos en entornos Serverless.

---
## ðŸ”— NavegaciÃ³n
- [[../Data Bases|Volver a Bases de Datos]]
- [[ Backend | Volver al Índice Principal ]]


---
## ??? Núcleo de Carpeta
- [[Relational DB]]
