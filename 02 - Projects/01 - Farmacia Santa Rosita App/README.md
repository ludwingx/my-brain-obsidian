# Farmacia Santa Rosita App

## 🧠 Descripción
Sistema web de gestión para farmacias pequeñas. Permite administrar productos, inventario, ventas y punto de venta desde una interfaz moderna y sencilla.

## 🎯 Objetivo
Digitalizar la gestión interna de Farmacia Santa Rosita, eliminando el control manual en papel y brindando visibilidad en tiempo real del stock y ventas.

## ⚙️ Stack
> *⚠️ Supuesto: inferido del contexto del vault y proyectos similares del desarrollador*

| Tecnología | Uso |
|---|---|
| Next.js (App Router) | Framework principal |
| TypeScript | Lenguaje tipado |
| Prisma ORM | Acceso a base de datos |
| PostgreSQL / Supabase | Base de datos |
| ShadCN UI | Componentes de interfaz |
| TailwindCSS | Estilos |

## 📁 Estructura
```
/farmacia-santa-rosita-app
├── src/
│   ├── app/
│   ├── components/
│   ├── lib/
│   └── prisma/
```

## 🔑 Módulos Clave
- Productos (CRUD): nombre, código, precio compra/venta, stock
- Punto de venta
- Control de inventario y alertas de stock bajo
- Roles: administrador y vendedor

## 📌 Estado
🔨 En progreso

## 🔗 Relacionado
- [[../02 - Sabores Bolivianísimos App/README|Sabores Bolivianísimos]] — otro proyecto con backend similar
- [[../06 - FundaMania ERP/README|FundaMania ERP]] — misma arquitectura Prisma + ShadCN
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
