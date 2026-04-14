---
tags:
  - C.R.E.Insight
  - prisma
  - migraciones
---
# Migraciones y Dumps

## 🧠 Estrategia de Versionamiento de BD
Con Prisma, cualquier modificación al esquema de datos genera un script SQL inmutable en la carpeta `/prisma/migrations/`.

### Ejecución Segura en Producción
Se prohíbe el uso de `prisma db push` en entornos superiores a Localhost.
Los despliegues dependen de:
```bash
npx prisma migrate deploy
```

### Cargas Virgenes (Seeds)
Para lanzar un entorno de QA / Demostración a Stakeholders sin ensuciar la base productiva, se utiliza `prisma db seed`, basado en datos falsos insertados estratégicamente.
*(Nota temporal: Observé que existe el archivo `tsconfig.seed.json` en el repositorio original, confirmando esta práctica de diseño robusto).*
