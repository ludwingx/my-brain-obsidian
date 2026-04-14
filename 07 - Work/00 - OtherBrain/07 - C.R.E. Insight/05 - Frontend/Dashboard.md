---
tags:
  - C.R.E.Insight
  - frontend
  - ui
  - dashboard
---
# Dashboard y Visualizaciones

## 🧠 Enfoque de Interfaz de Usuario (UI)

El frontend se construye utilizando el framework **Next.js 15** bajo el patrón del *App Router*. La experiencia debe ser fluida, instantánea e intuitiva para los analistas de comunicación de C.R.E.

### Layout Base (`app/(dashboard)/layout.tsx`)
- **Sidebar (Panel Lateral):** Oscuro o con acentos del color corporativo de C.R.E. Enlaces a *Overview*, *Posts de la Fanpage*, *Monitor de Crisis (Menciones Externas)* y *Configuración*.
- **TopBar:** Contiene el *Avatar del usuario*, buscador general (por palabra clave en posts) y la **campana de notificaciones** (indicador visual rojo cuando existen alertas de riesgo alto por revisar).

### Vistas Críticas
#### 1. Overview (Pantalla Principal)
- **KPI Cards (Tarjetas superiores):**
  - *Engagement Total del Mes* y un indicador de crecimiento (%) comparado al mes anterior.
  - *Comentarios Negativos Críticos* (conteo directo bloqueante que llama a la acción).
- **Gráfica Principal (AreaChart de Tremor o Recharts):** Evolución de la interacción diaria, sumando la tracción lograda día a día.

#### 2. Radar de Crisis (Menciones Externas)
- Utiliza **Shadcn/UI DataTable**.
- Filtros potentes por *Plataforma* (TikTok, FB), por *Nivel de Riesgo* (Alto, Medio) y por estado (Pendiente, Resuelto).
- **Acción Rápida:** Botón desplegable que abre el texto íntegro del comentario, el link directo a la red social y permite al operador marcar la alerta como "Atendida".

## 🔗 Relacionado
- [[Componentes UI|Guía de Sistemas de Diseño y Componentes]]
- [[../04 - Backend_API/Endpoints de la API|Endpoints Internos usados en esta vista]]
