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
    - Opción de crear **Orden de Comentarios**.
    - Opción de **Rechazar** (en caso de error inicial).
- **Vista de Rechazadas**:
    - Listado de publicaciones descartadas.
    - Opción de **Aprobar** (para recuperar publicaciones rechazadas por error).

### RF-4: Generación de Órdenes de Comentarios
- El sistema permite generar órdenes solo sobre publicaciones **Aprobadas**.
- El usuario ingresa instrucciones (notas) para la IA.
- La IA genera comentarios únicos basados en el objetivo (Apoyar/Criticar).

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
