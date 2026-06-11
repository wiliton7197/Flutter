# Explicación del Código Flutter para Consumir una API con MockAPI

## 1. Importaciones

```dart
import 'package:flutter/material.dart';
```

Esta librería importa todos los componentes visuales de Flutter. Gracias a ella podemos utilizar widgets como `MaterialApp`, `Scaffold`, `AppBar`, `Text`, `ElevatedButton` y `ListView`.

Sin esta librería Flutter no podría construir la interfaz gráfica de la aplicación.

```dart
import 'dart:convert';
```

Esta librería permite trabajar con datos en formato JSON. Cuando una API responde, generalmente devuelve la información en formato JSON. Para convertir esos datos a una estructura que Flutter pueda utilizar, se emplea la función `jsonDecode()`.

### Ejemplo de respuesta JSON

```json
[
  {
    "id": "1",
    "name": "Juan"
  }
]
```

```dart
import 'package:http/http.dart' as http;
```

Esta librería permite realizar peticiones HTTP a servidores o APIs.

Gracias a ella podemos utilizar métodos como:

* `http.get()` → Obtener información.
* `http.post()` → Enviar información.
* `http.put()` → Actualizar información.
* `http.delete()` → Eliminar información.

En este proyecto utilizamos `http.get()` para obtener usuarios desde MockAPI.

---

## 2. Método Principal

```dart
void main() {
  runApp(const MyApp());
}
```

El método `main()` es el punto de inicio de toda aplicación Flutter. Cuando ejecutamos el proyecto mediante `flutter run`, Flutter comienza la ejecución desde este método.

La función `runApp()` indica cuál será el widget principal de la aplicación.

---

## 3. Clase Principal

```dart
class MyApp extends StatelessWidget
```

Esta clase representa la aplicación principal.

Se utiliza `StatelessWidget` porque esta parte de la aplicación no cambia durante la ejecución.

### Constructor

```dart
const MyApp({super.key});
```

Permite crear una instancia del widget `MyApp`.

---

## 4. Método Build

```dart
Widget build(BuildContext context)
```

El método `build()` se encarga de construir la interfaz gráfica que verá el usuario.

```dart
return const MaterialApp(
```

Crea la aplicación utilizando Material Design.

```dart
home: PantallaUsuarios(),
```

Define que la primera pantalla mostrada será `PantallaUsuarios`.

---

## 5. StatefulWidget

```dart
class PantallaUsuarios extends StatefulWidget
```

Se utiliza `StatefulWidget` porque los datos de la pantalla pueden cambiar cuando se obtienen los usuarios desde Internet.

### Método createState()

```dart
State<PantallaUsuarios> createState()
```

Conecta el widget con su estado.

```dart
return _PantallaUsuariosState();
```

Crea el objeto que administrará los cambios de la pantalla.

---

## 6. Estado de la Pantalla

```dart
class _PantallaUsuariosState extends State<PantallaUsuarios>
```

Aquí se encuentra toda la lógica de la pantalla.

### Lista de usuarios

```dart
List usuarios = [];
```

Se crea una lista vacía para almacenar los usuarios obtenidos desde la API.

### Al iniciar

```dart
[]
```

### Después de obtener los datos

```dart
[Juan, María, Pedro]
```

---

## 7. Método Obtener Datos

```dart
Future<void> obtenerDatos() async
```

Esta función se encarga de solicitar información a la API.

### Petición HTTP

```dart
final response = await http.get(...)
```

Realiza una petición GET a la API.

Es equivalente a decir:

> "API, envíame la lista de usuarios."

### URL de la API

```dart
Uri.parse(...)
```

Convierte la dirección web en un formato válido para Flutter.

```text
https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios
```

Es la dirección donde se encuentran almacenados los usuarios en MockAPI.

---

## 8. Verificación de la Respuesta

```dart
if (response.statusCode == 200)
```

Verifica si la petición fue exitosa.

Algunos códigos comunes son:

* `200` → Correcto.
* `404` → Recurso no encontrado.
* `500` → Error interno del servidor.

---

## 9. Actualización de la Pantalla

```dart
setState(() {
```

Indica a Flutter que los datos han cambiado y que debe actualizar la interfaz.

### Conversión del JSON

```dart
usuarios = jsonDecode(response.body);
```

Convierte la respuesta JSON recibida desde la API en una lista que Flutter puede utilizar.

### Ejemplo

#### Antes

```dart
[]
```

#### Después

```dart
[
  {name: Juan},
  {name: María}
]
```

---

## 10. Manejo de Errores

```dart
else {
  print("Error: ${response.statusCode}");
}
```

Si ocurre algún problema, se muestra el código de error en la consola.

### Ejemplo

```text
Error: 404
```

---

## 11. Construcción de la Interfaz

```dart
return Scaffold(
```

El widget `Scaffold` proporciona la estructura básica de la pantalla.

---

## 12. Barra Superior

```dart
appBar: AppBar(
```

Crea una barra superior.

```dart
title: const Text("MockAPI"),
```

Muestra el título de la aplicación.

### Resultado

```text
MockAPI
```

---

