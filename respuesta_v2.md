# Arquitectura Profesional para Flutter + Firebase — Proyecto “MUSICMAN”

---

# 📦 1. Archivo `pubspec.yaml`

```yaml
name: musicman
description: E-commerce profesional de instrumentos y piezas técnicas.
publish_to: 'none'

version: 1.0.0+1

environment:
  sdk: ">=3.3.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter

  # Firebase
  firebase_core: ^3.1.0
  firebase_auth: ^5.1.0
  cloud_firestore: ^5.0.1

  # Estado y UI
  provider: ^6.1.2
  google_fonts: ^6.2.1
  font_awesome_flutter: ^10.7.0
  intl: ^0.19.0

  cupertino_icons: ^1.0.6

dev_dependencies:
  flutter_test:
    sdk: flutter

  flutter_lints: ^4.0.0

flutter:
  uses-material-design: true

  assets:
    - assets/images/
    - assets/icons/
```

---

# 📁 2. Estructura Profesional del Proyecto

```text
musicman/
├── android/app/google-services.json
├── assets/
│   ├── images/
│   └── icons/
├── lib/
│   ├── core/
│   │   ├── constants/
│   │   ├── widgets/
│   │   └── theme.dart
│   │
│   ├── models/
│   │   ├── usuario_modelo.dart
│   │   ├── empleado_modelo.dart
│   │   ├── inventario_modelo.dart
│   │   ├── proveedor_modelo.dart
│   │   └── venta_modelo.dart
│   │
│   ├── providers/
│   │   └── carrito_provider.dart
│   │
│   ├── services/
│   │   ├── firebase_service.dart
│   │   └── autenticacion_service.dart
│   │
│   ├── views/
│   │   ├── pantalla_bienvenida.dart
│   │   ├── autenticacion/
│   │   │   └── pantalla_autenticacion.dart
│   │   ├── inicio/
│   │   │   └── pantalla_inicio.dart
│   │   ├── inventario/
│   │   │   └── pantalla_inventario.dart
│   │   └── ventas/
│   │       └── pantalla_carrito.dart
│   │
│   └── main.dart
│
├── pubspec.yaml
└── README.md
```

---

# 🎨 3. `lib/core/theme.dart`

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

class AppTheme {

  static const Color colorPrimario = Color(0xFF006064);
  static const Color colorAcento = Color(0xFF00ACC1);
  static const Color colorFondo = Color(0xFFF5F7F7);

  static ThemeData temaPrincipal = ThemeData(
    useMaterial3: true,

    scaffoldBackgroundColor: colorFondo,

    colorScheme: ColorScheme.fromSeed(
      seedColor: colorPrimario,
      primary: colorPrimario,
      secondary: colorAcento,
      background: colorFondo,
    ),

    textTheme: GoogleFonts.poppinsTextTheme(),

    appBarTheme: const AppBarTheme(
      backgroundColor: colorPrimario,
      foregroundColor: Colors.white,
      centerTitle: true,
      elevation: 0,
    ),

    cardTheme: CardTheme(
      color: Colors.white,
      elevation: 3,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(20),
      ),
    ),

    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        backgroundColor: colorPrimario,
        foregroundColor: Colors.white,
        padding: const EdgeInsets.symmetric(
          horizontal: 24,
          vertical: 16,
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
      ),
    ),
  );
}
```

---

# 🚀 4. `main.dart`

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:provider/provider.dart';

import 'core/theme.dart';
import 'providers/carrito_provider.dart';

import 'views/pantalla_bienvenida.dart';
import 'views/autenticacion/pantalla_autenticacion.dart';
import 'views/inicio/pantalla_inicio.dart';

void main() async {

  WidgetsFlutterBinding.ensureInitialized();

  await Firebase.initializeApp();

  runApp(const MusicmanApp());
}

class MusicmanApp extends StatelessWidget {
  const MusicmanApp({super.key});

  @override
  Widget build(BuildContext context) {

    return MultiProvider(
      providers: [
        ChangeNotifierProvider(
          create: (_) => CarritoProvider(),
        ),
      ],

      child: MaterialApp(
        debugShowCheckedModeBanner: false,

        title: 'MUSICMAN',

        theme: AppTheme.temaPrincipal,

        initialRoute: '/',

        routes: {
          '/': (context) => const PantallaBienvenida(),
          '/autenticacion': (context) => const PantallaAutenticacion(),
          '/inicio': (context) => const PantallaInicio(),
        },
      ),
    );
  }
}
```

