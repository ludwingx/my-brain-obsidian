# Módulos y Herramientas de OB Universe

A nivel de interfaz y arquitectura de portales, el OB Universe estructura las distintas utilidades de la empresa en un **Catálogo de Utilidades** (`/tools`).

## Categorías y Módulos Expuestos

Actualmente, el sistema reconoce y cataloga las siguientes aplicaciones como parte de la suite interna:

| Módulo | Tipo (Tag) | Descripción de su Finalidad | Estado |
| :--- | :--- | :--- | :--- |
| **Inventario** | PRODUCCIÓN | Control de stock y trazabilidad de los productos físicos en tiempo real. | `Online` |
| **Ventas** | ANÁLISIS | Visualización de métricas comerciales orientadas al rendimiento global de la empresa. | `Online` |
| **Sistemas** | IT | Herramienta crítica para monitorear la salud de los servidores e infraestructura. | `Mantenimiento` |
| **Reportes** | REPORTING | Automatiza la generación de informes consolidados exportables a PDF o Excel. | `Online` |
| **Consola** | ADMIN | Centro de control de acceso para configurar permisos globales y de cada usuario. | `Online` |
| **Mobile** | OPERATIVO | Funcionalidades de campo, orientadas a la app operativa con capacidades offline para zonas sin cobertura. | `Online` |

## Arquitectura de Navegación Lateral (Sidebar)
Para una fácil interconexión entre la vista macro (el Dashboard de OB Universe) y la vista micro (las apps particulares), la interfaz emplea una barra de navegación que divide los recursos en:

1. **Jerarquías de Datos ("Nav Main")**: Aplicaciones, Reportes, y Configuración.
2. **Contextos de Proyecto ("Nav Projects")**: Rutas rápidas que apuntan a proyectos top-level como Infraestructura, Logística y Analytics.
3. **Contexto Organizacional ("Team Switcher")**: Permite al administrador cambiar de organización (Ej: De la 'Internal Suite' de OtherBrain a posibles subsidiarias o áreas estancas).

---
🔙 **Principal:** [[OB-Universe]]
