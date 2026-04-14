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
# BRIEF COMPLETO – SISTEMA DE AUTOMATIZACIÓN DE CONTENIDO (N8N + IA + Next.js + PostgreSQL)

Este brief está **totalmente reescrito**, profesional, estructurado y ajustado a tu flujo real.

---

# ⚙️ **BRIEF GENERAL DEL SISTEMA**

# 🧭 **Sistema de Planificación, Generación y Publicación de Contenidos con IA**

**Tecnologías:**  
n8n · PostgreSQL · EasyPanel · Nextcloud · Next.js 15 · Agentes Multimodales IA

---

# 🎯 **Objetivo del sistema**

Automatizar todo el ciclo de marketing digital de un negocio:

1. **Configuración del negocio (frontend + PostgreSQL)**
    
2. **Pre–planificación automática usando scraping + análisis + agentes IA**
    
3. **Generación inteligente del calendario de contenidos**
    
4. **Generación de textos, guiones e imágenes**
    
5. **Aprobación humana**
    
6. **Autoposteo en redes sociales**
    

Todo desde un frontend intuitivo y fácil de usar.

---

---

# 🧩 **Arquitectura del Sistema (Resumen técnico)**

## **Frontend (Next.js 15)**

- Formulario de configuración del negocio
    
- Dashboard para revisar planificación
    
- Aprobación/rechazo de ideas
    
- Gestor de órdenes de planificación
    
- Vista de contenido generado e imágenes
    
- Conexión a API interna vía server actions + middleware
    

---

## **Backend / Orquestación (n8n)**

- Workflow 1: Preplanificación
    
- Workflow 2: Scraping (competencia + tendencias)
    
- Workflow 3: Generador de planificación
    
- Workflow 4: Generador de contenido
    
- Workflow 5: Generador de imágenes
    
- Workflow 6: Publicación automática
    

---

## **Base de datos (PostgreSQL)**

Tablas principales:

- negocios
    
- configuraciones_negocio
    
- productos
    
- objetivos
    
- ordenes_planificacion
    
- planificaciones_generadas
    
- contenido_generado
    
- imagenes_generadas
    
- logs_publicacion
    
- scraping_competencia
    
- tendencias_scrapeadas
    

---

## **Storage (Nextcloud)**

- Biblioteca de imágenes del negocio
    
- Imagenes generadas por IA
    
- Material de referencia
    
- Backups del contenido aprobado
    

---

---

# 🤖 **AGENTES DEL SISTEMA**

## 🧠 **Agente 0 – Contextualizador del negocio (PRE–PLANIFICACIÓN)**

**Rol:** Entender profundamente el negocio para permitir una planificación inteligente.

### Datos de entrada:

- Nombre del negocio
    
- Identidad de marca
    
- Tono de comunicación
    
- Buyer persona
    
- Propuesta de valor
    
- Productos estrella
    
- Problemas que resuelve
    
- Redes sociales actuales
    
- Contenido que mejor funcionó antes
    
- Competencia (scrapeada)
    
- Tendencias del sector
    

👉 **Todo guardado en PostgreSQL**

---

---

## 🧠 **Agente 1 – Planificador Estratégico (GENERA EL CALENDARIO)**

**Rol:** Crear el “calendario inteligente” del mes o semana.  
**Tareas:**

- Tomar rango de fechas de la “orden de planificación”
    
- Evitar repetición de temas
    
- Tomar en cuenta festivos nacionales (BDD propia)
    
- Elegir formatos recomendados (reels, carrusel, imagen, story)
    
- Proponer productos usados en cada día
    
- Combinar objetivos + insights de competencia
    
- Crear estructura JSON de planificación diaria
    

---

---

## 🧠 **Agente 2 – Generador de Contenido**

**Rol:** Generar texto listo para usar.  
**Incluye:**

- Copy de publicación
    
- CTA
    
- Hashtags optimizados
    
- Tono definido
    
- Guion de video (si aplica)
    
- Idea de ganchos de 3 segundos
    
- Guion de tomas (si es reel)
    
- Idea musical
    
- Subtítulos sugeridos
    

👉 Exactamente como tu output demo.

---

---

## 🧠 **Agente 3 – Evaluador y Revisor**

**Rol:** Revisar cada contenido antes de aprobar o producir multimedia.  
Evalúa:

- Coherencia
    
- Tono
    
- Objetivos de negocio
    
- Evitar repetición
    
- Calidad del copy
    

Marca:

- aprobado
    
- requiere cambios
    
- rechazado
    

---

---

## 🧠 **Agente 4 – Generador de Imágenes / Videos**

**Rol:** Tomar el contenido aprobado y generar:

- Imagen
    
- Miniaturas
    
- Material visual complementario
    
- Variantes según estilo del negocio
    

Usa Nextcloud para almacenar outputs.

---

---

## 🧠 **Agente 5 – Publicador Automático**

**Rol:** Publicar según calendario.

- Instagram
    
- Facebook
    
- TikTok (si agregas)
    
- WhatsApp Business (opcional)
    

Guarda logs y confirma ejecución.

---

---

# 🧭 FASES DEL FLUJO COMPLETO

## 1️⃣ Configuración del negocio (Frontend → PostgreSQL)

Cliente ingresa todo:  
✔ Datos básicos  
✔ Identidad  
✔ Buyer persona  
✔ Objetivos  
✔ Productos  
✔ Horarios  
✔ Promociones  
✔ Tono  
✔ Palabras prohibidas  
✔ Competencia para scraping  
✔ Festivos relevantes  
✔ Frecuencia de publicación

---

## 2️⃣ Pre–planificación (n8n)

Tareas automáticas:

- Scraping de competencia
    
- Scraping de tendencias
    
- Análisis del agente insights
    
- Normalización en base de datos
    
- Generación de reporte pre-planificación
    
- Variables que afectarán la planificación final
    

---

## 3️⃣ Orden de planificación (Frontend → n8n)

Cliente elige:

- Fechas
    
- Productos destacados
    
- Objetivos del período
    
- Presupuesto (opcional)
    
- Promociones del mes
    

n8n recibe la orden y lanza el Agente Planificador.

---

## 4️⃣ Planificación generada (Agente 1 → BD → Frontend)

El sistema genera el calendario y lo muestra.

Cliente: aprobar / pedir correcciones.

---

## 5️⃣ Generación de contenido (Agente 2 + Evaluador)

Para cada día aprobado:

- Se genera el contenido
    
- Se revisa
    
- Se modifica
    
- Se aprueba
    

---

## 6️⃣ Generación de imágenes (Agente 4)

Solo si formato requiere imágenes.

Se guarda en Nextcloud.

---

## 7️⃣ Publicación automática (Agente 5)

Publica en fecha programada.  
Guarda logs.  
Genera informe.