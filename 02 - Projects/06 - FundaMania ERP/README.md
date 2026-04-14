# FundaMania ERP

## 🧠 Descripción
Sistema de gestión integral (ERP) orientado a un negocio de venta de fundas y protectores de pantalla para celulares. Gestiona inventario, punto de venta y compras a proveedores.

## 🎯 Objetivo
Tener un control preciso del inventario de productos (por modelo, tipo, color), registrar ventas diarias, generar alertas de stock bajo y gestionar el aprovisionamiento.

## ⚙️ Stack

| Tecnología | Propósito |
|---|---|
| Next.js 15 | Framework principal |
| TypeScript | Lenguaje tipado principal |
| Supabase (PostgreSQL) | Base de datos relacional y BaaS |
| Prisma ORM | Acceso a base de datos |
| ShadCN | Componentes UI |

## 🔑 Módulos Principales
- **Autenticación:** Roles para admin y vendedor.
- **Punto de Venta (POS):** Búsqueda por modelo, registro rápido.
- **Control de Inventario:** CRUD productos, stock actual, alertas < 5 unidades.
- **Módulo de Compras:** Registro de ingresos, proveedores.

## 📁 Documentación de Ingeniería
- **Arquitectura:** [[01 - Arquitectura/Arquitectura General|Arquitectura General]]
- **Base de Datos:** [[02 - Base de Datos/Esquema DB|Design Database & Prisma]]
- **Backend:** [[03 - Backend/Server Actions y API|Logica de Negocio]]
- **Frontend:** [[04 - Frontend/Vistas y UI|Interfaz y Vistas Críticas]]

## 📌 Estado
🔨 En progreso

## 🔗 Relacionado
- [[Sistema de Gestión de Mercancía]] — Especificación técnica inicial
- [[../01 - Farmacia Santa Rosita App/README|Farmacia SR App]] — Comparten base arquitectónica de ERP
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
