# Estructura de Componentes UI

El proyecto **OB Universe** confía gran parte de su interactividad a una colección de componentes altamente modulares en `/components`.

## 1. Componentes Centrales (Layout)
- **`app-sidebar.tsx`**: Es el esqueleto modular de navegación. Usa contextos de datos estáticos temporales (`data.navMain`, `data.projects`) para construir la jerarquía lateral.
- **`nav-main.tsx` & `nav-projects.tsx`**: Componentes hijos del Sidebar que renderizan las listas colapsables y accesos rápidos con iconografía de `lucide-react`.
- **`team-switcher.tsx`**: Un componente pensado para escalabilidad empresarial, permitiendo al usuario cambiar el entorno o "tenant" activo.

## 2. Componentes de Estética Retro
- **`vortex-background.tsx`**: Genera el fondo dinámico de partículas y ondas que se observa en la Landing Page, otorgando el "look and feel" inmersivo y de alta tecnología.
- **`retro-window.tsx`**: Simula una ventana de terminal clásica (estilo Windows 95/98 mezclado con cyberpunk). Se usa para exhibir los procesos `NEURAL_FEED.EXE` y `CORE_MONITOR.BAT` y contiene imágenes y gifs que simulan carga de procesos para asombrar al usuario final.

---
🔙 **Principal:** [[OB Universe]]
