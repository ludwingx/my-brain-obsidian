# 🧩 Componentes UI

> [[Market GS]] > Componentes UI

---

## Componentes de Aplicación

Componentes propios del negocio en `src/components/`:

| Componente            | Archivo                   | Descripción                                        |
|-----------------------|---------------------------|----------------------------------------------------|
| `AppSidebar`          | `app-sidebar.tsx`         | Sidebar principal con navegación                   |
| `NavMain`             | `nav-main.tsx`            | Menú de navegación principal                       |
| `NavProjects`         | `nav-projects.tsx`        | Navegación de proyectos                            |
| `NavUser`             | `nav-user.tsx`            | Info del usuario en sidebar (dropdown)             |
| `CompanyHeader`       | `company-header.tsx`      | Logo y nombre de la empresa en sidebar             |
| `TeamSwitcher`        | `team-switcher.tsx`       | Selector de equipo/empresa                         |
| `ProductForm`         | `product-form.tsx`        | Formulario completo de producto (35KB)             |
| `ModelCombobox`       | `model-combobox.tsx`      | Combobox de búsqueda para modelos de teléfono      |
| `ColorPickerModal`    | `color-picker-modal.tsx`  | Modal para seleccionar/crear colores               |
| `LoginForm`           | `login-form.tsx`          | Formulario de inicio de sesión                     |
| `RegisterForm`        | `register-form.tsx`       | Formulario de registro de usuario                  |
| `ModeToggle`          | `mode-toggle.tsx`         | Toggle de tema claro/oscuro                        |
| `Footer`              | `footer.tsx`              | Pie de página                                      |

---

## Componentes Base (shadcn/ui)

En `src/components/ui/`, 35 componentes basados en Radix UI:

### Layout
`card` · `separator` · `sidebar` · `sheet` · `collapsible`

### Formularios
`button` · `input` · `textarea` · `label` · `checkbox` · `switch` · `select` · `form` · `popover` · `command`

### Feedback
`alert` · `alert-dialog` · `badge` · `progress` · `skeleton` · `spinner` · `toast` · `toaster` · `tooltip`

### Datos
`table` · `avatar` · `breadcrumb`

### Navegación
`dialog` · `dropdown-menu`

### Personalizados
`color-selector` · `delete-modal` · `restore-modal` · `metric-card` · `quick-action-card` · `activity-item`
