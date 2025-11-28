# 🛍️ Proyecto Final E-commerce - Angular

![Angular](https://img.shields.io/badge/Angular_v20-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=for-the-badge&logo=railway&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)

> Una plataforma de comercio electrónico moderna, escalable y robusta construida con las últimas tecnologías web.

🔗 **[Ver Demo en Vivo](https://proyecto-final-angular-tau.vercel.app/)** | 📂 **[Repositorio](https://github.com/AlejandroLeon2/Proyecto-final-Angular)**

---

## 📑 Tabla de Contenidos

- [Descripción del Proyecto](#-descripción-del-proyecto)
- [Screenshots](#-screenshots)
- [Stack Tecnológico](#️-stack-tecnológico)
- [Arquitectura y Estructura](#-arquitectura-y-estructura)
- [Guía de Instalación](#-guía-de-instalación-y-ejecución)
- [Escalabilidad y Extensibilidad](#-escalabilidad-y-extensibilidad)
- [Roadmap](#️-roadmap)
- [Contribución](#-contribución)

---

## 📖 Descripción del Proyecto

Este proyecto es una aplicación de **E-commerce** completa diseñada para ofrecer una experiencia de usuario fluida y segura. Permite a los usuarios navegar por un catálogo de productos, gestionar un carrito de compras, realizar pedidos y autenticarse de manera segura.

La arquitectura está pensada para ser **modular y escalable**, facilitando el mantenimiento y la incorporación de nuevas funcionalidades sin comprometer el rendimiento.

### ✨ Características Principales

*   **Catálogo Dinámico**: Visualización de productos con filtrado y búsqueda.
*   **Carrito de Compras**: Gestión de estado global para el carrito.
*   **Autenticación Segura**: Integración con Firebase Auth.
*   **Diseño Responsivo**: Interfaz adaptada a móviles y escritorio gracias a TailwindCSS.
*   **Alto Rendimiento**: Optimizado con las mejores prácticas de Angular.

---

## � Screenshots

Explora la interfaz de nuestra aplicación en diferentes dispositivos:

````carousel
![Vista principal del E-commerce en escritorio](./screenshots/home-desktop.png)
<!-- slide -->
![Catálogo de productos - Vista de escritorio](./screenshots/catologos.png)
<!-- slide -->
![Catálogo responsive - Vista móvil](./screenshots/catalogo-mobile.png)
<!-- slide -->
![Detalle de producto - Vista móvil](./screenshots/producto-mobile.png)
````

---

## �🛠️ Stack Tecnológico

El proyecto se ha construido sobre una base sólida de tecnologías modernas:

| Tecnología | Propósito |
| :--- | :--- |
| **Angular (v20)** | Framework principal para la construcción de la SPA (Single Page Application). |
| **Railway** | Plataforma de despliegue para el backend y servicios API. |
| **Firebase** | Backend-as-a-Service para autenticación, base de datos y hosting. |
| **TailwindCSS** | Framework de utilidades CSS para un diseño rápido y consistente. |
| **RxJS** | Manejo de programación reactiva y flujos de datos asíncronos. |
| **Pnpm** | Gestor de paquetes eficiente y rápido. |

---

## 📂 Arquitectura y Estructura

El proyecto sigue una arquitectura **basada en características (Feature-Based)**, lo que promueve la separación de responsabilidades y la escalabilidad.

```text
src/
├── app/
│   ├── core/           # Servicios singleton, guards, interceptors, modelos globales.
│   ├── feature/        # Módulos de funcionalidad (ej. auth, productos, carrito).
│   ├── layouts/        # Componentes de estructura (Header, Footer, Sidebar).
│   ├── page/           # Componentes que actúan como páginas (Vistas principales).
│   ├── components/     # Componentes compartidos/reutilizables (UI Kit).
│   ├── data/           # Datos estáticos o mocks.
│   └── icons/          # Iconografía SVG o componentes de iconos.
├── environments/       # Configuración de variables de entorno (Dev/Prod).
└── styles.css          # Estilos globales y configuración de Tailwind.
```

---

## 🚀 Guía de Instalación y Ejecución

Sigue estos pasos para levantar el proyecto en tu entorno local:

1.  **Clonar el repositorio**:
    ```bash
    git clone https://github.com/AlejandroLeon2/Proyecto-final-Angular.git
    cd Proyecto-final-Angular
    ```

2.  **Instalar dependencias**:
    ```bash
    pnpm install
    ```

3.  **Configurar Variables de Entorno**:
    Crea un archivo `.env` en la raíz del proyecto basándote en `.example.env` y añade tus credenciales de Firebase.

4.  **Ejecutar el servidor de desarrollo**:
    ```bash
    pnpm start
    ```
    La aplicación estará disponible en `http://localhost:4200/`.

---

## 📈 Escalabilidad y Extensibilidad

Este proyecto ha sido diseñado pensando en el crecimiento futuro. Aquí te explicamos cómo escalarlo:

### 1. Añadir Nuevas Funcionalidades (Features)
Para agregar una nueva funcionalidad (por ejemplo, un módulo de "Reseñas"), crea una nueva carpeta en `src/app/feature/reseñas`. Mantén toda la lógica, componentes y servicios específicos de esa funcionalidad dentro de su directorio.

### 2. Gestión de Estado
Actualmente se utiliza RxJS (BehaviorSubjects/Signals) para el estado. Para una escalabilidad mayor si la complejidad aumenta, se recomienda integrar **NgRx** o **Elf** para un manejo de estado más robusto y predecible.

### 3. Optimización de Rendimiento
*   **Lazy Loading**: Asegúrate de que las nuevas rutas en `app.routes.ts` utilicen `loadComponent` o `loadChildren` para cargar el código solo cuando sea necesario.
*   **Componentes Standalone**: El proyecto utiliza componentes standalone de Angular, lo que reduce el tamaño del bundle y simplifica la inyección de dependencias.

### 4. Integración de Pagos
Para convertirlo en un e-commerce transaccional real, se puede integrar fácilmente una pasarela de pagos como **Stripe** o **PayPal** creando un servicio en `core/services/payment.service.ts` y conectándolo con el backend (Firebase Functions o API propia).

---

## 🗺️ Roadmap

Funcionalidades planeadas para futuras versiones:

- [ ] **Sistema de Reseñas y Calificaciones**: Permitir a los usuarios valorar y comentar productos.
- [ ] **Integración con Pasarela de Pagos**: Implementar Stripe o PayPal para transacciones reales.
- [ ] **Panel de Administración**: Dashboard para gestionar productos, pedidos y usuarios.
- [ ] **Notificaciones Push**: Alertas en tiempo real sobre ofertas y estado de pedidos.
- [ ] **Wishlist/Lista de Deseos**: Guardar productos favoritos para comprar más tarde.
- [ ] **Sistema de Cupones y Descuentos**: Códigos promocionales y ofertas especiales.
- [ ] **Historial de Pedidos**: Visualización completa de compras anteriores.
- [ ] **Búsqueda Avanzada**: Filtros por categoría, precio, marca y valoraciones.

---

## 🤝 Contribución

¡Las contribuciones son bienvenidas! Si deseas mejorar este proyecto:

1.  Haz un **Fork** del repositorio.
2.  Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`).
3.  Haz tus cambios y **commit** (`git commit -m 'Add: nueva funcionalidad'`).
4.  Haz **push** a la rama (`git push origin feature/nueva-funcionalidad`).
5.  Abre un **Pull Request**.

---

Hecho con ❤️ por el equipo de desarrollo.
