# Especificación de Requisitos de Software (SRS)

## 1. Requisitos Funcionales (RF)

### RF-1: Gestión de Campañas
- El sistema debe permitir a los usuarios crear, leer, actualizar y eliminar (CRUD) campañas.
- Cada campaña debe estar asociada a una **palabra clave** (ej. El Deber) y una **persona/causa/tema** (ej. Luis Fernando Camacho).
- El usuario debe poder definir las **fuentes** (páginas de redes sociales) a monitorear por campaña.
- El usuario debe poder activar/desactivar el monitoreo en tiempo real.

### RF-2: Ingesta y Clasificación de Publicaciones
- El sistema debe recibir publicaciones desde un scraper externo.
- Cada publicación debe ser clasificada automáticamente por sentimiento (`positivo`, `neutro`, `negativo`) utilizando IA.
- El sistema debe permitir la revisión manual de publicaciones (aprobar/rechazar).
- El sistema debe permitir la edición manual del sentimiento detectado.

### RF-3: Generación de Órdenes de Comentarios
- El sistema debe permitir crear una **Orden de Comentarios** a partir de una publicación aprobada.
- El usuario debe poder ingresar instrucciones adicionales (notas) para guiar la generación de la IA.
- La IA debe generar múltiples comentarios únicos (sin duplicados entre sí) basados en el sentimiento objetivo de la orden.

### RF-4: Gestión y Asignación de Bots
- El sistema debe separar estrictamente los pools de bots:
    - Bots para sentimientos positivos (apoyo).
    - Bots para sentimientos negativos (crítica).
- El sistema debe exponer una interfaz (API/Cola) para que los bots externos consuman las órdenes activas.

### RF-5: Monitoreo y Seguimiento
- El sistema debe mostrar el progreso de ejecución de las órdenes en tiempo real.
- Se debe registrar la trazabilidad completa: fecha de creación, fecha de generación de comentarios y fecha en que el bot efectivamente comentó.

---

## 2. Requisitos No Funcionales (RNF)

### RNF-1: Escalabilidad
- La arquitectura debe permitir agregar nuevas acciones en el futuro (Likes, Compartir) sin afectar el núcleo del sistema.

### RNF-2: Seguridad
- Autenticación y autorización robusta para el manejo multiusuario.
- Almacenamiento seguro de llaves de API (OpenRouter) mediante variables de entorno.

### RNF-3: Rendimiento y Disponibilidad
- El sistema debe ser capaz de procesar múltiples campañas en paralelo sin degradar la experiencia de usuario.
- Interfaz reactiva y rápida (Next.js + shadcn/ui).

### RNF-4: Observabilidad
- Implementación de estados claros (`borrador`, `generado`, `activado`, `en_progreso`, `finalizado`, `error`) para facilitar el debugging y soporte.
