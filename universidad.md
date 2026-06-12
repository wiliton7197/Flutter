# Explicación Línea por Línea del Código Flutter - Buscador de Universidades

## Introducción

Este proyecto desarrollado en Flutter consume la API pública de Hipolabs para mostrar información de universidades de diferentes países. La aplicación carga una lista de universidades al iniciar y permite filtrar los resultados escribiendo el nombre de un país.

---

# 1. Importación de Librerías

```dart
import 'package:flutter/material.dart';
```

### Explicación

Importa la biblioteca principal de Flutter para crear interfaces gráficas.

Permite utilizar widgets como:

* MaterialApp
* Scaffold
* AppBar
* TextField
* ListView
* Card
* ElevatedButton

---

```dart
import 'dart:convert';
```

### Explicación

Importa herramientas para trabajar con JSON.

Se utiliza para convertir la respuesta de la API en objetos que Flutter pueda manipular.

Ejemplo:

```dart
jsonDecode(respuesta.body);
```

---

```dart
import 'package:http/http.dart' as http;
```

### Explicación

Permite realizar peticiones HTTP.

Gracias a esta librería la aplicación puede conectarse a internet y consultar la API.

---

# 2. Función Principal

```dart
void main() {
  runApp(const MyApp());
}
```

### Explicación

Es el punto de entrada de la aplicación.

Cuando Flutter inicia:

1. Ejecuta `main()`
2. Ejecuta `runApp()`
3. Muestra el widget principal `MyApp`

---

# 3. Modelo Universidad

```dart
class Universidad {
```

### Explicación

Representa una universidad dentro de la aplicación.

Cada universidad tendrá:

* nombre
* país
* código de país
* dominios
* páginas web

---

```dart
final String nombre;
final String pais;
final String codigoPais;
final List<dynamic> dominios;
final List<dynamic> paginasWeb;
```

### Explicación

Son los atributos del objeto Universidad.

Ejemplo de una universidad:

```json
{
  "name": "Universidad Nacional de Cajamarca",
  "country": "Peru",
  "alpha_two_code": "PE",
  "domains": ["unc.edu.pe"],
  "web_pages": ["http://www.unc.edu.pe"]
}
```

---

```dart
Universidad({
  required this.nombre,
  required this.pais,
  required this.codigoPais,
  required this.dominios,
  required this.paginasWeb,
});
```

### Explicación

Constructor de la clase.

Permite crear objetos Universidad.

Ejemplo:

```dart
Universidad(
 nombre: "UNC",
 pais: "Peru",
 codigoPais: "PE",
 dominios: [],
 paginasWeb: []
);
```

---

# 4. Factory Constructor

```dart
factory Universidad.fromJson(Map<String, dynamic> json)
```

### Explicación

Convierte un objeto JSON recibido desde la API en un objeto Universidad.

---

```dart
nombre: json['name'] ?? 'Sin nombre',
```

### Explicación

Obtiene el nombre.

Si no existe:

```dart
Sin nombre
```

---

```dart
pais: json['country'] ?? 'Sin país',
```

### Explicación

Obtiene el país.

---

```dart
codigoPais: json['alpha_two_code'] ?? '',
```

### Explicación

Obtiene el código internacional del país.

Ejemplos:

```text
PE
MX
CO
AR
```

---

```dart
dominios: json['domains'] ?? [],
```

### Explicación

Obtiene la lista de dominios.

Ejemplo:

```text
unc.edu.pe
unmsm.edu.pe
```

---

```dart
paginasWeb: json['web_pages'] ?? [],
```

### Explicación

Obtiene las páginas web oficiales.

---

# 5. Clase Principal MyApp

```dart
class MyApp extends StatelessWidget
```

### Explicación

Widget principal de la aplicación.

Es Stateless porque no cambia su estado.

---

```dart
return const MaterialApp(
```

### Explicación

MaterialApp configura toda la aplicación.

---

```dart
debugShowCheckedModeBanner: false,
```

### Explicación

Oculta la etiqueta DEBUG.

---

```dart
home: PantallaUniversidades(),
```

### Explicación

Define la pantalla inicial.

---

# 6. PantallaUniversidades

```dart
class PantallaUniversidades extends StatefulWidget
```

### Explicación

Se usa StatefulWidget porque los datos cambian cuando se consulta la API.

---

# 7. Estado de la Pantalla

```dart
class _PantallaUniversidadesState
```

### Explicación

Aquí se almacena toda la lógica de la aplicación.

---

```dart
final TextEditingController controladorPais =
    TextEditingController();
```

### Explicación

Controla el contenido del TextField.

Permite leer lo que escribe el usuario.

Ejemplo:

```dart
controladorPais.text
```

---

```dart
List<Universidad> universidades = [];
```

### Explicación

Lista donde se almacenan las universidades obtenidas de la API.

---

```dart
bool cargando = false;
```

### Explicación

Controla si la aplicación está cargando datos.

---

# 8. initState()

```dart
@override
void initState() {
```

### Explicación

Se ejecuta una sola vez cuando la pantalla es creada.

---

