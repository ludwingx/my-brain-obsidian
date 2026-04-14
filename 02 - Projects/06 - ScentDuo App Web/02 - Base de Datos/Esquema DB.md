---
tags:
  - ScentDuo
  - Database
  - Prisma
---
## 🗄️ Esquema Principal de BD

### Modelos y Entidades Centrales
Sujeto al modelado para Perfumería Premium (Volúmenes en ml, decants y fragancias completas).

```prisma
model Product {
  id          String   @id @default(uuid())
  name        String
  description String?
  price       Decimal
  brand       String
  category    String   // Ej: Nicho, Diseñador, Árabe
  notes       String?  // Notas olfativas (Salida, Corazón, Fondo)
  size        String   // Ej: 50ml, 100ml, Decant 10ml
  stock       Int      @default(0)
  isPublished Boolean  @default(false)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
  images      Image[]
  movements   InventoryMovement[]
}

model InventoryMovement {
  id          String   @id @default(uuid())
  productId   String
  product     Product  @relation(fields: [productId], references: [id])
  type        String   // IN / OUT
  quantity    Int
  description String?  // "Venta vía web", "Ingreso almacén"
  createdAt   DateTime @default(now())
}
```

### Notas Relacionales
Las ventas restan el `stock` en `Product` pero siempre deben insertarse en `InventoryMovement` para rastreabilidad histórica.
