# Roadmap y Futuro

Dado que OB Universe funciona actualmente como la capa de presentación consolidada de **OtherBrain**, el progreso del proyecto depende de la paulatina finalización de las herramientas independientes.

## 📌 Hitos a Corto y Mediano Plazo

1. **Autenticación Global (SSO)**:
   - Añadir un proveedor de identidad (como NextAuth/Auth.js o Supabase Auth).
   - El objetivo es que al iniciar sesión en el *Universe*, el token sea válido para acceder remotamente sin re-autorización a las herramientas de *Ventas*, *Inventario*, etc.

2. **Integrar Micro-Frontends o Rutas Proxy**:
   - Enlazar la herramienta de "Inventario" a su infraestructura (ej. la base de datos que maneja TeLoVendo).
   - Levantar el estado "Mantenimiento" de "Sistemas" conectando monitores reales de estado (endpoints de salud).

3. **Panel de Notificaciones Global**:
   - Incorporar un feed transversal de alertas para que un usuario pueda ver, desde el OB Universe, si se cayó una integración de WhatsApp (en TeLoVendo) o si hubo un reporte inusual en "C.R.E. Insight".

---
