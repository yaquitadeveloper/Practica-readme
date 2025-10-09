# Ejemplo de diagrama del proyecto Gestion de Proyectos Flujo Base

```mermaid
flowchart TD
  A[Inicio de sesión] -->|Usuario autenticado| B[Dashboard de tableros]
  B --> C[Crear nuevo tablero]
  B --> D[Abrir tablero existente]
  D --> E[Agregar lista]
  E --> F[Agregar tarjeta]
  F --> G[Editar o mover tarjeta]
  G --> H[Ver detalles de tarjeta]
  H --> I[Comentar o adjuntar archivo]
```
---

# Ejemplo de diagrama para usuarios
```mermaid
flowchart TD
  U[Dashboard] --> V[Abrir módulo de usuarios]
  V --> W[Crear nuevo usuario]
  V --> X[Editar usuario existente]
  X --> Y[Actualizar permisos]
  X --> Z[Eliminar usuario]
  Y --> AA[Guardar cambios]
  Z --> BB[Confirmar eliminación]
```
---

# Ejemplo de diagrama para Registro e inicio de Sesion
```mermaid
flowchart TD
  A[Inicio] --> B[¿Tiene cuenta?]
  B -->|Sí| C[Iniciar sesión]
  B -->|No| D[Registrarse]
  C --> E[Validar credenciales]
  E -->|Correctas| F[Ir al Dashboard]
  E -->|Incorrectas| G[Mostrar error]
  D --> H[Llenar formulario]
  H --> I[Crear cuenta]
  I --> F
```
---

# Ejemplo de diagrama para Gestion de Tableros
```mermaid
flowchart TD
  A[Dashboard de tableros] --> B[Ver tableros existentes]
  A --> C[Crear nuevo tablero]
  B --> D[Seleccionar tablero]
  D --> E[Abrir vista del tablero]
  C --> F[Asignar nombre y color]
  F --> G[Guardar tablero]
  G --> B
```
---

# Ejemplo de diagrama para Gestión de Listas y Tarjetas
```mermaid
flowchart TD
  A[Vista del tablero] --> B[Agregar nueva lista]
  B --> C[Nombrar lista]
  C --> D[Agregar tarjeta]
  D --> E[Editar tarjeta]
  E --> F[Mover tarjeta a otra lista]
  F --> G[Ver detalles de tarjeta]
  G --> H[Comentar o adjuntar archivo]
```
---

# Ejemplo de diagrama para Colaboración y Notificaciones
```mermaid
flowchart TD
  A[Vista del tablero] --> B[Invitar miembros]
  B --> C[Seleccionar usuario por correo]
  C --> D[Asignar permisos]
  D --> E[Enviar invitación]
  E --> F[Miembro acepta invitación]
  F --> G[Colaboración en tiempo real]
  G --> H[Notificaciones de actividad]

```
---

# Ejemplo de diagrama para Configuración y Cierre de Sesión
```mermaid
flowchart TD
  A[Dashboard principal] --> B[Ir a perfil de usuario]
  B --> C[Editar nombre, correo o avatar]
  C --> D[Guardar cambios]
  B --> E[Ver preferencias]
  E --> F[Cambiar tema o idioma]
  A --> G[Cerrar sesión]
  G --> H[Volver a pantalla de inicio]
```
---


