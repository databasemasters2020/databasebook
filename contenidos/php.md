---
description: Guía rápida de iniciación en PHP
---

# PHP

**PHP** \(Sigla recursiva para PHP Hypertext preprocessor\) es un lenguaje de scripting de propósito general.

### Front-end

Hasta ahora hemos visto HTML, CSS y bootstrap mostrando un contenido estático y no persistente, el cual no guarda datos ingresados por el usuario en el tiempo. A toda la parte del desarrollo web con la cual interactúa y ve el usuario se le llama Front-end.

### Back-end

En la actualidad requerimos más que solo Front-end. Las páginas web son cada vez más complejas, requerimos almacenar información en el tiempo, creación de usuarios, páginas privilegiadas \(registro obligatorio o suscripción mensual\), seguridad y, lo que nos concierne, uso de **Bases de Datos.** Para esto, requerimos de un servidor que tenga disponible continuamente cada uno de los recursos necesarios para que nuestra página web funcione íntegramente. Existen múltiples lenguajes de programación aptos para ser usados en el lado del servidor de una página web, los más comunes son: PHP, JavaScript, Java, .NET y Python. Además, existen múltiples Bases de Datos a utilizar, por ejemplo: PostgreSQL, MySQL, MongoDB, Oracle, MS SQL Server, etc. Toda estas herramientas componen lo que hoy llamamos el **Back-end**.

### PHP - PostgreSQL

Dada las ventajas didácticas de PHP en el aprendizaje de las bases de datos, lo utilizaremos en el desarrollo de algunas tareas, además, haremos uso de PostgreSQL como base de datos y pgAdmin4 como sistema gestor de bases de datos.

{% hint style="warning" %}
Para seguir sin problemas este tutorial, deberías haber completado el tutorial **Instalación de PostgreSQL, PHP y Apache** contenido en este **gitbook**.
{% endhint %}

Una de las tareas recurrentes al momento de desarrollar una página web con PHP, es la conexión a la Base de Datos, en nuestro caso, crearemos un archivo llamado db\_config.php que contendrá lo siguiente:

{% code title="db\_config.php" %}
```php
<?php
// detalles de la conexion
$conn_string = "host=localhost port=5432 dbname=postgres user=postgres password=<contraseña>";
// establecemos una conexion con el servidor postgresSQL
$dbconn = pg_connect($conn_string);
// Revisamos el estado de la conexion en caso de errores.
if(!$dbconn) {
echo "Error: No se ha podido conectar a la base de datos\n";
} else {
echo "Conexión exitosa!\n";
}
// Cerramos la conexion
?>
```
{% endcode %}

Cada vez que necesitemos hacer uso de la base de datos en algún archivo php de nuestro proyecto, podemos incluir este archivo y utilizar la variable $dbconn para realizar las consultas respectivas, para incluir el archivo, escribimos lo siguiente:

```php
<?php include 'db_config.php';?>
```

### PHP - HTML

Una de las grandes ventajas que nos otorga implementar un backend en nuestras páginas web, es que el usuario pueda visualizar información almacenada en la base de datos. 

En la herramienta **Query Editor** de pgAdmin4 realizaremos la siguiente query:

```sql
CREATE TABLE Informe(
	pais VARCHAR(15) PRIMARY KEY,
	nro_cont INTEGER
)
```

Con esto ya podremos añadir datos a la tabla desde nuestros archivos php.

Para escribir código PHP y HTML en mismo archivo, tenemos dos opciones

* **PHP dentro de HTML:**

```php
<p> <?php echo 'Código php dentro de html' ?> </p>
```

* **HTML dentro de PHP:**

```php
<?php 
$variable = "Esto es una variable en php";
echo "<p> $variable </p>";
?>
```

Ambas formas de escribir código son válidas, pero se debe conservar el orden del código lo más posible.

#### Formularios

El manejo de formularios en el desarrollo web es una herramienta obligatoria, permitirán a los usuarios ingresar datos y que estos queden guardados en la base de datos con ayuda de nuestro lenguaje de back-end, PHP. A continuación implementamos un formulario que enviará los datos a una página PHP que configuraremos para guardar los datos en la Base de Datos:

{% tabs %}
{% tab title="index.php" %}
```markup
<?php include 'db_config.php';?>
<!DOCTYPE html>
<html>
<head>
<title>Informe COVID</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
</head>
<body>

<h1 style="color:blue">Informe COVID-19</h1>
<form action="form.php" method="POST">
    <div class="form-group">
      <label for="país">País</label>
      <input type="país" class="form-control" name="pais" placeholder="Ingresa tu país" id="país">
    </div>
    <div class="form-group">
      <label for="nro_cont">Número de contagiados:</label>
      <input type="number" class="form-control" name="nro_cont" placeholder="Ingresa el Número de contagiados" id="nro_cont">
    </div>
    <button type="submit" class="btn btn-primary">Enviar</button>
</form> 

</body>
</html> 
```
{% endtab %}

