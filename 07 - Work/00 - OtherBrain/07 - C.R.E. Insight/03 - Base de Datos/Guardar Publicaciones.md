---
created:
  - 03-10-2025
tags:
aliases:
  - C.R.E. Insight
---
# Guardar Publicaciones

##  Descripción

Nodo de base de datos encargado de **insertar o actualizar publicaciones** obtenidas desde **Facebook, Instagram y TikTok** en la tabla `Posts`.  
Este nodo se conecta al **flujo diario de scraping** y asegura que:

- No existan duplicados.
- Las métricas se actualicen correctamente.
- Se guarde texto OCR y contenido multimedia si cambian.

---

##  Tabla Principal: `Posts`

| Campo           | Tipo        | Notas                                |
| --------------- | ----------- | ------------------------------------ |
| id_publicacion  | string (PK) | ID único de la publicación           |
| plataforma      | string      | Origen: Facebook, Instagram o TikTok |
| pagina          | string      | Nombre de la página o usuario        |
| fecha           | datetime    | Fecha de publicación                 |
| timestamp       | bigint      | Timestamp de publicación             |
| texto           | text        | Contenido de la publicación          |
| enlace_externo  | string      | Link externo (si aplica)             |
| me_gusta        | int         | Número de likes                      |
| compartidos     | int         | Número de shares                     |
| reacciones      | int         | Número de reacciones totales         |
| hashtags        | text[]      | Lista de hashtags                    |
| tiene_imagen    | boolean     | True/False                           |
| url_imagen      | string      | URL de la imagen principal           |
| texto_ocr       | text        | Texto extraído de la imagen          |
| url_publicacion | string      | Link directo a la publicación        |
| url_facebook    | string      | Link a la fanpage (solo FB)          |
| feedback_id     | string      | ID interno para seguimiento          |
| created_at      | datetime    | Fecha de inserción                   |
| updated_at      | datetime    | Última actualización                 |

---

##  Estrategia de Inserción/Actualización

- **Insertar** publicaciones nuevas usando `id_publicacion` como clave primaria.
    
- **Actualizar** métricas (`me_gusta`, `compartidos`, `reacciones`), `texto_ocr` y `url_imagen` si hay cambios.
    
- **Evitar duplicados** y mantener consistencia en datos históricos.
    
- Mantener trazabilidad con `created_at` y `updated_at`.
    

---

##  Ejemplo de Mapeo JSON → DB

`{   "id_publicacion": {{$json["id_publicacion"]}},   "plataforma": {{$json["plataforma"]}},   "pagina": {{$json["pagina"]}},   "fecha": {{$json["fecha"]}},   "timestamp": {{$json["timestamp"]}},   "texto": {{$json["texto"]}},   "enlace_externo": {{$json["enlace_externo"]}},   "me_gusta": {{$json["estadisticas"]["me_gusta"]}},   "compartidos": {{$json["estadisticas"]["compartidos"]}},   "reacciones": {{$json["estadisticas"]["reacciones"]}},   "hashtags": {{$json["hashtags"]}},   "tiene_imagen": {{$json["tiene_imagen"]}},   "url_imagen": {{$json["url_imagen"]}},   "texto_ocr": {{$json["texto_ocr"]}},   "url_publicacion": {{$json["url_publicacion"]}},   "url_facebook": {{$json["url_facebook"]}},   "feedback_id": {{$json["feedback_id"]}} }`

> 💡 Si la base de datos soporta arrays (`Postgres`), guardar `hashtags` directamente como array.  
> Si no, usar string separado por comas.

---

## Flujo en n8n

1. **Nodo HTTP Request** → Obtiene JSON desde Apify o API de scraping.
    
2. **Nodo Function / Set** → Mapea JSON al formato compatible con `Posts`.
    
3. **Nodo Database (Postgres/Prisma/MySQL)** → Inserta o actualiza publicaciones.
    
4. **Nodo Logging / Slack (opcional)** → Envía notificaciones sobre nuevas publicaciones o errores.

## Query PostgreSQL

```
-- Tabla de usuarios
CREATE TABLE UsersCRE (
    id_usuario SERIAL PRIMARY KEY,
    nombre_completo VARCHAR(150) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL, -- Nunca guardes contraseñas planas
    rol VARCHAR(50) DEFAULT 'user', -- Ejemplo: admin, user, analista
    activo BOOLEAN DEFAULT TRUE,
    ultimo_login TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Trigger para updated_at
CREATE OR REPLACE FUNCTION update_user_updated_at()
RETURNS TRIGGER AS $$
BEGIN
   NEW.updated_at = NOW();
   RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER trg_update_users
BEFORE UPDATE ON UsersCRE
FOR EACH ROW
EXECUTE FUNCTION update_user_updated_at();

```