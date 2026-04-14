# FreeLog (NotificationApp)

## 🧠 Descripción
Aplicación móvil para Android que se encarga de interceptar y registrar automáticamente notificaciones de sistemas financieros y bancos (Yape, BCP, BBVA, etc.) y las centraliza en un Sheet de Google para control financiero autónomo y gratuito.

## 🎯 Objetivo
Evitar la recolección manual de recibos y transferencias. Extrae datos relevantes directo de las notificaciones push locales y hace un puente hacia la nube para tener visibilidad de finanzas en tiempo real.

## ⚙️ Stack

| Tecnología | Propósito |
|---|---|
| React Native + Expo | Framework híbrido para la App |
| Java / Kotlin | Servicio nativo (Notification Listener) |
| Google Apps Script | Backend ligero / Webhook de recepción |
| Google Sheets | Base de datos gratuita |
| EAS Build | Compilación en la nube (APK) |

## 📁 Archivos
- [[freeLog - Apk]] — Notas técnicas de desarrollo

## 📌 Estado
🔨 En progreso activo (Servicio de webhook y setup básico completado)

## 🔗 Relacionado
- [[Mi Nucleo Cerebral|Volver a Mi Núcleo Cerebral (Root)]]
