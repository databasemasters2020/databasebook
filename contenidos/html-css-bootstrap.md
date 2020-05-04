---
description: Guía rápida de iniciación en HTML y CSS
---

# HTML, CSS y Bootstrap

Las Bases de Datos son aplicadas en diversos contextos, sin duda, uno de los mayores campos de aplicación es el Desarrollo Web. En este tutorial aprenderemos dos de los lenguajes más utilizados en esta área.

## HTML

Corresponde a las siglas de Hypertext Markup Language \(Lenguaje de marcas de hipertexto\). Es importante comprender el uso de HTML, una confusión común es que se considere HTML como un lenguaje de programación, lo cual es incorrecto, no es que sea menos importante que un lenguaje de programación, sino que, fueron creados para ser útiles en distintas tareas.   
HTML nos permite definir estructuras de texto y contenido multimedia con un estándar comprensible para los navegadores actuales. En una etapa temprana del aprendizaje del lenguaje Español se nos enseñaron distintos tipos de texto, tales como: texto literario, texto expositivo, texto científico, texto argumentativo, texto informativo, cada uno con sus particularidades y aplicaciones, en este caso realizaremos una analogía entre un texto informativo, la noticia y el lenguaje HTML.

Si observamos páginas de diarios, notaremos una estructura similar en cada una de las páginas, existe un estándar de como entregar la información. Analicemos esta estructura: 

