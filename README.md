

# 🪄 Actividad: Mini-App del Mundo Mágico (Harry Potter)

El objetivo de esta actividad es construir una aplicación utilizando **Vue 3**, **Vue Router** y **Axios** que permita explorar los personajes del universo de Harry Potter consumiendo una API real.

## 🚀 Requisitos Técnicos

1.  **Instalación de Dependencias**: Deberás instalar e integrar en el proyecto:
    *   `axios`: Para las peticiones HTTP.
    *   `vue-router`: Para gestionar la navegación entre vistas.

2.  **API a utilizar**: 
    *   Listado completo: `https://hp-api.onrender.com/api/characters`
    *   Personaje específico: `https://hp-api.onrender.com/api/character/:id`

## 📋 Instrucciones de la Tarea

### 1. Configuración de Rutas
Configura el router de Vue para que soporte al menos dos rutas principales:
*   `/`: Mostrará el listado general de personajes.
*   `/personaje/:id`: Mostrará el detalle de un personaje específico basado en su ID único.

### 2. Vista de Listado
*   Realiza una petición `GET` a la API para obtener todos los personajes al cargar el componente.
*   Muestra una cuadrícula (grid) o lista con tarjetas que incluyan el nombre y la imagen del personaje.
*   Cada tarjeta debe ser clickable para navegar a la vista de detalle.
*   **Reto**: Implementa un filtro de búsqueda por nombre.

### 3. Vista de Detalle
*   Captura el parámetro `id` de la URL.
*   Realiza una petición a la API para obtener la información específica de ese personaje.
*   Muestra los atributos principales: Casa, Varita, Patrono, Actor y estado actual.
*   **🔒 Desafío de Seguridad**: Por "protección mágica", el valor del **Patronus** debe mostrarse **encriptado** al cargar la página. **Utiliza como referencia la lógica de encriptación/desencriptación que implementamos en la actividad de hoy.**
*   **Interacción**: Añade un botón "Revelar Encantamiento" que desencripte y muestre el Patronus real al usuario.
*   Incluye un botón "Volver" para regresar al listado principal de forma programática o mediante enlace.

### 4. Estándares de Diseño (UX/UI)
*   Aplica un sistema de espaciado basado en 8px (márgenes y paddings).
*   Implementa estados de carga (Loading) para mejorar la experiencia de usuario mientras Axios obtiene los datos.
*   Asegura que el diseño sea responsivo y visualmente atractivo.

## 💡 Tips de Ayuda
*   Organiza tus componentes en la carpeta `src/components` y tus vistas en `src/views`.
*   Utiliza los hooks de ciclo de vida de Vue 3 (`onMounted`) para disparar las peticiones.
*   Revisa la consola del navegador para depurar las respuestas de la API.
