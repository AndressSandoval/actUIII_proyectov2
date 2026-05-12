# 🎵 MUSICMAN — Proyecto Full Stack Flutter + Firebase

Aplicación profesional desarrollada con Flutter y Firebase para una tienda de:

* Instrumentos musicales
* Refacciones
* Accesorios
* Piezas técnicas

---

# PASO 1 — pubspec.yaml

```yaml id="fd8r7a"
name: musicman

description: Aplicación profesional para tienda de instrumentos musicales.

publish_to: 'none'

version: 1.0.0+1

environment:
  sdk: ">=3.3.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.6

  # Firebase
  firebase_core: ^3.3.0
  firebase_auth: ^5.1.4
  cloud_firestore: ^5.2.1

  # Estado
  provider: ^6.1.2

  # UI
  google_fonts: ^6.2.1
  intl: ^0.19.0

dev_dependencies:
  flutter_test:
    sdk: flutter

  flutter_lints: ^4.0.0

flutter:
  uses-material-design: true

  assets:
    - assets/images/
```

---

# PASO 2 — Estructura Profesional

```bash id="mz3n0d"
lib/
│
├── core/
│   └── tema.dart
│
├── modelos/
│   ├── producto_modelo.dart
│   ├── empleado_modelo.dart
│   ├── venta_modelo.dart
│   └── proveedor_modelo.dart
│
├── servicios/
│   └── firebase_servicio.dart
│
├── providers/
│   ├── auth_provider.dart
│   ├── producto_provider.dart
│   └── carrito_provider.dart
│
├── vistas/
│   ├── welcome_screen.dart
│   ├── auth_screen.dart
│   ├── inicio_screen.dart
│   ├── catalogo_screen.dart
│   ├── carrito_screen.dart
│   ├── perfil_screen.dart
│   └── main_shell.dart
│
├── widgets/
│   ├── tarjeta_producto.dart
│   └── boton_personalizado.dart
│
├── firebase_options.dart
│
└── main.dart
```

---

# PASO 3 — Tema Global

# lib/core/tema.dart

```dart id="dz7t6u"
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

class TemaApp {

  static const Color principal = Color(0xFF006064);
  static const Color secundario = Color(0xFF00ACC1);
  static const Color fondo = Color(0xFFF5F7F7);

  static ThemeData temaClaro = ThemeData(
    useMaterial3: true,

    scaffoldBackgroundColor: fondo,

    colorScheme: ColorScheme.fromSeed(
      seedColor: principal,
      primary: principal,
      secondary: secundario,
    ),

    textTheme: GoogleFonts.poppinsTextTheme(),

    appBarTheme: const AppBarTheme(
      centerTitle: true,
      elevation: 0,
      backgroundColor: principal,
      foregroundColor: Colors.white,
    ),

    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        backgroundColor: principal,
        foregroundColor: Colors.white,

        elevation: 4,

        padding: const EdgeInsets.symmetric(
          horizontal: 28,
          vertical: 18,
        ),

        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(20),
        ),
      ),
    ),

    inputDecorationTheme: InputDecorationTheme(
      filled: true,
      fillColor: Colors.white,

      border: OutlineInputBorder(
        borderRadius: BorderRadius.circular(20),
        borderSide: BorderSide.none,
      ),
    ),

    cardTheme: CardTheme(
      elevation: 4,

      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(20),
      ),
    ),
  );
}
```

---

# PASO 4 — main.dart

```dart id="g0zytx"
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:provider/provider.dart';

import 'core/tema.dart';
import 'firebase_options.dart';

import 'providers/producto_provider.dart';
import 'providers/carrito_provider.dart';

import 'vistas/welcome_screen.dart';

void main() async {

  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(const MusicManApp());
}

class MusicManApp extends StatelessWidget {
  const MusicManApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MultiProvider(
      providers: [

        ChangeNotifierProvider(
          create: (_) => ProductoProvider(),
        ),

        ChangeNotifierProvider(
          create: (_) => CarritoProvider(),
        ),

      ],

      child: MaterialApp(
        debugShowCheckedModeBanner: false,

        title: 'MUSICMAN',

        theme: TemaApp.temaClaro,

        home: const WelcomeScreen(),
      ),
    );
  }
}
```

---

# PASO 5 — Modelo Producto

# lib/modelos/producto_modelo.dart

