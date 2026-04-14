---
tags:
  - C.R.E.Insight
  - arquitectura
  - flujograma
  - n8n
---
# Flujo General del Sistema C.R.E. Insight

## 🧠 Descripción del Flujo
El flujo del sistema se basa en un paradigma de captación de datos periódicos (ETL) automatizado mediante flujos de trabajo en **n8n** y presentados en un dashboard **Next.js**.

## 🔄 1. Flujo de Ingesta de Datos (Scraping)
1. **Trigger Cronizado:** n8n se ejecuta en intervalos definidos (ej. cada hora o diariamente).
2. **Extracción (Extract):** n8n orquesta llamadas a los **Apify Actors** (o scripts locales) para recolectar información de la Fanpage oficial y menciones externas de C.R.E en Meta/TikTok.
3. **Transformación & Análisis (Transform):**
   - El payload recibido en JSON es evaluado por un nodo de Inteligencia Artificial (LLM) o un sistema basado en palabras clave para detectar **sentimiento negativo** asociado a la publicación o comentario.
   - Limpieza de datos (normalización de fechas, mapeo de engagement: Likes, Shares, Comments).
4. **Carga (Load):** Inserción en la base de datos central PostgreSQL a través del ORM o un endpoint local.

## 👥 2. Flujo de Usuario (Frontend)
1. **Autenticación:** El operador de C.R.E inicia sesión en la plataforma web (Next.js) mediada por JWT o sesión segura.
2. **Vista General (Dashboard):** El backend extrae un resumen del engagement de los últimos 7 días.
3. **Alertas en Tiempo Real:** Las publicaciones catalogadas como de "Riesgo Alto" (comentarios negativos o quejas) aparecen destacadas en un panel lateral.
4. **Respuesta Rápida:** El operador revisa el link original de la queja y puede marcar el ticket como `resuelto` o `ignorado` en la BD.

## 🔗 Relacionado
- [[ArquitecturaGeneral|Volver a Arquitectura General]]
- [[../02 - Scraping/Flujo General en n8n|Ver a detalle el flujo de n8n]]
