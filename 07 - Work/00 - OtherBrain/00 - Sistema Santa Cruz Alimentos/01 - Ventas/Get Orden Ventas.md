### Get Orden de Venta: 

```
"sales": [
  {
    "id_venta": 1,
    "id_usuario_registro": 1,
    "nombre_usuario_registro": "Ludwing",
    "id_cliente": 1,
    "nombre_cliente": "Juan Perez",
    "fecha_entrega_estimada": "2025-09-30T14:00:00-04:00",
    "fecha_entrega_real": "2025-09-30T14:00:00-05:00",
    "observaciones": "Pedido para fiesta infantil, entregar por la tarde",
    "fecha_registro": "2025-09-30T13:45:20-04:00",
    "fecha_actualizacion": "2025-09-30T13:45:20-04:00",
    "id_estado_entrega": 1,
    "nombre_estado_entrega": "Pendiente",
    "entrega": {
      "id_entrega": 1,
      "id_tipo_entrega": 1,
      "nombre_tipo_entrega": "Delivery",
      "direccion_entrega": "Av. Siempre Viva 742, Planta Baja",
      "nombre_receptor": "Ana Valdez",
      "telefono_receptor": "77712345",
      "costo_delivery": 15.00,
      "estado_delivery": "Pendiente",
      "nombre_sucursal": null,
      "observaciones": "Llamar antes de llegar",
      "costo_total": 415.00
    },
    "detalles": [
      {
        "id_detalle": 1,
        "id_producto": 5,
        "sabor_producto": "Chocolate",
        "tamaño_producto": "XL",
        "precio_producto": 206.00,
        "id_negocio": 1,
        "nombre_negocio": "Mil Sabores",
        "cantidad": 1,
        "precio_unitario_venta": 150.00,
        "subtotal": 160.00,
        "frase": {
          "frase": "Feliz Cumpleaños, Campeón!",
          "costo_frase": 10.00,
          "comentario_frase": "Además de la frase, dibujar un símbolo sencillo como lo pidió el cliente"
        },
        "personalizacion": {
          "imagen_base64": "data:image/jpeg;base64,/9j/4AAQ...",
          "costo_personalizacion": 25.00,
          "comentario_personalizacion": "Usar colores pastel y agregar borde dorado"
        }
      }
    ],
    "total_general": 415.00
  }
]
```