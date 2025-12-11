# Proyecto Cupcake

En esta rama se muestra el proyecto inicial que el estudiante deberá completar
para llegar al objetivo

**Cupcake** es una aplicación de ejemplo para Android, desarrollada con las últimas tecnologías recomendadas por Google, como **Jetpack Compose** para la interfaz de usuario, **Navigation for Compose** para la navegación y **ViewModel** para la gestión del estado de la UI.

El objetivo de la aplicación es guiar al usuario a través de un proceso de pedido de cupcakes en varios pasos, mostrando cómo estructurar una app sencilla pero robusta.

---

## 📋 Características Principales

- **Interfaz de Usuario Moderna**: Construida 100% en Kotlin y Jetpack Compose.
- **Navegación Fluida**: Utiliza `NavHostController` de Navigation for Compose para moverse entre pantallas.
- **Gestión de Estado Centralizada**: Un `OrderViewModel` gestiona el estado del pedido (cantidad, sabor, fecha, precio), que sobrevive a los cambios de configuración.
- **Flujo de Pedido en Múltiples Pasos**:
    1. **Inicio**: Elegir la cantidad de cupcakes.
    2. **Sabor**: Seleccionar un sabor de una lista.
    3. **Recogida**: Escoger una fecha de recogida.
    4. **Resumen**: Revisar el pedido completo antes de finalizar.
- **Funcionalidad de "Compartir"**: Permite al usuario enviar los detalles de su pedido a través de otras aplicaciones mediante un `Intent` de tipo `ACTION_SEND`.
- **Botones de Navegación Dinámicos**: La `TopAppBar` muestra un botón de retroceso solo cuando es posible navegar hacia atrás.
- **Lógica de Cancelación**: El usuario puede cancelar el pedido en cualquier momento, reiniciando el estado y volviendo a la pantalla de inicio.

---

## 🚀 Arquitectura y Funcionamiento

El proyecto está estructurado siguiendo las mejores prácticas de arquitectura de UI en Android.

### 1. CupcakeApp (Composable Principal)

Punto de entrada de la interfaz de usuario. Sus responsabilidades:

- Inicializar el `NavHostController` y el `OrderViewModel`.
- Configurar el `Scaffold` que contiene la `TopAppBar` y el contenido principal.
- Observar el estado del back stack para determinar la pantalla actual y mostrar o no el botón de retroceso.

---

### 2. Navegación con NavHost

La navegación se gestiona mediante un `NavHost` con cuatro pantallas principales, definidas en el enum `CupcakeScreen`.

#### **CupcakeScreen.Start → StartOrderScreen**
- Muestra opciones para elegir la cantidad (1, 6, 12 cupcakes).
- Al pulsar "Siguiente", actualiza la cantidad en el ViewModel y navega a la pantalla de sabor.

#### **CupcakeScreen.Flavor → SelectOptionScreen**
- Pantalla genérica reutilizable.
- Muestra la lista de sabores de `DataSource`.
- El subtotal se actualiza en tiempo real desde `OrderUiState`.
- Al pulsar "Siguiente", navega a la pantalla de recogida.

#### **CupcakeScreen.Pickup → SelectOptionScreen**
- Reutiliza la misma pantalla genérica.
- Muestra las opciones de fecha calculadas por el ViewModel.
- Al pulsar "Siguiente", navega a la pantalla de resumen.

#### **CupcakeScreen.Summary → OrderSummaryScreen**
- Muestra cantidad, sabor, fecha y precio total.
- Acciones disponibles:
    - **Enviar pedido**: función `shareOrder`.
    - **Cancelar**: función `cancelOrderAndNavigateToStart`.

---

### 3. Gestión de Estado con OrderViewModel

El `OrderViewModel` contiene toda la lógica de negocio del pedido.

- Expone un `StateFlow<OrderUiState>` para que la UI se reactive automáticamente.
- Ofrece funciones de modificación:
    - `setQuantity`
    - `setFlavor`
    - `setDate`
- Incluye `resetOrder()` para reiniciar los datos.

---

### 4. Funciones Auxiliares

- **cancelOrderAndNavigateToStart()**:  
  Reestablece el estado del pedido y limpia la pila de navegación.
- **shareOrder()**:  
  Construye un `Intent` implícito para compartir texto plano con otras apps.

---

## 🛠️ Tecnologías y Librerías Utilizadas

- **Kotlin**
- **Jetpack Compose** (UI declarativa)
- **Material3**: `Scaffold`, `TopAppBar`
- **Navigation for Compose**: gestión de navegación
- **ViewModel (Lifecycle)**
- **StateFlow**: estado reactivo para la interfaz

---