## 13. Cuerpo de la Pantalla

```dart
body: Column(
```

Organiza los widgets verticalmente.

### Resultado

```text
Botón
Lista de usuarios
```

---

## 14. Botón

```dart
ElevatedButton(
```

Crea un botón.

```dart
onPressed: obtenerDatos,
```

Cuando el usuario presiona el botón, se ejecuta el método `obtenerDatos()`.

```dart
child: const Text("Obtener Usuarios"),
```

Texto mostrado en el botón.

### Resultado

```text
Obtener Usuarios
```

---

## 15. Widget Expanded

```dart
Expanded(
```

Permite que la lista ocupe el espacio disponible dentro de la pantalla.

---

## 16. ListView.builder

```dart
ListView.builder(
```

Genera automáticamente una lista dinámica.

```dart
itemCount: usuarios.length,
```

Cuenta la cantidad de usuarios obtenidos desde la API.

### Ejemplo

```text
Juan
María
Pedro
```

### Cantidad

```text
3 usuarios
```

---

## 17. itemBuilder

```dart
itemBuilder: (context, index)
```

Recorre la lista elemento por elemento.

### Primero

```text
Juan
```

### Después

```text
María
```

### Finalmente

```text
Pedro
```

---

## 18. ListTile

```dart
return ListTile(
```

Representa una fila dentro de la lista.

### Mostrar datos

```dart
title: Text(
  usuarios[index]["name"].toString(),
)
```

Obtiene el valor del campo `name` de cada usuario y lo muestra en pantalla.

### Ejemplo

```json
{
  "id": "1",
  "name": "Juan"
}
```

### Resultado mostrado

```text
Juan
```

---

# Codigo completo 
```dart
import 'package:flutter/material.dart';
import 'dart:convert';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: PantallaUsuarios(),
    );
  }
}

class PantallaUsuarios extends StatefulWidget {
  const PantallaUsuarios({super.key});

  @override
  State<PantallaUsuarios> createState() {
    return _PantallaUsuariosState();
  }
}

class _PantallaUsuariosState extends State<PantallaUsuarios> {
  List usuarios = [];

  Future<void> obtenerDatos() async {
    final response = await http.get(
      Uri.parse(
        "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
      ),
    );

    if (response.statusCode == 200) {
      setState(() {
        usuarios = jsonDecode(response.body);
      });
    } else {
      print("Error: ${response.statusCode}");
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("MockAPI"),
      ),
      body: Column(
        children: [
          ElevatedButton(
            onPressed: obtenerDatos,
            child: const Text("Obtener Usuarios"),
          ),
          Expanded(
            child: ListView.builder(
              itemCount: usuarios.length,
              itemBuilder: (context, index) {
                return ListTile(
                  title: Text(
                    usuarios[index]["name"].toString(),
                  ),
                  //PARA PODER MOSTARR MAS CAMPOS
                  subtitle: Text(
                    "Email: ${usuarios[index]["email"]}\n"
                    "ID: ${usuarios[index]["id"]}",
                  ),                 
                  
                );
              },
            ),
          ),
        ],
      ),
    );
  }
}
```

###Con cirulo cargando 
```dart
import 'package:flutter/material.dart';
import 'dart:convert';
import 'package:http/http.dart' as http;

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: PantallaUsuarios(),
    );
  }
}

class PantallaUsuarios extends StatefulWidget {
  const PantallaUsuarios({super.key});

  @override
  State<PantallaUsuarios> createState() {
    return _PantallaUsuariosState();
  }
}

class _PantallaUsuariosState extends State<PantallaUsuarios> {
  // LISTA DONDE SE GUARDAN LOS USUARIOS
  List usuarios = [];

  // VARIABLE PARA CONTROLAR LA CARGA
  bool cargando = false;

  Future<void> obtenerDatos() async {
    // MUESTRA EL CÍRCULO DE CARGA
    setState(() {
      cargando = true;
    });

    final response = await http.get(
      Uri.parse(
        "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
      ),
    );

    if (response.statusCode == 200) {
      setState(() {
        usuarios = jsonDecode(response.body);

        // OCULTA EL CÍRCULO CUANDO TERMINA LA CARGA
        cargando = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("MockAPI"),
      ),
      body: Column(
        children: [
          ElevatedButton(
            onPressed: obtenerDatos,
            child: const Text("Obtener Usuarios"),
          ),

          Expanded(
            child: cargando

                // SI ESTÁ CARGANDO, MUESTRA EL CÍRCULO
                ? const Center(
                    child: CircularProgressIndicator(),
                  )

                // SI YA TERMINÓ, MUESTRA LA LISTA
                : ListView.builder(
                    itemCount: usuarios.length,
                    itemBuilder: (context, index) {
                      return ListTile(
                        title: Text(
                          usuarios[index]["name"].toString(),
                        ),

                        // PARA MOSTRAR MÁS CAMPOS
                        subtitle: Text(
                          "Email: ${usuarios[index]["email"]}\n"
                          "ID: ${usuarios[index]["id"]}",
                        ),
                      );
                    },
                  ),
          ),
        ],
      ),
    );
  }
}
```



