---
tags:
  - Frontend
  - JS
  - DesarrolloDeSoftware
---
# ⚛️ React: La Biblioteca de Interfaces Dinámicas

React ha redefinido cómo se construyen las aplicaciones web modernas mediante un modelo declarativo y basado en componentes.

## 🧠 Conceptos Nucleares de mi Aprendizaje

### 1. El Virtual DOM
Aprendí que React no manipula el navegador directamente a cada cambio. Primero actualiza un "clon inteligente" y solo aplica los cambios necesarios (Reconciliation). Esto optimiza el rendimiento exponencialmente.

### 2. Flujo de Datos Unidireccional
La información viaja de arriba hacia abajo (Props). Para que un hijo "hable" con un padre, le pasamos funciones. Esta jerarquía hace que depurar sea predecible.

### 3. State Management (Hooks)
- **`useState`**: Mi pan de cada día para la interactividad local.
- **`useReducer`**: Cuando el estado se vuelve complejo (como el carrito de compras en TeLoVendo).
- **`useContext`**: Evitar el "prop-drilling" enviando datos a través del árbol.

## 🛠️ Temario de Profundización
- **Arquitectura**: [[Anatomía]], [[Componentes]].
- **Gestión de Datos**: [[Formularios]], [[Gestion de estado de componentes|Estado local]].
- **Ecosistema**: [[React router|Ruteo]], [[react query|Server State (Query)]], [[Gestión global del estado|Estado Global (Zustand/Redux)]].

## 🚀 Requisitos Previos
- Dominio de [[JavaScript]] moderno (ES6+, [[Fat arrow function|Arrow Functions]], [[fetch|Promesas]]).
- Estructura con [[HTML]] y estilo con [[CSS]].
- Tipado con [[TypeScript]].

---
## 🔗 Navegación
- [[../Frameworks|Volver a Frameworks]]
- [[ Frameworks | Volver al Índice Principal ]]
