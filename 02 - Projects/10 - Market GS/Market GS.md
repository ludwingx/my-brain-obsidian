---
created: 2026-04-18 21:46
tags:
  - fullstack
  - NextJS
  - postgreSQL
  - Project
  - erp
  - inventario
---
# Market GS

> Sistema de gestión de inventario, ventas y compras para accesorios de celulares.
> **Desarrollado por:** Ludwing Dev
> **Colores del negocio:** Negro y blanco

---

## Información del Proyecto
Creación: 2026-04-18
Plataforma: Web
Stack: Next.js 16 + Prisma + PostgreSQL

---

## 📋 Secciones de la Documentación

- 00 - Resumen
  - [[02 - Projects/10 - Market GS/00 - Resumen/Requerimientos|📝 Requerimientos del Cliente]]
  - [[02 - Projects/10 - Market GS/00 - Resumen/StackTecnologico|⚙️ Stack Tecnológico]]
- 01 - Arquitectura
  - [[02 - Projects/10 - Market GS/01 - Arquitectura/EstructuraDelProyecto|🗂️ Estructura del Proyecto]]
  - [[02 - Projects/10 - Market GS/01 - Arquitectura/ComponentesUI|🧩 Componentes UI]]
  - [[02 - Projects/10 - Market GS/01 - Arquitectura/ApiEndpoints|🔌 API Endpoints]]
  - [[02 - Projects/10 - Market GS/01 - Arquitectura/Autenticacion|🔐 Autenticación y Sesiones]]
- 02 - Base de Datos
  - [[02 - Projects/10 - Market GS/02 - Base de Datos/BaseDeDatos|🗄️ Prisma Schema y Modelos]]
- 03 - Módulos
  - [[02 - Projects/10 - Market GS/03 - Modulos/Dashboard|📊 Dashboard]]
  - [[02 - Projects/10 - Market GS/03 - Modulos/Inventario|📦 Inventario]]
  - [[02 - Projects/10 - Market GS/03 - Modulos/Ventas|🛒 Ventas]]
  - [[02 - Projects/10 - Market GS/03 - Modulos/Compras|🛍️ Compras]]
  - [[02 - Projects/10 - Market GS/03 - Modulos/Reportes|📈 Reportes]]
  - [[02 - Projects/10 - Market GS/03 - Modulos/Configuracion|⚙️ Configuración (Catálogos)]]

---

## 🧭 Estado Actual

| Módulo         | Estado       |
|----------------|--------------|
| Dashboard      | ✅ Funcional  |
| Inventario     | ✅ Funcional  |
| Ventas         | ✅ Funcional  |
| Compras        | ✅ Funcional  |
| Reportes       | ✅ Funcional  |
| Configuración  | ✅ Funcional  |
| Autenticación  | ✅ Funcional  |

---

## 📌 Próximos Pasos (Requerimientos Nuevos)

Según los [[02 - Projects/10 - Market GS/00 - Resumen/Requerimientos|requerimientos del cliente]], se necesitan 3 funcionalidades nuevas:

1. **Venta mayorista** — Despacho con precio definido al momento
2. **Precio variable** — Costo de compra fijo, precio de venta por transacción
3. **Seguimiento de pedidos** — Control de dañados, devoluciones y wallet
