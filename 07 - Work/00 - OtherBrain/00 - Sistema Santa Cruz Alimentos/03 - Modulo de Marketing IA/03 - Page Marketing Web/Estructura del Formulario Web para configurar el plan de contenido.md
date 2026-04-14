# 📋 Formulario Web para Configuración de Plan de Contenido

---

## 🏢 SECCIÓN 1: INFORMACIÓN DEL NEGOCIO

### Datos Básicos del Negocio

- **Nombre del Negocio** → `Input texto`  
  _Ejemplo: Torta Express_

- **Eslogan** → `Input texto`  
  _Ejemplo: El mejor sabor, al mejor precio_

- **Ubicación** → `Textarea (área de texto grande)`  
  _Ejemplo: Zona Norte, Radial 26 y 5to. anillo, calle #2, Santa Cruz de la Sierra, Bolivia_

- **Zonas de Entrega** → `Multi-select o checkboxes`  
  - Zona Norte  
  - Plan 3000  
  - Centro  
  - Otro: `[Input texto]`

### Información de Contacto

- **Teléfono** → `Input teléfono`  
  _Ejemplo: 62013533_

- **WhatsApp** → `Input teléfono`  
  _Ejemplo: 59162013533_

- **Instagram** → `Input texto`  
  _Ejemplo: @tortaexpress.sc_

- **Facebook** → `Input texto`  
  _Ejemplo: TortaExpressSantaCruz_

- **Sitio Web** → `Input URL`  
  _Ejemplo: (dejar vacío si no tiene)_

### Descripción del Negocio

- **Descripción Larga** → `Textarea grande`  
  _Ejemplo: Somos una pastelería familiar con 10 años de endulzar los momentos especiales..._

- **Propuesta de Valor Única** → `Multi-select con opción "Otro"`  
  - Entrega en 2 horas  
  - Personalización gratuita de mensajes  
  - Ingredientes 100% frescos  
  - Otro: `[Input texto]`

---

## 👥 SECCIÓN 2: AUDIENCIA Y TONO

### Público Objetivo

- **Selección de Audiencia** → `Checkboxes múltiples`  
  - Familias de clase media  
  - Jóvenes profesionales (25–35 años)  
  - Empresas que celebran cumpleaños  
  - Estudiantes universitarios  
  - Otro: `[Input texto]`

### Problemas que Resuelve

- **Dolores del Cliente** → `Checkboxes + opción personalizada`  
  - Falta de tiempo para hornear  
  - Necesidad de postre para visita sorpresa  
  - Buscar torta personalizada económica  
  - Urgencia de entrega rápida  
  - Otro: `[Input texto]`

### Estilo de Comunicación

- **Tono y Estilo** → `Selector con opciones predefinidas`  
  - ○ Cercano y amigable (como hablar con un vecino)  
  - ○ Profesional pero accesible  
  - ○ Divertido y juvenil  
  - ○ Elegante y sofisticado  
  - ○ Personalizar: `[Textarea]`

- **Uso de Emojis** → `Selector`  
  - ○ Moderado (2–3 por post)  
  - ○ Abundante (4+ emojis)  
  - ○ Mínimo (solo cuando sea necesario)  
  - ○ Sin emojis

---

## 🎯 SECCIÓN 3: OBJETIVOS Y PERIODO

### Periodo del Plan

- **Fecha de Inicio** → `Selector de fecha`  
  _Ejemplo: 01/10/25_

- **Fecha de Fin** → `Selector de fecha`  
  _Ejemplo: 31/10/25_

### Metas del Contenido

- **Objetivos Principales** → `Checkboxes + prioridad`  
  - Aumentar reconocimiento de marca (Prioridad: Alto/Medio/Bajo)  
  - Incrementar ventas de producto específico (¿Cuál? `[Select]`)  
  - Fidelizar clientes existentes  
  - Generar leads nuevos  
  - Otro: `[Input texto]`

---

## 🎂 SECCIÓN 4: PRODUCTOS A PROMOCIONAR

### Producto 1

- **Nombre del Producto** → `Input texto`  
  _Ejemplo: Torta de Chocolate Intenso_

- **Descripción** → `Textarea`  
  _Ejemplo: Capas de bizcocho de chocolate húmedo, relleno de ganache..._

- **Público Ideal** → `Selector`  
  - ○ Amantes del chocolate  
  - ○ Familias  
  - ○ Jóvenes  
  - ○ Regalos para hombres/mujeres  
  - ○ Personalizar: `[Input]`

- **Precio Promedio** → `Input número`  
  _Ejemplo: 90 Bs._

- `[Botón: + Agregar otro producto]`

---

## 📅 SECCIÓN 5: CONTEXTO Y PLATAFORMAS

### Eventos Relevantes

- **Selección de Eventos** → `Checkboxes con fechas`  
  - Mes de la Primavera (Septiembre en Bolivia)  
  - Halloween (31 Octubre)  
  - Día de la Madre (27 Octubre Bolivia)  
  - Navidad (Diciembre)  
  - Otro evento: `[Input texto + fecha]`

### Plataformas Disponibles

- **Redes Sociales** → `Checkboxes`  
  - Facebook  
  - Instagram  
  - TikTok  
  - Twitter/X  
  - LinkedIn

### Restricciones

- **Limitaciones** → `Multi-select`  
  - Presupuesto limitado  
  - Equipo pequeño  
  - Tiempo limitado para creación de contenido  
  - Falta de material fotográfico  
  - Otro: `[Input texto]`

---

## 📝 SECCIÓN 6: REFERENCIAS Y EJEMPLOS

### Ejemplo de Contenido Exitoso

- **Post que Funcionó** → `Textarea grande`  
  _Ejemplo: ¡Hola Santa Cruz! 🎂 ¿Cumpleaños sorpresa y sin torta?..._

### Contenidos a Evitar

- **Qué NO Hacer** → `Checkboxes`  
  - Publicaciones muy largas  
  - Fotos de baja calidad  
  - Promociones agresivas  
  - Lenguaje muy técnico  
  - Falta de llamados a la acción  
  - Otro: `[Input texto]`

---

## 🔄 FLUJO DEL FORMULARIO

1. Paso 1: Información del Negocio (obligatorio)  
2. Paso 2: Audiencia y Tono (obligatorio)  
3. Paso 3: Objetivos y Periodo (obligatorio)  
4. Paso 4: Productos (obligatorio al menos 1)  
5. Paso 5: Contexto (opcional pero recomendado)  
6. Paso 6: Referencias (opcional)

- `[Botón Final: GENERAR PLAN DE CONTENIDO]`

---

## 📤 RESULTADO ESPERADO

Al enviar el formulario, se genera un JSON estructurado con los valores ingresados por el usuario, listo para ser procesado por el webhook de n8n y enviado al **Agente 1 (Planificador)**.
