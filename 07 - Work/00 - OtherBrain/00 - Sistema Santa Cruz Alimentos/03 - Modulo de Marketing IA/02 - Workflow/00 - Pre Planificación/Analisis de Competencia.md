### 🧠 Agente 2: _Sintetizador de insights para contenido_

![[Pasted image 20251120092253.png]]
#### 📌 Función principal:

Convertir el análisis de publicaciones (Agente 1) en un resumen modular que:

- Identifique _temas dominantes_ y _emociones clave_
    
- Detecte _formatos recurrentes_ (imagen, video, carrusel)
    
- Extraiga _hashtags útiles_ y _evite saturados_
    
- Proponga _microtemas_ o _ángulos nuevos_ para el plan de contenido
    

### 🧩 Output ideal para alimentar al generador de plan de contenidos

json

```
{
  "temas_relevantes": ["Navidad corporativa", "Regalos emocionales", "Postres personalizados"],
  "formatos_sugeridos": ["imagen", "carrusel", "reel"],
  "hashtags_utilizables": ["#regaloscorporativos", "#dulcefinaldeano", "#navidadempresarial"],
  "hashtags_saturados": ["#navidad", "#pasteleria", "#sczbolivia"],
  "tono_preferido": "festivo, emocional, institucional",
  "microtemas": [
    "Cómo agradecer a tu equipo con detalles dulces",
    "Ideas de regalos para clientes VIP",
    "Postres que cuentan historias familiares"
  ]
}
```

### 🧠 Prompt base para este agente sintetizador

markdown

```
Actuá como un sintetizador estratégico de contenido. Recibís un conjunto de publicaciones analizadas (texto, hashtags, tipo, tono, interpretación visual). Tu tarea es extraer los temas dominantes, formatos más usados, hashtags útiles y saturados, tono emocional predominante, y proponer microtemas diferenciados que puedan alimentar un plan de contenido mensual. No generes contenido final, solo insights modulares listos para alimentar otro agente.
```

### 🧵 Flujo modular sugerido en n8n

mermaid

```
graph TD
A[Scraping o análisis de publicaciones] --> B[Agente 1: Analizador]
B --> C[Agente 2: Sintetizador de insights]
C --> D[Agente 3: Generador de plan de contenido por fechas]
```
