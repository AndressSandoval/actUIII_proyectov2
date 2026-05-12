# Arquitectura Profesional para Flutter + Firebase

Proyecto: **MUSICMAN** — Tienda de Instrumentos, Accesorios y Piezas Técnicas

---

# 📦 pubspec.yaml

```yaml
name: musicman
description: Aplicación profesional para tienda de instrumentos y piezas musicales.
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

  # State Management
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

# 📂 Estructura Profesional

```bash
lib/
│
├── core/
│   └── theme.dart
│
├── models/
│   ├── product_model.dart
│   ├── employee_model.dart
│   ├── sale_model.dart
│   └── supplier_model.dart
│
├── providers/
│   ├── auth_provider.dart
│   ├── product_provider.dart
│   └── cart_provider.dart
│
├── services/
│   └── firebase_service.dart
│
├── views/
│   ├── welcome_screen.dart
│   ├── auth_screen.dart
│   ├── home_screen.dart
│   ├── catalog_screen.dart
│   ├── cart_screen.dart
│   ├── profile_screen.dart
│   └── main_shell.dart
│
├── widgets/
│   ├── product_card.dart
│   └── custom_button.dart
│
├── firebase_options.dart
│
└── main.dart
```

---

# 🎨 lib/core/theme.dart

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

class AppTheme {
  static const Color primary = Color(0xFF006064);
  static const Color accent = Color(0xFF00ACC1);
  static const Color background = Color(0xFFF5F7F7);

  static ThemeData lightTheme = ThemeData(
    useMaterial3: true,
    scaffoldBackgroundColor: background,

    colorScheme: ColorScheme.fromSeed(
      seedColor: primary,
      primary: primary,
      secondary: accent,
    ),

    textTheme: GoogleFonts.poppinsTextTheme(),

    cardTheme: CardTheme(
      elevation: 4,
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(20),
      ),
    ),

    elevatedButtonTheme: ElevatedButtonThemeData(
      style: ElevatedButton.styleFrom(
        padding: const EdgeInsets.symmetric(
          horizontal: 30,
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
  );
}
```

---

# 📱 main.dart

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';

import 'core/theme.dart';
import 'views/welcome_screen.dart';
import 'firebase_options.dart';

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
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'MUSICMAN',
      theme: AppTheme.lightTheme,
      home: const WelcomeScreen(),
    );
  }
}
```

---

# 🔥 Configuración Firebase (Compatible con Antigravity / VS Code)

## Comando de Inicialización

```bash
flutterfire configure
```

Esto generará automáticamente:

```bash
lib/firebase_options.dart
```

---

# 📦 MODELOS FIRESTORE

# lib/models/product_model.dart

```dart
class Product {
  final String id;
  final String name;
  final String type;
  final int stock;
  final double price;
  final String brand;
  final List<String> compatibilityTags;

  Product({
    required this.id,
    required this.name,
    required this.type,
    required this.stock,
    required this.price,
    required this.brand,
    required this.compatibilityTags,
  });

  factory Product.fromMap(Map<String, dynamic> data, String documentId) {
    return Product(
      id: documentId,
      name: data['name'],
      type: data['type'],
      stock: data['stock'],
      price: data['price'].toDouble(),
      brand: data['brand'],
      compatibilityTags:
          List<String>.from(data['compatibility_tags']),
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'name': name,
      'type': type,
      'stock': stock,
      'price': price,
      'brand': brand,
      'compatibility_tags': compatibilityTags,
    };
  }
}
```

---

# lib/models/employee_model.dart

```dart
class Employee {
  final String id;
  final String name;
  final String role;
  final String status;
  final int salesCount;

  Employee({
    required this.id,
    required this.name,
    required this.role,
    required this.status,
    required this.salesCount,
  });

  factory Employee.fromMap(Map<String, dynamic> data, String id) {
    return Employee(
      id: id,
      name: data['name'],
      role: data['role'],
      status: data['status'],
      salesCount: data['salesCount'],
    );
  }

