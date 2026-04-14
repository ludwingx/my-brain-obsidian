### 🧩 Escenario 1: Cliente **tiene pedido pendiente**

- **¿Qué significa?** El cliente ya hizo un pedido, pero aún no se ha enviado ni asignado dirección.
    
- **Acción:** Asociar la ubicación como **dirección de entrega** del pedido pendiente.
    
- **Respuesta:**
    
    > "Gracias por compartir tu ubicación 📍. Ya la hemos asignado a tu pedido pendiente. ¡En breve te confirmamos el envío! 🚚"
    

---

### 🧩 Escenario 2: Cliente **no tiene pedido** y está **consultando sucursales**

- **¿Qué significa?** El cliente probablemente quiere saber cuál sucursal le queda más cerca.
    
- **Acción:** Usar la lat/lng para calcular la sucursal más cercana.
    
- **Respuesta:**
    
    > "Con base en tu ubicación, la sucursal más cercana es _Sucursal XYZ_, a 1.2 km. ¿Te gustaría hacer un pedido ahí? 🍰"
    

---

### 🧩 Escenario 3: Cliente **no tiene pedido** pero **quiere delivery**

- **¿Qué significa?** El cliente envió ubicación como primer paso para saber si hacen delivery o iniciar uno.
    
- **Acción:** Verificar si hay cobertura en esa zona.
    
- **Respuesta:**
    
    - Si hay cobertura:
        
        > "¡Sí, hacemos entregas en esa zona! ¿Qué te gustaría pedir hoy? 🛵"
        
    - Si no:
        
        > "Lo sentimos 😞, aún no llegamos a esa zona. Podés pasar por nuestra sucursal más cercana: _Sucursal ABC_."
        

---

### 🧩 Escenario 4: Cliente **ya mandó ubicación antes**

- **¿Qué significa?** Repetido, o puede ser un error.
    
- **Acción:** Validar si es diferente a la anterior.
    
- **Respuesta:**
    
    > "Ya teníamos tu ubicación registrada. ¿Querés actualizarla o mantener la anterior?"
    

---

### 🧩 Escenario 5: Ubicación sin contexto claro

- **¿Qué significa?** El cliente solo manda ubicación sin decir nada más y sin historial.
    
- **Acción:** Pedir aclaración.
    
- **Respuesta:**
    
    > "Recibimos tu ubicación. ¿Querés saber qué sucursal está cerca o estás haciendo un pedido para delivery? 📦"


graph TD
     
     A[Webhook: locationMessage] --> B[Verificar si hay pedido pendiente]
    
    B -- Sí --> C[Guardar como dirección de entrega]
    
    B -- No --> D[Verificar si ya existe ubicación previa]
    D -- Sí --> E[Pedir confirmación de cambio]
    D -- No --> F[Pedir intención: Delivery o Sucursal]
    F --> G[Mostrar opciones]
