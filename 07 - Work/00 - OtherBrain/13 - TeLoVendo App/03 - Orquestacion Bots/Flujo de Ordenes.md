---
tags:
  - TeLoVendo
  - Engine
---
# Flujo de Orquestación

## 🧠 ¿Cómo se publica un post?

1. **Creación:** El admin crea una `BotOrder` llenando el formulario del Marketplace con sus imágenes y precio.
2. **Device Bidding:** Se elijen explícitamente cuáles `deviceIds` van a correr esta orden (Almacenados en `BotOrder.selectedDeviceIds`).
3. **Dispatch:** El sistema interno particiona la orden creando registros `GenMarketplace` para cada `Device` con el estado `PENDIENTE`.
4. **Agente Físico / Bot script:** El agente físico que expone TeLoVendo (Quizás Appium, un Script local de Puppeteer o una app React Native hosteada en los celulares) hace *Long Polling* o consulta la base de datos buscando: `Select * where deviceId = MI_MAC_ADDRESS && Status = PENDIENTE`.
5. **Ejecución y Cierre:** El Celular cambia su estado propio a `OCUPADO`, realiza los clics en Facebook, extrae la URL final de la publicación, y actualiza el `GenMarketplace` inyectando la `postUrl` y mudando el estado a `PUBLICADO`.
