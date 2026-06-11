Explicación Línea por Línea del Código Flutter para Consumir una API con MockAPI
Introducción

Este proyecto desarrollado en Flutter permite consumir datos desde una API creada en MockAPI y mostrarlos en una lista dentro de la aplicación. Además, incluye un indicador de carga (CircularProgressIndicator) para informar al usuario que los datos se están obteniendo desde internet.

1. Importación de Librerías
import 'package:flutter/material.dart';
Explicación

Importa la librería principal de Flutter para construir interfaces gráficas.

Permite utilizar widgets como:

MaterialApp
Scaffold
AppBar
Text
ElevatedButton
ListView
ListTile
import 'dart:convert';
Explicación

Importa herramientas para convertir datos JSON a objetos de Dart.

Se utilizará:

jsonDecode()

para convertir la respuesta de la API en una lista que Flutter pueda manejar.

import 'package:http/http.dart' as http;
Explicación

Importa el paquete HTTP.

Permite realizar peticiones a servidores web utilizando métodos como:

GET
POST
PUT
DELETE

El alias http se utiliza para acceder a sus métodos.

Ejemplo:

http.get(...)
2. Función Principal
void main() {
  runApp(const MyApp());
}
Explicación

Es el punto de entrada de toda aplicación Flutter.

Línea por línea
void main()

Función principal que se ejecuta primero.

runApp(...)

Inicia la aplicación Flutter.

const MyApp()

Crea una instancia del widget principal.

3. Widget Principal
class MyApp extends StatelessWidget {
Explicación

Se crea una clase llamada MyApp.

Hereda de:

StatelessWidget

porque no necesita almacenar estados.

const MyApp({super.key});

Constructor de la clase.

@override
Widget build(BuildContext context)

Método encargado de construir la interfaz.

return const MaterialApp(

Crea la aplicación utilizando Material Design.

home: PantallaUsuarios(),

Define la primera pantalla que verá el usuario.

4. Creación de la Pantalla de Usuarios
class PantallaUsuarios extends StatefulWidget {
Explicación

Se crea una pantalla con estado.

Se utiliza StatefulWidget porque los datos cambiarán cuando lleguen desde la API.

const PantallaUsuarios({super.key});

Constructor de la pantalla.

@override
State<PantallaUsuarios> createState()

Genera el estado asociado al widget.

return _PantallaUsuariosState();

Devuelve la clase donde se manejarán los datos y cambios visuales.

5. Clase de Estado
class _PantallaUsuariosState extends State<PantallaUsuarios> {
Explicación

Aquí se controla toda la lógica de la pantalla.

6. Lista de Usuarios
List usuarios = [];
Explicación

Crea una lista vacía donde se almacenarán los usuarios obtenidos desde la API.

Inicialmente:

[]

está vacía.

Después de consumir la API contendrá algo similar a:

[
  {
    "id":"1",
    "name":"Juan",
    "email":"juan@gmail.com"
  }
]
7. Variable de Carga
bool cargando = false;
Explicación

Variable booleana que indica si la aplicación está obteniendo información.

Valores posibles:

true

Está cargando.

false

No está cargando.

8. Método Obtener Datos
void obtenerDatos() {
Explicación

Método encargado de conectarse a la API.

9. Activar Indicador de Carga
setState(() {
  cargando = true;
});
Explicación

Actualiza la interfaz.

Indica que comenzó la carga de datos.

setState()

Le informa a Flutter que debe reconstruir la pantalla.

10. Solicitud GET
http.get(
Explicación

Realiza una petición HTTP GET.

Uri.parse(
  "https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios",
)

Convierte la URL en un objeto URI válido.

La URL apunta al endpoint:

https://69e1842eb1cb62b9f316e8cf.mockapi.io/usuarios
11. Procesar Respuesta
.then((response) {
Explicación

Se ejecuta cuando el servidor responde.

12. Verificar Código de Estado
if (response.statusCode == 200) {
Explicación

Comprueba si la petición fue exitosa.

Código:

200 = OK
13. Actualizar Interfaz
setState(() {

Permite modificar variables y actualizar la pantalla.

14. Convertir JSON a Lista
usuarios = jsonDecode(response.body);
Explicación

Convierte el JSON recibido en una lista de Dart.

Ejemplo:

JSON recibido:

[
  {
    "id":"1",
    "name":"Carlos",
    "email":"carlos@gmail.com"
  }
]

Resultado:

usuarios[0]["name"]

Retorna:

Carlos
15. Ocultar Indicador de Carga
cargando = false;
Explicación

Indica que la carga terminó.

16. Manejo de Error HTTP
else {

Se ejecuta si el servidor devuelve un error.

setState(() {
  cargando = false;
});

Oculta el indicador de carga.

print("Error: ${response.statusCode}");

Muestra el error en consola.

Ejemplo:

Error: 404
17. Captura de Excepciones
.catchError((error) {
Explicación

Captura errores de conexión.

Ejemplos:

Sin internet
Servidor caído
URL incorrecta
setState(() {
  cargando = false;
});

Oculta el indicador.

print("Error: $error");

Muestra el error en consola.

18. Método Build
@override
Widget build(BuildContext context)
Explicación

Construye toda la interfaz de usuario.

19. Scaffold
return Scaffold(
Explicación

Estructura principal de la pantalla.

20. Barra Superior
appBar: AppBar(

Crea la barra superior.

title: const Text("MockAPI"),

Muestra el título:

MockAPI
21. Cuerpo de la Pantalla
body: Column(

Organiza widgets verticalmente.

22. Botón Obtener Usuarios
ElevatedButton(

Crea un botón elevado.

onPressed: obtenerDatos,

Cuando se presiona ejecuta:

obtenerDatos()
child: const Text(
  "Obtener Usuarios",
),

Texto visible en el botón.

23. Expanded
Expanded(

Permite que el contenido ocupe todo el espacio restante.

24. Operador Ternario
child: cargando
Explicación

Evalúa la variable:

cargando

Si es:

true

muestra:

CircularProgressIndicator()

Si es:

false

muestra:

ListView.builder()
25. Indicador de Carga
Center(
  child: CircularProgressIndicator(),
)
Explicación

Muestra una animación circular indicando que la aplicación está esperando datos.

26. Lista Dinámica
ListView.builder(
Explicación

Genera una lista de elementos dinámicamente.

itemCount: usuarios.length,

Cantidad de usuarios obtenidos.

27. Constructor de Elementos
itemBuilder: (context, index)
Explicación

Construye cada fila de la lista.

28. ListTile
return ListTile(
Explicación

Representa una fila de información.

29. Nombre del Usuario
title: Text(
  usuarios[index]["name"].toString(),
),

Muestra el nombre.

Ejemplo:

Juan Pérez
30. Información Adicional
subtitle: Text(

Muestra información secundaria.

"Email: ${usuarios[index]["email"]}\n"

Muestra el correo electrónico.

"ID: ${usuarios[index]["id"]}"

Muestra el ID del usuario.

Resultado visual:

Juan Pérez

Email: juan@gmail.com
ID: 1
Flujo Completo del Programa
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