{% tab title="form.php" %}
```php
<?php include 'db_config.php';


if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $pais = $_POST["pais"];
    $nro_cont = $_POST["nro_cont"];
    $sql = 'INSERT INTO Informe (pais, nro_cont) VALUES ($1, $2)';
    if( pg_query_params($dbconn, $sql, array($pais,$nro_cont)) !== FALSE ) {
        echo "Dato ingresado correctamente <br>";
        echo '<a href="lista.php"> lista de datos </a> <br>';
        echo '<a href="index.php"> Ingresar más datos </a> <br>';
        pg_close($dbconn);
    } else {
        echo "Hubo un error al ingresar el dato";
        pg_close($dbconn);
    }
}
?>
```
{% endtab %}

{% tab title="lista.php" %}
```php
<?php include 'db_config.php';?>
<!DOCTYPE html>
<html>
<head>
<title>Informe COVID</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
</head>
<body>
<?php
$pais = $_POST["pais"];
$nro_cont = $_POST["nro_cont"];
$sql = "SELECT * FROM Informe";
$result = pg_query_params($dbconn, $sql, array());
if( pg_num_rows($result) > 0 ) {
    while($row = pg_fetch_assoc($result)) {
        echo '<br>' . $row["pais"] . " = " . $row["nro_cont"] . '<br>';
    }
    pg_close($dbconn);
} else {
    echo "Hubo un error al solicitar los datos";
    pg_close($dbconn);
}
echo '<a href="index.php"> Ingresar más datos </a>';
?>
</body>
</html> 
```
{% endtab %}
{% endtabs %}

En el archivo Index el usuario encontrará un formulario que envía datos a la página **form.php** con el método POST \(para más información, investigar sobre tipos de peticiones HTTP\),  en form.php validamos el tipo de petición HTTP, en nuestro caso debe ser POST. Para obtener los datos enviados haremos uso de **$\_POST,** el cual es un array clave-valor de PHP \(similar a diccionarios en python y Mapas en Java\), donde la clave corresponde al atributo **name** del campo de entrada en el código HTML. 

```php
<input type="país" class="form-control" name="pais" placeholder="Ingresa tu país" id="país">
<input type="number" class="form-control" name="nro_cont" placeholder="Ingresa el Número de contagiados" id="nro_cont">
```

Posteriormente creamos la consulta en la variable $sql 

{% tabs %}
{% tab title="form.php" %}
```php
$sql = 'INSERT INTO Informe (pais, nro_cont) VALUES ($1, $2)';
```
{% endtab %}
{% endtabs %}

Donde $1 y $2 serán remplazados posteriormente por las variables que queramos utilizar. 

En la condicional del if se realiza la consulta, donde como primer parámetro pasamos la conexión a la base de datos, como segundo parámetro pasamos la consulta y finalmente pasamos las variables que tomarán el lugar de $1 y $2 respectivamente, si la consulta sale bien lo imprimimos en pantalla y le damos la opción al usuario de ir a la página para ingresar más datos o listar todos los datos ingresados.

{% hint style="warning" %}
Por Seguridad recuerda siempre cerrar la conexión a la base de datos con :

```php
pg_close($dbconn);
```

Una vez la hayas terminado de utilizar.
{% endhint %}

En el archivo **lista.php** guardamos el resultado de la consulta en la variable $result, corroboramos si la consulta se realizó correctamente con:

{% tabs %}
{% tab title="lista.php" %}
```php
if( pg_num_rows($result) > 0 ) {
```
{% endtab %}
{% endtabs %}

Y recorremos cada uno de los resultados:

{% tabs %}
{% tab title="lista.php" %}
```php
while($row = pg_fetch_assoc($result)) {
        echo '<br>' . $row["pais"] . " = " . $row["nro_cont"] . '<br>';
    }
```
{% endtab %}
{% endtabs %}

Con **pg\_fetch\_assoc** solicitamos la siguiente fila encontrada en la consulta, cuando hayamos recorrido todas las filas, entonces se apuntará a **null** y el **while** terminará.

{% hint style="info" %}
Para concatener textos y variables en php podemos usar la sintaxis de punto **.** lo cual puede resultar muy cómodo.
{% endhint %}

{% hint style="info" %}
Utilizamos la etiqueta &lt;a href="miPagina.php"&gt;Link aquí&lt;/a&gt;  para insertar enlaces en la página web.
{% endhint %}

### Puesta en marcha

Ya puedes correr tu servidor Apache y tu base de datos PostgreSQL, y dejar tu aplicación en la carpeta señalada en el tutorial **Instalación de PostgreSQL, PHP y Apache** contenido en este gitbook, a continuación imágenes del resultado:

![En la imagen el formulario para Ingresar nuevas cifras de contagiados en un Pa&#xED;s.](../.gitbook/assets/image%20%2811%29.png)

![En la imagen la respuesta luego de ingresar un dato.](../.gitbook/assets/image%20%288%29.png)

![En la imagen la lista de pa&#xED;ses ingresados con sus n&#xFA;meros de contagiados.](../.gitbook/assets/image%20%285%29.png)

{% hint style="info" %}
Para más información de PHP visitar:

[https://www.w3schools.com/php/default.asp](https://www.w3schools.com/php/default.asp)

[https://www.php.net/manual-lookup.php?pattern=pg&scope=quickref](https://www.php.net/manual-lookup.php?pattern=pg&scope=quickref)
{% endhint %}



