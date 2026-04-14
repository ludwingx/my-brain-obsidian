# 🎯 WORKFLOW 1: ANALIZADOR DE COMPETENCIA Y TENDENCIAS

## 🔍 **SCOPE EXCLUSIVO:** Solo scraping y análisis externo (sin datos del negocio aún)

## 🔄 **WORKFLOW COMPLETO - SOLO ANALIZADOR**

### **NODO 1: Scraper Competencia Directa**
**Frecuencia:** Cada 24 horas
**Fuentes:** Instagram, Facebook competencia local pastelerías SCZ
```json
{
  "competidores": ["DulceTentacionSCZ", "PasteleriaMiaSCZ", "TortasSanMiguel"],
  "datos_extraer": {
    "posts_ultimos_3_dias": true,
    "engagement_metrics": ["likes", "comentarios", "shares", "vistas"],
    "hashtags_usados": "extraer_y_contar",
    "horarios_publicacion": "analizar_patrones",
    "tipo_contenido": ["imagen", "video", "reel", "carousel"],
    "copy_publicacion": "analizar_tono_y_estructura"
  }
}
```

### **NODO 2: Scraper Tendencias Sociales**
**Frecuencia:** Cada 12 horas  
**Fuentes:**
- Twitter Trends Bolivia
- Google Trends (keywords: "pasteles", "tortas", "postres Santa Cruz")
- TikTok hashtags trends Bolivia
```json
{
  "parametros_tendencias": {
    "ubicacion": "Bolivia, Santa Cruz",
    "categorias_relevantes": ["comida", "postres", "eventos sociales"],
    "ventana_tiempo": "ultimas_48_horas",
    "minimo_volumen": 100
  }
}
```

### **NODO 3: Scraper Eventos Locales SCZ**
**Frecuencia:** Cada 6 horas
**Fuentes:**
- Página municipal Santa Cruz
- Ferias y eventos confirmados
- Festividades locales
```json
{
  "tipos_eventos": {
    "feriados_oficiales": true,
    "ferias_locales": true,
    "eventos_municipales": true,
    "festividades_populares": true
  }
}
```

### **NODO 4: Scraper Clima/Estacionalidad**
**Frecuencia:** Cada 24 horas
```json
{
  "datos_clima": {
    "pronostico_5_dias": true,
    "condiciones_especiales": ["lluvia_fuerte", "ola_calor", "frio_extremo"],
    "temperaturas_extremas": true
  }
}
```

## 🗃️ **TABLAS ERP PARA ALMACENAMIENTO**

### **Tabla 1: `scraping_competencia`**
```sql
CREATE TABLE scraping_competencia (
  id SERIAL PRIMARY KEY,
  fecha_scraping TIMESTAMP,
  competidor_nombre VARCHAR(100),
  plataforma VARCHAR(50),
  tipo_contenido VARCHAR(50),
  contenido_texto TEXT,
  hashtags JSONB,
  engagement_metrics JSONB,
  horario_publicacion TIME,
  url_publicacion VARCHAR(500)
);
```

### **Tabla 2: `scraping_tendencias`**
```sql
CREATE TABLE scraping_tendencias (
  id SERIAL PRIMARY KEY,
  fecha_scraping TIMESTAMP,
  fuente VARCHAR(100),
  tendencia VARCHAR(200),
  volumen INTEGER,
  crecimiento_24h DECIMAL,
  categoria VARCHAR(100),
  ubicacion VARCHAR(100),
  relevancia_comida INTEGER -- 1-10
);
```

### **Tabla 3: `scraping_eventos`**
```sql
CREATE TABLE scraping_eventos (
  id SERIAL PRIMARY KEY,
  fecha_deteccion TIMESTAMP,
  nombre_evento VARCHAR(200),
  fecha_evento DATE,
  ubicacion VARCHAR(100),
  tipo_evento VARCHAR(100),
  confirmado BOOLEAN,
  fuente VARCHAR(100),
  impacto_estimado VARCHAR(50) -- alto, medio, bajo
);
```

### **Tabla 4: `scraping_clima`**
```sql
CREATE TABLE scraping_clima (
  id SERIAL PRIMARY KEY,
  fecha_pronostico DATE,
  fecha_scraping TIMESTAMP,
  condiciones VARCHAR(100),
  temperatura_max INTEGER,
  temperatura_min INTEGER,
  condiciones_especiales JSONB,
  recomendaciones_ventas VARCHAR(200)
);
```

## 📊 **NODO 5: ANALIZADOR DE PATRONES (n8n)**

### **Análisis de Competencia:**
```json
{
  "patrones_detectados": {
    "horarios_optimos": ["17:00-19:00", "12:00-14:00"],
    "tipo_contenido_popular": ["reels", "videos_tutorial"],
    "hashtags_efectivos": ["#pasteleriaSCZ", "#tortasbolivia"],
    "brechas_identificadas": ["falta_contenido_educativo", "poco_uso_stories"],
    "frecuencia_publicacion_optima": "1-2 posts_dia"
  }
}
```

### **Análisis de Tendencias:**
```json
{
  "tendencias_aplicables": {
    "hashtags_trending": ["#LunesDePostres", #MiércolesDulce"],
    "temas_virales_locales": ["feria_emprendedores_octubre"],
    "oportunidades_contenido": ["tortas_tematicas_clima", "postres_lluvia"]
  }
}
```

### **Análisis de Eventos:**
```json
{
  "eventos_proximos_7_dias": [
    {
      "evento": "Feria Emprendedores",
      "fecha": "2024-10-25",
      "oportunidad_contenido": "tortas_tematicas_feria",
      "relevancia": "alta"
    }
  ]
}
```

## 🎯 **OUTPUT FINAL DEL WORKFLOW**

### **Tabla: `analisis_externo_consolidado`**
```sql
CREATE TABLE analisis_externo_consolidado (
  id SERIAL PRIMARY KEY,
  fecha_analisis TIMESTAMP,
  insights_competencia JSONB,
  tendencias_aplicables JSONB,
  eventos_proximos JSONB,
  factores_climaticos JSONB,
  recomendaciones_generales JSONB
);
```

**Ejemplo de registro:**
```json
{
  "fecha_analisis": "2024-10-15 08:00:00",
  "insights_competencia": {
    "horario_pico_engagement": "17:30",
    "contenido_mas_exitoso": "videos_proceso_elaboracion",
    "hashtags_top_3": ["#pasteleriacreativa", "#tortaspersonalizadas", "#santacruzbolivia"]
  },
  "tendencias_aplicables": {
    "hashtags_trending": ["#MiercolesDeAntojos", "#ComidaBoliviana"],
    "temas_populares": ["postres_lluvia", "meriendas_calientes"]
  },
  "eventos_proximos": [
    {"evento": "Feria Municipal", "fecha": "2024-10-20", "oportunidad": "alta"}
  ],
  "factores_climaticos": {
    "pronostico": "lluvias_moderadas",
    "recomendacion": "contenido_para_quedarse_en_casa"
  }
}
```

## ⚙️ **CONFIGURACIÓN PROGRAMACIÓN**

| Componente | Frecuencia | Hora Recomendada |
|------------|------------|------------------|
| **Scraper Competencia** | 24h | 08:00 AM |
| **Scraper Tendencias** | 12h | 08:00 AM / 20:00 PM |
| **Scraper Eventos** | 6h | 06:00, 12:00, 18:00, 00:00 |
| **Scraper Clima** | 24h | 07:00 AM |
| **Analizador Patrones** | 24h | 09:00 AM |
