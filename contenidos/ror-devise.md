---
description: >-
  Guía rápida del uso de gemas en Ruby on Rails. Gema Devise para el manejo de
  sesiones.
---

# RoR Devise

## Gemas de Ruby <a id="gemas-de-ruby"></a>

Una gema es, básicamente, la manera en que Ruby permite distribuir programas, módulos o librerías que extienden funcionalidad, casi siempre específica, y que hacen no tener que repetirnos \(Don't Repeat Yourself\) volviendo más fácil nuestro flujo de desarrollo. Por ejemplo, hay gemas para Rails que se encargan de la autenticación de usuarios como es el caso de Devise, o Carrierwave, una gema que se encarga de subidas de archivos.

Para casi cada acción que se realiza repetitivamente a la hora de programar, existe una gema que lo vuelve todo más fácil.

Cabe decir que las gemas pueden depender de otras gemas para funcionar.

## **Devise** <a id="devise"></a>

Como se mencionó anteriormente Devise es una gema de Ruby on Rails que se encarga de la autenticación de usuarios, en pocos pasos se puede tener listo el sistema de registro, login y manejo de sesiones para distintos usuarios.

Luego de instalar y configurar la gema se pueden crear modelos de Devise, que vienen por defecto con correo y contraseña entre otros atributos, cada modelo de Devise va a tener sus propias rutas, pero comparten la misma vista, por lo que si se desea tener Usuarios distintos, se tendrá que editar las rutas y vistas que vienen por defecto.

Los pasos para configurar [Devise](https://github.com/plataformatec/devise) son los siguientes:

* Agregar la gema al `Gemfile` \(dentro de su directorio de proyecto\)

```text
gem 'devise'
```

* Ejecuta `bundle install` para instalarla.
* Ejecuta el generador de [Devise](https://github.com/plataformatec/devise):

```text
rails generate devise:install
```

* El siguiente paso es crear el modelo del o de los usuarios de la aplicación, generalmente `User`

```text
 rails generate devise User
```

* Ejecuta `rails db:migrate` para correr las migraciones.

## Rutas que crea Devise <a id="rutas-que-crea-devise"></a>

Asumiendo que el usuario es `User`, [Devise](https://github.com/plataformatec/devise) crea, entre otras, las siguientes rutas:

| Ruta | Descripción |
| :--- | :--- |
| `GET /users/sign_up` | Formulario de registro |
| `POST /users` | Registrarse |
| `GET /users/sign_in` | Formulario de login |
| `POST /users/sign_in` | Login |
| `DELETE /users/sign_out` | Salir |

Para ver el listado completo ejecuta `rails routes`. Las demás rutas son, en su mayoría, para el manejo de contraseñas \(recuperar, cambiar, etc\).

## Filtros y helpers de controlador <a id="filtros-y-helpers-de-controlador"></a>

​[Devise](https://github.com/plataformatec/devise) crea algunos métodos de ayuda y filtros para que puedas manejar la autenticación en tu aplicación.

Asumiendo que tu usuario se llama `User`, el filtro que puedes agregar sobre los controladores que quieres proteger es el siguiente:

```text
before_action :authenticate_user!
```



Los métodos de ayuda a los que vas a tener acceso en los controladores y vistas son los siguientes:

| Método | Descripción |
| :--- | :--- |
| `user_signed_in?` | Retorna `true` si el usuario está autenticado, `false` de lo contrario |
| `current_user` | Retorna el usuario que está autenticado o `nil` |
| `user_session` | Retorna la sesión del usuario |

## El modelo de usuario <a id="el-modelo-de-usuario"></a>

Asumiendo que al usuario lo llamaste `User` al generarlo, el modelo que genera [Devise](https://github.com/plataformatec/devise) es el siguiente:

```ruby
class User< ApplicationRecord  
    # Include default devise modules. Others available are:  # :confirmable, :lockable, :timeoutable and :omniauthable  
    devise :database_authenticatable, :registerable,         
    :recoverable, :rememberable, :trackable, :validatable
end
```



Este es un modelo normal pero utiliza el método `devise` que recibe una serie de módulos que puedes activar o desactivar.

Los módulos que vienen activados son los siguientes:

| Módulo | Descripción |
| :--- | :--- |
| `database_authenticatable` | Los usuarios se van a poder autenticar con un nombre de usuario y contraseña almacenado en la base de datos. |
| `registereable` | Los usuarios van a poder registrarse, actualizar y eliminar su perfil. |
| `recoverable` | Los usuarios van a poder recuperar su contraseña |
| `rememberable` | Habilita la opción "Recordarme" en el login |
| `trackable` | Habilita el seguimiento al usuario \(de dónde se autenticó, cuantas veces, de qué IP, etc.\) |
| `validatable` | Valida el email y el password |

## Modificando las vistas <a id="modificando-las-vistas"></a>

Por defecto no vas a ver vistas ni controladores de [Devise](https://github.com/plataformatec/devise) en tu aplicación. Sin embargo, si quieres modificarlos puedes decirle a [Devise](https://github.com/plataformatec/devise) que los genere.

Para generar las vistas utiliza el siguiente comando:

```ruby
$ rails generate devise:views users
```

De esta forma puedes modificar las vistas \(Análogo para admins\).

