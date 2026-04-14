# 📱 Sistema de Gestión de Mercancía - Versión 1.0

**Stack Tecnológico:**  
- Framework: Next.js 15  
- Lenguaje: TypeScript  
- Base de datos: PostgreSQL via Supabase  
- ORM: PrismaORM  
- UI: ShadCN

**Objetivo del sistema:**  
Aplicación web para gestionar compras, inventario y punto de venta de fundas y protectores de pantalla para celulares.

---

## 🧩 Módulos Principales

### 🔐 Autenticación
- Registro/Login
- Roles: administrador, vendedor
- Protección de rutas

### 🛒 Punto de Venta
- Búsqueda por modelo de teléfono
- Visualización de stock
- Registro de ventas por producto, fecha y vendedor
- Venta rápida
- Resumen de ventas

### 📦 Control de Inventario
- CRUD de productos (modelo, tipo, color, precio, cantidad)
- Stock por modelo
- Alertas de bajo stock (< 5 unidades)
- Registro de movimientos (entrada/salida)

### 📤 Módulo de Compras
- Registro de compra por proveedor
- Actualización automática de inventario

### 🧾 Historial y Seguimiento
- Filtros por fecha, producto y proveedor
- Exportación básica (CSV, JSON)

---

## 🚀 Extras Recomendados

### 📊 Dashboard
- Métricas: ventas totales, productos más vendidos, alertas de stock
- Estadísticas semanales/mensuales

### 🖼️ Soporte Multimedia
- Imagen de productos

### 🔔 Notificaciones
- Feedback visual tipo toast para acciones realizadas

### 👥 Multiusuario
- Gestión básica de múltiples cuentas/logins

---

## 🧱 Modelo de Datos Propuesto (PrismaORM + PostgreSQL)

| Tabla         | Campos Clave                                                |
|---------------|--------------------------------------------------------------|
| `users`       | id, nombre, email, rol                                       |
| `products`    | id, modelo, tipo, color, precio_unitario                     |
| `inventory`   | id, producto_id, cantidad_actual                             |
| `sales`       | id, producto_id, cantidad, fecha, usuario_id                 |
| `purchases`   | id, producto_id, cantidad, precio_unitario, proveedor_id     |
| `suppliers`   | id, nombre, contacto                                         |

---

## 🗂️ Plan de Crecimiento (Para V.2+)
- Módulo contable
- Integración con tienda en línea
- Reportes más avanzados
- Gestión de devoluciones
