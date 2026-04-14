---
tags:
  - FundaMania
  - Database
  - Prisma
---
## 🗄️ Esquema Principal de BD

### Migraciones y Modelos Clave
Gestión de inventario para una tienda física de fundas y accesorios.

**Models principales en Prisma**:
```prisma
model Product {
  id             String      @id @default(uuid())
  model          String      // Ej: iPhone 15, Galaxy S23
  type           String      // Ej: Funda de Silicona, Vidrio Templado
  color          String
  price          Decimal
  stock          Int         @default(0)
  movements      Movement[]
  createdAt      DateTime    @default(now())
}

model Movement {
  id             String      @id @default(uuid())
  productId      String
  product        Product     @relation(fields: [productId], references: [id])
  type           MovementType // IN (Compra) | OUT (Venta)
  quantity       Int
  priceUnit      Decimal?
  date           DateTime    @default(now())
  user           String      // Referencia a Auth
}

enum MovementType {
  IN
  OUT
}
```

### Reglas de Integridad
- Todo registro de `MovementType.OUT` debe restar automáticamente al stock general del producto usando una transacción atómica.
- No se permiten stocks en negativo.
