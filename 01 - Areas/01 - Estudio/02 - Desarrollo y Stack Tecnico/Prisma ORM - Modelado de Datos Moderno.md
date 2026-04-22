---
tags:
  - Prisma
  - SQL
  - Database
---
# 📊 Prisma ORM: El Puente entre Código y Datos

Prisma es la razón por la que mis bases de datos son robustas y predecibles.

## 🧱 Filosofía del `schema.prisma`
Aprendí que el esquema es la **Única Fuente de Verdad**.
- **Relaciones:** Uso obligatorio de `@relation` para manejar integridad referencial.
- **Enums:** Tipado estricto para estados (ej: `TicketStatus: TODO, IN_PROGRESS, DONE`).
- **Indices:** Optimización de consultas mediante `@@index` en campos de búsqueda frecuente (como `userId` o `projectId`).

## 🛠️ Workflow de Migraciones
- `npx prisma migrate dev`: Nunca edito la BD manualmente. Todo rastro queda en archivos `.sql` preventivos.
- `seed.ts`: Automatización de carga de datos iniciales para entornos de desarrollo.
