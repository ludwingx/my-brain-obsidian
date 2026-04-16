---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Negocio
  - Modulos
---
# Módulos de Gestión de Negocio

##  Más allá del Código: Visión Empresarial

**OB Workspace** no es solo para programadores; es la herramienta de control total para el **CEO** de la agencia.

###  Módulo de Finanzas (`/finances`)

#### Funcionalidades Core

**Control de Gastos**
- **Gastos Fijos:** Registro de costos operativos mensuales (SaaS, Servidores, Licencias, Oficina)
- **Gastos Variables:** Pagos a colaboradores por hora, comisiones, bonos
- **Gastos por Proyecto:** Asignación específica de gastos a cada cliente
- **Recurring Expenses:** Gestión de suscripciones con alertas de renovación

**Relación Gasto-Proyecto**
- **Rentabilidad Real:** Comparación entre presupuesto asignado (`Project.budget`) vs gastos acumulados (`Expenses`)
- **Margen de Beneficio:** Cálculo automático de (Ingresos - Gastos) / Ingresos
- **Alertas de Presupuesto:** Notificaciones cuando gastos > 80% del presupuesto
- **Forecasting:** Proyección de gastos basada en tendencias históricas

**Suscripciones y Servicios**
- **Tracking Recurrente:** Monitoreo de servicios mensuales/anuales
- **Alertas de Renovación:** Notificaciones 30 días antes de vencimiento
- **Cost Optimization:** Identificación de servicios subutilizados
- **Vendor Management:** Centralización de proveedores y contratos

#### Casos de Uso

**Caso 1: Análisis de Rentabilidad por Cliente**
El CEO necesita saber qué proyectos son más rentables:
1. Navegar a `/finances/projects`
2. Ver tabla con: Proyecto | Presupuesto | Gastos | Margen | Estado
3. Ordenar por Margen descendente
4. Identificar proyectos con margen < 15% y tomar acción

**Caso 2: Control de Gastos Operativos**
El CEO quiere reducir costos fijos:
1. Revisar `/finances/expenses?type=fixed`
2. Identificar servicios con uso < 20%
3. Cancelar o downgrade de suscripciones
4. Proyectar ahorro anual

###  Módulo de Analytics (`/analytics`)

#### Métricas y KPIs

**Velocidad del Equipo**
- **Tickets Completados/Semana:** Gráfico de barras por desarrollador
- **Story Points Resueltos:** Puntos de historia completados por sprint
- **Throughput:** Tasa de entrega de tickets (tickets/día)
- **Cycle Time:** Tiempo promedio desde TODO hasta DONE

**Burn-down Chart Técnico**
- **Tiempo Real vs Estimado:** Comparación de `realTime` vs `estimatedTime` por ticket
- **Desviación de Presupuesto:** % de diferencia entre tiempo estimado y real
- **Tendencia de Estimación:** Precisión de estimaciones de IA/PM over time
- **Alertas de Desviación:** Tickets con realTime > estimatedTime * 1.3

**Eficiencia por Desarrollador**
- **Horas Productivas:** Tiempo registrado en WorkSessions activas
- **Horas Totales:** Tiempo total disponible (40h/semana)
- **Productivity Ratio:** Horas productivas / Horas totales
- **Ticket Quality:** Promedio de tickets completados sin rework

#### Dashboards

**Dashboard CEO (Privado)**
- **Overview:** Resumen de todos los proyectos activos
- **Financial Health:** Ingresos vs Gastos por mes
- **Team Performance:** Métricas agregadas del equipo
- **Risk Alerts:** Tickets estancados, presupuestos excedidos

**Dashboard Developer**
- **My Work:** Mis tickets asignados y progreso
- **Time Tracking:** Resumen de mis WorkSessions
- **Performance:** Mis métricas personales vs promedio del equipo
- **Suggestions:** IA recommendations para mejorar eficiencia

###  Gestión de Tickets y Kanban (`/kanban`)

#### Funcionalidades

**Vista Fluida**
- **Drag & Drop:** Movimiento intuitivo de tickets entre columnas
- **Real-time Updates:** Sincronización instantánea entre usuarios
- **Filtros Dinámicos:** Por estado, prioridad, asignado, módulo
- **Search:** Búsqueda instantánea por título, descripción, tags

**Filtros por Módulo**
- **Isolación de Contexto:** Ver solo tickets de módulo específico
- **Cross-Module View:** Vista global de todo el proyecto
- **Dependency Tracking:** Visualizar dependencias entre tickets
- **Bulk Actions:** Mover múltiples tickets simultáneamente

#### Estados del Kanban

**Columnas Principales**
- **TODO:** Tickets pendientes de comenzar
- **IN PROGRESS:** Tickets actualmente en desarrollo
- **IN REVIEW:** Tickets listos para code review
- **DONE:** Tickets completados y verificados
- **BLOCKED:** Tickets con bloqueos externos

**Estados Adicionales**
- **BACKLOG:** Tickets futuros no priorizados
- **ON HOLD:** Tickets temporalmente pausados
- **CANCELLED:** Tickets cancelados por cliente o equipo

###  Módulo de Time Tracking (`/tracking`)

#### Funcionalidades

**Timer Flotante**
- **Start/Stop:** Control de tiempo de WorkSession activa
- **Auto-Pause:** Pausa automática al cerrar navegador
- **Session History:** Registro de todas las sesiones
- **Export:** Descarga de reportes en CSV/PDF

**Reportes Detallados**
- **Por Ticket:** Tiempo acumulado por ticket y subtarea
- **Por Desarrollador:** Horas trabajadas por persona
- **Por Proyecto:** Distribución de tiempo por cliente
- **Por Período:** Reportes semanales/mensuales/anuales

#### Casos de Uso

**Caso 1: Facturación por Horas**
El CEO necesita facturar a un cliente por horas trabajadas:
1. Navegar a `/tracking/reports?project=XYZ&period=month`
2. Ver resumen de horas por ticket y desarrollador
3. Generar invoice detallado
4. Enviar a cliente con desglose de actividades

**Caso 2: Análisis de Productividad**
El CEO quiere identificar desarrolladores menos productivos:
1. Revisar `/tracking/performance?period=quarter`
2. Comparar horas productivas vs horas totales por desarrollador
3. Identificar patrones y tomar acciones correctivas

##  Relacionado
- [[../../02 - Base de Datos/Modelos Prisma|Ver Modelos Expense y WorkSession]]
- [[../../04 - Frontend y Autenticacion/Layouts y Navegacion|Interfaz y Dashboard]]
- [[../../03 - Inteligencia Artificial/IA Flow - Del Chat al Ticket|IA Assistant para Tickets]]
- [[../../05 - Backend/Server Actions|Acciones de Servidor para Finanzas]]
