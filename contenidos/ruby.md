---
description: Guía rápida de iniciación en Ruby
---

# Ruby

Ruby es un lenguaje muy expresivo, enfocado en la simplicidad y la productividad del programador. Es ideal para principiantes por su sintaxis clara y fácil de entender, pero lo suficiente robusto como para crear aplicaciones que pueden escalar a millones de usuarios.

### Mostrar por Pantalla

```ruby
puts "Hola Mundo" # incluye un salto de linea al final
print "Hola Mundo" # no incluye el salto de linea
```

### Variables

```ruby
var_1 = "Soy la variable 1"
var_2 = 2
```

Un ejemplo para usar variables dentro de un String:

```ruby
name = "Bastián" # cámbialo por tu nombre
puts "Hola #{name}"
```

### Condicionales

#### Operadores Lógicos

**AND** = and o &&  
**OR** = or o \|\|

Las condiciones if son muy parecidas a Python, no llevan ":" y terminan con un "end"

```ruby
if condicion_1
  # código que se ejecuta si se cumple la condición_1
elsif condicion_2
  # código si <condicion_1> NO se cumple, pero <condicion_2> sí
elsif condicion_3
  # código si <condicion_1> y <condicion_2> NO se cumplen, pero <condicion_3> sí
else
  # código si ninguna de las condiciones se cumple
end
```

### Ciclos

#### While

```ruby
while <condición>
  # acá va el código que se va a repetir mientras la condición sea verdadera
end
```

#### For

```ruby
for i in (0..9)
    puts i
end
```

### Iterando N Veces <a id="iterando-n-veces"></a>

```ruby
num = 850
num.times do
  puts "Hola mundo"
end
```

#### Si necesitas el valor sobre el que iteras: <a id="si-necesitas-el-valor-sobre-el-que-iteras"></a>

```ruby
850.times do |i|
  puts "#{i} Hola mundo"
end
```

### Iterando sobre un rango <a id="iterando-sobre-un-rango"></a>

```ruby
(10..15).each do |i|
  puts "#{i} Hola mundo"
end
```

## Arreglos <a id="arreglos"></a>

Parecidos a las listas de python.

```ruby
array = [1, "Pedro", true, false, "Juan"]
​
#Obteniendo el primer elemento
array[0]

#Reemplazando un elemento
array[1] = "Germán"

#insertando un nuevo elemento al final
array.push("Germán")

#insertando un elemento dado un indice
array.insert(0, "Juan") 

#eliminando un elemento dado un indice
array.delete_at(1)

#Recorriendo elementos
array.each do |element|
  puts element
end

#Recorriendo elementos e indice a la vez
array.each_with_index do |element, index|
  puts "#{index}: #{element}"
end
```

#### Métodos útiles

| Método | Descripción |
| :--- | :--- |
| **first** | Retorna el primer elemento del arreglo |
| **last** | Retorna el último elemento del arreglo |
| **shuffle** | Retorna un nuevo arreglo mezclado aleatoriamente |
| **length** | Retorna el tamaño del arreglo |

## Hashes

La versión de Ruby de los diccionarios de Python.

```ruby
#nuevo hash
persona = {"nombre" => "Germán", "apellido" => "Escobar", "edad" => 34}

#obteniendo un valor
persona["nombre"]

#agregando nuevos elementos
persona["peso"] = 65

#modificar un elemento
persona["peso"] = 70

#eliminando elemento
persona.delete("peso")

#recorriendo
persona.each do |llave, valor|
  puts "#{llave}: #{valor}"
end
```

### Usando símbolos como llaves <a id="usando-simbolos-como-llaves"></a>

En los ejemplos anteriores hemos usado cadenas de texto y números como llaves de los hashes. Sin embargo, en Ruby hay un tipo de datos que es muy utilizado como llaves en los hashes: los símbolos.

```ruby
status = :encendido # nuestro primer símbolo
```

Los símbolos son muy parecidos a las cadenas de texto pero con las siguientes diferencias:

* No están envueltos en comillas.
* Empiezan con dos puntos \(:\).
* No contienen espacios en blanco.

Creemos nuestro primer hash con símbolos:

```ruby
persona = {:nombre => "Germán", :apellido => "Escobar", :edad => 34, :estatura => 1.8}
```

Muy parecido al ejemplo anterior. Pero entonces ¿cuál es la ventaja de usar símbolos como llaves de un hash? La respuesta es que cuando usamos símbolos podemos escribir los hashes de forma más corta:

```ruby
persona = {nombre: "Germán", apellido: "Escobar", edad: 34, estatura: 1.8}
```

Los cambios que hicimos son los siguientes:

* Eliminamos el `=>` \(hash rocket\)
* Movimos los dos puntos \(:\) al final del símbolo.

Para obtener algún valor del hash utilizamos el símbolo:

```ruby
persona[:nombre]
```

## Métodos <a id="metodos"></a>

Como las funciones de Python

```ruby
def hello(name)
  return "Hola #{name}"
end
```

## Orientación a Objetos <a id="orientacion-a-objetos"></a>

Como comentamos anteriormente, Ruby es un lenguaje orientado objeto. En Ruby casi todo es un objeto.

Por ejemplo, cuando ejecutar la siguiente linea de código

```ruby
array1 = []
```

Es exactamente lo mismo que 

```ruby
array1 = Array.new
```

En donde llamamos al método "new" de la clase "Array", para crear un nuevo objeto que será guardado en la variable "array1"

### Clases y objetos <a id="clases-y-objetos"></a>

 Las clases y los objetos son los conceptos más importantes de la Programación Orientada por Objetos, y están fuertemente relacionados. **Los objetos se crean a partir de clases, y las clases definen los atributos y el comportamiento que tendrán los objetos**. A los objetos también se les llama _instancias de clase_.

```ruby
class Dado

  def roll
    1 + rand(6)
  end

end

#  Vamos a crear un nuevo objeto dado...
dado1 = Dado.new

#  ...y hacerlo rodar.
puts dado1.roll
```