```dart
cargarTodasLasUniversidades();
```

### Explicación

Carga automáticamente los datos al iniciar.

---

# 9. Método cargarTodasLasUniversidades()

```dart
Future<void> cargarTodasLasUniversidades() async
```

### Explicación

Obtiene los datos de la API.

Future significa que la operación tardará un tiempo.

async permite usar await.

---

```dart
setState(() {
  cargando = true;
});
```

### Explicación

Activa el indicador de carga.

---

```dart
final url = Uri.parse(
 "http://universities.hipolabs.com/search?name="
);
```

### Explicación

Construye la URL de consulta.

---

```dart
final respuesta = await http.get(url);
```

### Explicación

Realiza una petición GET.

El programa espera la respuesta.

---

```dart
respuesta.statusCode == 200
```

### Explicación

Verifica que la consulta fue exitosa.

200 significa éxito.

---

```dart
final List datos =
    jsonDecode(respuesta.body);
```

### Explicación

Convierte el JSON recibido en una lista.

---

```dart
universidades = datos
    .map((e) => Universidad.fromJson(e))
    .toList();
```

### Explicación

Convierte cada JSON en un objeto Universidad.

---

# 10. Método buscarUniversidades()

```dart
Future<void> buscarUniversidades() async
```

### Explicación

Permite filtrar universidades por país.

---

```dart
final pais =
    controladorPais.text.trim();
```

### Explicación

Obtiene el texto escrito por el usuario.

trim elimina espacios.

---

```dart
if (pais.isEmpty)
```

### Explicación

Si el usuario no escribe nada.

---

```dart
cargarTodasLasUniversidades();
```

### Explicación

Vuelve a cargar todos los registros.

---

```dart
"http://universities.hipolabs.com/search?country=$pais"
```

### Explicación

Filtra por país.

Ejemplo:

```text
country=Peru
country=Mexico
country=Colombia
```

---

# 11. Método dispose()

```dart
@override
void dispose()
```

### Explicación

Se ejecuta cuando la pantalla se destruye.

---

```dart
controladorPais.dispose();
```

### Explicación

Libera memoria utilizada por el TextEditingController.

---

# 12. Método construirTarjeta()

```dart
Widget construirTarjeta(
  Universidad universidad
)
```

### Explicación

Construye visualmente cada universidad.

---

```dart
Card(
```

### Explicación

Crea una tarjeta.

---

```dart
Icon(Icons.school)
```

### Explicación

Muestra un ícono de educación.

---

```dart
universidad.nombre
```

### Explicación

Muestra el nombre de la universidad.

---

```dart
universidad.pais
```

### Explicación

Muestra el país.

---

```dart
universidad.codigoPais
```

### Explicación

Muestra el código del país.

---

```dart
universidad.dominios.join(", ")
```

### Explicación

Une todos los dominios en una sola cadena.

Ejemplo:

```text
unc.edu.pe, unmsm.edu.pe
```

---

```dart
universidad.paginasWeb.join("\n")
```

### Explicación

Muestra las páginas web en líneas separadas.

---

# 13. Método build()

```dart
Widget build(BuildContext context)
```

### Explicación

Construye toda la interfaz visual.

---

# 14. Scaffold

```dart
Scaffold(
```

### Explicación

Estructura principal de la pantalla.

---

# 15. AppBar

```dart
AppBar(
 title: Text("Universidades del Mundo")
)
```

### Explicación

Barra superior de la aplicación.

---

# 16. TextField

```dart
TextField(
 controller: controladorPais
)
```

### Explicación

Campo donde el usuario escribe el país.

---

```dart
onSubmitted: (_)
```

### Explicación

Permite buscar al presionar Enter.

---

# 17. IconButton

```dart
IconButton(
 icon: Icon(Icons.search)
)
```

### Explicación

Botón de búsqueda.

---

```dart
onPressed: buscarUniversidades
```

### Explicación

Ejecuta la búsqueda.

---

# 18. Expanded

```dart
Expanded(
```

### Explicación

Hace que la lista ocupe el espacio disponible.

---

# 19. Indicador de Carga

```dart
CircularProgressIndicator()
```

### Explicación

Muestra una animación mientras se consultan datos.

---

# 20. ListView.builder

```dart
ListView.builder(
```

### Explicación

Crea una lista dinámica.

Solo construye los elementos visibles para mejorar el rendimiento.

---

```dart
itemCount: universidades.length
```

### Explicación

Cantidad de universidades mostradas.

---

```dart
itemBuilder:
```

### Explicación

Construye cada elemento de la lista.

---

```dart
return construirTarjeta(
  universidades[index]
);
```

### Explicación

Genera una tarjeta para cada universidad obtenida desde la API.

---

# Resumen

La aplicación:

1. Se inicia con `main()`.
2. Consulta la API de universidades.
3. Convierte JSON a objetos Universidad.
4. Guarda los datos en una lista.
5. Muestra las universidades en tarjetas.
6. Permite filtrar por país.
7. Actualiza la interfaz usando `setState()`.
8. Utiliza `ListView.builder()` para mostrar los resultados eficientemente.
   """