  Map<String, dynamic> toMap() {
    return {
      'name': name,
      'role': role,
      'status': status,
      'salesCount': salesCount,
    };
  }
}
```

---

# lib/models/sale_model.dart

```dart
class Sale {
  final String id;
  final String clientId;
  final String employeeId;
  final List itemsList;
  final double subtotal;
  final double tax;
  final double total;
  final DateTime timestamp;

  Sale({
    required this.id,
    required this.clientId,
    required this.employeeId,
    required this.itemsList,
    required this.subtotal,
    required this.tax,
    required this.total,
    required this.timestamp,
  });

  Map<String, dynamic> toMap() {
    return {
      'client_id': clientId,
      'employee_id': employeeId,
      'items_list': itemsList,
      'subtotal': subtotal,
      'tax': tax,
      'total': total,
      'timestamp': timestamp,
    };
  }
}
```

---

# lib/models/supplier_model.dart

```dart
class Supplier {
  final String id;
  final String company;
  final String contact;
  final String category;

  Supplier({
    required this.id,
    required this.company,
    required this.contact,
    required this.category,
  });

  Map<String, dynamic> toMap() {
    return {
      'company': company,
      'contact': contact,
      'category': category,
    };
  }
}
```

---

# 🔥 Firebase Service CRUD

# lib/services/firebase_service.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';

class FirebaseService {
  final FirebaseFirestore _db = FirebaseFirestore.instance;

  // PRODUCTS
  Future<void> addProduct(Map<String, dynamic> data) async {
    await _db.collection('products').add(data);
  }

  Stream<QuerySnapshot> getProducts() {
    return _db.collection('products').snapshots();
  }

  Future<void> updateProduct(
      String id,
      Map<String, dynamic> data,
      ) async {
    await _db.collection('products').doc(id).update(data);
  }

  Future<void> deleteProduct(String id) async {
    await _db.collection('products').doc(id).delete();
  }

  // SALES
  Future<void> createSale(Map<String, dynamic> data) async {
    await _db.collection('sales').add(data);
  }

  // EMPLOYEES
  Stream<QuerySnapshot> getEmployees() {
    return _db.collection('employees').snapshots();
  }

  // SUPPLIERS
  Stream<QuerySnapshot> getSuppliers() {
    return _db.collection('suppliers').snapshots();
  }
}
```

---

# 👋 WelcomeScreen

# lib/views/welcome_screen.dart

```dart
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
              'Instrumentos • Accesorios • Piezas',
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
              child: const Text("Comenzar"),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

# 🔐 AuthScreen (Login/Register)

# lib/views/auth_screen.dart

```dart
import 'package:flutter/material.dart';

class AuthScreen extends StatefulWidget {
  const AuthScreen({super.key});

  @override
  State<AuthScreen> createState() => _AuthScreenState();
}

class _AuthScreenState extends State<AuthScreen>
    with SingleTickerProviderStateMixin {

  late TabController _tabController;

  @override
  void initState() {
    super.initState();

    _tabController = TabController(length: 2, vsync: this);
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
                "Bienvenido",
                style: TextStyle(
                  fontSize: 32,
                  fontWeight: FontWeight.bold,
                ),
              ),

              const SizedBox(height: 30),

              TabBar(
                controller: _tabController,
                tabs: const [
                  Tab(text: "Login"),
                  Tab(text: "Registro"),
                ],
              ),

              Expanded(
                child: TabBarView(
                  controller: _tabController,
                  children: [
                    buildLogin(),
                    buildRegister(),
                  ],
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Widget buildLogin() {
    return Column(
      children: [
        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: "Correo",
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          obscureText: true,
          decoration: const InputDecoration(
            hintText: "Contraseña",
          ),
        ),

        const SizedBox(height: 30),

        ElevatedButton(
          onPressed: () {},
          child: const Text("Ingresar"),
        ),
      ],
    );
  }

  Widget buildRegister() {
    return Column(
      children: [
        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: "Nombre",
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          decoration: const InputDecoration(
            hintText: "Correo",
          ),
        ),

        const SizedBox(height: 20),

        TextField(
          obscureText: true,
          decoration: const InputDecoration(
            hintText: "Contraseña",
          ),
        ),

        const SizedBox(height: 30),

        ElevatedButton(
          onPressed: () {},
          child: const Text("Crear Cuenta"),
        ),
      ],
    );
  }
}
```

---

# 🧭 MainShell

```dart
import 'package:flutter/material.dart';

