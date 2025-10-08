# Ejemplo de diagrama

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

### 🔹 Claves para que funcione

1. Cada diagrama **abre y cierra su propio bloque** con ```mermaid```.  
2. Mantén **una línea en blanco o separación** entre bloques (por ejemplo el `---` o solo un salto de línea).  
3. Al guardar y refrescar el README en GitHub, **verás los dos diagramas renderizados uno debajo del otro**.  

Si quieres, puedo hacer una **versión unificada** que combine ambos diagramas en **un único flujo completo** desde login hasta la gestión de usuarios y acciones en el tablero.  
¿Quieres que haga eso?
