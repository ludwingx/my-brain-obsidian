---
tags:
  - TeLoVendo
  - Componentes
  - Zustand
---
# UI y Manejo de Estado

## 🧠 Desafíos del Frontend
Crear una publicación de "Marketplace" de Vehículos vs una de "Bienes Raíces" requieren campos de input diametralmente distintos, lo cual rompe los estados tradicionales de React.

### Implementación mediante `Zustand`
Zustand gestiona el "Formulario Multipaso" en la memoria del cliente.
- Permite almacenar el Dropdown de *Marca de Auto*, *Año* y *Kilometraje* solo si el dropdown inicial de categoría tiene seleccionado `AUTOS_Y_CAMIONETAS`.
- Mantiene las `imageUrls` renderizadas en memoria visual mediante un componente `Embla Carousel` antes de enviarlas al Payload final.

### Shadcn UI Extendida
Se observó una implementación pesada de primitivas Radix. Las Badges (Píldoras semánticas) deben programarse utilizando la herramienta `class-variance-authority` (cva) mapeando los Colores a los Enums de la base de datos (Ej: `OrderStatus.COMPLETADA` siempre debe tender a Tailwind `bg-green-100 text-green-800`).
