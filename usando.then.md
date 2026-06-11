# Explicación Línea por Línea del Código Flutter para Consumir una API con MockAPI

## Introducción

Este proyecto desarrollado en Flutter permite consumir datos desde una API creada en MockAPI y mostrarlos en una lista dentro de la aplicación. Además, incluye un indicador de carga (`CircularProgressIndicator`) para informar al usuario que los datos se están obteniendo desde internet.

---

# 1. Importación de Librerías

```dart
import 'package:flutter/material.dart';
```

## Explicación

Importa la librería principal de Flutter para construir interfaces gráficas.

Permite utilizar widgets como:

* MaterialApp
* Scaffold
* AppBar
* Text
* ElevatedButton
* ListView
* ListTile

```dart
import 'dart:convert';
```

## Explicación

Importa herramientas para convertir datos JSON a objetos de Dart.

Se utilizará:

```dart
jsonDecode()
```

para convertir la respuesta de la API en una lista que Flutter pueda manejar.

```dart
import 'package:http/http.dart' as http;
```

## Explicación

Importa el paquete HTTP.

Permite realizar peticiones a servidores web utilizando métodos como:

* GET
* POST
* PUT
* DELETE

El alias `http` se utiliza para acceder a sus métodos.

### Ejemplo

```dart
http.get(...)
```

---

# 2. Función Principal

```dart
void main() {
  runApp(const MyApp());
}
```

## Explicación

Es el punto de entrada de toda aplicación Flutter.

### Línea por línea

```dart
void main()
```

Función principal que se ejecuta primero.

```dart
runApp(...)
```

Inicia la aplicación Flutter.

```dart
const MyApp()
```

Crea una instancia del widget principal.

---

# 3. Widget Principal

```dart
class MyApp extends StatelessWidget {
```

## Explicación

Se crea una clase llamada `MyApp`.

Hereda de:

```dart
StatelessWidget
```

porque no necesita almacenar estados.

```dart
const MyApp({super.key});
```

Constructor de la clase.

```dart
@override
Widget build(BuildContext context)
```

Método encargado de construir la interfaz.

```dart
return const MaterialApp(
```

Crea la aplicación utilizando Material Design.

```dart
home: PantallaUsuarios(),
```

Define la primera pantalla que verá el usuario.

---

# 4. Creación de la Pantalla de Usuarios

```dart
class PantallaUsuarios extends StatefulWidget {
```

## Explicación

Se crea una pantalla con estado.

Se utiliza `StatefulWidget` porque los datos cambiarán cuando lleguen desde la API.

```dart
const PantallaUsuarios({super.key});
```

Constructor de la pantalla.

```dart
@override
State<PantallaUsuarios> createState()
```

Genera el estado asociado al widget.

```dart
return _PantallaUsuariosState();
```

Devuelve la clase donde se manejarán los datos y cambios visuales.

---

# 5. Clase de Estado

```dart
class _PantallaUsuariosState extends State<PantallaUsuarios> {
```

## Explicación

Aquí se controla toda la lógica de la pantalla.

---

# 6. Lista de Usuarios

```dart
List usuarios = [];
```

## Explicación

Crea una lista vacía donde se almacenarán los usuarios obtenidos desde la API.

Inicialmente:

```dart
[]
```

está vacía.

Después de consumir la API contendrá algo similar a:

```json
[
  {
    "id":"1",
    "name":"Juan",
    "email":"juan@gmail.com"
  }
]
```

---

# 7. Variable de Carga

```dart
bool cargando = false;
```

## Explicación

Variable booleana que indica si la aplicación está obteniendo información.

### Valores posibles

```dart
true
```

Está cargando.

```dart
false
```

No está cargando.

---

# 8. Método Obtener Datos

```dart
Future<void> obtenerDatos() async
```

## Explicación

Método encargado de conectarse a la API.

---

# 9. Activar Indicador de Carga

```dart
setState(() {
  cargando = true;
});
```

## Explicación

Actualiza la interfaz.

Indica que comenzó la carga de datos.

```dart
setState()
```

Le informa a Flutter que debe reconstruir la pantalla.

---

# 10. Solicitud GET

```dart
final response = await http.get(
```

## Explicación

Realiza una petición HTTP GET.

```dart
Uri.parse(
  "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
)
```

Convierte la URL en un objeto URI válido.

La URL apunta al endpoint:

```text
https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios
```

---

# 11. Procesar Respuesta

```dart
if (response.statusCode == 200)
```

## Explicación

Comprueba si la petición fue exitosa.

### Código

```text
200 = OK
```

---

# 12. Actualizar Interfaz

```dart
setState(() {
```

Permite modificar variables y actualizar la pantalla.

---

# 13. Convertir JSON a Lista

```dart
usuarios = jsonDecode(response.body);
```

## Explicación

Convierte el JSON recibido en una lista de Dart.

### JSON recibido

```json
[
  {
    "id":"1",
    "name":"Carlos",
    "email":"carlos@gmail.com"
  }
]
```

