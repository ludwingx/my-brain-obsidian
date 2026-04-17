---
tags:
  - OBWorkspace
  - Frontend
  - Styling
  - Themes
---
# Arquitectura de Estilo y Tema

El diseño visual de OB-Workspace se rige por principios de minimalismo, eficiencia y alto contraste, utilizando la suite de herramientas **shadcn/ui** y **Tailwind CSS**.

##  Sistema de Diseño: B&W Minimalist

Se ha adoptado una estética de "Blanco y Negro" para reducir la carga cognitiva y dar un aspecto profesional y atemporal.

### Componentes Basales

Todos los componentes se basan en **Radix UI** para accesibilidad:
- **Dialog:** Modales accesibles con focus management
- **Dropdown Menus:** Menús desplegables con keyboard navigation
- **Tabs:** Navegación por tabs con ARIA attributes
- **Tooltip:** Tooltips con delay y positioning inteligente
- **Popover:** Popovers con dismissible behavior

### Paleta de Colores

Uso intensivo de `slate`, `zinc` y `black/white` puro:

**Variables CSS (:root):**
- `--background`: 0 0% 100% (blanco puro)
- `--foreground`: 240 10% 3.9% (casi negro)
- `--card`: 0 0% 100% (blanco)
- `--card-foreground`: 240 10% 3.9% (texto oscuro)
- `--primary`: 240 5.9% 10% (negro principal)
- `--primary-foreground`: 0 0% 98% (blanco sobre negro)
- `--secondary`: 240 4.8% 95.9% (gris claro)
- `--secondary-foreground`: 240 5.9% 10% (texto oscuro)
- `--muted`: 240 4.8% 95.9% (gris suave)
- `--muted-foreground`: 240 3.8% 46.1% (texto gris)
- `--accent`: 240 4.8% 95.9% (gris de acento)
- `--accent-foreground`: 240 5.9% 10% (texto oscuro)
- `--destructive`: 0 84.2% 60.2% (rojo para acciones destructivas)
- `--destructive-foreground`: 0 0% 98% (blanco sobre rojo)
- `--border`: 240 5.9% 90% (bordes gris claro)
- `--input`: 240 5.9% 90% (inputs gris claro)
- `--ring`: 240 5.9% 10% (anillo de foco negro)
- `--radius`: 0.5rem (radio de borde)

**Variables CSS (.dark):**
- `--background`: 240 10% 3.9% (negro casi puro)
- `--foreground`: 0 0% 98% (blanco)
- `--card`: 240 10% 3.9% (negro)
- `--card-foreground`: 0 0% 98% (blanco)
- `--primary`: 0 0% 98% (blanco principal)
- `--primary-foreground`: 240 5.9% 10% (negro sobre blanco)
- `--secondary`: 240 3.7% 15.9% (gris oscuro)
- `--secondary-foreground`: 0 0% 98% (blanco)
- `--muted`: 240 3.7% 15.9% (gris oscuro suave)
- `--muted-foreground`: 240 5% 64.9% (texto gris claro)
- `--accent`: 240 3.7% 15.9% (gris oscuro de acento)
- `--accent-foreground`: 0 0% 98% (blanco)
- `--destructive`: 0 62.8% 30.6% (rojo oscuro)
- `--destructive-foreground`: 0 0% 98% (blanco)
- `--border`: 240 3.7% 15.9% (bordes gris oscuro)
- `--input`: 240 3.7% 15.9% (inputs gris oscuro)
- `--ring`: 240 4.9% 83.9% (anillo de foco blanco)

### Tipografía

Inter (Google Fonts) configurada como variable de CSS para una carga optimizada:

**Configuración:**
- Import de Google Fonts con pesos 100-900
- Variable CSS `--font-sans` con fallback a system-ui
- Aplicación global en body
- Uso de `@layer base` para estilos base

**Estilos Base:**
- Todos los elementos tienen border-border
- Body tiene bg-background y text-foreground
- Font-family usa variable --font-sans

##  Gestión de Tema (Light/Dark Mode)

A diferencia de implementaciones estándar que solo usan `localStorage`, OB-Workspace utiliza un sistema híbrido que prioriza las **cookies** para evitar el flash de contenido desestilizado (FOUC).

### Flujo de Implementación

1. **Server-Side Detection:** En `app/layout.tsx`, el servidor lee la cookie `theme`.
2. **Hydration:** El valor se inyecta en la clase de la etiqueta `<html>` antes de que la página llegue al cliente.
3. **Persistence:** El `ThemeProvider` escucha los cambios del `setTheme` y actualiza la cookie `document.cookie` para futuras cargas.

### Configuración Técnica

#### ThemeProvider

**Propósito:** Wrapper para next-themes con configuración personalizada

**Implementación:**
- Client Component con 'use client'
- Wrapper de NextThemesProvider
- Props pasadas directamente al provider
- Configuración externa en layout

