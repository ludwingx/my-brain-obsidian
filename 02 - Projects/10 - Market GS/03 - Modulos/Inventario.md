# 📦 Inventario

> [[Market GS]] > Inventario

---

## Rutas

| Ruta                         | Descripción                    |
|------------------------------|--------------------------------|
| `/inventory`                 | Página principal del inventario|
| `/inventory/products`        | Listado/gestión de productos   |
| `/inventory/movements`       | Historial de movimientos       |
| `/inventory/brands`          | CRUD de marcas                 |
| `/inventory/phone-models`    | CRUD de modelos de teléfono    |
| `/inventory/colors`          | CRUD de colores                |
| `/inventory/types`           | CRUD de tipos de producto      |
| `/inventory/materials`       | CRUD de materiales             |
| `/inventory/compatibility`   | CRUD de compatibilidad         |

## Producto — Campos Principales

Un producto en el sistema tiene:

- **Modelo de teléfono** (obligatorio) — de qué teléfono es la funda
- **Color** (obligatorio) — color del producto
- **Tipo** (obligatorio) — categoría (funda, vidrio, etc.)
- **Material** (opcional) — material del producto
- **Proveedor** (opcional) — quién lo suministra
- **Stock** — cantidad disponible
- **Stock dañado** — cantidad dañada en inventario
- **Stock mínimo** — umbral para alertas
- **Precio de compra** (`costPrice`)
- **Precio minorista** (`priceRetail`)
- **Precio mayorista** (`priceWholesale`)
- **Descuento** — porcentaje y precio con descuento

## Formulario de Producto

El componente `ProductForm` (`src/components/product-form.tsx`) es el más complejo del sistema. Maneja:

- Selección de modelo con combobox (`ModelCombobox`)
- Selector de color visual (`ColorPickerModal`)
- Validación con Zod + React Hook Form
- Modo crear y modo editar

## Movimientos de Inventario

Los movimientos registran cada cambio de stock con:
- Tipo de movimiento (entrada, salida, ajuste)
- Cantidad
- Razón
- Referencia (ej: ID de venta o compra)
- Notas adicionales
- Usuario que ejecutó el movimiento
