# 📦 Inventario

> [[Market-GS]] > Inventario

---

## Rutas

| Ruta                         | Descripción                    |
|------------------------------|--------------------------------|
| `(dashboard)/inventario`                 | Página principal del inventario|
| `(dashboard)/inventario/productos`       | Listado/gestión de productos   |
| `(dashboard)/inventario/movimientos`     | Historial de movimientos       |

*Nota: Los CRUDs de catálogos (marcas, colores, modelos, tipos, materiales) ahora viven en el módulo de [[02 - Projects/10 - Market GS/03 - Modulos/Configuracion|Configuración]] bajo `(dashboard)/configuracion/`.*

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
