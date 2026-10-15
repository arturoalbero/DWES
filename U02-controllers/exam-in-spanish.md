# 1. Parte 1: Prueba de conocimientos (2 puntos)

### 1. ¿Qué tipo de sitio web sería el más adecuado para cada uno de los siguientes casos y por qué? (0,25 puntos)

- Un sitio web para promocionar un perfil artístico.
- Un blog personal.
- El sitio web de una librería.
- Un sitio web sobre el tiempo meteorológico.

### 2. Describe la **inyección de dependencias (*Dependency Injection*)** en el contexto de Spring. (0,25 puntos)

### 3. Describe el paradigma **Modelo-Vista-Controlador (MVC)**. (0,5 puntos)

### 4. Nombra al menos cuatro tipos de peticiones HTTP. (0,25 puntos)

### 5. En Spring, ¿cuál es la diferencia entre `@GetMapping` y `@RequestMapping`? (0,25 puntos)

### 6. En Spring, ¿cuál es la diferencia entre un **fragmento (*fragment*)** y un **bloque (*block*)**? (0,25 puntos)

### 7. Explica cómo funcionan los dos operadores que utiliza Thymeleaf para gestionar valores nulos: el **operador de navegación segura (`?.`)** y el **operador Elvis (`?:`)**. (0,25 puntos)

---

# 2. Parte 2: Desarrollo práctico (8 puntos)

## 1. Ejercicio de vistas: Crear una vista para un aula con las siguientes características (4 puntos)

- Debes añadir una cabecera (*head*) como fragmento y un pie de página (*footer*) como bloque. Ambos se encuentran en el archivo `/fragments.html` y se llaman `custom-head` y `custom-footer`, respectivamente. (0,5 puntos)

- La vista recibe un objeto `STUDENT(NIA, name, surname, birth, course, nationality, chairNumberPosition, observations)`. La nacionalidad es un enumerado (`enum`) con los valores `SPANISH`, `EUROPEAN` y `NON-EU` (aunque puedes gestionarlo como si fuera un `String`).

  Para cada estudiante, muestra sus apellidos, nombre, curso y posición en el aula. Muestra también las observaciones, si las hay. En caso contrario, muestra «sin observaciones». (1 punto)

- Los estudiantes españoles deben resaltarse en verde, los estudiantes de la Unión Europea en azul y los estudiantes de fuera de la Unión Europea en naranja. (1 punto)

- **Si los estudiantes son mayores de edad (>= 18), debes mostrar «ADULTO»; en caso contrario, debes mostrar «MENOR».** Supón que el año actual es 2026 y que `birth` es un número entero que representa el año de nacimiento. Esto se hace por simplicidad. (0,5 puntos)

- Al hacer clic en cada estudiante, debe abrirse una página que muestre su información detallada. Supón que esta página está implementada en `/student/{nia}`. (1 punto)

## 2. Ejercicio de controladores: Crear un controlador para un aula con las siguientes características (4 puntos)

Supón que tenemos un *singleton* llamado `MyManager` con un método `.getStudents()` que devuelve una lista de objetos `STUDENT(NIA, name, surname, birth, course, nationality, chairNumberPosition, observations)`.

La nacionalidad es un enumerado (`enum`) con los valores `SPANISH`, `EUROPEAN` y `NON-EU`, y `birth` es un número entero que representa el año de nacimiento, por simplicidad.

Supón también que existe un método estático `Student findById(Integer id, List<Student> l)`. El identificador (`id`) es el NIA y las observaciones son de tipo `String`.

- Crea un método del controlador para el endpoint `/student/{nia}`. Debe devolver la vista `studentView` y añadir a esta el estudiante correspondiente. (1 punto)

- Si el estudiante no existe, redirige a la vista a la que se accede mediante el endpoint `/index`. (0,5 puntos)

- Define la clase `STUDENT` (no es necesario implementar los métodos *getters* y *setters*, y supón que el enumerado ya está definido como `Nationality`). (1 punto)

- Define el *singleton* `MyManager`. Debe tener un método `getInstance`, una lista de estudiantes, un método `getStudents` y un método para inicializar la lista de estudiantes a partir de un archivo `.csv`.

  El método para leer un archivo `.csv` y convertirlo en una `List<String[]>` ya está definido (`readCSV`), al igual que el método para convertir un `String[]` en un estudiante (`Student(String[] st)`). La gestión de errores ya está implementada.

  - Parámetros básicos del *singleton* (constructor, método *getter* y atributos). **(0,75 puntos)**

  - Método `initStudents`. **(0,75 puntos)**

---

# 3. Ejercicio de ampliación

### 1. Redefine el controlador y la vista para que utilicen parámetros de petición (*request parameters*) en lugar de variables de ruta (*path variables*). **(0,75 puntos)**

### 2. Implementa manualmente el método estático `findById`. **(0,25 puntos)**