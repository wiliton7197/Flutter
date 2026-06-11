# Flutter para Consumir una API con MockAPI sin usar async

## Introducción

Este proyecto desarrollado en Flutter permite consumir datos desde una API creada en MockAPI y mostrarlos en una lista dentro de la aplicación. Además, incluye un indicador de carga (`CircularProgressIndicator`) para informar al usuario que los datos se están obteniendo desde internet.

---

# 1. Importación de Librerías

```dart
import 'package:flutter/material.dart';
```

### Explicación

Importa la biblioteca principal de Flutter para construir interfaces gráficas.

Permite utilizar widgets como:

- MaterialApp
- Scaffold
- AppBar
- ElevatedButton
- ListView
- Text
- CircularProgressIndicator

---

```dart
import 'dart:convert';
```

### Explicación

Importa herramientas para trabajar con JSON.

Se utiliza principalmente para convertir la respuesta de la API en objetos que Flutter pueda manejar.

Ejemplo:

```dart
jsonDecode(response.body)
```

---

```dart
import 'package:http/http.dart' as http;
```

### Explicación

Importa la librería HTTP que permite realizar peticiones a servicios web.

El alias `http` facilita escribir:

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

### Explicación

Es el punto de entrada de toda aplicación Flutter.

### ¿Qué hace?

- Flutter inicia la ejecución desde aquí.
- `runApp()` carga el widget principal de la aplicación.

---

# 3. Clase Principal de la Aplicación

```dart
class MyApp extends StatelessWidget {
```

### Explicación

Se crea una clase llamada `MyApp`.

Hereda de:

```dart
StatelessWidget
```

Esto significa que no tiene estados que cambien durante la ejecución.

---

```dart
const MyApp({super.key});
```

### Explicación

Constructor de la clase.

Permite identificar el widget dentro del árbol de widgets de Flutter.

---

```dart
@override
Widget build(BuildContext context) {
```

### Explicación

El método `build()` construye visualmente el widget.

---

```dart
return const MaterialApp(
  home: PantallaUsuarios(),
);
```

### Explicación

Se crea una aplicación Material Design.

La pantalla inicial será:

```dart
PantallaUsuarios()
```

---

# 4. Widget Stateful para Manejar Datos

```dart
class PantallaUsuarios extends StatefulWidget {
```

### Explicación

Se utiliza un StatefulWidget porque los datos cambiarán después de llamar a la API.

---

```dart
const PantallaUsuarios({super.key});
```

### Explicación

Constructor de la pantalla.

---

```dart
@override
State<PantallaUsuarios> createState() {
  return _PantallaUsuariosState();
}
```

### Explicación

Asocia este widget con su estado.

---

# 5. Clase Estado

```dart
class _PantallaUsuariosState extends State<PantallaUsuarios> {
```

### Explicación

Aquí se almacenan los datos y la lógica de la aplicación.

---

# 6. Lista para Guardar Usuarios

```dart
List usuarios = [];
```

### Explicación

Se crea una lista vacía donde se almacenarán los usuarios obtenidos desde la API.

Ejemplo:

```json
[
  {
    "id": "1",
    "name": "Juan",
    "email": "juan@gmail.com"
  }
]
```

---

# 7. Variable de Carga

```dart
bool cargando = false;
```

### Explicación

Controla si el indicador de carga debe mostrarse.

Valores posibles:

| Valor | Significado |
|---------|------------|
| true | Está cargando |
| false | No está cargando |

---

# 8. Método Obtener Datos

```dart
void obtenerDatos() {
```

### Explicación

Método encargado de conectarse a la API.

---

# 9. Activar Indicador de Carga

```dart
setState(() {
  cargando = true;
});
```

### Explicación

Antes de llamar a la API se activa el indicador de carga.

---

# 10. Petición GET

```dart
http.get(
```

### Explicación

Realiza una solicitud GET al servidor.

---

```dart
Uri.parse(
  "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
),
```

### Explicación

Convierte la URL en un objeto URI válido.

La API consultada es:

```text
https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios
```

---

# 11. Procesamiento de Respuesta

```dart
.then((response) {
```

### Explicación

Se ejecuta cuando el servidor responde.

---

