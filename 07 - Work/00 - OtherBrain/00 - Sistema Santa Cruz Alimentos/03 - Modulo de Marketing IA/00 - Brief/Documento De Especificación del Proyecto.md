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
# 🚀 **Sistema Automatizado de Planificación, Generación y Publicación de Contenidos con IA**

**Tecnologías:** n8n · PostgreSQL · Next.js 15 · Nextcloud

---

# 1. 🎯 Objetivo del Proyecto

Construir un sistema integral que automatiza el ciclo completo de marketing de contenidos:

1.1. Configuración del negocio y análisis competitivo  
1.2. Pre-planificación con insights inteligentes  
1.3. Generación de calendarios estratégicos  
1.4. Creación de contenido multicanal (texto, guiones, imágenes IA)  
1.5. Revisión humana  
1.6. Publicación automática + análisis post-campaña

El sistema utiliza workflows en **n8n** combinados con módulos LLM que analizan, planifican y generan assets creativos.  
La intervención humana se limita a supervisión y aprobación.

---

# 2. 🧩 Arquitectura General del Sistema

El sistema se divide en **seis módulos principales**:

2.1. Configuración del Negocio + Buyer Persona + Análisis Competitivo  
2.2. Pre-Planificación Inteligente  
2.3. Generador de Calendario (LLM 1 - _El Estratega_)  
2.4. Generación de Contenido (LLM 2 - _El Copywriter_)  
2.5. Revisión Humana  
2.6. Publicación Automática

---

# 3. 📘 Módulo 1 – Configuración del Negocio + Buyer Persona + Análisis Competitivo

**Objetivo:** definir identidad del negocio, cliente ideal y mapa competitivo.

---

## 3.1. Identidad de Marca

### 3.1.1 Parámetros del Negocio

| Parámetro                         | Descripción                  |
| --------------------------------- | ---------------------------- |
| 3.1.1.1 `nombre_negocio`          | Nombre comercial             |
| 3.1.1.2 `descripcion_servicio`    | Qué ofrece                   |
| 3.1.1.3 `objetivos_comerciales`   | Metas SMART                  |
| 3.1.1.4 `propuesta_valor`         | Diferenciador único          |
| 3.1.1.5 `tono_marca`              | Voz y estilo comunicacional  |
| 3.1.1.6 `estilo_visual`           | Paleta, tipografía, estética |
| 3.1.1.7 `productos_destacados`    | Productos clave              |
| 3.1.1.8 `restricciones_contenido` | Líneas rojas de comunicación |
| 3.1.1.9 `canales_activos`         | Redes en uso                 |
| 3.1.1.10 `cta_preferidos`         | Llamados a la acción         |
| 3.1.1.11 `festivos_relevantes`    | Fechas importantes           |
| 3.1.1.12 `palabras_prohibidas`    | Términos a evitar            |
| 3.1.1.13 `zonas_cobertura`        | Alcance geográfico           |
|                                   |                              |

---

## 3.2. Buyer Persona (Cliente Ideal)

### 3.2.1 Parámetros del Buyer Persona

|Parámetro|Descripción|
|---|---|
|3.2.1.1 `nombre_buyer`|Nombre del perfil|
|3.2.1.2 `edad_rango`|Rango etario|
|3.2.1.3 `genero`|Género predominante|
|3.2.1.4 `ubicacion`|Zona geográfica|
|3.2.1.5 `ocupacion`|Profesión|
|3.2.1.6 `nivel_ingresos`|Rango económico|
|3.2.1.7 `intereses`|Temas relevantes|
|3.2.1.8 `dolores`|Problemas que quiere resolver|
|3.2.1.9 `motivaciones`|Por qué compra|
|3.2.1.10 `objeciones`|Por qué NO compraría|
|3.2.1.11 `comportamiento_digital`|Cómo consume contenido|
|3.2.1.12 `formatos_preferidos`|Reels, carrusel, etc.|
|3.2.1.13 `momentos_compra`|Situaciones que impulsan la compra|
|3.2.1.14 `beneficios_valorados`|Qué considera valor|
|3.2.1.15 `lenguaje_optimo`|Cómo se debe comunicar|

---

## 3.3. Análisis Competitivo Avanzado

