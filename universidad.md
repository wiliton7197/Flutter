# Explicación Línea por Línea del Código Flutter - Buscador de Universidades

## Introducción

Este proyecto desarrollado en Flutter permite consultar una API pública de universidades utilizando peticiones HTTP. El usuario ingresa el nombre de un país, presiona el botón **Buscar** y la aplicación muestra una lista de universidades pertenecientes a dicho país.

---

# 1. Importación de Librerías

```dart
import 'package:flutter/material.dart';
import 'dart:convert';
import 'package:http/http.dart' as http;
```

### Explicación

```dart
import 'package:flutter/material.dart';
```

Importa la librería principal de Flutter para construir la interfaz gráfica.

---

```dart
import 'dart:convert';
```

Permite convertir datos JSON en objetos de Dart mediante funciones como `jsonDecode()`.

---

```dart
import 'package:http/http.dart' as http;
```

Importa el paquete HTTP para realizar solicitudes a APIs desde internet.

---

# 2. Función Principal

```dart
void main() {
  runApp(const MyApp());
}
```

### Explicación

```dart
void main()
```

Es el punto de entrada de toda aplicación Flutter.

---

```dart
runApp(const MyApp());
```

Inicia la aplicación mostrando el widget principal llamado `MyApp`.

---

# 3. Modelo Universidad

```dart
class Universidad {
```

Se crea una clase para representar cada universidad obtenida desde la API.

---

```dart
final String nombre;
final String sitioWeb;
```

Variables que almacenan:

* Nombre de la universidad.
* Sitio web de la universidad.

La palabra `final` indica que el valor no cambiará después de ser asignado.

---

```dart
Universidad({
  required this.nombre,
  required this.sitioWeb,
});
```

Constructor de la clase.

La palabra `required` obliga a enviar valores para ambas propiedades.

---

## Método fromJson

```dart
factory Universidad.fromJson(Map<String, dynamic> json)
```

Permite convertir un objeto JSON en un objeto Universidad.

---

```dart
nombre: json['name'] ?? 'Sin nombre',
```

Obtiene el valor del campo `name`.

Si no existe, muestra `"Sin nombre"`.

---

```dart
sitioWeb: (json['web_pages'] != null &&
        json['web_pages'].isNotEmpty)
```

Verifica que exista la lista de sitios web y que no esté vacía.

---

```dart
? json['web_pages'][0]
```

Obtiene el primer sitio web.

---

```dart
: 'Sin sitio web'
```

Si no existe un sitio web, muestra ese texto.

---

# 4. Widget Principal

```dart
class MyApp extends StatelessWidget
```

Representa la aplicación principal.

---

```dart
const MyApp({super.key});
```

Constructor del widget.

---

```dart
Widget build(BuildContext context)
```

Método encargado de construir la interfaz.

---

```dart
return const MaterialApp(
```

Crea la aplicación utilizando Material Design.

---

```dart
debugShowCheckedModeBanner: false,
```

Oculta la cinta roja de "Debug".

---

```dart
home: PantallaUniversidades(),
```

Indica cuál será la pantalla inicial.

---

# 5. StatefulWidget

```dart
class PantallaUniversidades extends StatefulWidget
```

Se utiliza porque la pantalla cambia constantemente cuando llegan datos desde internet.

---

```dart
State<PantallaUniversidades> createState()
```

Crea el estado asociado al widget.

---

# 6. Variables de Estado

```dart
final TextEditingController controladorPais =
    TextEditingController();
```

Controla el contenido escrito dentro del TextField.

---

```dart
List<Universidad> universidades = [];
```

Lista donde se almacenarán las universidades obtenidas.

Inicialmente está vacía.

---

```dart
bool cargando = false;
```

Variable para controlar cuándo mostrar el indicador de carga.

* false = no está cargando.
* true = está cargando.

---

# 7. Función HTTP

```dart
Future<void> buscarUniversidades() async
```

Función asíncrona encargada de consultar la API.

---

## Validación del campo

```dart
if (controladorPais.text.trim().isEmpty) {
  return;
}
```

Si el usuario no escribe ningún país, la función termina.

---

## Activar carga

```dart
setState(() {
  cargando = true;
});
```

Actualiza la interfaz para mostrar el CircularProgressIndicator.

---

## Bloque try

```dart
try {
```

Permite capturar errores durante la petición.

---

