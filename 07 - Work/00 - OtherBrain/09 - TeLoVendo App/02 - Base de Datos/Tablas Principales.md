---
tags:
  - TeLoVendo
  - DB
  - Prisma
---
# Modelos Core de Prisma Postgres

## 🧠 Entidades Protagonistas

A diferencia de proyectos informativos, este proyecto es altamente transaccional y guarda "Estado Lógico" en casi todas sus tablas (mediante uso intensivo de `enum`).

### 1. Sistema de Tareas (BotOrders)
Una `BotOrder` no se publica instantáneamente, se *encola*.
```prisma
model BotOrder {
  id                  String              @id @default(cuid())
  type                OrderType           @default(MARKETPLACE)
  status              OrderStatus         @default(LISTA)
  listingTitle        String?
  listingPrice        Decimal?            @db.Decimal(10, 2)
  listingCategory     MarketplaceCategory 
  hasSecurityAlert    Boolean             @default(false)
  imageUrls           String[]            
  selectedDeviceIds   String[]            
  project             Project             @relation(fields: [projectId])
}
```

### 2. Dispositivos e Instancias de Generación
El modelo de dispositivos es tu puente entre el "SaaS Web" y el "Mundo Físico".
```prisma
model Device {
  serial          String           @unique
  alias           String?
  estadoDb        String?
  redesSociales   Json?            // Cuentas logueadas
  status          DeviceStatus     @default(LIBRE)
}
```
`GenMarketplace` es la "Instancia en ejecución". Una orden crea uno o varios `GenMarketplace` ligados a un device físico que es quien reporta `PENDIENTE -> PUBLICADO`.

### 3. Enumeración Robusta
La BD dicta las reglas de front-end gracias a los Enums:
- `MarketplaceCategory`: Más de 30 categorías.
- `OrderStatus`: `LISTA | GENERANDO | GENERADA | CANCELADA | COMPLETADA`
