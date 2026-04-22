---
title: Usuarios y Perfil
status: Completado
priority: Media
---

# Gestión de Usuarios y Perfil

> [[Market-GS]] > [[Usuarios]]

Este módulo permite a los usuarios gestionar su información personal y a los administradores gestionar los accesos al sistema.

## Mi Perfil (`/ajustes`)
Todos los usuarios pueden acceder a su perfil desde el menú de usuario (esquina inferior izquierda) para:
- Ver su rol en el sistema.
- Cambiar su nombre a mostrar.
- Cambiar su contraseña por razones de seguridad.

## Gestión de Usuarios (`/configuracion/usuarios`)
**Acceso exclusivo para Administradores.**

Permite a los administradores de Market GS:
- **Crear nuevos usuarios**: Definiendo su nombre, usuario (login), contraseña y rol (Vendedor o Administrador).
- **Control de Acceso**: Activar o desactivar cuentas. Si un empleado deja la empresa, su cuenta se puede "Desactivar" para que no pueda iniciar sesión, en lugar de borrarla (lo cual corrompería el historial de [[Ventas]] o movimientos de [[Inventario]] que realizó).
- **Auditoría**: Ver el último login de cada usuario.

## Modelo de Datos
- Tabla `User` con campos de `username`, `password` (hasheado con bcrypt), `role`, y `isActive`.
