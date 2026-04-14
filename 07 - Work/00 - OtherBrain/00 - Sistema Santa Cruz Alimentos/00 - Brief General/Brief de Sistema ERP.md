---
created:
  - 27-11-2025
tags:
  - Project/
  - n8n
  - NextJS
  - fullstack
  - postgreSQL
  - appweb
  - polocruz
---

# 📑 Brief de Sistema ERP - Pastelería y Panadería

## 1. Objetivo del Sistema
Centralizar la gestión de dos negocios (pastelería y panadería) en un solo ERP, optimizando **ventas, inventario, producción, compras y finanzas**, con reportes unificados o separados según cada negocio.  
Además, integrar un **sistema automatizado de marketing** para la planificación y publicación de contenidos en redes sociales.

---

## 2. Stack Tecnológico
- **Frontend**: Next.js 15  
- **Backend**: Node.js  
- **Base de datos**: PostgreSQL  
- **Autenticación**: JWT + Roles  
- **Infraestructura**: VPS Contabo con Easypanel  
- **Automatización**: n8n  
- **Integración externa**: Evolution API (WhatsApp)  

---

## 3. Usuarios y Roles
- **Administrador General**: acceso total.  
- **Encargado de Negocio**: gestiona solo su negocio (pastelería o panadería).  
- **Vendedor/Cajero**: registra ventas y gestiona pedidos.  
- **Producción**: controla recetas, lotes y stock de materias primas.  
- **Contabilidad**: acceso a reportes financieros.  
- **Marketing**: gestiona y aprueba planes de contenido.  

---

## 4. Módulos Principales

### Ventas
- POS con Delivery, Retiro en tienda y Consumo en local.  
- Órdenes personalizadas (ej: tortas especiales).  
- Control de clientes (CRM básico).  
- **Agente de Ventas de Tortas Express**: automatización en n8n que gestiona pedidos por WhatsApp, envía cotizaciones, genera QR de pago y confirma pedidos.  

### Inventario
- Stock de productos terminados y materias primas.  
- Control por negocio (pastelería/panadería).  

### Producción
- Recetario digital con fórmulas.  
- Consumo automático de insumos.  
- Control de lotes y desperdicios.  

### Compras y Proveedores
- Registro de pedidos a proveedores.  
- Entrada de insumos al inventario.  

### Finanzas
- Ingresos por ventas.  
- Egresos por compras y gastos.  
- Reportes por negocio y consolidado.  

### Reportes y Dashboard
- Ventas diarias, semanales y mensuales.  
- Inventario actual y alertas de stock bajo.  
- Rentabilidad por negocio.  

### Marketing Automatizado
- **Gestión de plan de contenidos**: creación automática de propuestas con IA.  
- **Flujo de aprobación**: aprobar o rechazar propuestas antes de generar el contenido y después de generar el contenido.  
- **Automatización de publicaciones**: distribución de contenidos en redes sociales según calendario aprobado.  

---
## 6. Flujo General del Sistema
1. Cliente hace pedido por WhatsApp → Evolution API lo recibe → n8n procesa la información → pedido registrado en ERP.  
2. ERP muestra pedidos en frontend (Next.js).  
3. Backend (Node.js) gestiona lógica de negocio y registra en PostgreSQL.  
4. Módulos operativos procesan ventas, producción, inventario y finanzas.  
5. Marketing automatizado genera propuestas de contenido con IA, se aprueban/rechazan y se publican en redes sociales.  
6. Reportes permiten ver métricas individuales o consolidadas.  

---

## 7. Consideraciones Clave
- **Multi-negocio**: separación de datos por pastelería y panadería.  
- **Escalabilidad**: backend modular con Node.js + PostgreSQL.  
- **Automatización**: pedidos por WhatsApp y marketing integrados con n8n y Evolution API.  
- **Seguridad**: JWT y control de roles.  
- **Visibilidad completa**: ERP como centro único para operaciones y marketing digital.  

[[00.- Sistema de Marketing IA]]
