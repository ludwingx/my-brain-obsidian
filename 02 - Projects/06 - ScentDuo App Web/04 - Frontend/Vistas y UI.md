---
tags:
  - ScentDuo
  - Frontend
  - UX
---
## 🎨 Capa de Presentación (Frontend)

El objetivo es proyectar "Premium" y "Lujo" a través de un diseño minimalista.

### Guía de Diseño
- **Paleta de Colores:** Sobria (Negro #000000, Dorado elegante #D4AF37, Beige claro/crema para fondos legibles).
- **Tipografía:** Moderna y geométrica para títulos (Ej: *Playfair Display* o *Cinzel*), sin-serif legible para cuerpo (Ej: *Inter*, *Montserrat*).

### Rutas Frontend
`/(public)`:
- `page.tsx`: Landing principal con hero llamativo (fotos macro del producto, frascos).
- `/productos`: Catálogo filtrable.
- `/productos/[id]`: Ficha completa (Desglose de notas olfativas y CTA a compra por Whatsapp).

`(admin)`:
- Tablas ShadCN minimalistas, sidebar fijo oscuro.
- Feedback usando toasts para cualquier acción.