**Configuración:**
- `attribute="class"`: Usa clases CSS para temas
- `defaultTheme="system"`: Usa tema del sistema por defecto
- `enableSystem`: Permite tema del sistema
- `disableTransitionOnChange`: Evita transiciones al cambiar tema

#### Layout con Server-Side Theme

**Propósito:** Detectar tema en servidor y evitar FOUC

**Implementación:**
- Import de cookies de next/headers
- Lectura de cookie 'theme' o default 'system'
- Inyección de clase en etiqueta `<html>`
- Wrapper de ThemeProvider con configuración

**Beneficios:**
- Sin flash de contenido desestilizado
- Tema correcto desde el primer render
- Persistencia en cookies
- Soporte para tema del sistema

#### Mode Toggle Component

**Propósito:** Botón para alternar entre temas light/dark

**Implementación:**
- Client Component con 'use client'
- Hook useTheme de next-themes
- Estado mounted para evitar hydration mismatch
- Iconos Sun y Moon de lucide-react
- Animaciones de rotación y escala

**Comportamiento:**
- No renderiza hasta estar montado
- Alterna entre light y dark
- Iconos animados con transiciones
- Botón ghost para no distraer
- Accesible con sr-only text

**Animaciones:**
- Sun: rotate-0 scale-100 en light, -rotate-90 scale-0 en dark
- Moon: rotate-90 scale-0 en light, rotate-0 scale-100 en dark

##  Responsividad y UX

Se aplican reglas estrictas para el manejo de contenido dinámico:
- **Wrap vs Truncate:** En elementos críticos como botones de selección o diálogos, se prefiere el ajuste de línea (`whitespace-normal`) sobre el truncado para evitar pérdida de información en títulos largos.
- **Micro-interacciones:** Uso de `transition-all`, hover effects y rotaciones de iconos para dar feedback táctil al usuario sin romper la sobriedad del diseño.

##  Componentes Personalizados

### Button Variants

**Propósito:** Botón reutilizable con múltiples variantes

**Variantes Disponibles:**
- **default:** bg-primary con hover
- **destructive:** bg-destructive para acciones peligrosas
- **outline:** border con hover bg-accent
- **secondary:** bg-secondary con hover
- **ghost:** hover bg-accent sin fondo
- **link:** text-primary con underline

**Tamaños:**
- **default:** h-10 px-4 py-2
- **sm:** h-9 px-3
- **lg:** h-11 px-8
- **icon:** h-10 w-10 cuadrado

**Características:**
- Usa class-variance-authority (cva)
- Ring offset para focus visible
- Transiciones suaves
- Disabled states con opacity
- Slot pattern para asChild

### Card Component

**Propósito:** Contenedor reutilizable para contenido agrupado

**Subcomponentes:**
- **Card:** Contenedor principal con border y shadow
- **CardHeader:** Header con flex-col y espacio vertical
- **CardTitle:** Título con text-2xl font-semibold
- **CardContent:** Contenido con padding sin top

**Estilos:**
- Border: border
- Background: bg-card
- Foreground: text-card-foreground
- Shadow: shadow-sm
- Radius: rounded-lg
- Padding: p-6 para header y content

##  Patrones de Diseño

### 1. Atomic Design

Estructura de componentes en capas:

**components/ui/:**
- Átomos (Button, Input, Card)
- Componentes base reutilizables
- Sin lógica de negocio

**components/layout/:**
- Moléculas (Header, Sidebar, Footer)
- Combinación de átomos
- Estructura de layout

**components/features/:**
- Organismos (KanbanBoard, TicketList)
- Componentes con lógica de negocio
- Combinación de moléculas

**components/templates/:**
- Templates (ProjectTemplate, DashboardTemplate)
- Estructuras completas de página
- Combinación de organismos

### 2. Compound Components

Componentes que trabajan juntos:

**Ejemplo: Tabs**
- **TabsList:** Contenedor de tabs
- **TabsTrigger:** Tab individual clickable
- **TabsContent:** Contenido de cada tab
- Comparten estado interno
- Accesible con ARIA

**Beneficios:**
- API declarativa
- Estado compartido implícito
- Composición flexible
- Accesibilidad integrada

### 3. Render Props Pattern

Componentes que aceptan funciones como children para máxima flexibilidad:

**Ejemplo: DataTable**
- Props: data, columns, renderRow
- renderRow es función opcional
- Permite personalizar renderizado
- Máxima flexibilidad de uso

**Uso:**
- Si renderRow existe, usa función personalizada
- Si no, usa valor directo de item[column.key]
- Permite inyección de lógica de renderizado

##  Relacionado
- [[Layouts y Navegacion|Ver Estructura de Navegación]]
- [[../01 - Arquitectura/Arquitectura General|Arquitectura del Sistema]]