![En la imagen una noticia donde se se&#xF1;alan sus partes\(t&#xED;tulo, entrada, imagen, cuerpo, etc\)](../.gitbook/assets/anotacion-2020-05-03-225738.jpg)

Como vemos, el escritor de la noticia definió una estructura mediante la ubicación de cada uno de los elementos de la estructura de la noticia. Nosotros, amantes del código, le indicaremos a cada uno de los navegadores la estructura de nuestra página web mediante etiquetas en el código, las cuales, pueden coincidir con las etiquetas utilizadas en la estructura de la noticia.

![En la imagen la estructura web m&#xE1;s b&#xE1;sica\(DOM\)](../.gitbook/assets/390px-simpe_html_page_dom.svg.png)

En la imagen observamos uno de los document object model\(DOM\) más básicos, el cual muestra la etiqueta HTML como padre de HEAD y BODY, y Body como padre de H1 y P. Explicamos estas etiquetas a continuación:

* **HTML:** Con esta etiqueta indicamos la raíz de un documento HTML, todo lo que esté dentro de ella será considerado por los navegadores.
* **HEAD:** Dentro de esta etiqueta insertaremos toda la información\(metadata\) referente al documento, por ejemplo: título de la pagina web, autor, etc.
* **BODY:** Dentro de esta etiqueta insertaremos el cuerpo de nuestra página web\(similar al cuerpo de la noticia\).
* **H1:** Dentro de esta etiqueta insertamos títulos principales \(Podemos utilizar h2, h3,...,h6 para indicar titulos de menor importancia\)
* **P:** Dentro de esta etiqueta insertamos párrafos. 

Las etiquetas aquí nombradas son un grupo muy reducido de todas las disponibles, ya que, el lenguaje es una rama del conocimiento muy amplia y está muy presente en los documentos HTML.

{% hint style="info" %}
Para consultar todas las etiquetas de HTML y sus descripciones, visitar el siguiente link:  
[https://www.w3schools.com/tags/default.asp](https://www.w3schools.com/tags/default.asp)
{% endhint %}

{% hint style="warning" %}
Es importante utilizar las etiquetas correctamente, es decir, concordante a su significado, los títulos principales deben ir en h1 y no en h2, h3, h4, etc..., ya que, si no utilizamos correctamente las etiquetas, los buscadores de la web penalizarán nuestra web y será más dificil alcanzar visitas\(Para más información investigar sobre SEO\).
{% endhint %}

La mayoría de etiquetas que utilizaremos en HTML deben ser escritas con el siguiente formato: 

```markup
<p> Insertar párrafo aquí </p>
```

Es decir, abrimos la etiqueta con &lt;nombre de la etiqueta&gt; y cerramos con &lt;/nombre de la etiqueta&gt;, existen excepciones, tal como: la etiqueta &lt;br&gt; que utilizamos para hacer un salto de linea. 

Con nuestro editor de código favorito crearemos un archivo llamado index.html, e insertamos la típica estructura de inicio para una página web: 

```markup
 <!DOCTYPE html>
<html>
<head>
<title>Título de la página web</title>
</head>
<body>

<h1>Este es un título principal</h1>
<p>Esto es un párrafo</p>

</body>
</html> 
```

Para visualizar el resultado, puede abrir este archivo con tu navegador de preferencia.

## CSS

Pero, una noticia no contiene solamente texto separado en párrafos y etiquetas linealmente, también hay textos destacados, títulos llamativos, elementos pocisionados en un lugar u otro,  colores, distintos tamaños de texto, uso de sombras, etc. Para cada de estas características nombradas, existe un lenguaje adicional complementario a HTML llamado Cascading Style Sheets\(CSS, hojas de estilo en cascada\), el cual, nos permite hacer todo lo relacionado al diseño de la página web. 

A continuación las dos principales formas de añadir estilos a nuestra página web:

* **Atributo Style:** Podemos añadir fácilmente un estilo a los elementos dentro de una etiqueta, añadiendo el atributo Style en la etiqueta, a continuación un ejemplo: 

```markup
<!DOCTYPE html>
<html>
<head>
<title>Título de la página web</title>
</head>
<body>

<h1 style="color:red">Este es un título principal</h1>
<p>Esto es un párrafo</p>

</body>
</html> 
```

* **Crear un archivo styles.css en la carpeta css/:** Podemos separar nuestros estilos del código HTML, organizándolos en su propio archivo css, para esto debemos indicar en el HEAD del código HTML que se requiere de un archivo css adicional, a continuación un ejemplo:

{% tabs %}
{% tab title="index.html" %}
```markup
<!DOCTYPE html>
<html>
<head>
<title>Título de la página web</title>
<link rel="stylesheet" type="text/css" href="css/styles.css">
</head>
<body>

<h1 style="color:red">Este es un título principal</h1>
<p>Esto es un párrafo</p>

</body>
</html> 
```
{% endtab %}

{% tab title="css/styles.css" %}
```css
p {
    background-color: blue;
    color: white;
}
```
{% endtab %}
{% endtabs %}

Se aconseja utilizar la segunda opción en la mayoría de los casos, la primera solo nos vendría bien en el caso de estar haciendo pruebas de diseño.

## Bootstrap

La cantidad de diseños que podemos realizar con css es abismante, con el avance del desarrollo web se identificaron elementos comunes en la mayoría de sitios web, por lo que, surgieron frameworks\(herramientas de apoyo en el desarrollo\) con este tipo de diseños en css listos para usar, uno de los más conocidos es **Bootstrap**, para utilizarlo deberás incluir en tu archivo HTML los códigos de estos frameworks que se encuentran en la web\(también los pueden descargar localmente\), a continuación su implementacion: 

```markup
<!DOCTYPE html>
<html>
<head>
<title>Título de la página web</title>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
</head>
<body>

<h1 style="color:red">Este es un título principal</h1>
<p>Esto es un párrafo</p>
<form action="#">
    <div class="form-group">
      <label for="email">Correo</label>
      <input type="email" class="form-control" placeholder="Ingresa tu email" id="email">
    </div>
    <div class="form-group">
      <label for="pwd">Contraseña:</label>
      <input type="password" class="form-control" placeholder="Ingresa tu contraseña" id="pwd">
    </div>
    <div class="form-group form-check">
      <label class="form-check-label">
        <input class="form-check-input" type="checkbox"> Recordar usuario
      </label>
    </div>
    <button type="submit" class="btn btn-primary">Enviar</button>
</form> 
</body>
</html> 
```

Como vemos, en pocos pasos podemos crear formularios realmente atractivos, el revisar cada una de las herramientas que nos ofrecen estos lenguajes y frameworks está fuera del alcance de este tutorial. Por lo que, te invitamos a explorar este basto mundo y puedas crear tus propios diseños y páginas web.

{% hint style="info" %}
Podrás editar los elementos predeterminados de bootstrap con tus propias hojas de estilo, linkeando tu hoja de estilo después de linkear la hoja de estilo de bootstrap y sobrescribirás los cambios que ingreses en tu propia hoja de estilo.
{% endhint %}

{% hint style="success" %}
Podrás obtener más información y tutoriales de gran calidad en la página:

[https://www.w3schools.com/](https://www.w3schools.com/)
{% endhint %}

