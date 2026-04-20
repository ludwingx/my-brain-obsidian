# 🔌 API Endpoints

> [[Market-GS]] > API Endpoints

---

## API Routes (REST)

Todos los endpoints están en `src/app/api/` y siguen el patrón REST estándar de Next.js App Router.

| Recurso              | Ruta                           | Métodos      | Modelo Prisma   |
|----------------------|--------------------------------|-------------|-----------------|
| Marcas               | `/api/brands`                  | GET, POST   | `Brand`         |
| Colores              | `/api/colors`                  | GET, POST   | `Color`         |
| Compatibilidad       | `/api/compatibility`           | GET, POST   | `Compatibility` |
| Movimientos          | `/api/inventory-movements`     | GET, POST   | `InventoryMovement` |
| Materiales           | `/api/materials`               | GET, POST   | `Material`      |
| Modelos de Teléfono  | `/api/phone-models`            | GET, POST   | `PhoneModel`    |
| Tipos de Producto    | `/api/product-types`           | GET, POST   | `ProductType`   |
| Productos            | `/api/products`                | GET, POST   | `Product`       |
| Proveedores          | `/api/providers`               | GET, POST   | `Supplier`      |

## Server Actions

Acciones del lado del servidor en `src/app/actions/`:

| Archivo       | Funciones                                        | Descripción                       |
|---------------|--------------------------------------------------|-----------------------------------|
| `auth.ts`     | `login()`, `logout()`, `getSession()`            | Autenticación y sesiones          |
| `products.ts` | CRUD de productos                                | Crear, editar, eliminar productos |
| `register.ts` | `register()`                                     | Registro de nuevos usuarios       |

## Patrón de Uso

```
Catálogos (brands, colors, etc.)  →  API Routes  (/api/*)
Productos (crear, editar)         →  Server Actions (actions/products.ts)
Auth (login, logout)              →  Server Actions (actions/auth.ts)
```

> **Nota:** Los catálogos usan API Routes porque se consumen desde el cliente con `fetch()`. Las acciones de productos y autenticación usan Server Actions porque se invocan directamente desde formularios del servidor.
