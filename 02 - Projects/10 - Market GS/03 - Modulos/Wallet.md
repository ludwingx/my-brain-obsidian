---
title: Wallet de Compensaciones
status: Completado
priority: Alta
---

# Wallet de Compensaciones

> [[Market-GS]] > [[Wallet]]

Módulo vital para el control financiero de mermas y devoluciones. Cuando un producto llega dañado desde el proveedor en el módulo de [[Compras]], la "Wallet" se encarga de rastrear qué sucede con ese dinero perdido.

## Funcionalidades Implementadas

1. **Gestión de Transacciones:**
   - Permite registrar ingresos y egresos vinculados a un proveedor.
   - Tipos de movimiento: Devolución de dinero, compensación en próxima compra, ajustes por daño, entre otros.
   - Cálculo automático del "Saldo a favor" (Ingresos - Egresos).

2. **Integración con Filtro de Realidad:**
   - Los productos que se declaran como "dañados" en el módulo de compras quedan bajo el ojo de este módulo, esperando que el CEO negocie su devolución o su venta a precio rebajado.

3. **Métricas en Vivo:**
   - Muestra tarjetas de estado con:
     - Saldo a Favor.
     - Total Recuperado (Ingresos).
     - Pérdidas Asumidas (Egresos).

## Notas Técnicas
- **Modelo Prisma:** Utiliza la tabla `WalletTransaction`.
- **Ruta:** `(dashboard)/wallet/page.tsx`
- **Server Action:** `createWalletTransactionAction` maneja la validación e inserción atómica en base de datos.
