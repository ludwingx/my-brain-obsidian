# ⚙️ Configuración (Catálogos)

> [[Market-GS]] > Configuración

---

## Ruta

`/settings` → `src/app/settings/page.tsx`

## Descripción

Centro de administración de catálogos del sistema. Es la puerta de entrada a todos los CRUDs de entidades base que alimentan al resto del sistema.

## Catálogos Disponibles

| Catálogo              | Ruta                         | Modelo Prisma   |
|-----------------------|------------------------------|-----------------|
| Modelos de Teléfono   | `/inventory/phone-models`    | `PhoneModel`    |
| Colores               | `/inventory/colors`          | `Color`         |
| Tipos de Producto     | `/inventory/types`           | `ProductType`   |
| Materiales            | `/inventory/materials`       | `Material`      |
| Compatibilidad        | `/inventory/compatibility`   | `Compatibility` |
| Proveedores           | `/purchases/providers`       | `Supplier`      |

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
