---
tags:
  - Protocolo
  - Facebook
  - Marketing
  - Identidad
  - Seguridad
---

# 🛡️ PROTOCOLO FARM-C

---

## 📋 Resumen
Protocolo completo para la creación y calentamiento de cuentas de Facebook con seguridad extrema. Incluye gestión de identidad, fotos generadas, y comportamiento humano.

---

#### **FASE 0: Preparación del Entorno (Infraestructura)**

1. **Hardening de Dispositivos (NO - Limpieza):**
    - Desactivar servicios de ubicación inicialmente (se activarán estratégicamente después).
    - Idioma y región: Configurarlos según la IP/LTE que vayas a usar. Si la LTE es de México, el celular debe estar en español (México) y zona horaria de México.
2. **Gestión de LTE vs WiFi:**
    - **Regla de Oro:** El registro _siempre_ se hace con la LTE de la línea asignada. El algoritmo detecta que el número de teléfono coincide con la IP de la operadora (Carrier NAT), lo cual aumenta la confianza ("Trust Score") masivamente.
    - **Proxies/Compartidos:** Una vez verificada la cuenta (día 2 o 3), se puede migrar al MB compartido, pero _nunca_ en el momento del registro.

#### **FASE 1: Identidad y Huella Digital (El "Sello Humano")**


- 2.**Generación de Identidad:**

- Usar generadores de identidad falsa o el Sheet que tenemos de personas con numeros (para nombre, DNI  para recordar datos, fecha nacimiento).
- **Protocolo de Foto:**

1. **Protocolo de Foto:**
    
    - **Prompt 1 (Boceto):** Usar foto real como base o descripción. Generar imagen principal (rostro, fondo neutro). Asegurar que la IA cambie rasgos clave si se usó una base real.
        
    - **Prompt 2 (Evolución):** Usar la imagen generada como base para crear fotos de contexto (persona en un café, en un parque, etc.).
        
    - **Limpieza:** Eliminar todos los metadatos EXIF de las imágenes generadas antes de subirlas.

- 1. **Huella Digital del Navegador/App:**
    - Los celulares de 4GB RAM y 2 Core son de gama baja. Los algoritmos saben esto. No intentes simular un iPhone 15 en un Android de gama baja.
    - **User Agent:** Mantén el navegador o la App de Facebook nativa actualizada a la última versión que soporte el hardware.

#### **FASE 2: Creación de Gmail (El Cimiento)**

**Tiempo estimado por cuenta:** 30 - 40 minutos.

1. **Acción:** Crear cuenta Google.
2. **Input:** Nombre y Apellido de la identidad creada.
3. **Verificación:** Usar el número LTE del chip insertado.
4. **Perfil:**
    - Subir la foto de perfil generada (ya tratada sin metadatos).
    - **NO** activar inmediatamente todos los servicios de Google (News, Pay, Drive).
    - **Acción Humana:** Abrir YouTube. Buscar un video de música o noticias locales (del país de la IP). Verlo completo (3-5 min). Darle "Like". Suscribirse a un canal genérico (Música, Deportes).
    - **Objetivo:** Google y Facebook comparten datos. Si Google ve actividad en YouTube, le da más confianza al Gmail.
5. **Seguridad:** Guardar los códigos de respaldo en un Excel offline (no en la nube todavía).

#### **FASE 3: Creación y Calentamiento de Facebook (El Objetivo)**

**Tiempo estimado por cuenta:** 30 - 45 minutos (Día 1).

1. **Instalación:** Descargar Facebook y Messenger desde la Play Store (usando la cuenta Gmail recién creada). Esto vincula el "Google Advertising ID" del dispositivo con Facebook.
2. **Registro:**
    - Seleccionar "Registrarse con correo electrónico".
    - Poner el Gmail creado.
    - Fecha de nacimiento: Coherente con la identidad (mayor de 21 años para evitar restricciones de edad).
    - Nombre: Real.
3. **Verificación:** Facebook pedirá el número. Poner el LTE. Ingresar el código.
4. **Perfilado Inmediato:**
    - Subir foto de perfil (generada con Protocolo de Imagen).
    - Subir foto de portada (Cover Photo). _Consejo: Que sea genérica, paisajes, un equipo de fútbol, etc. Evita fotos de personas en portada al inicio._
    - **Añadir "Acerca de":** Ciudad actual, ciudad natal. NO poner lugar de trabajo ni estudios el primer día (se ve automatizado).