import 'home_screen.dart';
import 'catalog_screen.dart';
import 'cart_screen.dart';
import 'profile_screen.dart';

class MainShell extends StatefulWidget {
  const MainShell({super.key});

  @override
  State<MainShell> createState() => _MainShellState();
}

class _MainShellState extends State<MainShell> {

  int currentIndex = 0;

  final screens = [
    const HomeScreen(),
    const CatalogScreen(),
    const CartScreen(),
    const ProfileScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: screens[currentIndex],

      bottomNavigationBar: NavigationBar(
        selectedIndex: currentIndex,

        onDestinationSelected: (value) {
          setState(() {
            currentIndex = value;
          });
        },

        destinations: const [
          NavigationDestination(
            icon: Icon(Icons.home),
            label: 'Home',
          ),

          NavigationDestination(
            icon: Icon(Icons.build),
            label: 'Piezas',
          ),

          NavigationDestination(
            icon: Icon(Icons.shopping_cart),
            label: 'Carrito',
          ),

          NavigationDestination(
            icon: Icon(Icons.person),
            label: 'Perfil',
          ),
        ],
      ),
    );
  }
}
```

---

# 🛒 Diferenciación Profesional: Instrumentos vs Piezas

La aplicación debe separar claramente:

| Tipo       | Descripción                                                              |
| ---------- | ------------------------------------------------------------------------ |
| instrument | Instrumentos completos como guitarras, baterías, teclados                |
| part       | Refacciones y piezas técnicas como pastillas, clavijas, puentes, cuerdas |

Esto permitirá:

* Filtros inteligentes
* Compatibilidad por tags
* Inventario técnico avanzado
* Recomendaciones automáticas
* Gestión de stock especializada

Ejemplo Firestore:

```json
{
  "name": "Pastilla Humbucker EMG",
  "type": "part",
  "brand": "EMG",
  "compatibility_tags": [
    "guitar",
    "electric",
    "humbucker"
  ]
}
```

---

# ⭐ Diseño UI Premium Recomendado

## HomeScreen

Incluye:

* Banner promocional
* Productos destacados
* Accesos rápidos
* Cards glassmorphism
* Categorías horizontales

---

# 🧠 Provider Architecture

```dart
ChangeNotifierProvider(
  create: (_) => ProductProvider(),
)
```

---

# 📘 README.md Profesional

````md
# MUSICMAN

Aplicación profesional desarrollada con Flutter y Firebase
para la gestión de instrumentos musicales, accesorios y piezas técnicas.

---

## Tecnologías

- Flutter
- Firebase Auth
- Cloud Firestore
- Provider
- Material 3

---

## Arquitectura

- Clean UI Structure
- Provider State Management
- Firebase Services Layer
- Firestore Collections

---

## Instalación

```bash
flutter pub get
flutterfire configure
flutter run
````

---

## Colecciones Firestore

### products

### employees

### sales

### suppliers

---

## Características

* Login y Registro
* Catálogo dinámico
* Gestión de piezas musicales
* Carrito de compras
* CRUD Firestore
* UI Premium Turquesa

```

---

# 🚀 Recomendaciones Finales

Para que el proyecto quede realmente profesional en :contentReference[oaicite:2]{index=2} o Antigravity:

## Agrega después:

- Animaciones con `flutter_animate`
- Skeleton Loaders
- Responsive Design
- Roles admin/sales
- Dashboard Analytics
- Escáner QR de inventario
- Cloud Functions
- Sistema POS
- Historial de ventas
- Dark Mode dinámico
- Upload de imágenes con Firebase Storage

---

# 🔥 Resultado Esperado

La app tendrá:

✅ Arquitectura escalable  
✅ Firebase listo  
✅ UI moderna premium  
✅ Separación profesional entre instrumentos y piezas  
✅ CRUD Firestore  
✅ Navegación moderna Material 3  
✅ Código limpio y mantenible  
✅ Compatible con VS Code y Antigravity  
✅ Diseño tipo aplicación comercial real
```
