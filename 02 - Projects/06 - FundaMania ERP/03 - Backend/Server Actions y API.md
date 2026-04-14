---
tags:
  - FundaMania
  - Backend
  - API
  - ServerActions
---
## ⚙️ Backend y Server Actions

La lógica está concentrada en *Server Actions* en lugar de API Routes RESTful. 
Esto disminuye la carga en el cliente e hidrata en SSR la aplicación.

### Listado de Server Actions Críticos
- `registerSale(productId, quantity, user)`: 
  - Inicia una transacción Prisma.
  - Verifica si `stock >= quantity`.
  - Registra el `Movement` OUT.
  - Genera registro en `Sale`.
  - Llama a `revalidatePath('/dashboard/inventory')`.
- `addStock(productId, quantity, supplierId)`:
  - Transacción para sumar inventario y registrar la entrada.

### Autenticación
Protección en Middleware de Next.js para rutas `/dashboard/*`. Solo usuarios con rol `ADMIN` pueden añadir stock o modificar precios. Todos los roles operativos pueden hacer ventas.
