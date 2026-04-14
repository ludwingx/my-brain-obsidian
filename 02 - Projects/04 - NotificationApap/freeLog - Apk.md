# 📱 Proyecto: FreeLog

**Descripción**  
Aplicación móvil para Android que registra automáticamente las notificaciones del sistema (Yape, BCP, etc.), filtra las relevantes y las envía a una hoja de Google Sheets para llevar un control financiero sin intervención manual.

---

## 🎯 Objetivo

- Extraer notificaciones del sistema relacionadas con apps financieras.
- Filtrar por apps como Yape, BCP, BBVA, etc.
- Enviar los datos extraídos a una hoja de Google Sheets.
- Hacerlo todo de manera **automática y gratuita**.

---

## 🛠 Stack Tecnológico

- **Frontend / App**: React Native con Expo + EAS Dev Client
- **Lenguaje**: JavaScript / Kotlin (módulo nativo)
- **Backend**: Google Apps Script (con webhook)
- **Almacenamiento**: Google Sheets
- **Construcción**: EAS CLI

---

## 🔧 Módulos principales

### 1. Notification Listener (Android Native)
- Servicio en Kotlin/Java que detecta las notificaciones entrantes.
- Extrae: `packageName`, `title`, `text`, `timestamp`.

### 2. Filtro de Notificaciones
- Se filtran solo notificaciones de paquetes como:
  - `pe.yape.wallet`
  - `com.bcp.mobile`
  - `com.bbva.netbanking`

### 3. Conector a Google Sheets
- Petición `POST` al Webhook de Apps Script con los datos.
- Formato: JSON → `[fecha, título, texto, paquete]`

### 4. Interfaz (opcional)
- Mostrar últimos registros dentro de la app.
- Permitir configurar la hoja o cambiar filtros.

---

## 🚧 Pasos de desarrollo

### 🔹 Inicialización
- [x] Crear proyecto con Expo `npx create-expo-app FreeLog`
- [x] Inicializar EAS `eas init`
- [x] Instalar Dev Client `npm install expo-dev-client`

### 🔹 Android (Código Nativo)
- [ ] Crear servicio `NotificationListenerService` en Java/Kotlin.
- [ ] Agregar permisos en `AndroidManifest.xml`.
- [ ] Registrar servicio como plugin de Expo.

### 🔹 Google Sheets API
- [x] Crear hoja de cálculo.
- [x] Crear Apps Script como Webhook:

```js
function doPost(e) {
  var sheet = SpreadsheetApp.openById("ID_DE_HOJA").getSheets()[0];
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([new Date(), data.title, data.text, data.package]);
  return ContentService.createTextOutput("OK");
}
