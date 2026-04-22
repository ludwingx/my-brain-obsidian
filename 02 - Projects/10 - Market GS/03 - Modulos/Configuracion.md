# ⚙️ Configuración (Catálogos)

> [[Market-GS]] > Configuración

---

## Ruta

`(dashboard)/configuracion` → `src/app/(dashboard)/configuracion/page.tsx`

## Descripción

Centro de administración de catálogos del sistema. Es la puerta de entrada a todos los CRUDs de entidades base que alimentan al resto del sistema.

## Catálogos Disponibles

| Catálogo              | Ruta                         | Modelo Prisma   |
|-----------------------|------------------------------|-----------------|
| Marcas                | `(dashboard)/configuracion/marcas` | `Brand`         |
| Modelos de Teléfono   | `(dashboard)/configuracion/modelos`| `PhoneModel`    |
| Colores               | `(dashboard)/configuracion/colores`| `Color`         |
| Tipos de Producto     | `(dashboard)/configuracion/tipos`  | `ProductType`   |
| Materiales            | `(dashboard)/configuracion/materiales` | `Material`      |
| Proveedores           | `(dashboard)/configuracion/proveedores` | `Supplier`      |

## Funcionalidad de cada Catálogo

Cada catálogo implementa un CRUD completo:
- ✅ Crear nuevo registro
- ✅ Listar registros existentes
- ✅ Editar registro
- ✅ Eliminar (soft delete vía campo `status`)
- ✅ Restaurar registros eliminados

## Relación con Otros Módulos

Los catálogos alimentan directamente al módulo de [[02 - Projects/10 - Market GS/03 - Modulos/Inventario|Inventario]]:
- Al **crear un producto**, se seleccionan valores de estos catálogos
- Al **registrar una compra**, se usa el catálogo de proveedores
- Las **marcas** contienen modelos de teléfono que a su vez se asignan a productos