---

# 🧠 5. Modelos Firestore (`lib/models/`)

## `usuario_modelo.dart`

```dart
class UsuarioModelo {

  final String uid;
  final String nombre;
  final String email;
  final String direccion;
  final String rol;

  UsuarioModelo({
    required this.uid,
    required this.nombre,
    required this.email,
    required this.direccion,
    required this.rol,
  });

  Map<String, dynamic> toMap() {
    return {
      'uid': uid,
      'nombre': nombre,
      'email': email,
      'direccion': direccion,
      'rol': rol,
    };
  }

  factory UsuarioModelo.fromMap(Map<String, dynamic> mapa) {

    return UsuarioModelo(
      uid: mapa['uid'],
      nombre: mapa['nombre'],
      email: mapa['email'],
      direccion: mapa['direccion'],
      rol: mapa['rol'],
    );
  }
}
```

---

## `inventario_modelo.dart`

```dart
class InventarioModelo {

  final String id;
  final String nombre;
  final String marca;
  final double precio;
  final int stock;
  final String tipo;
  final List<dynamic> etiquetas;

  InventarioModelo({
    required this.id,
    required this.nombre,
    required this.marca,
    required this.precio,
    required this.stock,
    required this.tipo,
    required this.etiquetas,
  });

  Map<String, dynamic> toMap() {

    return {
      'id': id,
      'nombre': nombre,
      'marca': marca,
      'precio': precio,
      'stock': stock,
      'tipo': tipo,
      'etiquetas': etiquetas,
    };
  }

  factory InventarioModelo.fromMap(Map<String, dynamic> mapa) {

    return InventarioModelo(
      id: mapa['id'],
      nombre: mapa['nombre'],
      marca: mapa['marca'],
      precio: mapa['precio'],
      stock: mapa['stock'],
      tipo: mapa['tipo'],
      etiquetas: mapa['etiquetas'],
    );
  }
}
```

---

## `venta_modelo.dart`

```dart
class VentaModelo {

  final String id;
  final String idCliente;
  final List<dynamic> listaItems;
  final double total;
  final double impuestos;
  final DateTime fechaHora;

  VentaModelo({
    required this.id,
    required this.idCliente,
    required this.listaItems,
    required this.total,
    required this.impuestos,
    required this.fechaHora,
  });

  Map<String, dynamic> toMap() {

    return {
      'id': id,
      'id_cliente': idCliente,
      'lista_items': listaItems,
      'total': total,
      'impuestos': impuestos,
      'fecha_hora': fechaHora.toIso8601String(),
    };
  }
}
```

---

# 🔥 6. Servicio Firebase (`services/firebase_service.dart`)

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirebaseService {

  final FirebaseFirestore firestore =
      FirebaseFirestore.instance;

  // CREAR
  Future<void> crearDocumento({
    required String coleccion,
    required String id,
    required Map<String, dynamic> datos,
  }) async {

    await firestore
        .collection(coleccion)
        .doc(id)
        .set(datos);
  }

  // LEER
  Stream<QuerySnapshot> obtenerColeccion(
      String coleccion) {

    return firestore
        .collection(coleccion)
        .snapshots();
  }

  // ACTUALIZAR
  Future<void> actualizarDocumento({
    required String coleccion,
    required String id,
    required Map<String, dynamic> datos,
  }) async {

    await firestore
        .collection(coleccion)
        .doc(id)
        .update(datos);
  }

  // ELIMINAR
  Future<void> eliminarDocumento({
    required String coleccion,
    required String id,
  }) async {

    await firestore
        .collection(coleccion)
        .doc(id)
        .delete();
  }
}
```

---

# 🛒 7. Provider del Carrito (`providers/carrito_provider.dart`)

```dart
import 'package:flutter/material.dart';

class CarritoProvider extends ChangeNotifier {

  final List<Map<String, dynamic>> _items = [];

  List<Map<String, dynamic>> get items => _items;