```dart id="i8a72p"
class Producto {

  final String id;
  final String nombre;
  final String tipo;
  final int stock;
  final double precio;
  final String marca;
  final List<String> etiquetasCompatibilidad;

  Producto({
    required this.id,
    required this.nombre,
    required this.tipo,
    required this.stock,
    required this.precio,
    required this.marca,
    required this.etiquetasCompatibilidad,
  });

  factory Producto.fromMap(
      Map<String, dynamic> datos,
      String documentoId,
      ) {

    return Producto(
      id: documentoId,
      nombre: datos['nombre'],
      tipo: datos['tipo'],
      stock: datos['stock'],
      precio: datos['precio'].toDouble(),
      marca: datos['marca'],
      etiquetasCompatibilidad:
      List<String>.from(datos['etiquetas_compatibilidad']),
    );
  }

  Map<String, dynamic> toMap() {

    return {
      'nombre': nombre,
      'tipo': tipo,
      'stock': stock,
      'precio': precio,
      'marca': marca,
      'etiquetas_compatibilidad':
      etiquetasCompatibilidad,
    };
  }
}
```

---

# PASO 6 — Modelo Empleado

# lib/modelos/empleado_modelo.dart

```dart id="4n53ea"
class Empleado {

  final String id;
  final String nombre;
  final String rol;
  final String estado;
  final int cantidadVentas;

  Empleado({
    required this.id,
    required this.nombre,
    required this.rol,
    required this.estado,
    required this.cantidadVentas,
  });

  factory Empleado.fromMap(
      Map<String, dynamic> datos,
      String documentoId,
      ) {

    return Empleado(
      id: documentoId,
      nombre: datos['nombre'],
      rol: datos['rol'],
      estado: datos['estado'],
      cantidadVentas: datos['cantidad_ventas'],
    );
  }

  Map<String, dynamic> toMap() {

    return {
      'nombre': nombre,
      'rol': rol,
      'estado': estado,
      'cantidad_ventas': cantidadVentas,
    };
  }
}
```

---

# PASO 7 — Modelo Venta

# lib/modelos/venta_modelo.dart

```dart id="l6v9d1"
class Venta {

  final String id;
  final String idCliente;
  final String idEmpleado;

  final List productos;

  final double subtotal;
  final double impuesto;
  final double total;

  final DateTime fecha;

  Venta({
    required this.id,
    required this.idCliente,
    required this.idEmpleado,
    required this.productos,
    required this.subtotal,
    required this.impuesto,
    required this.total,
    required this.fecha,
  });

  Map<String, dynamic> toMap() {

    return {
      'id_cliente': idCliente,
      'id_empleado': idEmpleado,
      'lista_productos': productos,
      'subtotal': subtotal,
      'impuesto': impuesto,
      'total': total,
      'fecha': fecha,
    };
  }
}
```

---

# PASO 8 — Modelo Proveedor

# lib/modelos/proveedor_modelo.dart

```dart id="x1ot1k"
class Proveedor {

  final String id;
  final String empresa;
  final String contacto;
  final String categoria;

  Proveedor({
    required this.id,
    required this.empresa,
    required this.contacto,
    required this.categoria,
  });

  Map<String, dynamic> toMap() {

    return {
      'empresa': empresa,
      'contacto': contacto,
      'categoria': categoria,
    };
  }
}
```

---

# PASO 9 — Firebase Servicio

# lib/servicios/firebase_servicio.dart

```dart id="14u22u"
import 'package:cloud_firestore/cloud_firestore.dart';

class FirebaseServicio {

  final FirebaseFirestore firestore =
  FirebaseFirestore.instance;

  // PRODUCTOS

  Future<void> agregarProducto(
      Map<String, dynamic> datos,
      ) async {

    await firestore
        .collection('productos')
        .add(datos);
  }

  Stream<QuerySnapshot> obtenerProductos() {

    return firestore
        .collection('productos')
        .snapshots();
  }

  Future<void> actualizarProducto(
      String id,
      Map<String, dynamic> datos,
      ) async {

    await firestore
        .collection('productos')
        .doc(id)
        .update(datos);
  }

  Future<void> eliminarProducto(
      String id,
      ) async {

    await firestore
        .collection('productos')
        .doc(id)
        .delete();
  }

  // EMPLEADOS

  Stream<QuerySnapshot> obtenerEmpleados() {

    return firestore
        .collection('empleados')
        .snapshots();
  }

  // VENTAS

  Future<void> registrarVenta(
      Map<String, dynamic> datos,
      ) async {

    await firestore
        .collection('ventas')
        .add(datos);
  }

  // PROVEEDORES

  Stream<QuerySnapshot> obtenerProveedores() {

    return firestore
        .collection('proveedores')
        .snapshots();
  }
}
```

---

# PASO 10 — Provider Productos

# lib/providers/producto_provider.dart

```dart id="pn46ph"
import 'package:flutter/material.dart';

import '../modelos/producto_modelo.dart';

class ProductoProvider extends ChangeNotifier {

  final List<Producto> _productos = [];

  List<Producto> get productos => _productos;

  void agregarProducto(Producto producto) {

    _productos.add(producto);

    notifyListeners();
  }
}
```

---

# PASO 11 — Provider Carrito

# lib/providers/carrito_provider.dart