|Parámetro|Descripción|
|---|---|
|3.3.1 `competencia_directa`|Lista de competidores|
|3.3.2 `scraping_automatico_posts`|Extracción automática de posts|
|3.3.3 `topics_competencia`|Temas que cubren|
|3.3.4 `formatos_exitosos_rivales`|Formatos con mejor rendimiento|
|3.3.5 `engagement_promedio`|Interacción promedio|
|3.3.6 `horarios_publicacion`|Mejores horarios|
|3.3.7 `hashtags_competitivos`|Hashtags comunes|
|3.3.8 `palabras_clave_competencia`|Keywords|
|3.3.9 `debilidades_detectadas`|Puntos débiles|
|3.3.10 `oportunidades_identificadas`|Gaps a explotar|

---

## 3.4. Analizador de Copywriting Competitivo

|Parámetro|Descripción|
|---|---|
|3.4.1 `estructuras_copy_exitosas`|Patrones útiles|
|3.4.2 `hooks_efectivos`|Ganchos|
|3.4.3 `cta_competitivos`|CTAs que convierten|
|3.4.4 `objeciones_comunes`|Objeciones trabajadas|
|3.4.5 `beneficios_destacados`|Beneficios repetidos|
|3.4.6 `patrones_narrativos`|Estilos de storytelling|
|3.4.7 `social_proof_competitivo`|Prueba social usada|

📌 **Resultado Módulo 1:** Identidad completa + Buyer Persona + Mapa competitivo almacenados en PostgreSQL.

---

# 4. 📙 Módulo 2 – Pre-Planificación Inteligente con Insights

Objetivo: generar insights accionables combinando datos internos, competencia y tendencias.

---

## 4.1. Rendimiento Histórico

|Parámetro|Descripción|
|---|---|
|4.1.1 `contenido_exitoso`|Posts con mejor rendimiento|
|4.1.2 `ctas_efectivos`|CTAs que mejor convierten|
|4.1.3 `feedback_publico`|Comentarios|
|4.1.4 `preguntas_frecuentes`|Dudas recurrentes|
|4.1.5 `metricas_engagement`|Interacción por formato|
|4.1.6 `conversion_por_formato`|Conversiones por tipo|

---

## 4.2. Tendencias en Tiempo Real

|Parámetro|Descripción|
|---|---|
|4.2.1 `tendencias_sector`|Tópicos del sector|
|4.2.2 `keywords_en_crecimiento`|SEO|
|4.2.3 `eventos_proximos`|Eventos relevantes|
|4.2.4 `noticias_impactantes`|Oportunidades noticiosas|
|4.2.5 `estacionalidades_detectadas`|Tendencias por época|
|4.2.6 `viralidad_potencial`|Temas virales|

---

## 4.3. Matriz de Oportunidades

|Parámetro|Descripción|
|---|---|
|4.3.1 `huecos_competitivos`|Lo que los rivales no cubren|
|4.3.2 `fortalezas_explotables`|Diferenciadores|
|4.3.3 `debilidades_competencia`|Puntos flojos del rival|
|4.3.4 `tendencias_alineadas_marca`|Tendencias aprovechables|
|4.3.5 `oportunidades_urgentes`|Acciones inmediatas|
|4.3.6 `riesgos_detectados`|Alertas|

---

## 4.4. Consolidación de Inteligencia (LLM 0.75)

### 4.4.1 Output unificado

|Parámetro|Descripción|
|---|---|
|4.4.1.1 `Estrategia_Diferenciacion_Central`|Eje estratégico|
|4.4.1.2 `Patron_Debilidad_Competencia`|Debilidad común|
|4.4.1.3 `Insights_Accionables_Prioritarios`|Acciones clave|
|4.4.1.4 `Tendencias_Tematicas_Competencia`|Temas recurrentes|

---

# 5. 📗 Módulo 3 – Generador del Calendario Estratégico (LLM 1)

---

## 5.1. Entradas

|Parámetro|Descripción|
|---|---|
|5.1.1 `FECHA_PLANIFICACION`|Día a planificar|
|5.1.2 `INSIGHTS_CONSOLIDADOS`|Salida M2|
|5.1.3 `CONTEXTO_NEGOCIO`|Desde M1|
|5.1.4 `objetivos_comerciales`|Metas SMART|
|5.1.5 `productos_destacados`|Prioridades|
|5.1.6 `eventos_importantes`|Fechas relevantes|
|5.1.7 `tendencias_detectadas`|Insights M2|
|5.1.8 `formatos_probados`|Históricos|
|5.1.9 `recursos_disponibles`|Equipo / presupuesto|
|5.1.10 `presupuesto_asignado`|Coste del mes|

---

## 5.2. Salidas