  void agregarProducto(
      Map<String, dynamic> producto) {

    _items.add(producto);

    notifyListeners();
  }

  void eliminarProducto(int index) {

    _items.removeAt(index);

    notifyListeners();
  }

  double get total {

    double suma = 0;

    for (var item in _items) {
      suma += item['precio'];
    }

    return suma;
  }
}
```

---

# 🌊 8. `PantallaBienvenida`

```dart
import 'package:flutter/material.dart';

class PantallaBienvenida extends StatelessWidget {

  const PantallaBienvenida({super.key});

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

            Image.asset(
              'assets/images/logo.png',
              height: 150,
            ),

            const SizedBox(height: 30),

            const Text(
              'MUSICMAN',
              style: TextStyle(
                color: Colors.white,
                fontSize: 38,
                fontWeight: FontWeight.bold,
              ),
            ),

            const SizedBox(height: 20),

            ElevatedButton(
              onPressed: () {

                Navigator.pushNamed(
                  context,
                  '/autenticacion',
                );
              },

              child: const Text(
                'Comenzar',
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

# 🎼 9. Pantalla de Inventario con Filtro de Piezas Técnicas

```dart
DropdownButton<String>(
  value: filtroSeleccionado,

  items: const [

    DropdownMenuItem(
      value: 'Todos',
      child: Text('Todos'),
    ),

    DropdownMenuItem(
      value: 'Instrumento',
      child: Text('Instrumentos'),
    ),

    DropdownMenuItem(
      value: 'Pieza',
      child: Text('Piezas Técnicas'),
    ),
  ],

  onChanged: (valor) {

    filtroSeleccionado = valor!;
  },
)
```

---

# 🔐 10. Firebase Auth (`autenticacion_service.dart`)

```dart
import 'package:firebase_auth/firebase_auth.dart';

class AutenticacionService {

  final FirebaseAuth auth =
      FirebaseAuth.instance;

  Future<User?> iniciarSesion({
    required String email,
    required String password,
  }) async {

    UserCredential credencial =
        await auth.signInWithEmailAndPassword(
      email: email,
      password: password,
    );

    return credencial.user;
  }

  Future<User?> registrarUsuario({
    required String email,
    required String password,
  }) async {

    UserCredential credencial =
        await auth.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );

    return credencial.user;
  }

  Future<void> cerrarSesion() async {

    await auth.signOut();
  }
}
```

---

# 📘 11. `README.md`

```md
# MUSICMAN

Aplicación E-commerce profesional desarrollada en Flutter y Firebase
para la venta de instrumentos musicales y piezas técnicas.

---

# Tecnologías

- Flutter
- Dart
- Firebase Auth
- Cloud Firestore
- Provider
- Material 3

---

# Instalación

## 1. Clonar repositorio

git clone URL_REPOSITORIO

## 2. Instalar dependencias

flutter pub get

## 3. Configurar Firebase

- Crear proyecto en Firebase
- Descargar google-services.json
- Colocarlo en:

android/app/google-services.json

## 4. Ejecutar proyecto

flutter run

---

# Colecciones Firestore

## Usuarios

- uid
- nombre
- email
- direccion
- rol

## Empleados

- id_empleado
- nombre
- cargo
- conteo_ventas
- activo

## Inventario

- id
- nombre
- marca
- precio
- stock
- tipo
- etiquetas

## Proveedores

- id
- empresa
- telefono_contacto
- categoria

## Ventas

- id
- id_cliente
- lista_items
- total
- impuestos
- fecha_hora

---

# Características

- Autenticación Firebase
- CRUD Firestore
- Carrito en tiempo real
- Gestión de inventario
- Filtro de piezas técnicas
- Diseño Premium Turquesa
```

---

# ✅ Arquitectura Final Recomendada

La aplicación queda organizada bajo principios:

* Separación UI / lógica de negocio.
* Arquitectura escalable.
* Compatible con VS Code + Antigravity.
* Preparada para producción.
* Firestore completamente modular.
* Navegación limpia y profesional.
* Gestión diferenciada entre instrumentos y piezas técnicas.
* Preparada para agregar:

  * Pasarela de pagos.
  * Panel administrador.
  * Analytics.
  * Notificaciones push.
  * Gestión avanzada de stock.
