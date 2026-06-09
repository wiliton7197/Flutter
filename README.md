# 📱 Conceptos Utilizados en el Proyecto Flutter con MockAPI

## 1. ¿Qué es Flutter?

Flutter es un framework desarrollado por Google que permite crear aplicaciones para Android, iOS, Web, Windows, Linux y macOS utilizando un solo código fuente.

### ¿Para qué sirve?

Permite desarrollar aplicaciones multiplataforma de forma rápida y eficiente.

### En este proyecto

Se utilizó Flutter para crear la interfaz gráfica y consumir datos desde una API.

---

## 2. ¿Qué es Dart?

Dart es el lenguaje de programación utilizado por Flutter.

### ¿Para qué sirve?

Permite desarrollar toda la lógica de la aplicación.

### En este proyecto

Se utilizó Dart para programar la obtención de datos desde MockAPI y mostrarlos en pantalla.

---

## 3. ¿Qué es una API?

Una API (Application Programming Interface) es un servicio que permite que dos aplicaciones se comuniquen entre sí.

### ¿Para qué sirve?

Permite solicitar o enviar información entre sistemas.

### Ejemplo

La aplicación Flutter solicita usuarios y la API responde con esos datos.

---

## 4. ¿Qué es MockAPI?

MockAPI es una plataforma que permite crear APIs simuladas para realizar pruebas y prácticas.

### ¿Para qué sirve?

Permite trabajar con datos sin necesidad de crear un servidor real.

### En este proyecto

Se utilizó para almacenar usuarios y obtenerlos desde Flutter.

---

## 5. ¿Qué es JSON?

JSON (JavaScript Object Notation) es un formato utilizado para intercambiar información entre aplicaciones.

### Ejemplo

```json
[
  {
    "id": "1",
    "name": "Juan"
  }
]
```

### ¿Para qué sirve?

Permite transportar datos de forma organizada.

### En este proyecto

La API devuelve los usuarios en formato JSON.

---

## 6. ¿Qué es HTTP?

HTTP (HyperText Transfer Protocol) es el protocolo utilizado para la comunicación entre aplicaciones y servidores.

### ¿Para qué sirve?

Permite enviar y recibir información por Internet.

### En este proyecto

Se utilizó para solicitar los usuarios almacenados en MockAPI.

---

## 7. ¿Qué es la librería HTTP?

```yaml
http: ^1.2.1
```

Es una dependencia de Flutter.

### ¿Para qué sirve?

Permite realizar peticiones a APIs.

### Funciones principales

* GET → Obtener datos.
* POST → Agregar datos.
* PUT → Actualizar datos.
* DELETE → Eliminar datos.

---

## 8. ¿Qué es un Widget?

Los Widgets son los componentes básicos de Flutter.

### ¿Para qué sirven?

Permiten construir toda la interfaz gráfica.

### Ejemplos

* Text
* Button
* AppBar
* ListView
* Scaffold

Todo lo que se ve en pantalla es un Widget.

---

## 9. ¿Qué es StatelessWidget?

Es un Widget que no cambia durante la ejecución.

### ¿Para qué sirve?

Se utiliza cuando la información mostrada es fija.

### Ejemplo

```dart
Text("Hola Mundo")
```

### Características

* No tiene variables que cambien con `setState()`.
* No administra su propio estado.
* Depende de los datos que recibe.

---

## 10. ¿Qué es StatefulWidget?

Es un Widget que puede cambiar durante la ejecución.

### ¿Para qué sirve?

Permite actualizar la pantalla cuando cambian los datos.

### En este proyecto

Se utilizó porque los usuarios llegan desde la API.

### Características

* Tiene estado propio.
* Puede usar `setState()`.
* Puede cambiar la interfaz desde dentro del widget.

---

## 11. ¿Qué es MaterialApp?

MaterialApp es el widget principal que envuelve toda la aplicación Flutter y aplica las características de Material Design de Google.

### ¿Para qué sirve?

* Configura la aplicación.
* Define la pantalla inicial.
* Permite configurar temas y colores.
* Gestiona la navegación entre pantallas.

---

## 12. ¿Qué es Scaffold?

Scaffold es la estructura básica de una pantalla Flutter.

### ¿Para qué sirve?

Organiza elementos como:

* AppBar
* Body
* Botones
* Menús
* Navegación

Es considerado el "esqueleto" de una pantalla.

---

## 13. ¿Qué es AppBar?

Es la barra superior de una aplicación.

### ¿Para qué sirve?

Permite mostrar títulos y acciones.

### Ejemplo

```dart
AppBar(
  title: Text("MockAPI")
)
```

---

## 14. ¿Qué es Text?

Es un Widget que muestra texto.

### Ejemplo

```dart
Text("Hola Mundo")
```

### ¿Para qué sirve?

Permite mostrar información al usuario.

---

## 15. ¿Qué es ElevatedButton?

Es un Widget que crea un botón.

### ¿Para qué sirve?

Permite ejecutar acciones cuando el usuario hace clic.

### En este proyecto

Se utiliza para obtener los usuarios desde la API.

---

## 16. ¿Qué es ListView?

Es un Widget que muestra listas desplazables.

### ¿Para qué sirve?

Permite mostrar grandes cantidades de información.

---

## 17. ¿Qué es ListView.builder?

Es una versión dinámica de ListView.

### ¿Para qué sirve?

Genera automáticamente los elementos de una lista.

### En este proyecto

Genera una fila por cada usuario recibido desde la API.

---

## 18. ¿Qué es ListTile?

Es un Widget que representa una fila dentro de una lista.

### ¿Para qué sirve?

Permite mostrar información organizada.

### Ejemplo

```dart
ListTile(
  title: Text("Juan")
)
```

---

## 19. ¿Qué es setState()?

Es un método que actualiza la interfaz gráfica.

### ¿Para qué sirve?

Le indica a Flutter que los datos cambiaron.

### En este proyecto

Actualiza la pantalla cuando llegan los usuarios desde la API.

---

## 20. ¿Qué es jsonDecode()?

Es una función de Dart.

### ¿Para qué sirve?

Convierte datos JSON en objetos que Flutter puede utilizar.

### Ejemplo

Convierte:

```json
[
  {
    "name":"Juan"
  }
]
```

en una lista de Dart.

---

## 21. ¿Qué es Future?

Future representa una tarea que tardará un tiempo en completarse.

### ¿Para qué sirve?

Permite trabajar con operaciones que no son inmediatas.

### Ejemplo

Obtener información desde Internet.

---

## 22. ¿Qué es async?

Permite que una función trabaje de forma asíncrona.

### ¿Para qué sirve?

Evita que la aplicación se bloquee mientras espera una respuesta.

---

## 23. ¿Qué es await?

Permite esperar el resultado de una operación asíncrona.

### Ejemplo

```dart
await http.get(...)
```

### ¿Para qué sirve?

Espera la respuesta de la API antes de continuar.

---

## 24. ¿Qué es una petición GET?

Es una solicitud utilizada para obtener información.

### Ejemplo

```dart
http.get(...)
```

### ¿Para qué sirve?

Permite solicitar datos a un servidor o API sin modificar la información almacenada.

### En este proyecto

Se utiliza para obtener la lista de usuarios almacenados en MockAPI.
