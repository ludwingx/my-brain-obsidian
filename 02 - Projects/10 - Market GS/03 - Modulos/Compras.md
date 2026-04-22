# 🛍️ Compras

> [[Market-GS]] > Compras

---

## Rutas

| Ruta                       | Descripción                          |
|----------------------------|--------------------------------------|
| `(dashboard)/compras`      | Historial de compras/pedidos         |
| `(dashboard)/compras/nueva`| Formulario de nueva compra           |
| `(dashboard)/compras/[id]/recibir` | Filtro de Realidad para declarar buenos/dañados |

## Modelo `Purchase` (Pedido a Proveedor)

| Campo           | Descripción                                           |
|-----------------|-------------------------------------------------------|
| `supplierId`    | Proveedor del pedido                                  |
| `totalAmount`   | Monto total del pedido                                |
| `status`        | Estado: `pendiente`, `recibido`, `parcial`, `cancelado` |
| `invoiceNumber` | Número de factura (opcional)                          |
| `notes`         | Notas del pedido                                      |
| `receivedAt`    | Fecha de recepción                                    |

## Modelo `PurchaseItem` (Ítems del Pedido)

Cada pedido tiene múltiples ítems:

| Campo             | Descripción                       |
|-------------------|-----------------------------------|
| `purchaseId`      | Pedido al que pertenece           |
| `productId`       | Producto solicitado               |
| `quantityOrdered` | Cantidad pedida                   |
| `quantityGood`    | Cantidad que llegó en buen estado |
| `quantityDamaged` | Cantidad dañada                   |
| `unitCost`        | Costo unitario                    |
| `totalCost`       | Costo total del ítem              |

## Flujo de una Compra y Filtro de Realidad

```mermaid
flowchart LR
    A[Crear pedido] --> B[Estado: Pendiente]
    B --> C[Recibir productos (Filtro de Realidad)]
    C --> D{¿Llegaron dañados?}
    D -- No --> E[Estado: Recibido / Stock Bueno]
    D -- Sí --> F[Declarar Dañados]
    F --> G{Evaluación del Daño}
    G -- "Daño Menor (ej. Gamuzadas sin logo)" --> H[Se asume como Pérdida Absoluta o se vende más barato]
    G -- "Daño Mayor / Gran Cantidad" --> I[Se pide Reembolso / Compensación]
    I --> J[Wallet de Compensaciones]
```

### Protocolo de Productos Dañados (Real-World)
Según la operativa de Market GS:
1. **Daños Menores (Pérdida Absoluta / Venta con Descuento):** Si llegan 1 o 2 fundas gamuzadas con fallas menores (ej. sin logo, logo borroso, mal centrado), **no se pide devolución**. El CEO decide si se asume como *Pérdida Absoluta* (se da de baja del inventario al 100%) o si se *vende más barato* para al menos recuperar el costo de transporte.
2. **Daños Mayores (Compensación):** Si una gran cantidad de productos llega mal (ej. de 300 llegan 100 mal), se pide **reembolso/compensación** al proveedor. Los 100 malos se bonifican en la próxima compra (costo cero). Los productos dañados físicos que quedan se evalúan para venderse más baratos o descartarse.
3. **Pérdida por Tiempo/Accidentes:** Productos en buen estado pueden sufrir daños con el tiempo (sol, humedad, accidentes). El sistema debe permitir registrar una **Pérdida Absoluta (100%)** dinámicamente desde el inventario.

## Proveedores

Los proveedores se gestionan en `(dashboard)/configuracion/proveedores` (parte del módulo de [[02 - Projects/10 - Market GS/03 - Modulos/Configuracion|Configuración]]). Se pueden añadir dinámicamente al momento de crear un producto.
