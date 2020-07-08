---
description: Guía breve de las operaciones CRUD en Ruby on Rails
---

# RoR CRUD

### Agregando vistas a un controlador ya creado <a id="agregando-vistas-a-un-controlador-ya-creado"></a>

La vinculación Vista-Controlador en Rails está determinada por 3 aspectos.  
  
Supongamos el controlador Tweets, al cual necesitamos agregar la vista "show":

1. Dentro de la carpeta **/app/views/tweets** debe existir un archivo de vista asociado con nombre _**show.html.erb**_  
2. En /app/controllers/tweets\_controller.rb debe existir un método con el mismo nombre que la vista.
3. En el archivo /config/routes.rb debe existir una ruta que asocie el controlador con una vista \(éste paso será realizado automáticamente con el comando resources\)

## Sobre las rutas... <a id="sobre-las-rutas"></a>

Para realizar un CRUD se necesitan peticiones tipo GET y tipo POST, un CRUD completo tiene las siguientes rutas:

```text
    articles GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
 new_article GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
     article GET    /articles/:id(.:format)      articles#show
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
```

El formulario en **new** llamará al método **create**, el cual será el responsable de crear el registro nuevo y retornarlo.

El formulario **edit** llamará al método **update**, el cual será el responsable de actualizar el registro.

El método **delete** será llamado desde un botón en la vista **show** y eliminará el registro.

Para generar todas estas rutas de forma automática, Rails tiene un simple comando, el cual debe ir en /config/routes.rb

```text
resources :articles
```

{% hint style="warning" %}
Creará automáticamente todas las rutas, pero ojo, deberán existir los métodos en el controlador y las vistas necesarias.
{% endhint %}

## Tenemos las vistas y métodos pero vacías ¿Ahora qué? <a id="tenemos-las-vistas-y-metodos-pero-vacias-ahora-que"></a>

### Sobre los métodos <a id="sobre-los-metodos"></a>

Use la siguiente plantilla para sus métodos, rellene con la información faltante:

{% hint style="info" %}
Puede ayudarse de la documentación de rails:
{% endhint %}

{% page-ref page="ror-crud.md" %}