# 12. Validar Código de Respuesta

```dart
if (response.statusCode == 200) {
```

### Explicación

Verifica si la respuesta fue exitosa.

Código:

```text
200 = OK
```

---

# 13. Actualizar la Interfaz

```dart
setState(() {
```

### Explicación

Permite actualizar la pantalla.

---

# 14. Convertir JSON a Lista

```dart
usuarios = jsonDecode(response.body);
```

### Explicación

Convierte el JSON recibido en una lista de objetos.

Ejemplo:

Antes:

```json
[
  {
    "name": "Juan"
  }
]
```

Después:

```dart
[
  {
    "name": "Juan"
  }
]
```

---

# 15. Ocultar Indicador de Carga

```dart
cargando = false;
```

### Explicación

Como los datos ya llegaron, se oculta el círculo de carga.

---

# 16. Manejo de Error HTTP

```dart
else {
```

### Explicación

Se ejecuta cuando la respuesta no es 200.

---

```dart
setState(() {
  cargando = false;
});
```

### Explicación

Oculta el indicador de carga.

---

```dart
print("Error: ${response.statusCode}");
```

### Explicación

Muestra el código de error en consola.

Ejemplo:

```text
Error: 404
```

---

# 17. Captura de Excepciones

```dart
.catchError((error) {
```

### Explicación

Captura errores de conexión o internet.

---

```dart
setState(() {
  cargando = false;
});
```

### Explicación

Oculta el indicador de carga.

---

```dart
print("Error: $error");
```

### Explicación

Muestra el error en consola.

---

# 18. Construcción de la Interfaz

```dart
@override
Widget build(BuildContext context) {
```

### Explicación

Construye visualmente la pantalla.

---

# 19. Scaffold

```dart
return Scaffold(
```

### Explicación

Es la estructura principal de una pantalla Flutter.

---

# 20. Barra Superior

```dart
appBar: AppBar(
```

### Explicación

Crea una barra superior.

---

```dart
title: const Text("MockAPI"),
```

### Explicación

Muestra el título:

```text
MockAPI
```

---

# 21. Contenido Principal

```dart
body: Column(
```

### Explicación

Organiza los widgets verticalmente.

---

# 22. Botón

```dart
ElevatedButton(
```

### Explicación

Crea un botón elevado.

---

```dart
onPressed: obtenerDatos,
```

### Explicación

Al presionar el botón se ejecuta:

```dart
obtenerDatos()
```

---

```dart
child: const Text(
  "Obtener Usuarios",
),
```

### Explicación

Texto visible en el botón.

---

# 23. Expanded

```dart
Expanded(
```

### Explicación

Permite que la lista ocupe todo el espacio disponible.

---

# 24. Operador Ternario

```dart
child: cargando
```

### Explicación

Evalúa si está cargando.

---

# 25. Mostrar Indicador de Carga

```dart
? const Center(
    child: CircularProgressIndicator(),
  )
```

### Explicación

Si:

```dart
cargando == true
```

se muestra:

```dart
CircularProgressIndicator()
```

---

# 26. Mostrar Lista

```dart
: ListView.builder(
```

### Explicación

Si:

```dart
cargando == false
```

se muestra la lista.

---

# 27. Cantidad de Elementos

```dart
itemCount: usuarios.length,
```

### Explicación

Indica cuántos usuarios se mostrarán.

---

# 28. Constructor de Cada Elemento

```dart
itemBuilder: (context, index) {
```

### Explicación

Construye cada fila de la lista.

---

# 29. ListTile

```dart
return ListTile(
```

### Explicación

Representa un elemento visual de la lista.

---

# 30. Nombre del Usuario

```dart
title: Text(
  usuarios[index]["name"].toString(),
),
```

### Explicación

Muestra el nombre del usuario.

Ejemplo:

```text
Juan Pérez
```

---

# 31. Información Adicional

```dart
subtitle: Text(
```

### Explicación

Muestra información secundaria.

---

```dart
"Email: ${usuarios[index]["email"]}\n"
"ID: ${usuarios[index]["id"]}",
```

### Explicación

Muestra:

```text
Email: juan@gmail.com
ID: 1
```

---

# codigo completo 
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
