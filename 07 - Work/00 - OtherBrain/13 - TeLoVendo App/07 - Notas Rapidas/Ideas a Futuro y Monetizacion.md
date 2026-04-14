---
tags:
  - TeLoVendo
  - Monetizacion
  - Ideas
  - Roadmap
---
# Ideas a Futuro y Modelos de Monetización

## 🧠 Economía del Sistema (TeLoVendo Coins / Tickets)
Dado que los recursos (Bot físicos, IPs residenciales de dispositivos y cuotas de publicación en FB Marketplace) tienen un límite, escalar la aplicación de B2B a B2C requiere un modelo de economía por tokens.

### 🪙 1. Sistema de "Bot-Coins" o "Tickets"
**Concepto:** Los usuarios del sistema (agencias y pequeños vendedores) en lugar de pagar una suscripción fija con uso ilimitado (lo que colapsaría nuestra granja de iPhones/Androids), compran "paquetes de tickets".

- **1 Ticket = 1 Acción de impacto.** Ejemplo: publicar 1 artículo en Facebook Marketplace.
- **Micro-transacciones:** 
  - Que 5 celulares (Dispositivos) publiquen un mismo producto en 5 ubicaciones diferentes cuesta **5 Tickets**.
  - Dejar 1 "Me gusta" o 1 "Comentario pre-armado" para calentar el algoritmo cuesta **0.2 Tickets**.
- **Renovación Mensual:** Planes de SaaS como *Pro Seller* (100 tickets/mes) y *Agency* (500 tickets/mes).

### ⚙️ Implicaciones Técnicas (Próximamente en Prisma)
Para soportar esto, requeriríamos escalar la base de datos de la siguiente manera:
```prisma
model UserWallet {
  id          String   @id @default(cuid())
  userId      String   @unique
  user        User     @relation(fields: [userId], references: [id])
  balance     Decimal  @default(0.00) // Bot-Coins actuales
  spentTotal  Decimal  @default(0.00)
  transactions WalletTransaction[]
}

model WalletTransaction {
  id          String   @id @default(cuid())
  walletId    String
  amount      Decimal  // Negativo si gastó, positivo si recargó
  reason      String   // "Despacho de BotOrder #...", "Recarga por Stripe"
  createdAt   DateTime @default(now())
}
```

### 🤖 2. Funciones de Crecimiento Agresivo (Growth Hacks)
- **Campañas Enjambre (Swarm Orders):** Un botón mágico que permita al usuario decir "Extiende esta publicación en toda Bolivia". El software internamente descuenta 20 tickets y despacha la `BotOrder` asignándole 20 celulares con rangos de latitud y longitud distintos.
- **Mercado de Cuentas:** Si un dispositivo (Celular viejo de un colaborador) entra y aporta sus propias cuentas a la red, gana un % pasivo de los tickets. Sistema "Grid-Computing" o Descentralizado para conseguir IPs naturales y no gastar en Proxies residenciales carísimos.

## 🔗 Relacionado
- [[../02 - Base de Datos/Tablas Principales|Eje Transaccional a Actualizar]]
- [[../03 - Orquestacion Bots/Flujo de Ordenes|Flujo a intervenir antes del Dispatch]]
