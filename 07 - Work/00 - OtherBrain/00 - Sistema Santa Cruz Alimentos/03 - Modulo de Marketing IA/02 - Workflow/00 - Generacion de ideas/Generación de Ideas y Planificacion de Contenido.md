
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
# 🧩 WorkFlow #1: Generación de Ideas y Planificación de Contenido

## 🎯 Propósito del Módulo

Este flujo se activa desde el módulo **Marketing** del ERP. Antes de iniciar el generador de ideas y el planificador de contenido con IA, es obligatorio completar:

- La **Etapa 1**, que configura la **marca** y su **información permanente**.
- La **Etapa 2**, que define los **objetivos mensuales**, eventos clave y ajustes operativos.

Estas dos etapas funcionan como **input estructurado** para el modelo de lenguaje (LLM) que se activa en n8n.

![[Pasted image 20250926133053.png]]

---

## 🧪 Etapas del Flujo

- [[Etapa 1|Configuración de Marca]]
- [[Etapa 2|Activación de Generador IA]]
##  Json merge: 

```
[
  {
    "id": 1,
    "negocio_id": "TORTA_EXPRESS_001",
    "nombre_negocio": "Torta Express",
    "eslogan": "El mejor sabor, al mejor precio",
    "ubicacion": "Zona Norte, Radial 26 y 5to. anillo, calle #2, Santa Cruz de la Sierra, Bolivia",
    "zonas_entrega": [
      "Zona Norte",
      "Plan 3000",
      "Centro"
    ],
    "contacto": {
      "telefono": "62013533",
      "whatsapp": "59162013533",
      "instagram": "@tortaexpress.sc",
      "facebook": "TortaExpressSantaCruz",
      "web": ""
    },
    "descripcion_larga": "Somos una pastelería familiar con 10 años de endulzar los momentos especiales de Santa Cruz. Nos especializamos en tortas personalizadas para cumpleaños, eventos y aniversarios, usando ingredientes frescos y de la más alta calidad.",
    "propuesta_valor": [
      "Entrega en 40 minutos",
      "Personalización gratuita de mensajes",
      "Ingredientes 100% frescos"
    ],
    "audiencia": [
      "Familias de clase media",
      "Jóvenes profesionales (25-35 años)",
      "Empresas que celebran cumpleaños"
    ],
    "dolores_cliente": [
      "Falta de tiempo para hornear",
      "Necesidad de un postre de calidad para una visita sorpresa",
      "Buscar una torta personalizada que no sea excesivamente cara"
    ],
    "tono_comunicacion": "Cercano, alegre y confiable. Usar un lenguaje cálido como si hablaras con un vecino.",
    "uso_emojis": "Moderado (2-3 por post)",
    "productos": [
      {
        "nombre": "Torta de Chocolate Intenso",
        "descripcion": "Capas de bizcocho de chocolate húmedo, relleno de ganache y cubierta de chocolate semi-amargo.",
        "publico_ideal": "Amantes del chocolate puro, regalo para hombres.",
        "precio_promedio": "90 Bs."
      },
      {
        "nombre": "Torta de Tres Leches Clásica",
        "descripcion": "Esponjosa, bañada en la mezcla de tres leches y coronada con merengue italiano.",
        "publico_ideal": "Para quienes buscan lo tradicional, familias.",
        "precio_promedio": "80 Bs."
      },
      {
        "nombre": "Torta de Oreo",
        "descripcion": "Bizcocho de vainilla con trozos de galleta Oreo, relleno y cubierta de crema de Oreo.",
        "publico_ideal": "Jóvenes, niños, cumpleaños informales.",
        "precio_promedio": "95 Bs."
      }
    ],
    "ejemplo_contenido": "¡Hola Santa Cruz! 🎂 ¿Cumpleaños sorpresa y sin torta? Nosotros te salvamos. Pide tu Torta Express y la tendrás en tu puerta en 2 horas. ¡Escribe al 62013533! #TortaExpress #SantaCruz #Cumpleaños",
    "contenidos_evitar": [
      "Publicaciones muy largas",
      "Fotos de baja calidad",
      "Promociones agresivas sin contexto"
    ],
    "fecha_creacion": "2024-10-15T10:30:00.000Z",
    "fecha_actualizacion": "2024-10-15T10:30:00.000Z",
    "planificacion_mensual": {
      "fecha": "2025-10-01",
      "objetivos_especificos": [
        "Incrementar ventas Torta Oreo",
        "Capitalizar Día de la Madre"
      ],
      "eventos_destacados": [
        "Día de la Madre (27/10)",
        "Halloween (31/10)"
      ],
      "ajustes": {
        "presupuesto": "medio",
        "plataformas_activas": [
          "Facebook",
          "Instagram"
        ],
        "producto_destacado": "Torta de Oreo"
      },
      "negocio_id": "TORTA_EXPRESS_001"
    }
  }
]
```



---

## 🧠 Integración con LLM en n8n

Una vez completadas las dos etapas anteriores, el sistema activa un **nodo de IA** que recibe los datos como contexto inicial. Este nodo ejecuta un **loop de generación** que se repite tantas veces como días tenga el mes planificado.

![[Pasted image 20250926133434.png]]


## 🗃️ Almacenamiento y Visualización en el ERP

Cada contenido generado se guarda como un registro JSON en la base de datos, asociado a su fecha correspondiente. Estos registros se visualizan en el ERP mediante una **tabla tipo calendario**, navegable por mes y día.

Esto permite a los equipos:

- Revisar el contenido generado por día
- Validar tono, objetivos y llamados a la acción
- Editar o aprobar publicaciones antes de su ejecución
### 📦 Ejemplo de JSON generado por Día

```
{
  "output": {
    "contenido_generado": {
      "fecha": "2025-10-01",
      "negocio_id": "TORTA_EXPRESS_001",
      "plataforma": "Facebook",
      "tipo_contenido": "publicación",
      "titulo": "¡Celebra este octubre con nuestra Torta Oreo!",
      "copy_publicacion": "¡Octubre llegó y las celebraciones no faltan! 🎉 Sorprende a tus seres queridos con nuestra deliciosa Torta de Oreo, la combinación perfecta de galleta y crema. Personalízala con un mensaje especial y te la llevamos en 40 minutos. Haz tu pedido ahora al 62013533 y endulza tu día. 🍰 #TortaExpress #TortaOreo #SantaCruz",
      "hashtags": ["#TortaExpress", "#TortaOreo", "#SantaCruz"],
      "producto_destacado": "Torta de Oreo",
      "objetivo_estrategico": "Incrementar ventas Torta Oreo",
      "tono_aplicado": "Cercano, alegre y confiable",
      "emojis_utilizados": ["🎉", "🍰"],
      "llamado_accion": "Haz tu pedido ahora!",
      "estado": "pendiente",
      "fecha_creacion": "2024-10-15T10:30:00.000Z",
      "fecha_actualizacion": "2024-10-15T10:30:00.000Z"
    },
    "metadata": {
      "estado_procesamiento": "exitoso",
      "total_caracteres": 266,
      "emojis_count": 2,f
      "hashtags_count": 3,
      "coherencia_estilo": "alta",
      "alineacion_objetivos": "alta"
    }
  }
}
```
