---
title: Requisitos del Sistema
nav_order: 1
parent: TeloComento
---

# Especificación de Requisitos de Software (SRS)

[⬅ Volver al Inicio](../TeloComento.md) | [Siguiente: Modelo de Datos ➡](../02%20-%20Arquitectura/Modelo%20de%20Datos.md)

## 1. Requisitos Funcionales (RF)

### RF-1: Gestión de Búsquedas (Configuración Inicial)
- Al registrarse por primera vez, el sistema debe obligar al usuario a configurar sus **Formatos de Búsqueda**.
- Cada usuario puede tener un **máximo de 3 formatos de búsqueda**.
- Cada formato de búsqueda consta de:
    - **Palabra Clave**: El medio (ej. El Deber), hashtag o tema.
    - **Causa/Persona/Tema**: El sujeto específico (ej. Luis Fernando Camacho).
- El sistema debe generar variantes de estas búsquedas para que el scraper externo amplíe la cobertura (ej. combinar "medio + causa", "causa + adjetivo", etc.).

### RF-2: Ingesta y Clasificación (Vista "Tinder")
- El sistema recibirá publicaciones del scraper externo basadas en los formatos de búsqueda.
- **Vista de Aprobación (Tipo Tinder)**: El usuario debe ver las publicaciones una por una para decidir rápidamente:
    - **Aprobar**: La publicación pasa al pool de "Aprobadas".
    - **Rechazar**: La publicación pasa al pool de "Rechazadas".
- La IA clasifica el sentimiento (`positivo`, `neutra`, `negativa`) antes de mostrar la publicación en esta vista.

### RF-3: Gestión de Publicaciones (Aprobadas/Rechazadas)
- **Vista de Aprobadas**:
    - Listado de publicaciones que el usuario aceptó.
    - **Acción: Crear Orden**: Botón directo para iniciar la generación de comentarios.
    - Opción de **Rechazar** (en caso de error inicial).
- **Vista de Rechazadas**:
    - Listado de publicaciones descartadas.
    - Opción de **Aprobar** (para recuperar publicaciones rechazadas por error).

### RF-4: Generación de Órdenes de Comentarios (Flujo IA)
- El sistema permite generar órdenes solo sobre publicaciones **Aprobadas**.
- **Configuración de la Orden**:
    - **Objetivo**: Seleccionar si se generarán comentarios `Positivos` (apoyo) o `Negativos` (crítica).
    - **Notas/Instrucciones**: Campo de texto para que el usuario guíe a la IA (ej. "enfócate en el tema de la salud").
- **Ejecución con OpenRouter**:
    - Al enviar, el sistema consume la API de OpenRouter enviando: `Contenido de la publicación` + `Notas del usuario` + `Objetivo`.
    - La IA genera una cantidad de comentarios únicos basada en el **Límite de Bots** del usuario.
- **Límites de Usuario**:
    - Cada usuario tiene un **Límite de Bots** (por defecto 40) que define cuántos comentarios se generan por orden.
    - Cada usuario tiene un **Límite de Órdenes** totales que puede crear.
- **Acceso a Resultados**: Desde la publicación aprobada o el feed de órdenes, se puede navegar para ver los comentarios generados.

### RF-5: Monitoreo y Bots
- Separación de pools de bots por sentimiento (Positivo/Negativo).
- API para que bots externos consuman órdenes activas.
- Trazabilidad de estados y tiempos de ejecución.

---

## 2. Requisitos No Funcionales (RNF)

### RNF-1: UX/UI Dinámica
- La interfaz de aprobación debe ser fluida y optimizada para decisiones rápidas (Swipe/Buttons).

### RNF-2: Restricciones de Usuario
- El límite de 3 búsquedas debe ser estricto a nivel de base de datos y backend.

### RNF-3: Arquitectura Escalable
- Preparada para futuras acciones (Like, Share) y nuevos formatos de búsqueda (Twitter, Instagram, etc.).
