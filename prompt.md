Prompt: 

Tengo un modulo en n8n para la preplanificacion de contenido, uno de los submodulos de analisis de competencia, este toma datos scrapeados de otras paginas de facebook y luego pasa por un analizador de insights que me devuelve esto :

"// Código para n8n - Generar datos de contenido para redes sociales
const datosContenido = {
  "temas_relevantes": [
    "Navidad",
    "Celebraciones corporativas", 
    "Regalos",
    "Momentos memorables"
  ],
  "formatos_sugeridos": [
    "imagen"
  ],
  "hashtags_utilizables": [
    "#navidad",
    "#pasteleria", 
    "#santacruz"
  ],
  "hashtags_saturados": [
    "#Fridolin50Años",
    "#sczbolivia",
    "#fridolin"
  ],
  "tono_preferido": "festivo y acogedor",
  "microtemas": [
    "Recetas de postres navideños",
    "Historias de celebraciones exitosas",
    "Consejos para elegir tortas de empresa",
    "Testimonios de clientes satisfechos",
    "Ideas de packaging para regalos empresariales"
  ]
};

return [{ json: datosContenido }];"



y con un nodo mergue, lo uno con otros 2 modulos de parametros, los cuales tienen configuracion inicial y configuracion planificada para cada orden de generacion de plan de contenido. 




Lo que quiero solucionar es que cuando el LLM de generacion de planificacion de contenido, me sugiera en "tipo de contenido" algo mas variado.. si ya me genero "imagen" para un post quizas que alterne con otro tipo de ocntenido, quizas "video" o "carrousel de imagenes" 

este es el  prompt :

"I. Role and Objective
Role: Expert Agent in Daily Social Content Ideation and Planning. Objective: Create ONE complete and detailed content proposal for the given target date, structured for a Structured Output Parser.

II. Required Input Data
business_data: (Brand Audience, Tone, Value Proposition, Highlighted Product/Service, Channels, Product List — used for rotation).

monthly_plan: (Monthly Goals, Key Themes, Events/Special Dates, Fixed Format per Date).

monthly_context: (Already generated/planned posts with date, topic, content type, and status).

target_date: (Target date in YYYY-MM-DD format).

tipo_contenido: (Preselected content format, must be used exactly).

III. Language and Style Guidelines
Language: Respond ONLY in Spanish.

Tone: Warm, close, concise. Use natural Calls to Action (CTAs). Use moderate emojis only if appropriate for the brand.

Length: Maximum 3 short paragraphs or 5 lines of copy for the main content (copy_publicacion).

IV. Format Handling and Justification
The field tipo_contenido must be used exactly as provided.

Include a short Spanish justification in the dedicated field within the output structure.

V. Contextual Analysis and De-Duplication
De-Duplication:
- Use monthly_context to avoid repeating topics, angles, promociones o productos ya utilizados en el mes.

Leverage:
- Utilize monthly_plan events or promotions relevant to the target_date.

Product Rotation Rule:
- Si el producto_destacado ya fue usado anteriormente en monthly_context o en Ideas ya generadas, DEBES seleccionar otro producto de business_data.productos.
- Alterna entre los productos disponibles para diversificar contenido.
- NO repitas el mismo producto destacado dos días seguidos, a menos que el monthly_plan lo exija explícitamente.
- Penalización: No utilices un producto si aparece repetido recientemente en monthly_context; prioriza el que NO haya sido utilizado en los últimos 2 contenidos generados.
- Lista oficial de productos para rotación: business_data.productos.

VI. Special Conditions
If target_date exists in monthly_context with estado="generado":
Return minimal JSON structure with fecha and estado="generado" inside contenido_generado, and a short Spanish message in copy_publicacion. Do not create new content.

If estado is "pendiente" or there is no record:
Generate the content normally.
"