```dart id="qqqgw2"
import 'package:flutter/material.dart';

import '../modelos/producto_modelo.dart';

class CarritoProvider extends ChangeNotifier {

  final List<Producto> _carrito = [];

  List<Producto> get carrito => _carrito;

  double get total {

    double suma = 0;

    for (var producto in _carrito) {
      suma += producto.precio;
    }

    return suma;
  }

  void agregarAlCarrito(Producto producto) {

    _carrito.add(producto);

    notifyListeners();
  }

  void eliminarDelCarrito(Producto producto) {

    _carrito.remove(producto);

    notifyListeners();
  }
}
```

---

# PASO 12 — Welcome Screen

# lib/vistas/welcome_screen.dart

```dart id="w1z70v"
import 'package:flutter/material.dart';

import 'auth_screen.dart';

class WelcomeScreen extends StatelessWidget {
  const WelcomeScreen({super.key});

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      body: Container(

        width: double.infinity,

        decoration: const BoxDecoration(
          gradient: LinearGradient(
            colors: [
              Color(0xFF006064),
              Color(0xFF00ACC1),
            ],

            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
        ),

        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,

          children: [

            const Icon(
              Icons.music_note,
              size: 120,
              color: Colors.white,
            ),

            const SizedBox(height: 20),

            const Text(
              'MUSICMAN',

              style: TextStyle(
                fontSize: 42,
                fontWeight: FontWeight.bold,
                color: Colors.white,
              ),
            ),

            const SizedBox(height: 10),

            const Text(
              'Instrumentos • Accesorios • Refacciones',

              style: TextStyle(
                color: Colors.white70,
                fontSize: 18,
              ),
            ),

            const SizedBox(height: 50),

            ElevatedButton(

              onPressed: () {

                Navigator.push(
                  context,

                  MaterialPageRoute(
                    builder: (_) => const AuthScreen(),
                  ),
                );
              },

              child: const Text('Comenzar'),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

# PASO 13 — Auth Screen

# lib/vistas/auth_screen.dart

```dart id="xwwsga"
import 'package:flutter/material.dart';

class AuthScreen extends StatefulWidget {
  const AuthScreen({super.key});

  @override
  State<AuthScreen> createState() =>
      _AuthScreenState();
}

class _AuthScreenState extends State<AuthScreen>
    with SingleTickerProviderStateMixin {

  late TabController controladorTabs;

  @override
  void initState() {
    super.initState();

    controladorTabs =
        TabController(length: 2, vsync: this);
  }

  @override
  Widget build(BuildContext context) {

    return Scaffold(

      body: SafeArea(

        child: Padding(
          padding: const EdgeInsets.all(24),

          child: Column(

            children: [

              const SizedBox(height: 40),

              const Text(
                'Bienvenido',

                style: TextStyle(
                  fontSize: 32,
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 30),

              TabBar(
                controller: controladorTabs,

                tabs: const [
                  Tab(text: 'Login'),
                  Tab(text: 'Registro'),
                ],
              ),

              Expanded(

                child: TabBarView(
                  controller: controladorTabs,

                  children: [
                    loginWidget(),
                    registroWidget(),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget loginWidget() {

    return Column(

      children: [

        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: 'Correo',
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          obscureText: true,

          decoration: const InputDecoration(
            hintText: 'Contraseña',
          ),
        ),

        const SizedBox(height: 30),

        ElevatedButton(
          onPressed: () {},

          child: const Text('Ingresar'),
        ),
      ],
    );
  }

  Widget registroWidget() {

    return Column(

      children: [

        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: 'Nombre',
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: 'Correo',
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          obscureText: true,

          decoration: const InputDecoration(
            hintText: 'Contraseña',
          ),
        ),

        const SizedBox(height: 30),

        ElevatedButton(
          onPressed: () {},

          child: const Text('Crear Cuenta'),
        ),
      ],
    );
  }
}
```

---

# PASO 14 — Configurar Firebase

Ejecutar:

```bash id="wf0x3o"
flutterfire configure
```

Esto generará:

```bash id="wq7g55"
firebase_options.dart
```

---

# PASO 15 — README.md

````md id="x5a4tz"
# MUSICMAN

Aplicación profesional desarrollada con Flutter y Firebase para la gestión de instrumentos musicales, refacciones y accesorios.

---

## Tecnologías

- Flutter
- Firebase
- Firestore
- Provider
- Material 3

---

## Instalación

```bash
flutter pub get
flutterfire configure
flutter run
```

---

## Arquitectura

### Providers
Manejo global de estado.

### Servicios
Conexión con Firestore.

### Modelos
Estructura tipada de datos.

### Firestore
Base de datos en tiempo real.

---

## Colecciones

- productos
- empleados
- ventas
- proveedores

---

## Características

- Login y registro
- Catálogo moderno
- Carrito de compras
- CRUD Firestore
- UI Premium
- Diseño responsive
- Material 3
````
