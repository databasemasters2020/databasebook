---
description: Librerías requeridas para realizar una API Rest en Flask
---

# Instalación Flask

Para realizar instalaciones en Python es sumamente recomendado utilizar el gestor de paquetes **PIP**, con el podremos instalar una gran cantidad de librerías para el lenguaje Python.

{% hint style="info" %}
En caso de no tener instalado PIP, seguir el siguiente tutorial: [https://pip.pypa.io/en/stable/installing/](https://pip.pypa.io/en/stable/installing/)
{% endhint %}

## Entorno virtual de desarrollo

Si ya has trabajo con Python previamente, notarás que a medida que pasa el tiempo y hemos trabajado en múltiples proyectos la cantidad de librerías que podríamos tener instaladas puede ser muy grande, esto podría generar algunos problemas en nuevos programas que desarrollemos utilizando python, ya que se podrían generar colisiones de nombres entre librerías, lentitud al cargar las librerías, etcétera. Para solucionar este problema es muy recomendable hacer uso de un entorno virtual de desarrollo, el cual nos provee un espacio de trabajo restringido a las instalaciones específicas que hagamos estando en ese entorno virtual. Por ejemplo, si instanciamos un entorno virtual X en python3 e instalamos numpy \(considerando que nunca antes la habíamos instalado\) entonces numpy solo estará disponible cuando activemos el entorno X, si salimos del entorno X, numpy no estará disponible. A continuación se muestra como crear un entorno virtual llamado **tarea3bd** en consola de comandos:

```text
python -m venv tarea3bd
```

Para activar el entorno virtual ejecute el siguiente comando:

{% tabs %}
{% tab title="MacOS/Linux" %}
```text
source mypython/bin/activate
```
{% endtab %}

{% tab title="Windows" %}
```
tarea3bd\Scripts\activate
```
{% endtab %}
{% endtabs %}

## Instalando Flask, psycopg2 y SQLAlchemy

Ejecutar en consola:

```text
pip install flask
```

```text
pip install psycopg2-binary
```

```text
pip install Flask-SQLAlchemy
```



