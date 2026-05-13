Aquí tienes el **Super Prompt Definitivo**, ajustado con la estructura de carpetas técnica que solicitaste y organizado por fases para una ejecución perfecta por parte de la IA.

---

# Super Prompt: Arquitecto de Software "MUSICMAN"

**Rol:** Actúa como un **Lead Full-Stack Developer** experto en Dart, Flutter y Firebase. Tu misión es desarrollar la arquitectura y el código base de "MUSICMAN", una E-commerce avanzada de instrumentos y refacciones técnicas.

---

## 🏗️ FASE 1: Configuración de Entorno y Dependencias

Genera el archivo `pubspec.yaml` asegurando la compatibilidad con **Antigravity/VS Code**. Incluye:

* **Firebase:** `firebase_core`, `firebase_auth`, `cloud_firestore`.
* **Estado & UI:** `provider`, `google_fonts`, `font_awesome_flutter`, `intl`.
* **Assets:** Define la estructura para `/assets/images/` y `/assets/icons/`.

---

## 🎨 FASE 2: Diseño Visual (Dark Turquoise Premium)

Configura el archivo `lib/core/theme.dart` con los siguientes parámetros:

* **Paleta de Colores:** Primario: `#006064` (Turquesa Oscuro), Acento: `#00ACC1` (Turquesa Vibrante), Fondo: `#F5F7F7`.
* **Estética:** Material 3, `borderRadius: 20` en todos los Cards y Buttons, y fuentes de `GoogleFonts.poppins()`.
* **Flujo de Pantallas:**
1. `WelcomeScreen`: Pantalla de entrada con gradiente turquesa, logo y botón de acción.
2. `AuthScreen`: Switcher elegante entre Login y Registro con Firebase Auth.
3. `MainShell`: Scaffold con `BottomNavigationBar` (Home, Catálogo de Piezas, Carrito, Perfil).



---

## 📊 FASE 3: Modelado de Datos (Firestore)

Genera las clases en `lib/models/` basadas en las siguientes estructuras de colecciones:

| Colección | Campos Principales | Propósito |
| --- | --- | --- |
| **Users** | `uid`, `name`, `email`, `address`, `role` | Clientes y Admins. |
| **Employees** | `emp_id`, `name`, `role`, `sales_count`, `active` | Gestión de staff y ventas. |
| **Inventory** | `id`, `name`, `brand`, `price`, `stock`, `type (Unit/Part)`, `tags` | Stock de instrumentos y piezas. |
| **Suppliers** | `id`, `company`, `contact_phone`, `category` | Proveedores de repuestos. |
| **Sales** | `id`, `client_id`, `items[]`, `total`, `tax`, `timestamp` | Histórico de transacciones. |

---

## 📂 FASE 4: Estructura de Carpetas

Genera el código asumiendo exactamente esta jerarquía de archivos:

```text
musicman/
├── android/app/google-services.json
├── assets/images/
├── lib/
│   ├── core/
│   │   ├── constants/
│   │   └── widgets/
│   ├── models/
│   ├── providers/
│   ├── services/
│   ├── views/
│   │   ├── welcome_screen.dart
│   │   ├── auth/
│   │   ├── home/
│   │   ├── inventory/
│   │   └── sales/
│   └── main.dart
├── pubspec.yaml
└── README.md

```

---

## 🚀 FASE 5: Entregables de Repositorio

1. **Código `main.dart**`: Inicialización de Firebase, configuración de `ThemeData` turquesa y rutas.
2. **Lógica de Negocio**: Implementación de `FirebaseService` para CRUD y `Provider` para el carrito.
3. **Archivo `README.md` para GitHub**:
* Descripción técnica de "MUSICMAN".
* Guía de instalación de Firebase y dependencias.
* Diccionario de datos de las colecciones.



---

**INSTRUCCIÓN FINAL:**
Proporciona el código de forma **modular**. No mezcles la lógica de negocio con la UI. Asegúrate de que la sección de **"Piezas Técnicas"** tenga filtros específicos. El resultado debe ser una app lista para producción, visualmente impactante y totalmente funcional en VS Code / Antigravity.
