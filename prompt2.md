Prompt: Full-Stack Developer para App "MUSICMAN"
Rol: Actúa como un Arquitecto de Software Senior experto en Flutter, Dart y Firebase. Tu objetivo es generar el código completo de una aplicación profesional llamada "MUSICMAN", una tienda especializada en instrumentos musicales, accesorios y piezas técnicas (sin cursos).

🎨 1. Identidad Visual y UI/UX (Estilo Dark Turquoise)
Colores: Primario: #006064 (Turquesa Oscuro), Acento: #00ACC1 (Turquesa Vibrante), Fondo: #F5F7F7.

Widgets: Usa Material 3 con bordes redondeados (borderRadius: 20), elevaciones suaves y GoogleFonts.poppins.

Flujo de Pantallas:

WelcomeScreen: Logo, eslogan y botón "Comenzar" con gradientes turquesa.

AuthScreen: Login y Registro (Firebase Auth) integrados en una sola vista con pestañas.

MainShell: Navegación inferior (Home, Catálogo de Piezas, Carrito, Perfil).

📂 2. Estructura de Archivos (Organización Profesional)
Genera el código asumiendo esta estructura de carpetas:

lib/core/theme.dart: Configuración del tema turquesa.

lib/models/: Modelos para Product, Employee, Sale, Supplier.

lib/services/firebase_service.dart: Lógica CRUD para Firestore.

lib/providers/: Gestión de estado con el paquete Provider.

lib/views/: Todas las pantallas mencionadas.

📊 3. Base de Datos Firestore (Modelado Completo)
Crea modelos de datos que soporten las siguientes tablas/colecciones:

Employees: id, name, role (admin/sales), status, salesCount.

Products & Parts: id, name, type (instrument/part), stock, price, brand, compatibility_tags.

Sales: id, client_id, employee_id, items_list, subtotal, tax, total, timestamp.

Suppliers: id, company, contact, category.

🛠️ 4. Requerimientos Técnicos y Dependencias
Proporciona el contenido para:

pubspec.yaml: Incluir firebase_core, firebase_auth, cloud_firestore, provider, google_fonts, intl.

Antigravity/VS Code: Asegúrate de que el código incluya las inicializaciones necesarias de Firebase para que sea compatible con estas herramientas de desarrollo.

📝 5. Entregables Adicionales
Un archivo README.md profesional para GitHub con descripción, instalación y guía de arquitectura.

Código de main.dart con la configuración del ThemeData personalizado.

Instrucción Final: Proporciona el código de manera estructurada, funcional y sin errores, priorizando que la interfaz se vea atractiva (UI Premium) y que la lógica de las piezas musicales esté claramente separada de los instrumentos completos.
