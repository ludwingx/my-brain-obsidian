[⬅ Volver al Inicio](../TeloComento.md)

# TeloComento — ¿Cómo funciona?

> Plataforma de monitoreo y respuesta automatizada en redes sociales.

---

## 1. Configura qué quieres monitorear

El usuario crea **Tarjetas de Monitoreo**, donde indica qué temas le interesan. Por ejemplo:
- "El Deber"
- "Evo Morales"
- "Alcaldía de Santa Cruz"

Opcionalmente puede agregar un contexto como: *"Solo publicaciones sobre política, ignorar deportes"*.

Cada usuario tiene un límite de tarjetas asignado por el administrador.

---

## 2. El sistema busca publicaciones automáticamente

Un servicio externo (scraper) busca constantemente en Facebook usando las palabras clave de todas las tarjetas activas.

Cuando encuentra una publicación relevante, la registra en el sistema con:
- El enlace al post original.
- La imagen/miniatura.
- El nombre del autor.
- El texto de la publicación.

Todas las publicaciones encontradas llegan al panel del usuario como **pendientes de revisión**.

---

## 3. El usuario decide qué hacer con cada publicación

En la sección de **Revisión**, el usuario ve las publicaciones una por una (estilo Tinder) y decide:

- ❌ **Rechazar** → La publicación se descarta.
- ✅ **Aprobar** → Se abre un formulario para configurar la respuesta.

---

## 4. Se genera una orden de comentarios con IA

Al aprobar una publicación, el usuario elige:

- **Apoyar (+)** o **Criticar (-)**: Define el tono de los comentarios.
- **Instrucciones** (opcional): Indicaciones específicas, como *"Mencionar que es una gran iniciativa"* o *"Cuestionar la fuente de los datos"*.

Con esa información, la **Inteligencia Artificial genera automáticamente 3 comentarios únicos** que suenan naturales y humanos, listos para ser publicados.

---

## 5. Los bots publican los comentarios

Un grupo de bots externos toman las órdenes activas y publican los comentarios generados directamente en la publicación de Facebook correspondiente.

Cada comentario publicado se registra con fecha y hora. Cuando todos los comentarios de una orden se publican exitosamente, la orden se marca como completada.

---

## 6. Panel de administración

Los administradores pueden:
- Activar o desactivar usuarios.
- Controlar cuántas tarjetas de monitoreo puede crear cada usuario.
- Ver todas las publicaciones de todos los usuarios en una vista global.

---

## Resumen Visual

```
🎯 Usuario configura palabras clave
        ↓
🔍 Scraper busca publicaciones en Facebook
        ↓
📥 Las publicaciones llegan al panel del usuario
        ↓
👆 El usuario aprueba o rechaza cada una
        ↓
🤖 La IA genera comentarios personalizados
        ↓
💬 Los bots publican los comentarios en Facebook
        ↓
✅ Misión cumplida
```

---

[[TeloComento]]
