# 🧱 **BRIEF TÉCNICO – SISTEMA MONOLITO NEXT.JS 16 PARA SCENT DUO**

## **1. Objetivo del sistema**

Construir un sistema monolito en **Next.js 16** que tenga:

1. **Landing e-commerce pública** (catálogo, información, contacto).
    
2. **Módulo de autenticación oculto** (ruta no visible en menú, accesible sólo por URL directa o atajo).
    
3. **Panel administrativo privado** accesible solo para el dueño del sistema (gestión completa del negocio).
    
4. **Base de datos centralizada** para catálogo, pedidos, inventario, usuarios, etc.
    
5. **APIs internas con Next.js App Router**.
    
6. **Seguridad y escalabilidad**, manteniendo simplicidad.
    

---

# **2. Arquitectura general**

### **Tecnologías principales**

- **Next.js 16** (App Router, Server Actions, RSC)
    
- **TypeScript**
    
- **Prisma ORM**
    
- **PostgreSQL** o **MySQL**
    
- **Auth.js** (antes NextAuth) o JWT + middleware
    
- **TailwindCSS** + UI library opcional (Shadcn, MUI)
    
- **Renderizado híbrido** (SSR + SSG)
    

---

# **3. Estructura de carpetas**

`src/  ├─ app/  │   ├─ (public)/           # Landing y ecommerce  │   │    ├─ page.tsx  │   │    ├─ productos/  │   │    ├─ contacto/  │   │    └─ nosotros/  │   ├─ (auth)/  │   │    └─ login/         # Login oculto  │   ├─ (admin)/            # Panel privado  │   │    ├─ dashboard/  │   │    ├─ productos/  │   │    ├─ pedidos/  │   │    ├─ ajustes/  │   │    └─ usuarios/  │   ├─ api/                # APIs del sistema  │   └─ layout.tsx  ├─ components/  ├─ lib/  ├─ prisma/  └─ types/`

---

# **4. Rutas del sistema**

## **4.1. Rutas públicas (landing e-commerce)**

**/ /** → Landing principal  
**/productos** → Catálogo  
**/productos/[id]** → Ficha de producto  
**/ofertas**  
**/sobre-nosotros**  
**/contacto**  
**/carrito** _(opcional, si implementas carrito real)_

---

## **4.2. Ruta de login (oculta)**

**/admin-login** → Página de login

- No aparece en menú
    
- Acceso sólo por URL
    
- Protección extra:
    
    - Rate limiting
        
    - Captcha invisible
        
    - Doble factor opcional
        

---

## **4.3. Rutas privadas (panel administrativo)**

_Protegidas por middleware_

**/admin/dashboard**  
**/admin/productos**  
**/admin/productos/nuevo**  
**/admin/pedidos**  
**/admin/inventario**  
**/admin/estadisticas**  
**/admin/usuarios**  
**/admin/ajustes**

---

# **5. Módulos funcionales**

## **5.1. Landing / e-commerce**

- Catálogo general
    
- Filtros (marca, género, precio)
    
- Buscador
    
- Página de producto con:
    
    - Fotos
        
    - Descripción
        
    - Notas olfativas
        
    - Precio
        
    - Tamaños disponibles (full size / decants)
        
    - Botón “Consultar por WhatsApp”
        
- Testimonios
    
- Información del negocio (Scent Duo)
    
- Información de métodos de pago
    
- Información de entregas
    
- Formularios de contacto
    

---

## **5.2. Autenticación (login oculto)**

- Login con email + contraseña
    
- Opcional: magic link o 2FA
    
- Protección del panel mediante middleware
    
- Sesión en cookies seguras
    

---

## **5.3. Panel administrativo**

Incluye los módulos:

### **A. Gestión de productos**

- Crear / editar / eliminar
    
- Fotos múltiples
    
- Precio
    
- Stock
    
- Notas olfativas
    
- Categoría
    
- Estado: publicado / no publicado
    

---

### **B. Gestión de inventario**

- Entrada de stock
    
- Salida de stock
    
- Alertas por bajo inventario
    
- Histórico de movimientos
    

---

### **C. Gestión de pedidos**

_(Si implementas un sistema de pedidos propio)_

- Seguimiento de pedidos
    
- Estado del pedido (nuevo, pagado, enviado, entregado)
    
- Cliente asignado
    
- Historial de ventas
    
- Descarga de PDF de pedidos
    

---

### **D. Usuarios (opcional)**

- Crear usuarios administrativos adicionales
    
- Roles: admin / editor / viewer
    

---

### **E. Estadísticas**

- Productos más vendidos
    
- Ingresos estimados
    
- Pedidos por día
    
- Stock general
    
- Compras realizadas
    
- Dashboard con gráficos
    

---

### **F. Ajustes del sistema**

- Información del negocio
    
- Redes sociales
    
- Teléfonos
    
- Logo, colores y marca
    
- Configuración del correo
    
- Configuración de WhatsApp API (opcional)
    

---

# **6. Base de datos (modelo principal Prisma)**

### **Product**

- id
    
- name
    
- description
    
- price
    
- brand
    
- category
    
- notes
    
- size (50, 80, 100, 125 ml)
    
- images[]
    
- stock
    
- isPublished
    
- createdAt
    
- updatedAt
    

---

### **InventoryMovement**

- id
    
- productId
    
- type (entrada / salida)
    
- quantity
    
- description
    
- userId
    
- createdAt
    

---

### **Order** (opcional si lo implementas)

- id
    
- customerName
    
- customerPhone
    
- items[]
    
- total
    
- status
    
- createdAt
    

---

### **User**

- id
    
- name
    
- email
    
- passwordHash
    
- role
    
- createdAt
    

---

# **7. Seguridad**

- Middleware de protección para rutas admin
    
- Login oculto (no indexado, no visible)
    
- Cookies httpOnly
    
- JWT cifrado o Auth.js con sessions
    
- Rate limit en login
    
- CAPTCHA invisible
    
- Sanitización de inputs
    
- Prevención de subida de archivos maliciosos
    

---

# **8. UI/UX del sistema**

- Landing minimalista
    
- Destacar perfumes premium en portada
    
- Paleta sobria (negro / dorado / beige)
    
- Diseño mobile-first
    
- Admin panel estilo dashboard
    
- Sidebar fijo
    
- Tabla con acciones
    
- Formularios limpios
    

---

# **9. Flujo del usuario**

### **Visitante**

1. Entra a landing
    
2. Navega catálogo
    
3. Ve productos
    
4. Llega a WhatsApp para comprar
    
5. (Opcional) Ve carrito
    

### **Dueño del sistema**

1. Entra a /admin-login
    
2. Accede al panel admin
    
3. Crea productos
    
4. Administra inventario
    
5. Gestiona pedidos
    
6. Mira estadísticas
    

---

# **10. Entregables**

- Código completo Next.js 16
    
- Base de datos Prisma
    
- Autenticación funcional
    
- CRUD completo
    
- APIs internas
    
- Panel admin responsive
    
- Landing e-commerce optimizada
    
- Documentación de endpoints
    
- Seed inicial de productos