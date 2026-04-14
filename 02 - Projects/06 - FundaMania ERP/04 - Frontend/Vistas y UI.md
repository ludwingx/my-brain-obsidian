---
tags:
  - FundaMania
  - Frontend
  - UI
---
## 🎨 Capa de Presentación (Frontend)

Todo componente visual sigue la guía de estilo proveída por **ShadCN UI**.

### Vistas Críticas
1. **Punto de Venta (POS):**
   - Buscador ultrarrápido (Client Component filtrando una lista estática precargada para evitar lag).
   - Botones rápidos para selección.
   - Panel lateral de "Carrito actual" con botón rápido de cierre y cobro (impresión ticket opcional).
   
2. **Dashboard de Stock:**
   - Tabla con `DataTable` de ShadCN (sintaxis Tanstack Table).
   - Filtros por: `Color`, `Modelo de teléfono` y `Nivel de Stock`.
   - Indicador rojo **urgente** para ítems con stock `< 5`.

### Reglas de Estado Global
No se utilizará Redux. El Carrito en el POS se maneja con estado local de React (`useState`) o `Zustand` por la simplicidad y bajo peso.
