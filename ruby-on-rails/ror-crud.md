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

```ruby
#Metodo GET que muestra la vista show, obtiene un tweet en especifico
#desde la URL users/:id_user/tweets/:id
def show
  #Debera guardar en la variable @tweet el tweet con el id dado,
  #Recuerde como se buscaban cosas en el modelo, puede ver un ejemplo
  #en el perfil del usuario.
  @tweet = ?
end

# metodo POST, es llamado al apretar "enviar" en el formulario de new, 
# Crea un tweet con los parametros del formulario.
def create
  #Deberá guardar en la variable @user el usuario que está actualmente logueado
  #recuerde como se obtiene, use las bondades de devise
  @user = ?
  #Use el siguiente codigo para guardar un nuevo tweet de ese usuario
  @tweet = @user.tweets.build(tweet_params)
  #Ya que en el formulario no se debe mostrar la fecha, añadiremos aquí la fecha actual
  #¿Como se obtiene la fecha actual en rails?
  @tweet.date = ?
  
  #Use el metodo para agregar el registro recien creado
  if @tweet.?
    redirect_to dashboard_user_index_path
  else
    flash[:notice] = 'Error'
    redirect_to dashboard_user_index_path
  end
end

#Metodo GET que muestra la vista edit, obtiene un tweet en especifico
#desde la URL users/:id_user/tweets/:id/edit para editarlo

def edit
  #Debera guardar en la variable @tweet el tweet con el id dado,
  #Recuerde como se buscaban cosas en el modelo, puede ver un ejemplo
  #en el perfil del usuario.
  @tweet = ?
end

# metodo POST, es llamado al apretar "enviar" en el formulario de edit, 
# actualiza un tweet con los parametros del formulario.
def update
  #Debera guardar en la variable @tweet el tweet con el id dado,
  #Recuerde como se buscaban cosas en el modelo, puede ver un ejemplo
  #en el perfil del usuario.
  @tweet = ?
  
  #una vez encontrado el tweet a actualizar, actualizaremos con los
  #parametros del formulario, use el metodo para actualizar un registro en rails
  if @tweet.?(tweet_params)
    redirect_to user_tweet_path(@tweet.user,@tweet)
  else
    render 'edit'
  end
end

# metodo POST, es llamado al apretar "eliminar", 
# elimina un tweet.

def destroy
  #Debera guardar en la variable @tweet el tweet con el id dado,
  #Recuerde como se buscaban cosas en el modelo, puede ver un ejemplo
  #en el perfil del usuario.
  @tweet = ?
  
  #use el metodo para eliminar un registro en rails.
  @tweet.?
 
  redirect_to dashboard_admin_index_path
end

#En este metodo privado se enlistan los parametros que requerirá el registro.
private
  def tweet_params
    params.require(:tweet).permit(:tweet, :date) #En este caso serán tweet y date
  end
  
end
```

## Links hacia los métodos

Ejecute `rails routes`

Fíjese que cada ruta tiene un nombre en especifico a su izquierda, por ejemplo el metodo "tweets\#destroy" tiene el siguiente nombre: "user\_tweet" \(viene desde el GET arriba solo que no lo muestra especificamente\), también se dará cuenta que este método es del tipo DELETE.

Para crear un link hacia un método puede usar la función link\_to de rails, tiene como parámetros:

* Lo que mostrara el link, en este ejemplo "Eliminar articulo"
* El nombre de la ruta, ésta terminada en "\_path" y entre paréntesis los parámetros que necesita, por ejemplo, article\#destroy necesita el id del articulo a eliminar, por lo que pasaremos el articulo que queremos eliminar como parámetro.
* Método a usar, si el método es del tipo GET se puede omitir.
* Parámetros varios, en este caso se usa data para confirmar la eliminación del articulo, no es necesario usarlo en otro tipo que no sea delete.

Use esta información para mostrar en cada tweet un link a show, edit y destroy.

{% code title="index.html.erb" %}
```ruby

<%= link_to 'Eliminar articulo', article_path(article),
                    method: :delete, data: { confirm: 'Are you sure?' } %>

```
{% endcode %}

### Sobre las vistas

Como se dijo anteriormente, las vistas necesarias son 3, show, edit, y new \(que en este caso será dashboard\_user\_index, ya que ahí crearemos un nuevo tweet\)

#### Sobre los formularios: <a id="sobre-los-formularios"></a>

Rails viene con unos métodos de ayuda para crear los campos del formulario. Aunque es posible crearlos manualmente con HTML, los métodos se encargan del nombramiento y asociar el campo al valor que está en el modelo.

```ruby
<%= form_with(model: lo que modificaremos (si es que lo hay), url: el metodo al que iremos do |f| %>
  
  #Campos del formulario
  <p>
    <%= f.label :title %>
    <%= f.text_field :title %>
  </p>
  
  #Boton para enviar
  <%= f.submit "Enviar"%>
  
<% end %>
```

Los campos se crean utilizando la variable que se le pasó en el bloque del `form_with`, en este caso `f`.

`label` se utiliza para crear una etiqueta `<label>` asociada al campo.

`text_field` se utiliza para crear una etiqueta de tipo `<input type="text">`.

`submit` se utiliza para crear un botón de submit `<input type="submit">.`

Otros campos de entrada incluyen:

* `text_area` - crea un área de texto:

```ruby
<%= f.text_area :description, rows: 20 %>
```

* `check_box` crea una caja de selección:

```ruby
<%= f.check_box :paid %>
```

* `date_field`
* `password_field`
* `radio_button` - crea un botón de radio:

```ruby
  <%= f.radio_button :gender, "male" %>
  <%= f.radio_button :gender, "female" %>
  `
```

* `hidden_field` - crea un campo oculto
* `email_field` - crea un campo de email
* `url_field` - crea un campo de URL
* `collection_select` - crea una lista desplegable

```ruby
  <%= f.collection_select(:city_id, City.all, :id, :name) %>
```

Con esto aprendido sus vistas deben de tener lo siguiente

En la Vista Show de Tweets

{% code title="show.html.erb" %}
```ruby
#Muestre aquí la informacion de @tweet (que obtuvo en el metodo show)
```
{% endcode %}

Para el formulario de edit de Tweets

{% code title="edit.html.erb" %}
```ruby
<h1>Editando un Tweet</h1>
 
<%= form_with(model:@tweet, url: user_tweet_path(@tweet.user, @tweet)) do |f| %>
  
    #Agregue el campo tweet y date  
  
    <%= f.submit "Editar"%>
  
<% end %>
 
<%= link_to 'Volver',  dashboard_admin_index_path %>
```
{% endcode %}

Para el formulario en index de DashboardUser

{% code title="index.html.erb" %}
```ruby
<%= form_with(model: [current_user, current_user.tweets.build]) do |f| %>
  
  #Agregue el campo tweet
  
  <%= f.submit "Tweetear" %>

<% end %>
```
{% endcode %}

