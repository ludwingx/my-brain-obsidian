# Market GS — Requerimientos del Cliente

> [[Market-GS]] > Requerimientos
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

---

## 🚀 Fase 2: Visión a Futuro y Automatización con IA (El Sueño a Largo Plazo)

Para llevar el sistema al siguiente nivel, se han mapeado los siguientes requerimientos como **objetivos a largo plazo**:

### 4. Flexibilidad de Catálogo e Ingreso Inteligente (Computer Vision)
- **Flexibilidad total:** El sistema debe permitir registrar no solo fundas, sino también cables, manillas y otros accesorios, dejando vacíos los campos que no apliquen.
- **Escaneo por Foto:** Permitir ingresar mercadería sacando una simple foto. La IA debe detectar el tipo de accesorio y extraer el color automáticamente.
- **Generación de Imágenes (Estudio IA):** Al tomar una foto de un producto (ej. sobre una mesa), la IA debe limpiar el fondo y generar una imagen profesional de "estudio" con fondo blanco en el mismo ángulo, lista para catálogo.

### 5. Catálogo Web Público
- **Tienda Online Integrada:** Implementar una web pública donde se visualice el inventario.
- **Gestión de Visibilidad:** Control absoluto desde el sistema interno sobre qué productos se muestran en la web y a qué precio al público.

### 6. Agente de Ventas IA 24/7 (Cierre de Ventas en Vivo)
- **Integración con TikTok Live:** Durante promociones en vivo, los clientes contactarán a un número automatizado.
- **Chatbot Cerrador:** Un bot con IA atenderá 24/7, mostrará el catálogo, negociará y gestionará el cobro.
- **Cero Intervención Humana en Chats:** Los empleados humanos no leerán chats; el sistema solo les mostrará los "Pedidos Pagados" listos para ser empaquetados.

### 7. Notificaciones Ejecutivas en Tiempo Real
- **Alertas al CEO:** Enviar correos electrónicos y notificaciones dentro de la app al dueño cada vez que se realice una venta importante o un cliente confirme su pago, permitiendo monitorear los ingresos de forma pasiva.
