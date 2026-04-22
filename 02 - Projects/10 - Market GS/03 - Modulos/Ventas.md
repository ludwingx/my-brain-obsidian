# 🛒 Ventas

> [[Market-GS]] > Ventas

---

## Rutas

| Ruta          | Descripción                                  |
|---------------|----------------------------------------------|
| `(dashboard)/ventas`      | Historial de ventas con filtros y búsqueda   |
| `(dashboard)/ventas/nueva`  | Formulario para registrar una nueva venta     |

## Modelo `Sale`

Cada venta registra:

| Campo             | Descripción                                    |
|-------------------|------------------------------------------------|
| `productId`       | Producto de [[Inventario]] vendido             |
| `quantity`        | Cantidad vendida                               |
| `unitPrice`       | Precio unitario al que se vendió               |
| `originalPrice`   | Precio original (antes de descuento)           |
| `discountApplied` | Monto de descuento aplicado                    |
| `totalPrice`      | Precio total de la venta                       |
| `customerName`    | Nombre del comprador (opcional)                |
| `customerPhone`   | Teléfono del comprador (opcional)              |
| `paymentMethod`   | Método de pago (`efectivo` por defecto)        |
| `type`            | Tipo: `minorista` o `mayorista`                |
| `notes`           | Notas de la venta                              |
| `clientId`        | Cliente asociado                               |

## Tipos de Venta

- **Minorista** — Venta unitaria al público general
- **Mayorista** — Venta en cantidad a negocios o revendedores

## Historial

La página `(dashboard)/ventas` muestra un listado con:
- Filtros por fecha, tipo, producto
- Totales de ventas (alimentan los [[Reportes]])
- Detalles de cada transacción