### Resultado

```dart
usuarios[0]["name"]
```

Retorna:

```text
Carlos
```

---

# 14. Ocultar Indicador de Carga

```dart
cargando = false;
```

## Explicación

Indica que la carga terminó.

---

# 15. Método Build

```dart
@override
Widget build(BuildContext context)
```

## Explicación

Construye toda la interfaz de usuario.

---

# 16. Scaffold

```dart
return Scaffold(
```

## Explicación

Estructura principal de la pantalla.

---

# 17. Barra Superior

```dart
appBar: AppBar(
```

Crea la barra superior.

```dart
title: const Text("MockAPI"),
```

Muestra el título:

```text
MockAPI
```

---

# 18. Cuerpo de la Pantalla

```dart
body: Column(
```

Organiza widgets verticalmente.

---

# 19. Botón Obtener Usuarios

```dart
ElevatedButton(
```

Crea un botón elevado.

```dart
onPressed: obtenerDatos,
```

Cuando se presiona ejecuta:

```dart
obtenerDatos()
```

```dart
child: const Text(
  "Obtener Usuarios",
),
```

Texto visible en el botón.

---

# 20. Expanded

```dart
Expanded(
```

Permite que el contenido ocupe todo el espacio restante.

---

# 21. Operador Ternario

```dart
child: cargando
```

## Explicación

Evalúa la variable:

```dart
cargando
```

Si es:

```dart
true
```

muestra:

```dart
CircularProgressIndicator()
```

Si es:

```dart
false
```

muestra:

```dart
ListView.builder()
```

---

# 22. Indicador de Carga

```dart
Center(
  child: CircularProgressIndicator(),
)
```

## Explicación

Muestra una animación circular indicando que la aplicación está esperando datos.

---

# 23. Lista Dinámica

```dart
ListView.builder(
```

## Explicación

Genera una lista de elementos dinámicamente.

```dart
itemCount: usuarios.length,
```

Cantidad de usuarios obtenidos.

---

# 24. Constructor de Elementos

```dart
itemBuilder: (context, index)
```

## Explicación

Construye cada fila de la lista.

---

# 25. ListTile

```dart
return ListTile(
```

## Explicación

Representa una fila de información.

---

# 26. Nombre del Usuario

```dart
title: Text(
  usuarios[index]["name"].toString(),
),
```

## Explicación

Muestra el nombre.

### Ejemplo

```text
Juan Pérez
```

---

# 27. Información Adicional

```dart
subtitle: Text(
```

Muestra información secundaria.

```dart
"Email: ${usuarios[index]["email"]}\n"
```

Muestra el correo electrónico.

```dart
"ID: ${usuarios[index]["id"]}"
```

Muestra el ID del usuario.

### Resultado visual

```text
Juan Pérez

Email: juan@gmail.com
ID: 1
```

---

# Flujo Completo del Programa

```text
Usuario presiona botón
            ↓
obtenerDatos()
            ↓
cargando = true
            ↓
Se muestra CircularProgressIndicator
            ↓
Petición GET a MockAPI
            ↓
Servidor responde
            ↓
jsonDecode(response.body)
            ↓
Usuarios guardados en la lista
            ↓
cargando = false
            ↓
Se oculta el loading
            ↓
ListView.builder muestra los usuarios
```
##Codigo completo sin usar Async
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

  // ==========================
  // NUEVO:
  // Variable que controla
  // si se muestra el círculo
  // de carga.
  // ==========================
  bool cargando = false;

  void obtenerDatos() {

    // ==========================
    // NUEVO:
    // Antes de llamar a la API
    // activamos el loading.
    // ==========================
    setState(() {
      cargando = true;
    });

    http.get(
      Uri.parse(
        "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
      ),
    )

    .then((response) {

      if (response.statusCode == 200) {

        setState(() {

          usuarios = jsonDecode(response.body);

          // ==========================
          // NUEVO:
          // Ocultamos el círculo
          // cuando llegan los datos.
          // ==========================
          cargando = false;

        });

      } else {

        setState(() {
          cargando = false;
        });

        print("Error: ${response.statusCode}");

      }

    })

    .catchError((error) {

      // ==========================
      // NUEVO:
      // Si ocurre un error,
      // también ocultamos
      // el indicador de carga.
      // ==========================
      setState(() {
        cargando = false;
      });

      print("Error: $error");

    });
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

            child: const Text(
              "Obtener Usuarios",
            ),
          ),

          Expanded(

            // ==========================
            // NUEVO:
            // Si cargando == true
            // mostramos el círculo.
            //
            // Si cargando == false
            // mostramos la lista.
            // ==========================
            child: cargando

                ? const Center(
                    child: CircularProgressIndicator(),
                  )

                : ListView.builder(

                    itemCount: usuarios.length,

                    itemBuilder: (context, index) {

                      return ListTile(

                        title: Text(
                          usuarios[index]["name"].toString(),
                        ),

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
