---
tags:
  - OBWorkspace
  - OB-Workspace
  - obworkspace
  - Prisma
  - SQL
---
# Prisma Schema y Tracking Avanzado

## 🧠 Modelos Clave del SaaS

La aplicación está diseñada sobre **PostgreSQL**, aprovechando relaciones para delegar cálculos a sub-queries.

### 1. Motor de Tickets y Subtareas
Los tiempos reales de finalización no son quemados manualmente en el ticket (`hardcoded`).
```prisma
model Subtask {
  id            String        @id
  status        SubtaskStatus // TODO, DOING, DONE
  estimatedTime Int           // En Minutos
  realTime      Int           @default(0) // Agrupado

  ticketId      String
  ticket        Ticket        @relation(fields: [ticketId], references: [id])
}
```
**Regla Dorada:** La sumatoria de `realTime` del Ticket, nace de agrupar las iteraciones subyacentes.

### 2. Máquina de Estado del "Punch Clock"
Para evitar fraude, las horas se miden en ventanas cerradas:
```prisma
model WorkSession {
  id        String    @id
  startTime DateTime  @default(now())
  endTime   DateTime?
  duration  Int       @default(0) // Mutado al cerrarse

  userId    String
  subtaskId String
}
```
Si un programador pausa una Subtarea, se manda un *PATCH* estampando `endTime` y sumando este lapso al `realTime` global de la subtarea. Existe una idea de control futura: Auto-stop si una `endTime` carece de imput tras 10 horas seguidas (prevenir errores humanos los viernes por la noche).
