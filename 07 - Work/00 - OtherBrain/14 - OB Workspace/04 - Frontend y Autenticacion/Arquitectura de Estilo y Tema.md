---
tags:
  - OBWorkspace
  - Frontend
  - Styling
  - Themes
---
# Arquitectura de Estilo y Tema

El diseño visual de OB-Workspace se rige por principios de minimalismo, eficiencia y alto contraste, utilizando la suite de herramientas **shadcn/ui** y **Tailwind CSS**.

## 🎨 Sistema de Diseño: B&W Minimalist

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

```css
/* app/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --background: 0 0% 100%;
  --foreground: 240 10% 3.9%;
  --card: 0 0% 100%;
  --card-foreground: 240 10% 3.9%;
  --popover: 0 0% 100%;
  --popover-foreground: 240 10% 3.9%;
  --primary: 240 5.9% 10%;
  --primary-foreground: 0 0% 98%;
  --secondary: 240 4.8% 95.9%;
  --secondary-foreground: 240 5.9% 10%;
  --muted: 240 4.8% 95.9%;
  --muted-foreground: 240 3.8% 46.1%;
  --accent: 240 4.8% 95.9%;
  --accent-foreground: 240 5.9% 10%;
  --destructive: 0 84.2% 60.2%;
  --destructive-foreground: 0 0% 98%;
  --border: 240 5.9% 90%;
  --input: 240 5.9% 90%;
  --ring: 240 5.9% 10%;
  --radius: 0.5rem;
}

.dark {
  --background: 240 10% 3.9%;
  --foreground: 0 0% 98%;
  --card: 240 10% 3.9%;
  --card-foreground: 0 0% 98%;
  --popover: 240 10% 3.9%;
  --popover-foreground: 0 0% 98%;
  --primary: 0 0% 98%;
  --primary-foreground: 240 5.9% 10%;
  --secondary: 240 3.7% 15.9%;
  --secondary-foreground: 0 0% 98%;
  --muted: 240 3.7% 15.9%;
  --muted-foreground: 240 5% 64.9%;
  --accent: 240 3.7% 15.9%;
  --accent-foreground: 0 0% 98%;
  --destructive: 0 62.8% 30.6%;
  --destructive-foreground: 0 0% 98%;
  --border: 240 3.7% 15.9%;
  --input: 240 3.7% 15.9%;
  --ring: 240 4.9% 83.9%;
}
```

### Tipografía

Inter (Google Fonts) configurada como variable de CSS para una carga optimizada:

```css
/* app/globals.css */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');

@layer base {
  :root {
    --font-sans: 'Inter', system-ui, sans-serif;
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
    font-family: var(--font-sans);
  }
}
```

## 🌗 Gestión de Tema (Light/Dark Mode)

A diferencia de implementaciones estándar que solo usan `localStorage`, OB-Workspace utiliza un sistema híbrido que prioriza las **cookies** para evitar el flash de contenido desestilizado (FOUC).

### Flujo de Implementación

1. **Server-Side Detection:** En `app/layout.tsx`, el servidor lee la cookie `theme`.
2. **Hydration:** El valor se inyecta en la clase de la etiqueta `<html>` antes de que la página llegue al cliente.
3. **Persistence:** El `ThemeProvider` escucha los cambios del `setTheme` y actualiza la cookie `document.cookie` para futuras cargas.

### Configuración Técnica

#### ThemeProvider

```typescript
// components/theme-provider.tsx
'use client'

import * as React from 'react'
import { ThemeProvider as NextThemesProvider } from 'next-themes'
import { type ThemeProviderProps } from 'next-themes/dist/types'

export function ThemeProvider({ children, ...props }: ThemeProviderProps) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

#### Layout con Server-Side Theme

```typescript
// app/layout.tsx
import { cookies } from 'next/headers'
import { ThemeProvider } from '@/components/theme-provider'

export default async function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  const cookieStore = await cookies()
  const theme = cookieStore.get('theme')?.value || 'system'

  return (
    <html lang="es" className={theme}>
      <body>
        <ThemeProvider
          attribute="class"
          defaultTheme="system"
          enableSystem
          disableTransitionOnChange
        >
          {children}
        </ThemeProvider>
      </body>
    </html>
  )
}
```

#### Mode Toggle Component

```typescript
// components/mode-toggle.tsx
'use client'

import * as React from 'react'
import { Moon, Sun } from 'lucide-react'
import { useTheme } from 'next-themes'
import { Button } from '@/components/ui/button'

