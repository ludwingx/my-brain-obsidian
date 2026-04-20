# 📊 Dashboard

> [[Market-GS]] > Dashboard

---

## Ruta

`/dashboard` → `src/app/dashboard/page.tsx`

## Descripción

Página principal del sistema después del login. Muestra un resumen general del estado del negocio.

## Contenido

El dashboard presenta métricas y resúmenes usando componentes `MetricCard` y `QuickActionCard`:

- **Métricas principales** — Ventas del día/mes, productos en stock, alertas de stock bajo
- **Acciones rápidas** — Accesos directos a las funciones más usadas
- **Actividad reciente** — Últimos movimientos del sistema

## Queries

Las consultas del dashboard están centralizadas en:
`src/lib/dashboard-queries.ts`

Esto incluye agregaciones de ventas, conteo de productos, alertas de stock mínimo y actividad reciente.

## Componentes Usados

- `MetricCard` — Tarjetas con números y tendencias
- `QuickActionCard` — Botones de acceso rápido
- `ActivityItem` — Ítems de actividad reciente
- `AppSidebar` — Sidebar de navegación
