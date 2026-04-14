---
tags:
  - FundaMania
  - ERP
  - architecture
---
## 🏗️ Arquitectura de FundaMania ERP

### 1. Patrón Arquitectónico
El sistema sigue un patrón **Monolítico Modular** con *Next.js App Router*, separando responsabilidades mediante carpetas y Server Components.

### 2. Capas de la Aplicación
- **Capa de Presentación (Frontend):** React (Server y Client components), ShadCN para UI y TailwindCSS.
- **Capa de Lógica de Negocio (Backend):** Server Actions de Next.js.
- **Capa de Acceso a Datos:** Prisma ORM operando sobre esquemas fuertemente tipados.
- **Base de Datos:** PostgreSQL alojada en Supabase.

### 3. Principios Solid y Estructura Organizacional
Cada característica del ERP (Ventas, Inventario) debe vivir dentro del scope de su propio feature o módulo (ej: `src/features/inventory`).