export function ModeToggle() {
  const { theme, setTheme } = useTheme()
  const [mounted, setMounted] = React.useState(false)

  React.useEffect(() => {
    setMounted(true)
  }, [])

  if (!mounted) {
    return null
  }

  return (
    <Button
      variant="ghost"
      size="icon"
      onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}
    >
      <Sun className="h-5 w-5 rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
      <Moon className="absolute h-5 w-5 rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
      <span className="sr-only">Toggle theme</span>
    </Button>
  )
}
```

## 📱 Responsividad y UX

Se aplican reglas estrictas para el manejo de contenido dinámico:
- **Wrap vs Truncate:** En elementos críticos como botones de selección o diálogos, se prefiere el ajuste de línea (`whitespace-normal`) sobre el truncado para evitar pérdida de información en títulos largos.
- **Micro-interacciones:** Uso de `transition-all`, hover effects y rotaciones de iconos para dar feedback táctil al usuario sin romper la sobriedad del diseño.

## 📈 Componentes Personalizados

### Button Variants

```typescript
// components/ui/button.tsx
import * as React from 'react'
import { Slot } from '@radix-ui/react-slot'
import { cva, type VariantProps } from 'class-variance-authority'

const buttonVariants = cva(
  'inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'bg-primary text-primary-foreground hover:bg-primary/90',
        destructive: 'bg-destructive text-destructive-foreground hover:bg-destructive/90',
        outline: 'border border-input bg-background hover:bg-accent hover:text-accent-foreground',
        secondary: 'bg-secondary text-secondary-foreground hover:bg-secondary/80',
        ghost: 'hover:bg-accent hover:text-accent-foreground',
        link: 'text-primary underline-offset-4 hover:underline',
      },
      size: {
        default: 'h-10 px-4 py-2',
        sm: 'h-9 rounded-md px-3',
        lg: 'h-11 rounded-md px-8',
        icon: 'h-10 w-10',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'default',
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : 'button'
    return (
      <Comp
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = 'Button'

export { Button, buttonVariants }
```

### Card Component

```typescript
// components/ui/card.tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

const Card = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn(
      'rounded-lg border bg-card text-card-foreground shadow-sm',
      className
    )}
    {...props}
  />
))
Card.displayName = 'Card'

const CardHeader = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn('flex flex-col space-y-1.5 p-6', className)}
    {...props}
  />
))
CardHeader.displayName = 'CardHeader'

const CardTitle = React.forwardRef<
  HTMLParagraphElement,
  React.HTMLAttributes<HTMLHeadingElement>
>(({ className, ...props }, ref) => (
  <h3
    ref={ref}
    className={cn('text-2xl font-semibold leading-none tracking-tight', className)}
    {...props}
  />
))
CardTitle.displayName = 'CardTitle'

const CardContent = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div ref={ref} className={cn('p-6 pt-0', className)} {...props} />
))
CardContent.displayName = 'CardContent'

export { Card, CardHeader, CardTitle, CardContent }
```

## 📈 Patrones de Diseño

### 1. Atomic Design

Estructura de componentes en capas:

```
components/
  ui/              # Átomos (Button, Input, Card)
  layout/          # Moléculas (Header, Sidebar, Footer)
  features/        # Organismos (KanbanBoard, TicketList)
  templates/       # Templates (ProjectTemplate, DashboardTemplate)
```

### 2. Compound Components

Componentes que trabajan juntos:

```tsx
// Ejemplo: Tabs
<Tabs defaultValue="overview">
  <TabsList>
    <TabsTrigger value="overview">Overview</TabsTrigger>
    <TabsTrigger value="details">Details</TabsTrigger>
  </TabsList>
  <TabsContent value="overview">
    {/* ... */}
  </TabsContent>
  <TabsContent value="details">
    {/* ... */}
  </TabsContent>
</Tabs>
```

### 3. Render Props Pattern

Componentes que aceptan funciones como children para máxima flexibilidad:

```tsx
// components/ui/data-table.tsx
function DataTable<T>({
  data,
  columns,
  renderRow,
}: DataTableProps<T>) {
  return (
    <table>
      {data.map((item, index) => (
        <tr key={index}>
          {columns.map((column) => (
            <td key={column.key}>
              {renderRow ? renderRow(item, column) : item[column.key]}
            </td>
          ))}
        </tr>
      ))}
    </table>
  )
}
```

## 📈 Relacionado
- [[Layouts y Navegacion|Ver Estructura de Navegación]]
- [[../01 - Arquitectura/Arquitectura General|Arquitectura del Sistema]]
