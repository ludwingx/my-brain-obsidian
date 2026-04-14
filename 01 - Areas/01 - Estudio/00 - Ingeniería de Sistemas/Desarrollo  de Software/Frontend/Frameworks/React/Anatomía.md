---
tags:
  - React
  - Anatomy
  - Patterns
  - DesarrolloDeSoftware
---
# 🦴 Anatomía de una Aplicación React

Entender cómo "respira" React antes de codificar.

## 🧬 Componentes y Propiedades
Todo en React es una función que acepta `props` y devuelve `JSX`.
- **JSX:** No es HTML, es azúcar sintáctico para `React.createElement`.
- **One-Way Data Flow:** La información baja de padres a hijos. Nunca sube directamente (para eso usamos funciones de callback).

## 🔋 El Ciclo de Vida y los Hooks
- `useState`: El estado local.
- `useEffect`: Efectos secundarios (suscripciones, fetchings).
- `useMemo` y `useCallback`: Optimización para evitar renders innecesarios.

---
## 🔗 Navegación
- [[React|Volver a React]]
- [[ Frameworks | Volver al Índice Principal ]]
