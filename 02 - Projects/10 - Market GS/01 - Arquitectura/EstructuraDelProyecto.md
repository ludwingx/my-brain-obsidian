# 🗂️ Estructura del Proyecto

> [[Market-GS]] > Estructura del Proyecto

---

## Árbol de Carpetas

```
todofundas/                        ← Raíz del proyecto (renombrar a market-gs)
├── prisma/
│   ├── schema.prisma              ← Esquema de base de datos
│   └── seed.ts                    ← Datos iniciales
├── public/                        ← Archivos estáticos
├── src/
│   ├── app/                       ← App Router (Next.js)
│   │   ├── layout.tsx             ← Layout raíz
│   │   ├── page.tsx               ← Página raíz (redirige)
│   │   ├── globals.css            ← Estilos globales
│   │   ├── actions/               ← Server Actions
│   │   │   ├── auth.ts            ← Login, logout, getSession
│   │   │   ├── products.ts        ← CRUD de productos
│   │   │   └── register.ts        ← Registro de usuarios
│   │   ├── api/                   ← API Routes (REST)
│   │   │   ├── brands/
│   │   │   ├── colors/
│   │   │   ├── compatibility/
│   │   │   ├── inventory-movements/
│   │   │   ├── materials/
│   │   │   ├── phone-models/
│   │   │   ├── product-types/
│   │   │   ├── products/
│   │   │   └── providers/
│   │   ├── dashboard/             ← Dashboard
│   │   ├── inventory/             ← Inventario + sub-catálogos
│   │   │   ├── products/
│   │   │   ├── movements/
│   │   │   ├── brands/
│   │   │   ├── phone-models/
│   │   │   ├── colors/
│   │   │   ├── types/
│   │   │   ├── materials/
│   │   │   └── compatibility/
│   │   ├── purchases/             ← Compras
│   │   │   ├── components/
│   │   │   └── supplier/
│   │   ├── sales/                 ← Ventas
│   │   │   └── new/
│   │   ├── reports/               ← Reportes
│   │   ├── settings/              ← Configuración
│   │   ├── login/                 ← Login
│   │   └── register/              ← Registro
│   ├── components/                ← Componentes React
│   │   ├── ui/                    ← Componentes base (shadcn/ui)
│   │   ├── providers/             ← Context providers
│   │   ├── app-sidebar.tsx        ← Sidebar principal
│   │   ├── product-form.tsx       ← Formulario de productos
│   │   ├── login-form.tsx         ← Formulario de login
│   │   └── ...otros
│   ├── data/                      ← Datos estáticos
│   │   ├── color-names.ts         ← Nombres de colores
│   │   └── phone-models-2015plus.ts ← Modelos de teléfonos
│   ├── hooks/                     ← Custom hooks
│   │   ├── use-mobile.ts          ← Detección de móvil
│   │   └── use-toast.ts           ← Sistema de toasts
│   └── lib/                       ← Utilidades y servicios
│       ├── auth.ts                ← Lógica de autenticación JWT
│       ├── prisma.ts              ← Cliente Prisma singleton
│       ├── supabase.ts            ← Configuración de Supabase
│       ├── dashboard-queries.ts   ← Queries del dashboard
│       ├── phone-models-api.ts    ← API de modelos
│       ├── color-utils.ts         ← Utilidades de colores
│       ├── utils.ts               ← Utilidad cn() para clases
│       └── api/
│           └── colors.ts          ← API de colores
└── archivos de configuración      ← tsconfig, eslint, next.config, etc.
```

## Patrones de Arquitectura

### Server Actions vs API Routes

El sistema usa **ambos patrones**:

| Patrón          | Usado para                          | Ubicación            |
|-----------------|-------------------------------------|----------------------|
| Server Actions  | Auth, productos (mutaciones directas) | `src/app/actions/`   |
| API Routes      | CRUDs de catálogos (REST)           | `src/app/api/`       |

### Componentes UI

Se usa **shadcn/ui** como sistema de componentes base. Los componentes están en `src/components/ui/` y se personalizan según necesidad. Ver [[02 - Projects/10 - Market GS/01 - Arquitectura/ComponentesUI|Componentes UI]].
