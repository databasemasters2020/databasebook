---
description: >-
  Guía de cómo hacer un dump y otros aspectos relevantes para el desarrollo de
  tareas.
---

# Cómo hacer un dump en PostgreSQL

## ¿Qué es un dump?

Un dump es, en pocas palabras, un archivo que contiene información relevante de una Base de Datos en formato de instrucciones SQL. Es decir, puede poseer información respecto al modelo \(tablas, restricciones, etc\) y/o respecto a los datos \(todos los valores insertados en las tablas\). Es una forma de respaldar una Base de Datos para poder ser traspasada a otra máquina o restaurarla en caso de pérdida.

### Haciendo un dump con pgAdmin4

En el menú izquierdo,  haga click derecho en la base de datos que desee respaldar y seleccione la opción **Backup...**

![Haciendo un dump de la BD &quot;Prueba&quot;](../.gitbook/assets/image%20%289%29.png)

Luego, en **Filename** especifique la ubicación donde desea guardar el archivo, junto con su nombre. El dump creará un archivo en formato **.sql**. En este ejemplo, guardaremos el dump en el escritorio con el nombre **tarea-1.sql**.

![](../.gitbook/assets/image%20%2816%29.png)

Le damos a **Create.** Finalmente, presionamos **Backup** y una notificación nos indicará si la creación del dump fue exitosa.

![](../.gitbook/assets/image%20%283%29.png)

{% hint style="warning" %}
Para evitar problemas de configuración, evite modificar otros ajustes.
{% endhint %}

Para restaurar una Base de Datos, haga click derecho en la Base de Datos deseada, seleccione **Restore...**, busque su archivo dump de forma análoga a como lo guardó. Si no encuentra el archivo, asegúrese de definir que busca un archivo en formato **sql.** 

  

![Por defecto, Restore abre la ventana buscando en formato &quot;backup&quot;.](../.gitbook/assets/image%20%2818%29.png)

Luego, en la pestaña **Restore options** habilite la opción **Clean before restore.** Finalmente, presionamos **Restore** y de forma análoga pgAdmin nos indicará si la restauración fue exitosa.

![Recuerde habilitar clean before restore.](../.gitbook/assets/image%20%288%29.png)

### Haciendo un dump desde la Consola.

Abra un Símbolo del Sistema en la carpeta que desea guardar el dump. Luego, simplemente ejecute el comando:

```text
    pg_dump -Fc -U postgres -d NombreBD > NombreArchivo.sql
```

Reemplace **NombreBD** y **NombreArchivo** por los relevantes en su situación.

Para cargar un dump, se utiliza el siguiente comando:

```text
pg_restore -U postgres -d NombreBD NombreArhivo.sql
```

Reemplace **NombreBD** y **NombreArchivo** por los relevantes en su situación.

{% hint style="info" %}
Al ejecutar alguno de estos comandos, la consola pedirá la contraseña del usuario **postgres.**
{% endhint %}

