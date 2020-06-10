---
description: Una guía rápida de introducción al manejo de sesiones en PHP
---

# Sesiones en PHP

### Comandos útiles

```php
// El costo define que tan encriptada estará una contraseña.
$opciones = array('cost'=>12);

// Encriptamos
$contrasena_hasheada = password_hash($contrasena, PASSWORD_BCRYPT, $opciones);
// Comparar una contraseña plana con una hasheada
password_verify($contrasena, $contrasena_hasheada);
// Sanitizamos los datos
$nombre = filter_var($_POST["nombre"], FILTER_SANITIZE_STRING);
// Utilizar datos enviados por formulario
$_POST["usuario"], $_POST["contrasena"], $_POST["mail"]

// Iniciamos el uso de sesiones
session_start();

// Iniciamos las variables en la sesión(las que necesitemos)
// Con las variables de sesión podríamos corroborar si un
// usuario está logueado y así permitirle acceder a 
// páginas exclusivas.
$_SESSION["usuario"] = $usuario;
$_SESSION["mail"]= $mail;

// Para terminar una sesión
session_destroy();
```

{% hint style="warning" %}
[Sanitizar los datos](https://xkcd.com/327/) de entrada \(como utilizar la función filter\_var\) es un factor importante en la **seguridad de una Base de Datos.** 
{% endhint %}

### Importancia

Todos estamos familiarizados con el concepto de sesiones, estas están presentes cuando nos registramos en alguna red social y cuando ingresamos a ella posteriormente con la misma cuenta. Pueden haber muchos motivos por los cuales implementar un sistema de sesiones, normalmente queremos tener un control de quién tiene acceso a  páginas específicas de nuestra página web, ya sea porque queremos exigir un precio para llegar a ciertas funcionalidades o como es el caso de las redes sociales, porque es parte de la idea misma del sistema, un sistema de redes sociales sin identificación de usuarios, pierde totalmente su sentido \(podrían haber propuestas distintas\).

### Idea central

La idea es simple, necesitamos mantener un registro de los usuarios de nuestro sistema que persista en el tiempo, que esté lejos del alcance de los malignos de internet y que podamos acceder en forma continua y eficiente. Cada vez que alguien quiera acceder al sistema ingresara sus datos y nosotros debemos corroborar si los datos son congruentes con alguno de nuestros registros.   
¿Alguna idea de que podríamos utilizar...? Claro..., estamos en la asignatura de Bases de Datos, por lo que, tendremos una tabla llamada usuario, la cual contendrá los atributos que lo caracterizan. 

Pero, ¿como hacemos para que solamente los usuarios puedan acceder a páginas exclusivas?, somos programadores, cada vez que un usuario quiera acceder a una página exclusiva, podríamos corroborar con el típico if si es que el solicitante tiene una sesión activa, PHP nos entrega herramientas para realizar estas tareas. 

1. El usuario solicita ingresar a determinada página.
2. \(usuario es registrado\)? permitir ingreso : no permitir, redireccionar al login;

{% hint style="info" %}
¿Sabías que existe una forma más breve del condicional if ?

Funciona de la siguiente manera:

$x = \(condición\)? resultado si condición es verdadera : resultado si condición es falsa;
{% endhint %}

### Registro

1. El usuario no registrado ingresa los datos del registro en un formulario HTML.
2. En un archivo PHP recibimos los datos comúnmente con $\_POST\["usuario"\] y $\_POST\["password"\].
3. Corroboramos que el usuario, email, rut o rol\(dependiendo de lo que queramos utilizar como identificador único\) no esté ya ingresado en nuestra tabla de usuarios.
4. Encriptamos la contraseña ingresada por el usuario.
5. Registramos los datos del usuario en la base de datos con la instrucción INSERT INTO, se envía la contraseña encriptada.

{% hint style="warning" %}
Nunca se guarda una contraseña en la base de datos sin antes encriptarla. En el mundo real, uno [nunca debería guardar contraseñas](https://www.youtube.com/watch?v=8ZtInClXe1Q) si es que tiene los medios para no hacerlo.
{% endhint %}

### Login

1. Usuario ingresa sus datos en formulario HTML
2. En un archivo PHP recibimos los datos comúnmente con $\_POST\["usuario"\] y $\_POST\["password"\].
3. Realizamos una consulta SELECT con los datos del usuario\(hasheando la contraseña\) a la base de datos.
4. Si encuentra un registro coincidente, iniciamos la sesión del usuario.
   * Redireccionamos a la página principal exclusiva para usuarios registrados.
5. Si no encuentra un registro coincidente, redireccionamos al login.

{% hint style="warning" %}
Recordar aplicar el algoritmo de encriptación a la contraseña antes de guardarla en la base de datos.
{% endhint %}

## Un Ejemplo Simple: El Tesoro del Pirata Barba\#000000

El Pirata Barba\#000000 necesita esconder su cofre del tesoro. Sin embargo, cavar un hoyo y enterrar el cofre ya está pasado de moda, por lo que ha decidido esconderlo en una página web, detrás de una vista de Login. De esta forma, piensa él, solo un usuario que haya iniciado sesión en la página podrá ver su tesoro y saber en cuántos dólares están valorizados todos esos doblones de oro.

Para lograr su cometido, Barba\#000000 le ofrece a usted **la mitad de su tesoro** si le diseña una página de tal forma que sólo un usuario que haya iniciado sesión correctamente pueda acceder, y se le muestre el valor de su tesoro en dólares. Considere para esta prueba que sólo existe un usuario, cuyas credenciales son:

* **Usuario: Barba\#000000**
* **Contraseña: pirata**

A Barba\#000000 le interesa que:

* Si un usuario no ha iniciado sesión, no puede acceder a la página del tesoro \(ni siquiera escribiendo directamente la URL\).
* Si ya hay una sesión activa, la página de inicio de sesión redireccione automáticamente al tesoro.
* Dentro de la página del tesoro, exista un botón para cerrar la sesión.



