Aquí tienes el **Prompt de Ingeniería de Software** definitivo. Está diseñado para que la IA genere el proyecto "MUSICMAN" de forma modular, evitando que el código se corte por límites de caracteres y asegurando que cada fase sea una pieza funcional del rompecabezas.

---

# Prompt Maestro: Desarrollo Integral App "MUSICMAN"

**Rol:** Actúa como un **Senior Full-Stack Developer** con maestría en Flutter y Firebase. Tu objetivo es construir la infraestructura completa de una tienda de instrumentos y despiece técnico utilizando una arquitectura limpia y una interfaz **Dark Turquoise**.

---

## 🚀 FASE 1: Configuración de Base y Dependencias

Genera el archivo `pubspec.yaml` asegurando que todas las dependencias sean compatibles entre sí.

* **Enumeración de dependencias:** 1. firebase_core, 2. firebase_auth, 3. cloud_firestore, 4. firebase_storage, 5. provider, 6. google_fonts, 7. font_awesome_flutter, 8. intl, 9. cached_network_image.
* **Configuración Main:** Crea el `main.dart` inicializando `Firebase.initializeApp()` y configurando el `MultiProvider` para envolver la aplicación.

## 🎨 FASE 2: Diseño de Identidad Visual (ThemeData)

Implementa `lib/core/theme.dart`.

* **Colores:** Primario `#006064`, Acento `#00ACC1`, Fondo `#F5F7F7`.
* **Estilos:** Define `ThemeData` con Material 3, `ElevatedButtonTheme` con bordes redondeados de 20px, y decoraciones de `InputDecoration` con bordes turquesas finos. Usa `Poppins` como fuente global.

## 📊 FASE 3: Modelado de Objetos de Negocio (Modelos)

Crea las clases en `lib/models/` con métodos `fromMap` y `toMap` para Firebase:

* **Usuario:** UID, nombre, correo, dirección, rol (cliente/admin).
* **Inventario:** ID, nombre, marca, precio, stock, tipo (enum: Instrumento/Pieza), etiquetas de compatibilidad.
* **Venta:** ID, id_cliente, lista de productos (ID y cantidad), total, impuestos, fecha.
* **Empleado:** ID, nombre, cargo, meta de ventas.

## 📂 FASE 4: Arquitectura de Archivos y Carpetas

Estructura el proyecto de la siguiente manera:

```text
musicman/
├── lib/
│   ├── core/ (theme, constants, widgets_reutilizables)
│   ├── models/ (usuario, inventario, venta, empleado)
│   ├── providers/ (auth_provider, inventario_provider, carrito_provider)
│   ├── services/ (firebase_service)
│   ├── views/ (pantallas divididas por carpetas: auth, inicio, tienda)
│   └── main.dart

```

## 🔐 FASE 5: Servicio de Autenticación y Onboarding

* **PantallaBienvenida:** UI impactante con gradiente turquesa y logo central.
* **Servicio Auth:** Implementa Login y Registro en `lib/services/auth_service.dart`.
* **Logic:** Al registrarse, el usuario debe guardarse automáticamente en la colección `Usuarios` de Firestore.

## 📦 FASE 6: Gestión de Inventario Real-Time

* **InventarioProvider:** Usa `Streams` para escuchar cambios en Firestore.
* **Logica de Piezas:** Implementa un sistema de filtrado donde el usuario pueda alternar entre ver "Instrumentos" o "Piezas por Separado".
* **Detalle:** El modelo de "Pieza" debe mostrar campos de compatibilidad (ej: "Compatible con Guitarras Fender").

## 🛒 FASE 7: Lógica del Carrito de Compras

Desarrolla el `CarritoProvider`.

* **Funciones:** Agregar item, remover item, aumentar/disminuir cantidad.
* **Cálculos:** Debe calcular automáticamente el IVA (impuestos) y el total neto.
* **Persistencia:** El carrito debe ser accesible desde cualquier pantalla mediante el Provider.

## 🧾 FASE 8: Procesamiento de Ventas (Checkout)

* **Transacción:** Al confirmar compra, se debe: 1. Crear documento en `Ventas`, 2. Restar el stock en `Inventario`, 3. Vaciar el carrito.
* **Seguridad:** Validar que haya stock suficiente antes de procesar el pago.

## 👥 FASE 9: Panel de Administración y Empleados

* **Vista Empleados:** Acceso exclusivo para usuarios con rol 'admin' o 'empleado'.
* **Funciones:** Formulario para agregar nuevos instrumentos/piezas y visualización de la tabla de `Proveedores` para gestión de pedidos de stock.

## 📖 FASE 10: Documentación para GitHub (README)

Genera el contenido del `README.md`.

* **Secciones:** Título, Tecnologías, Estructura del proyecto, Diccionario de Datos (colecciones de Firestore en español) e instrucciones de despliegue.

---

**RESTRICCIÓN DE ENTREGA:** Genera el código de cada fase de forma clara. Si el código es muy largo, detente al final de cada fase y espera mi confirmación para continuar. Prioriza la funcionalidad y el cumplimiento estricto de los colores turquesas.