5. **Acciones de "Calentamiento en Frío" (Día 1):**
    - Agregar a la cuenta de "Control" (tu cuenta personal o una cuenta madre) como amigo. Aceptar.
    - Unirse a **2 grupos máximos**. Que sean grupos grandes y locales (Ej: "Compra/Venta Usados [Ciudad]", "Fans de [Equipo Fútbol]").
    - **Scroll:** Navegar en el Feed por 10 - 15 minutos. Reaccionar (Me gusta/Me encanta) a 3 o 4 publicaciones aleatorias. NO compartir nada. NO comentar todavía.

### **CRONOGRAMA SEMANAL DE IMPLEMENTACIÓN**

Para cumplir con el objetivo de 20-35 cuentas priorizando **seguridad extrema**, no podemos crear las 20 el mismo día. Eso es un "spike" (pico) de actividad sospechoso. Usaremos un sistema de **Oleadas**.

**Lógica de Tiempos:**

- **Setup + Gmail:** 25 min.
- **Setup Facebook:** 35 min.
- **Calentamiento Inicial:** 30 min.
- **Total por unidad:** ~1:30 minutos de atención humana activa + tiempo muerto.

#### **Propuesta de Calendario (1 Operador Humano):**

- **Lunes (Día de Siembra - Lote 1):**
    
    - Creación de **5 cuentas** completas (Gmail + FB + Calentamiento inicial).
    - Tiempo invertido: ~7.5 horas.
    - _Riesgo:_ Bajo.
- **Martes (Cuidado Lote 1 + Siembra Lote 2):**
    
    - Mantenimiento Lote 1: 10 min por cuenta (Revisar notificaciones, hacer scroll, ver historias). Total: 50 min.
    - Creación de **5 cuentas** nuevas (Lote 2).
    - Tiempo invertido: ~8.4 horas.
- **Miércoles (Cuidado Lotes 1-2 + Siembra Lote 3):**
    
    - Mantenimiento Lotes anteriores: 1.5 horas.
    - Creación de **5 cuentas** nuevas (Lote 3).
    - Tiempo invertido: ~9.4 horas.
- **Jueves (Cuidado Lotes 1-3 + Siembra Lote 4):**
    
    - Mantenimiento Lotes anteriores: ~2 horas.
    - Creación de **5 cuentas** nuevas (Lote 4).
    - Tiempo invertido: ~10.5 horas. _Aquí empieza la fatiga del operador._
- **Viernes (Consolidación):**
    
    - NO crear cuentas nuevas. Priorizar seguridad de las 20 creadas.
    - Actividad intensiva: Publicar una foto real en cada cuenta (foto de comida, paisaje callejero, mascota). Nada de links ni ventas.
    - Interactuar en grupos.
- **Sábado y Domingo (Farming Pasivo):**
    
    - Conexión diaria breve (5-10 min por cuenta). Scroll, likes, contestar mensajes si los hay.

### **PROTOCOLO DE SEGURIDAD ADICIONAL (Indetectable)**

Para pasar de "cuenta creada" a "cuenta real", implementa estas reglas:

1. **La Regla del GPS:** No actives el GPS los primeros 3 días. El algoritmo suele asociar "Cuenta nueva + GPS activado pidiendo permisos" con bots de granja.
    - _Día 4:_ Activar GPS, abrir Google Maps, trazar una ruta (simulando que buscas dirección), y cerrar. Luego desactivar.
2. **Apps de Terceros:** No instales apps raras. Ten las básicas: WhatsApp, Instagram, tiktok (opcional pero recomendado vincular), Spotify.
    - Abre Spotify (aunque sea la versión free) y reproduce una playlist. Facebook detecta que escuchas música y te considera un humano real.
3. **Cambio de Red (The Switch):**
    - Día 1-3: Solo LTE (Datos móviles).
    - Día 4 en adelante: Alternar. 80% LTE, 20% WiFi compartido. El cambio repentino total de IP (pasar de LTE a WiFi fijo para siempre) dispara alarmas de "robo de cuenta".
 4. **Comportamiento en YouTube (Crucial):**
    
    - Antes de crear Facebook, **obligatoriamente** ver 1 video completo en YouTube con la cuenta de Google.
    - Si ambos operadores ponen el mismo video (ej. una canción famosa), Facebook lo detecta.
    - **Protocolo:** Cada cuenta debe ver un video diferente (uno noticias, otro música rock, otro música pop, otro video de recetas). Esto define el algoritmo de publicidad del celular.

---
## 📊 Métricas Clave
- **Tiempo por cuenta:** 1.5 - 2 horas (setup + calentamiento)
- **Oleadas:** 5 cuentas por día (máximo)
- **Ratio seguridad:** 20-35 cuentas por operador/semana
- **Trust Score:** Alto (LTE + comportamiento humano)
