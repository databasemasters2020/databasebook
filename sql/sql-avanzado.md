---
description: >-
  En esta sección del apunte se introducirán operaciones más complejas que se
  pueden realizar con SQL.
---

# SQL Avanzado

## ¿Qué sabemos hasta ahora?

A partir de la sección anterior del apunte, sabemos hacer las operaciones básicas para **crear, modificar y eliminar tablas,** además de **crear, modificar, eliminar y seleccionar datos** respecto de dichas tablas. En esta sección, nos enfocaremos en las capacidades para consultar a la base de datos, enfocándonos en la operación **SELECT.** 

{% hint style="info" %}
Durante el transcurso de esta guía se utilizarán las operaciones **JOIN** para poder demostrar los ejemplos. Por tanto, es recomendable entender bien estas operaciones \(al menos **INNER JOIN**\) para poder entender esta sección.
{% endhint %}

## Modelando el Mundo de Pokémon

Comenzaremos presentando un contexto que queremos modelar para así poder pensar en consultas que desearemos hacer. Partiendo por algo simple, sabemos que los [pokémones](https://www.biobiochile.cl/noticias/sociedad/curiosidades-group-sociedad/2016/07/21/este-es-el-plural-correcto-de-pokemon-lo-dice-la-rae.shtml) conocen distintos movimientos, los cuales pueden ser de distintos elementos y distintos tipos. Los modelaremos con la siguiente tabla:

{% tabs %}
{% tab title="SQL" %}
```sql
CREATE TABLE MOVE
(
  ID INT PRIMARY KEY,
  NOMBRE VARCHAR(20),
  CATEG VARCHAR(10) CHECK (CATEG IN ('Especial','Fisico','Status')),
  TIPO VARCHAR(10),
  DANO INT
);
```
{% endtab %}

{% tab title="Algunos Datos" %}
```sql
INSERT INTO MOVE (ID, NOMBRE, CATEG, TIPO, DANO) VALUES
    (1, 'Lanzallamas', 'ESPECIAL', 'Fuego', 90),
    (2, 'Colm. Fuego', 'FISICO', 'Fuego', 65),
    (3, 'Hidropulso',  'ESPECIAL', 'Agua', 60),
    (4, 'Hidrobomba', 'ESPECIAL', 'Agua', 110),
    (5, 'Drenadoras', 'STATUS', 'Planta', 0),
    ...
```
{% endtab %}
{% endtabs %}

Sólo con esta tabla ya podemos hacernos varias preguntas que no podemos contestar usando un **SELECT** simple como los ya vistos.

### Operaciones de Agregación

Una pregunta que puede surgir ahora es _**¿Cuántos movimientos hay registrados actualmente en la base de datos?**_  Probablemente se le ocurrirán algunas soluciones, pero que no son perfectas:

* **Traer todas las filas y ver el tamaño de la tabla resultante:** Si bien es una solución, es tremendamente ineficiente, pues no nos interesa ningún dato específico de la tabla.
* **Ordenar por ID de mayor a menor y traer solamente una fila:** Tampoco es una solución particularmente eficiente. Además, no podemos asumir que la cantidad de movimientos en la Base de Datos se relacionará siempre con el ID mayor, puesto que se pueden haber borrado registros.

Para responder a esta y otras preguntas de índole similar, existen los **Operadores de Agregación.** En este caso, utilizaremos el operador **COUNT\(\*\):**

{% tabs %}
{% tab title="SQL" %}
```sql
--Ejemplo 1
SELECT
    COUNT(*) AS NUM_MOVES
FROM
    MOVE;

--Ejemplo 2
SELECT
    COUNT(*) AS MOV_FISICOS
FROM
    MOVE
WHERE
    CATEG = 'FISICO';
```
{% endtab %}

{% tab title="Resultados" %}
#### Ejemplo 1

| NUM\_MOVES |
| :--- |
| 250 |

#### Ejemplo 2

| MOV\_FISICOS |
| :--- |
| 135 |
{% endtab %}
{% endtabs %}

{% hint style="success" %}
Podemos **asignarle un alias** a una Tabla o una columna utilizando la palabra clave **AS.** De esta forma, podemos referenciar dicha tabla o columna más fácilmente.
{% endhint %}

Como podemos ver, el operador **COUNT\(\*\)** nos permite contar cuántas filas cumplen con los requisitos pedidos.

#### Lista de Operaciones Comunes de Agregación

* **`COUNT(<columna>)`** Cuenta la cantidad de filas que poseen un valor **NO NULO** en **&lt;columna&gt;.** También se puede usar como **`COUNT(*)`** para contar todas las filas.
* **`MAX(<columna>); MIN(<columna>)`** Retorna el valor máximo y mínimo de una columna dada.
* **`AVG(<columna>)`** Retorna el valor promedio de la columna.
* **`SUM(<columna>)`** Retorna la suma de los valores de la columna.

A partir de estas operaciones, podemos contestar preguntas como las siguientes:

* ¿Cuántos movimientos de tipo Fuego hay?
* ¿Cuál es el movimiento Físico que causa más daño?
* ¿Cuántos movimientos de Status hay de tipo Planta?
* ¿Cuál es el daño promedio de los movimientos que no son de Status?

### GROUP BY y ORDER BY

Supongamos ahora que queremos saber cuánto daño es el máximo que puedo causar por cada tipo. Si bien podemos hacer un **SELECT** por cada uno de los elementos, existe una forma más rápida de plantear lo mismo:

{% tabs %}
{% tab title="SQL" %}
```sql
SELECT
    TIPO,
    MAX(DANO) AS DANO_MOV
FROM
    MOVE
GROUP BY
    TIPO
ORDER BY
    MAX(DANO) DESC;
```
{% endtab %}

{% tab title="Resultado" %}
| TIPO | DANO\_MOV |
| :--- | :--- |
| Normal | 250 |
| Fuego | 180 |
| Psíquico | 160 |
| Lucha | 150 |
| Planta | 150 |
| Agua | 150 |
| Roca | 150 |
| Hielo | 140 |
| Volador | 140 |
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Algunos, pero **no todos** los DBMS aceptan el alias de una operación de agrupación dentro del bloque ORDER BY.
{% endhint %}

En esta consulta están ocurriendo 2 cosas importantes:

* **GROUP BY** indica que las filas de la tabla deben ser agrupadas por la columna `TIPO`, para luego ejecutar la consulta individualmente en cada grupo y obtener **una única fila como resultado de cada agrupación.**
* **ORDER BY** indica que, una vez obtenida la tabla resultado, ésta sea ordenada según la columna `DANO_MOV`, de forma descendiente \(mayor a menor\).

Un aspecto relevante es que se puede agrupar y ordenar por **múltiples columnas al mismo tiempo.** De esta forma, se generará una fila en la tabla resultante **por cada combinación única** de las columnas del `GROUP BY`; y se ordenarán **en el orden en que se indiquen las columnas** en `ORDER BY`.   
****Por ejemplo, si me interesa saber cuánto daño hace el movimiento Físico y Especial más poderoso de cada tipo, para luego ordenar por daño y, en caso de empate, por id:

```sql
SELECT
    TIPO,
    CATEG,
    MAX(DANO) AS DANO_MOV
FROM
    MOVE
WHERE
    CATEG = 'FISICO' OR
    CATEG = 'ESPECIAL'
GROUP BY
    TIPO, CATEG
ORDER BY
    MAX(DANO) DESC, ID ASC;
```

Esto entregará, en caso que existan, 2 filas por cada tipo: una para el mejor movimiento especial y otra para el mejor movimiento físico.

{% hint style="danger" %}
Cuando se utiliza **GROUP BY** para operaciones de agregación, los atributos que se pueden seleccionar \(en el bloque **SELECT**\) deben tener relación directa con los atributos que se están agrupando. Por lo tanto, sólo puedo seleccionar los **atributos que sean parte del bloque GROUP BY** \(además de los atributos que estamos agrupando\).
{% endhint %}

### Consultas Anidadas

Ahora que tenemos un buen entendimiento de los movimientos que pueden aprender los pokémones, complejizaremos más el modelo. Agregaremos una tabla `ENTRADA_POKEDEX`, que representa un tipo de Pokémon \(por ejemplo, Pikachu\), una tabla `TRAINER` para representar un Entrenador específico \(por ejemplo, Ash Ketchum\); una tabla `PKMN` para representar algún Pokémon específico \(por ejemplo, el Pikachu de Ash Ketchum\), y finalmente una tabla `MOVE_PKMN` para indicar cuáles movimientos conoce un Pokémon específico.

```sql
CREATE TABLE ENTRADA_POKEDEX
(
  ID INT PRIMARY KEY,
  NOMBRE VARCHAR(45) NOT NULL,
  TIPO1 VARCHAR(10) NOT NULL,
  TIPO2 VARCHAR(10),
  ID_PREEV INT,
  FOREIGN KEY(ID_PREEV) REFERENCES ENTRADA_POKEDEX(ID)
);

CREATE TABLE TRAINER
(
  ID INT PRIMARY KEY,
  NOMBRE VARCHAR(45) NOT NULL,
  NUM_MEDALLAS INT
);

CREATE TABLE PKMN
(
  ID INT PRIMARY KEY,
  MOTE VARCHAR(45) NOT NULL,
  LVL INT,
  ID_POKEDEX INT NOT NULL,
  ID_TRAINER INTEGER NOT NULL,
  FOREIGN KEY(ID_POKEDEX) REFERENCES ENTRADA_POKEDEX(ID),
  FOREIGN KEY(ID_TRAINER) REFERENCES TRAINER(ID)
);

CREATE TABLE MOVE_PKMN
(
  ID_PKMN INT NOT NULL,
  ID_MOVE INT NOT NULL,
  FOREIGN KEY(ID_PKMN) REFERENCES PKMN(ID),
  FOREIGN KEY(ID_MOVE) REFERENCES MOVE(ID),
  PRIMARY KEY(ID_PKMN, ID_MOVE)
);
```

Supongamos que necesitamos saber:

* Las entradas de la Pokédex de pokémones que son **segundas evoluciones** \(su pre-evolución tiene una pre-evolución\)
* Los pokémones de un entrenador en específico que conocen exactamente 4 movimientos.

Para poder saber cuáles pokémones son segundas evoluciones, debo primero conocer cuáles pokémones no son una evolución, y luego checkear este grupo revisando que la pre-evolución también pertenezca a este grupo. Es decir, necesitamos hacer una consulta con los resultados de otra consulta.

Como en SQL lo que retorna una consulta es una tabla, podemos hacer esto sin mayores problemas:

```sql
SELECT
  *
FROM
  ENTRADA_POKEDEX
WHERE
  ENTRADA_POKEDEX.ID_PREEV IN
    (SELECT
      ENTRADA_POKEDEX.ID
    FROM
      ENTRADA_POKEDEX
    WHERE
      ENTRADA_POKEDEX.ID_PREEV IS NOT NULL);
```

Para responder la segunda pregunta, necesitamos primero hacer una consulta a `MOVE_PKMN` agrupando por el ID del Pokémon, para así contar cuántos movimientos tiene cada uno. Luego, cruzar los resultados de esa tabla con la tabla `PKMN`para obtener los deseados:

```sql
SELECT
  *
FROM
  PKMN
  INNER JOIN (
    SELECT
      ID_PKMN,
      COUNT(*) AS NUM_MOV
    FROM
      MOVE_PKMN
    GROUP BY
      ID_PKMN) AS MOVS
  ON MOVS.ID_PKMN = PKMN.ID
WHERE
  PKMN.ID_TRAINER = 1
  AND MOVS.NUM_MOV = 4;
```

{% hint style="warning" %}
Si necesitas referenciar a la tabla resultante o alguna de sus columnas, recuerda que **debes asignarle un alias.**
{% endhint %}

### DISTINCT y EXISTS

Supongamos que ahora queremos saber los nombres de todos los entrenadores que poseen un Pikachu. Una solución a primera vista sería algo como:

```sql
SELECT
  TRAINER.NOMBRE
FROM
  TRAINER
  INNER JOIN PKMN
    ON PKMN.ID_TRAINER = TRAINER.ID
WHERE
  PKMN.ID_POKEDEX = 25; --25 = numero de pokedex de Pikachu
```

Sin embargo, no estamos considerando la posibilidad de que un mismo entrenador capture más de un Pikachu, por lo que varios nombres aparecerían repetidos en la tabla resultante. Podemos solucionar esto utilizando la palabra clave **DISTINCT:**

```sql
SELECT DISTINCT
  TRAINER.NOMBRE
FROM
  TRAINER
  INNER JOIN PKMN
    ON PKMN.ID_TRAINER = TRAINER.ID
WHERE
  PKMN.ID_POKEDEX = 25;
```

Pensemos ahora en la Liga Pokémon. Supongamos que para poder participar en ella, necesitamos tener las 8 medallas y tener al menos un Pokémon que conozca el movimiento _Vuelo_ para poder llegar. Para encontrar todos los entrenadores que pueden entrar a la liga utilizamos la siguiente consulta:

```sql
SELECT
  T.NOMBRE
FROM
  TRAINER AS T
WHERE
  T.NUM_MEDALLAS = 8 AND
  EXISTS (
  SELECT
    *
  FROM
    PKMN
    INNER JOIN MOVE_PKMN ON
      PKMN.ID = MOVE_PKMN.ID_PKMN
  WHERE
    PKMN.ID_TRAINER = T.ID
    AND ID_MOVE = 100 -- Supongamos 100 = ID de Vuelo.
  );
```

{% hint style="info" %}
Notar el alias que se le asignó a la tabla TRAINER para **mantener la referencia en la sub-consulta** de la cláusula EXISTS.
{% endhint %}

La palabra clave **EXISTS** se encarga de ejecutar la consulta y retornar verdadero si retorna al menos un resultado. De esta forma, revisamos por cada entrenador si existe algún Pokémon que haya capturado que conozca el movimiento vuelo. Si es así y posee las 8 medallas, entonces su nombre aparecerá en la consulta final.

### Utilizando HAVING

Finalmente, supongamos que ahora necesitamos saber cuántos pokémones posee cada entrenador, pero, debido a la gran cantidad de entrenadores, sólo nos interesarán los entrenadores que hayan capturado al menos 10 pokémones. Lamentablemente, **no podemos utilizar funciones de agregación en la cláusula WHERE,** por lo que para situaciones como esta utilizamos la clausula **HAVING:**

```sql
SELECT
    TRAINER.NOMBRE,
    COUNT(*) AS NUM_PKMN
FROM
    TRAINER
    INNER JOIN PKMN ON
        TRAINER.ID = PKMN.ID_TRAINER
GROUP BY
    PKMN.ID_TRAINER
HAVING
    NUM_PKMN > 10;
```

## Resumen

### ¿Qué hace una consulta SQL?

La estructura completa de una consulta, utilizando todas las cláusulas, quedaría como:

```sql
SELECT
    <columnas>
FROM
    <tablas>
WHERE
    <condiciones>
GROUP BY
    <columnas>
HAVING
    <condiciones de agrupacion>
ORDER BY
    <condiciones de ordenamiento>
```

El orden en que SQL ejecuta dichas cláusulas es el siguiente:

* Genera las tablas planteadas en **FROM.**
* Filtra las filas a partir de las condiciones en **WHERE.**
* Agrupa a partir de las columnas dictadas en **GROUP BY.**
* Filtra los grupos calculando las agrupaciones de las condiciones en **HAVING.**
* Selecciona las columnas \(y calcula las agrupaciones\) pedidas en **SELECT.**
* Ordena los resultados según las condiciones en **ORDER BY.**

### **¿Qué hemos aprendido en este apunte?**

* Podemos agregar datos utilizando funciones como **MAX, MIN, COUNT, AVG, SUM.**
* Podemos asignarle un **alias** a tablas y columnas para referenciarlas fácilmente.
* Podemos agrupar los datos utilizando **GROUP BY.**
* Podemos ordenar nuestro resultado utilizando **ORDER BY.**
* Podemos ejecutar **Consultas Anidadas,** es decir, hacer una consulta dentro de otra.
* Podemos filtrar resultados repetidos con **DISTINCT** y utilizar el operador **EXISTS** para revisar si una sub-consulta entrega o no un resultado.
* Podemos hacer condiciones sobre funciones de agregación utilizando **HAVING.**