|Output|Descripción|
|---|---|
|5.2.1 `id_idea_base`|ID único|
|5.2.2 `tema_principal`|Idea estratégica|
|5.2.3 `microtema`|Subtema|
|5.2.4 `formato_recomendado`|Reel, carrusel, etc.|
|5.2.5 `producto_destacado`|Producto protagonista|
|5.2.6 `copy_referencia`|Línea base|
|5.2.7 `brief_visual`|Instrucciones IA|
|5.2.8 `objetivo_especifico`|Objetivo puntual|
|5.2.9 `kpis_asociados`|KPI por post|

---

# 6. 📒 Módulo 4 – Generación de Contenido (LLM 2)

---

## 6.1 Entradas

|Parámetro|Descripción|
|---|---|
|6.1.1 `IDEA_BASE_APROBADA`|Salida aprobada M3|
|6.1.2 `tema_principal`|Tema|
|6.1.3 `formato_recomendado`|Formato|
|6.1.4 `producto_destacado`|Producto|
|6.1.5 `estilo_marca`|Estética|
|6.1.6 `tono_marca`|Voz|
|6.1.7 `estrategia_contenido`|Tipo de contenido|
|6.1.8 `objetivo_especifico`|Objetivo puntual|

---

## 6.2 Salidas del Contenido Final

|Output|Descripción|
|---|---|
|6.2.1 `copy_completo`|Copy final por canal|
|6.2.2 `hashtags_optimizados`|Hashtags|
|6.2.3 `guion_video`|Script|
|6.2.4 `idea_gancho`|Hook|
|6.2.5 `ideas_visuales`|Propuestas|
|6.2.6 `imagenes_generadas`|URLs|
|6.2.7 `variantes_visuales`|Testing AB|
|6.2.8 `subtitulo_recomendado`|Carrusel|
|6.2.9 `miniaturas_video`|Thumbnails|
|6.2.10 `material_complementario`|Extras|
|6.2.11 `version_a_b`|Variantes|

---

# 7. 📘 Módulo 5 – Revisión Humana

---

## 7.1 Acciones

|Acción|Descripción|
|---|---|
|7.1.1 `aprobar_planificacion`|Aprobar idea base|
|7.1.2 `solicitar_regeneracion`|Rehacer idea base|
|7.1.3 `aprobar_contenido`|Aceptar contenido final|
|7.1.4 `solicitar_cambios`|Pedir ajustes|
|7.1.5 `pedir_regeneracion_contenido`|Regenerar|
|7.1.6 `editar_directamente`|Edición manual|
|7.1.7 `dejar_notas`|Notas estratégicas|
|7.1.8 `aprobar_lote`|Aprobación masiva|
|7.1.9 `comparar_variantes`|A/B testing|

---

# 8. 📓 Módulo 6 – Publicación Automática

---

## 8.1 Entradas

|Parámetro|Descripción|
|---|---|
|8.1.1 `contenido_final`|Contenido listo|
|8.1.2 `canales_publicacion`|Redes|
|8.1.3 `fecha_publicacion`|Día/hora óptima|
|8.1.4 `tokens_api`|Credenciales|
|8.1.5 `estrategia_publicacion`|Timing|

---

## 8.2 Salidas

|Output|Descripción|
|---|---|
|8.2.1 `registro_publicacion`|Log|
|8.2.2 `sincronizacion_nextcloud`|Backup|
|8.2.3 `metricas_iniciales`|Early Metrics|

---

# 9. 🔁 Flujo General del Sistema

1. Módulo 1: Configuración estratégica
    
2. Módulo 2: Insights + consolidación competitiva
    
3. Módulo 3: Idea base estratégica
    
4. Módulo 5: Aprobación humana
    
5. Módulo 4: Creación del contenido completo
    
6. Módulo 5: Aprobación del contenido
    
7. Módulo 6: Publicación automática
    
8. Análisis post-publicación
    

---

# 10. **Diferenciadores**

- Análisis competitivo automático
    
- Planificación estratégica autónoma
    
- Copywriting basado en datos
    
- Flujo integrado end-to-end
    
- Escalable para cientos de clientes
    

---

# 11. **Beneficios**

- Ahorro de +20h por cliente/semana
    
- Mayor rendimiento por contenido
    
- Mejor posicionamiento
    
- Flujo escalable
    
- Reporting automático
    

---

# 12. **Conclusión**

Sistema de marketing autónomo donde:

- La IA analiza, planifica y genera
    
- El humano solo supervisa
    
- Resultado: estrategia basada en datos, no intuición
    

**🚀 Valor Único: Dominio estratégico del nicho con velocidad y precisión.**


