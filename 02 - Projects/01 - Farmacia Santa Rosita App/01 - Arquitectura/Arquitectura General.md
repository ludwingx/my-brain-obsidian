---
tags:
  - Farmacia
  - App
  - NextJS
  - ServerActions
---
## 🏗️ Arquitectura de Farmacia Santa Rosita

El sistema se basa en la misma arquitectura general que está sirviendo como base (Boilerplate) para los otros sistemas ERP.

### Capas
- **UI:** ShadCN (para inputs, datatables rápidos).
- **Controladores (Backend):** Server Actions de Next.js (No API routes) que mandan a llamar a Prisma de manera síncrona/transaccional.
- **Persistencia:** PostgreSQL.

### Módulos Base
- `src/features/products`
- `src/features/sales`
- `src/features/inventory`
