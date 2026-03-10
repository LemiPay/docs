# Stack Tecnológico

Este documento describe la arquitectura y las tecnologías seleccionadas para el desarrollo del proyecto. El stack está orientado a maximizar el **rendimiento**, la **seguridad de tipos** y la **escalabilidad**.

---

## Arquitectura General



| Capa | Tecnología | Rol |
| :--- | :--- | :--- |
| **Frontend** | Svelte Kit| Interfaz de usuario (SPA) |
| **Backend** | Rust | API REST de alto rendimiento |
| **ORM** | Diesel | Consultas seguras y mapeo de datos |
| **Base de Datos** | PostgreSQL | Almacenamiento relacional |
| **Infraestructura**| Docker | Contenerización y entornos |

---

## Detalle de Componentes

### Base de Datos e Infraestructura
Utilizaremos **PostgreSQL** como motor de base de datos principal por su robustez y soporte avanzado de tipos.
* **Contenerización:** La base de datos correrá sobre **Docker**, garantizando que todos los desarrolladores trabajen sobre la misma versión y configuración.
* **Persistencia:** Se utilizarán volúmenes de Docker para asegurar la integridad de los datos durante el desarrollo.

### Backend (Rust)
El servidor se desarrollará en **Rust** para aprovechar su sistema de tipos y su gestión de memoria sin *Garbage Collector*.
* **Framework:** A definir entre **Axum** (basado en el ecosistema Tokio/Tower) o **Actix-web** (madurez y velocidad extrema).
* **ORM (Diesel):** Se eligió **Diesel** por ser un ORM de tipo seguro que permite detectar errores en las consultas SQL durante el tiempo de compilación.

### Frontend (Svelte)
Para la interfaz, se optó por **Svelte** configurado como una **SPA (Single Page Application)**.
* **Renderizado:** Client-Side Rendering (CSR).
* **Ventaja:** Al no utilizar un Virtual DOM, Svelte ofrece una reactividad mucho más ligera y tiempos de carga mínimos para el usuario final.
* **Comunicación:** Se conectará al backend mediante peticiones asincrónicas a la API REST.

---

## Beneficios del Stack
1. **Seguridad:** Rust y Diesel previenen gran parte de los errores comunes de memoria y lógica de BD.
2. **Velocidad:** Tanto el backend en Rust como el frontend en Svelte están entre las opciones más rápidas de la industria actual.
3. **Portabilidad:** Docker simplifica el despliegue y la configuración inicial del entorno de desarrollo.