## Construcción de URL

```dart
final url = Uri.parse(
  "http://universities.hipolabs.com/search?country=${controladorPais.text}",
);
```

Construye la dirección web usando el país ingresado.

Ejemplo:

```text
http://universities.hipolabs.com/search?country=Peru
```

---

## Solicitud HTTP

```dart
final respuesta = await http.get(url);
```

Envía una petición GET a la API.

La palabra `await` espera la respuesta antes de continuar.

---

## Verificar respuesta

```dart
if (respuesta.statusCode == 200)
```

Comprueba si la petición fue exitosa.

Código 200 significa éxito.

---

## Convertir JSON

```dart
List datos = jsonDecode(respuesta.body);
```

Convierte el JSON recibido en una lista de objetos Dart.

---

## Convertir a objetos Universidad

```dart
universidades = datos
    .map((json) => Universidad.fromJson(json))
    .toList();
```

Recorre cada elemento JSON y lo convierte en un objeto Universidad.

---

## Error de conexión

```dart
} catch (e) {
```

Captura cualquier error.

Por ejemplo:

* Sin internet.
* Error del servidor.
* URL incorrecta.

---

```dart
universidades = [];
```

Vacía la lista si ocurre un error.

---

## Finalizar carga

```dart
setState(() {
  cargando = false;
});
```

Oculta el indicador de progreso.

---

# 8. Interfaz de Usuario

## Scaffold

```dart
return Scaffold(
```

Estructura principal de la pantalla.

---

## AppBar

```dart
appBar: AppBar(
```

Barra superior de la aplicación.

---

```dart
title: const Text("Buscador de Universidades"),
```

Título mostrado en la barra.

---

```dart
centerTitle: true,
```

Centra el título.

---

# 9. Padding

```dart
Padding(
  padding: const EdgeInsets.all(16),
```

Agrega espacio alrededor de todo el contenido.

---

# 10. Column

```dart
Column(
```

Organiza los widgets verticalmente.

---

# 11. TextField

```dart
TextField(
```

Campo para ingresar el nombre del país.

---

```dart
controller: controladorPais,
```

Conecta el TextField con el controlador.

---

```dart
labelText: "Ingrese un país",
```

Texto de ayuda mostrado al usuario.

---

# 12. Botón Buscar

```dart
ElevatedButton(
```

Botón que inicia la búsqueda.

---

```dart
onPressed: buscarUniversidades,
```

Ejecuta la función HTTP al hacer clic.

---

```dart
child: const Text("Buscar"),
```

Texto del botón.

---

# 13. Expanded

```dart
Expanded(
```

Hace que la lista ocupe el espacio disponible.

---

# 14. Operador Ternario

```dart
cargando
```

Evalúa si la aplicación está cargando.

---

## Si está cargando

```dart
CircularProgressIndicator()
```

Muestra un círculo girando.

---

## Si no hay resultados

```dart
universidades.isEmpty
```

Comprueba si la lista está vacía.

---

```dart
Text("No hay resultados")
```

Muestra un mensaje informativo.

---

# 15. ListView.builder

```dart
ListView.builder(
```

Genera una lista dinámica.

---

```dart
itemCount: universidades.length,
```

Cantidad de universidades encontradas.

---

```dart
itemBuilder: (context, index)
```

Construye cada elemento de la lista.

---

```dart
final universidad = universidades[index];
```

Obtiene la universidad actual.

---

# 16. Card

```dart
Card(
```

Crea una tarjeta para mostrar la información.

---

# 17. ListTile

```dart
ListTile(
```

Widget especializado para mostrar datos organizados.

---

```dart
leading: const Icon(Icons.school),
```

Muestra un ícono de escuela.

---

```dart
title: Text(universidad.nombre),
```

Muestra el nombre de la universidad.

---

```dart
subtitle: Text(universidad.sitioWeb),
```

Muestra el sitio web de la universidad.

---

# Flujo General del Programa

1. El usuario escribe un país.
2. Presiona el botón Buscar.
3. Se activa el indicador de carga.
4. Flutter realiza una petición HTTP.
5. La API devuelve datos JSON.
6. Los datos se convierten en objetos Universidad.
7. La lista se actualiza.
8. Se muestran las universidades encontradas.
9. Si ocurre un error o no existen resultados, se muestra un mensaje informativo.
10. El indicador de carga desaparece.
