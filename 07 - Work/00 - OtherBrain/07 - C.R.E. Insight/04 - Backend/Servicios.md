---
tags:
  - C.R.E.Insight
  - backend
  - serverside
---
# Estructura de Servicios y Lógica

## 🧠 El patrón `Services` en Next.js
Para evitar rutas superpobladas con código (Fat Controllers), cualquier ruta dentro de `src/app/api/...` subcontrata la lógica compleja.

### Reglas de Diseño de Funciones `Server Actions`
1. **Unidirectional Data Flow:** Las inserciones en bloque de posts (`/api/posts`) primero parsean el Array, verifican IDs en caché (si los hay), y solo envían transacciones masivas (`createMany` o `$transaction`) para no colapsar las conexiones simultáneas hacia Supabase/PostgreSQL.
2. **Revalidación (ISR & Tags):**
   Después de ingerir una nueva ola crítica de datos, se utiliza la función nativa de Next.js `revalidateTag` o `revalidatePath` ("/") para invalidar la caché forzosa de la pantalla web de los analistas humanos, mostrando instantáneamente los nuevos datos sin forzar WebSockets costosos.
