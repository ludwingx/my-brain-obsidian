# Market GS — Requerimientos del Cliente

> **Colores del negocio:** Negro y blanco
> **Modelo de ingreso:** Porcentaje sobre las ganancias de ventas mensuales

---

## Contexto

El cliente ya cuenta con un sistema base. Se requieren **3 funcionalidades adicionales** que el sistema actual no cubre.

---

## 1. Venta mayorista (despacho a negocios/clientes)

- Poder gestionar el despacho de productos de forma **mayorista** a otros negocios o personas.
- Al momento de crear el pedido del cliente, se debe poder **declarar el precio de venta** en ese instante (no se usa un precio fijo del producto).
- La venta unitaria también debe permitir definir el precio al momento de vender.

---

## 2. Precio variable y registro de costos

- El precio de los productos **varía constantemente**, tanto de compra como de venta.
- Al registrar un producto, solo se declara el **precio de compra** (no un precio de venta fijo).
- El precio de venta se define al momento de cerrar cada venta, y debe quedar **registrado el monto exacto** al que se vendió.
- Se debe poder agregar una **nota** al registrar una venta (ej: promoción, producto con detalle, etc.).
- Cualquier cambio de precio (promociones, bajadas de costo) debe quedar reflejado en el historial.

---

## 3. Seguimiento de pedidos a proveedores (productos dañados / devoluciones)

- Al pedir productos a un proveedor, se registra:
  - **Cantidad solicitada**
  - **Costo total del pedido**
- Cuando el producto llega (ej: desde Brasil a la frontera), se verifica el estado.
- Si hay productos **dañados o faltantes**, se debe poder declarar:
  - Qué cantidad llegó bien y qué cantidad no.
  - El monto que el proveedor devuelve o el ajuste de precio acordado.
- Todo este flujo debe quedar registrado como un **historial del producto**: desde que se pidió, se recibió, se verificó y se ajustó.
- Estos movimientos deben reflejarse en la **wallet del sistema** (registro financiero interno).

---

## Resumen

| # | Funcionalidad                        | Descripción corta                                                  |
|---|--------------------------------------|--------------------------------------------------------------------|
| 1 | Venta mayorista                      | Despacho a negocios con precio definido al momento de la venta     |
| 2 | Precio variable                      | Registro de costo de compra y precio de venta por transacción      |
| 3 | Seguimiento de pedidos a proveedores | Control de productos dañados, devoluciones y ajustes en la wallet  |
