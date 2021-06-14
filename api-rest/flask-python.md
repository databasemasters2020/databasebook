---
description: >-
  En este apartado se desarrollará paso a paso una API Rest en el framework
  Flask de Python
---

# Flask \| Python

## Endpoints

Un endpoint es una ruta en la que podemos realizar una operación específica sobre la base de datos, a continuación el código principal de nuestra API donde se definen los endpoints \(main.py\): 

{% tabs %}
{% tab title="main.py" %}
```python
from flask import Flask
from flask import jsonify
from config import config
from models import db
from models import User
from flask import request

def create_app(enviroment):
	app = Flask(__name__)
	app.config.from_object(enviroment)
	with app.app_context():
		db.init_app(app)
		db.create_all()
	return app

# Accedemos a la clase config del archivo config.py
enviroment = config['development']
app = create_app(enviroment)

# Endpoint para obtener todos los usuarios
@app.route('/api/v1/users', methods=['GET'])
def get_users():
	users = [ user.json() for user in User.query.all() ] 
	return jsonify({'users': users })

# Endpoint para obtener el usuario con id <id>
@app.route('/api/v1/users/<id>', methods=['GET'])
def get_user(id):
	user = User.query.filter_by(id=id).first()
	if user is None:
		return jsonify({'message': 'User does not exists'}), 404

	return jsonify({'user': user.json() })

# Endpoint para insertar un usuario en la bd
@app.route('/api/v1/users/', methods=['POST'])
def create_user():
	json = request.get_json(force=True)

	if json.get('username') is None:
		return jsonify({'message': 'El formato está mal'}), 400

	user = User.create(json['username'])

	return jsonify({'user': user.json() })

# Endpoint para actualizar los datos de un usuario en la bd
@app.route('/api/v1/users/<id>', methods=['PUT'])
def update_user(id):
	user = User.query.filter_by(id=id).first()
	if user is None:
		return jsonify({'message': 'User does not exists'}), 404

	json = request.get_json(force=True)
	if json.get('username') is None:
		return jsonify({'message': 'Bad request'}), 400

	user.username = json['username']

	user.update()

	return jsonify({'user': user.json() })

# Endpoint para eliminar el usuario con id igual a <id>
@app.route('/api/v1/users/<id>', methods=['DELETE'])
def delete_user(id):
	user = User.query.filter_by(id=id).first()
	if user is None:
		return jsonify({'message': 'El usuario no existe'}), 404

	user.delete()

	return jsonify({'user': user.json() })

if __name__ == '__main__':
	app.run(debug=True)

```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Es probable que al ejecutar el código se les requiera instalar algunas librerías adicionales, en tal caso, se recomienda instalarlas con el gestor de paquetes PIP.
{% endhint %}

## Configuración

En el archivo config.py del proyecto estarán todas las configuraciones de la API, en la documentación de Flask podrán revisar todas las posibles, por ahora, solo configuraremos la conexión a la base de datos PostgreSQL:

{% tabs %}
{% tab title="config.py" %}
```python
class Config:
	pass

# Definimos una clase de configuración, heredamos de la clase Config
class DevelopmentConfig(Config):
	DEBUG = True
	# Ingresamos credenciales para conexión a base de datos
	SQLALCHEMY_DATABASE_URI = 'postgresql://<usuario>:<contraseña>@localhost/<nombre_bd>'
	SQLALCHEMY_TRACK_MODIFICATIONS = False

config = {
	'development': DevelopmentConfig,
}

```
{% endtab %}
{% endtabs %}

Donde deberán reemplazar los campos &lt;campo&gt; por sus datos  de PostgreSQL correspondientes.

## Modelos

En el archivo models.py definiremos lo que más nos interesa, el modelo de la base de datos. Aquí podrán definir relaciones, atributos, entidades, etcétera. A continuación un ejemplo:

```python
from datetime import datetime
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

# Creamos la entidad User
class User(db.Model):
	__tablename__ = 'users'
	id = db.Column(db.Integer, primary_key=True)
	username = db.Column(db.String(50), nullable=False) 
	created_at = db.Column(db.DateTime(), nullable=False, default=db.func.current_timestamp())
	
	@classmethod
	def create(cls, username):
		# Instanciamos un nuevo usuario y lo guardamos en la bd
		user = User(username=username)
		return user.save()

	def save(self):
		try:
			db.session.add(self)
			db.session.commit()

			return self
		except:
			return False
	def json(self):
		return {
			'id': self.id,
			'username': self.username,
			'created_at': self.created_at
		}
	def update(self):
		self.save()
	def delete(self):
		try:
			db.session.delete(self)
			db.session.commit()

			return True
		except:
			return False

```

Ahora solo nos falta correr nuestra API Rest en consola, para esto debemos indicarle a Flask cuál será el archivo principal de nuestra aplicación, para esto ejecutamos el siguiente comando en la carpeta donde tengamos los archivos del proyecto:

{% tabs %}
{% tab title="MacOS/Linux" %}
```
export FLASK_APP=main.py
```
{% endtab %}

{% tab title="Windows" %}
```python
$env:FLASK_APP = "main.py"
```
{% endtab %}
{% endtabs %}

Finalmente, corremos la API en consola de comandos:

```python
flask run
```

{% hint style="warning" %}
Para probar las APIs se recomienda investigar el uso de Postman. En ayudantía veremos como utilizarlo.
{% endhint %}

