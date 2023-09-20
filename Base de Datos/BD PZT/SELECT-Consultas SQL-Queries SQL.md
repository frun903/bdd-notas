Select no hace cambios pero **es el encargado de la recuperación** y por eso es la mas importante. Su escritura es clave para no afectar el rendimiento de mi BD.


Las consultas o queries me ayudan a obtener informacion efectivas de la BD y es aquella organiza tambien de cierto forma la organizacion. **Las queries es la forma en que yo le voy a hacer las preguntas a la base de datos**, entonces un queries le indica a la BD como  traer la informacion.

![[Pasted image 20230512084106.png]]

**SELECT nombre_atributo** : nos permite traer los datos 
**FROM nombre_tabla**: from nos idica **de** que tabla voy a traer esos elementos.
**WHERE** condicion(_ej edad=27)_: Where funciona como una condicion, nos filtra todo aquellos que no cumplan esa condicon 
**GROUP BY  nombre_atributo**: Ordena mi tabla por atrinuto 
**ORDER BY  total DESC** : Ordena en orden decendente
**HAVING total >= 2**: filtra cuando haya mas de dos en este ejemplo.

Select * me indica que va a selecionar todo 


# Select 

La instrucción select es la primera parte de las Queries y se encarga de proyectar o mostrar los datos. 

-->El selec mas basico es **SELECT *  este trea todo los campos** de una entedidad o tabla la cual la defino con el FROM    
```sql 

SELECT *
FROM people; 

```
--> Tabien puedo pedir atrubitos especificos
```sql 

SELECT firstname
FROM people 
--No necesariamente sera 1 pueden ser muchos de la siguinte forma--
SELECT firstname, lastname
FROM people; 

```

Select filtra que campos trae segun nuestras necesidades.

-->**AS:** Puedo por medio del SELECT **cambiar en el nombre de la entidad**  

```sql 

SELECT firstname AS apellido, lastname AS nombre
FROM people; 

```

Asi cambio el alias de la columna o campo, en futuras queries ya puedo usar este alias 

->**Count**: El count me permite contar los registros de una tabla 
```sql 
--Para toda la tabla--
SELECT COUNT(*)
FROM people; 
--Para una columna--
SELECT COUNT(firstname)
FROM people; 
--Para + de una columna--
SELECT COUNT(firstname,city)
FROM people; 
```
Esta propiedad arroja un numero, este numero es la cantidad de registro que tengo en mi tabla o columnas dependiendo lo que yo seleccion. A este valor numerico tambien puedo colocar un Alias 
```sql 
SELECT COUNT(*) AS TodaTabla
FROM people; 

```

->Distinct: es una propiedad en el select que **no me va dar los resultados repitidos** _ejemplo:_
![[Pasted image 20230512190503.png]]
##### Ejemplo: 
Debes imprimir los siguientes 3 bloques de información con el contenido de la tabla `cursos`:



-   Todas las columnas de todos los cursos en el orden por defecto de la base de datos.
-   La cantidad total de cursos con el nombre `cantidad`.
-   Las columnas `nombre`, `profe` y `n_calificaciones` (es decir, exluyendo la columna `id`) de todos los cursos renombradas en inglés (`name`, `teacher` y `n_reviews`).
![[Pasted image 20230512102449.png]]
Resolucion

```sql 
-- Escribe aquí tu código SQL 👇

SELECT * FROM cursos;

SELECT COUNT(*) AS cantidad FROM cursos;

SELECT nombre AS name, profe AS teacher, n_calificaciones AS n_reviews FROM cursos;

```

![[Pasted image 20230512102544.png]]



# From
La sentencia From indica de donde vamos a traer los datos pero ademas la permite traer datos específicos de mas de una(o +) tablas. Esto quiere decir que hace una _lista de las relaciones_ que se van a explorar en la evaluación de la expresión.
La siguiente expresión hace un **producto cartesiano** entre 2 entidades (en el ej seria entre proveedores con partes).
```sql
select *
from provedores, partes
```
De estos producto cartesiano entre tabla y tabla  puedo hacer mas preguntas para hallar las relaciones (para obtener la información que requerimos deberemos anidarlo con un Where): 

```sql
select provedores*, partes*
from provedores, partes
where provedores.ciudad=partes.ciudad
```
En  mi ejemplo busco obtener todas las combinaciones de información de proveedores y partes tales que el proveedor y la parte en cuestión estén situados en la misma ciudad. Para esto tanto proveedores como partes deben tener un atributo ciudad. 

_En ambos casos el producto cartesiano a contenido todos los elementos sin embargo con el SELECT  y el FROM puedo hacer el producto solo de las cosas que necesito_, para esto dependeremos exclusivamente del select:

```sql
select provedores.*, ciudad.cba
from provedores, ciudad;
```
Aquí solo traigo todos los elementos de proveedores pero solo hago el producto cartesiano con aquellos pertenecientes a la ciudad Cba.  

###### Observaciones:

- _Variable Tupla_ se definen en la clausula from mediante el as, funcionan como producto cartesiano **entre elementos de una misma tabla**.
```sql
select distinct p1*,p2*
from provedores as p1, provedores as p2
Where p1.ciudad=p2.ciudad and p1.snum < p2.snum;
```
En mi ejemplo quiero la combinacion de proveedores que viven en la misma ciudad para eso uso el producto cartesiano entre proveedores y proveedores pero renombrados por as (asi sql no se confunde). La ultima linea define por un lado que ambos proveedores tengan la misma ciudad y por el otro que no se repitan las tuplas haciendo que se muestre solo el caso de que   p1.snum < p2.snum salgo cuando p1.snum> p2.snum no saldra.


El compañero de la sentencia FROM es la sentencia **JOIN** que nos ayuda a unir tablas por medio de las llaves foráneas.

# JOINS :
Existen diferentes tipos
-->La diferencia 
![[Pasted image 20230512103824.png]]
Trae todos los datos de la tabla A o B sin traer la otra, yambien hay un join que no trae la interseccion 

--> Interseccion **INNER JOIN:
Trea los valores que estan tanto en A como en B

--> Union **OUTERJOIN**:
Trae todo sin discriminar de ambas tablas A y B

--> Diferencia simetrica **OUTERJOIN**: Excluye aquellos elementos que si tienen relacion y trae solamente aquellos que no. 
![[Pasted image 20230512104347.png]]


## Ejemplo

**LEFT JOIN/RIGHT JOIN** Toma la tabla que ya tiene que usuarios(a la izquierda) y la vamos a unir con post (a la derecha) por _medio de las llaves foráneas._
```sql 
-- Escribe aquí tu código SQL 

SELECT * 
FROM usuarios
--Los usarios vamos a unirlos con los Post por medio de un JOIN
LEFT JOIN post ON usuarios.id =post.usuario_id;


```
La idea de esto seria conectar la llave primaria de usuarios expresada como usuario.id con la llave foránea en post expresada como  post.usuario_id 
![[Pasted image 20230519062344.png]]
**Con esto traigo todos los datos de usuarios A tengan o no un elemento asociados B y trae los post que estén relacionados con esos usuarios(ya que puse el * ALL)**
```sql 
SELECT * 
FROM usuarios LEFT JOIN post ON usuarios.id =post.usuario_id;
```
Si yo quisiera sacar los usuarios que si tienen post y dejar los usuarios que no debo usar un WHERE

```sql 
SELECT * 
FROM usuarios LEFT JOIN post ON usuarios.id =post.usuario_id;
WHERE  post.usuario_id IS NULL
```
Trae los usuarios que no tienen nigun post
![[Pasted image 20230519062746.png]]
Trae los elementos de A sin ninguna coincidencia con el B 

El RIGHT JOIN funciona de forma similar solo que trairia los elementos de B con los de A, sin importar si A es null.



**INNER JOIN** : Trae solo los elementos que tengan correspondencia tanto en A como en B (es decir en ambos lados)
```sql 
SELECT * 
FROM usuarios INNER JOIN post ON usuarios.id =post.usuario_id;
```
Solo me interesa los datos que están relacionados ambos lados 

Ejemplo: ![[Trabajos Practicos BD-Practica#5. Obtener todas las películas en las que actué el señor Fred Costner.]]



**Union** Aquí debería hacer una consulta compuesta para ambos
```sql 
SELECT * 
FROM usuarios LEFT JOIN post ON usuarios.id =post.usuario_id;
UNION 
SELECT * 
FROM usuarios RIGHT JOIN post ON usuarios.id =post.usuario_id;
```
![[Pasted image 20230519064719.png]]
Esta consulta compuesta permite traer ambos lados incluyendo los NULL por lo tanto trae todo.



**Diferencia Simétrica** Aqui nos va traer solo los elementos de A y B que no tienen un par relacionado.
A1 B1
A2 null
null A3
El unico que tiene un par relacionado es A1 y B1, lo que hare con la sentencia compuesta es traer aquellos que no tengan ese par.
![[Pasted image 20230519064643.png]]
Nuevamente necesito una consulta compuesta pero con condición .
```sql 
SELECT * 
FROM usuarios LEFT JOIN post ON usuarios.id =post.usuario_id;
WHERE  post.usuario_id IS NULL
UNION 
SELECT * 
FROM usuarios RIGHT JOIN post ON usuarios.id =post.usuario_id;
WHERE  post.usuario_id IS NULL
```
Solo nos trae quellos que no tienen par asociado.

# Where
**WHERE** es la sentencia que nos ayuda a filtrar tuplas o registros dependiendo de las características que elegimos. Empezamos a filtrar de acuerdo a criterios que necesitemos.

```sql 
SELECT * 
FROM post
Where id<50;--incluye los menores a 50(sin el 50)
```
Puedo usar los operadores lógicos y el igual es solo con un simbolo(=). El operador igual es util cuando sabemos la cadena exacta que a vamos a utilizar:

```sql 
SELECT * 
FROM post
Where status=activo;
```
También podemos usar la sentencia distrito **!=** para filtrar los datos.

La propiedad **LIKE** nos ayuda a traer registros de los cuales conocemos sólo una parte de la información. Por ejemplo tenemos una tabla con titulos 
| Titulo      |Fecha |
|----------|---------|
|PIKACHU |3-10 |
|OLOZOM  | 2-25| 
| CLUCHU | 15-10 |


Yo solo se que mi titulo contiene un CHU en el titulo, puedo filtar y traer solo los titulos que tengan en el nombre chu.
```sql 
SELECT * 
FROM post
Where titulo LIKE '%CHU%%'
```
Entre comillas y el LIKE   % %  tiene diferentes configuaraciones:  
- Where titulo LIKE 'CHU% %'--> me indica que el texto debera comenzar con chu
- Where itulo LIKE '% %chu'--> me indica que el texto debera terminar con chu
- Where titulo LIKE '%CHU% ' -> me indica que el texto cotrendra al medio la palabra chu


-   La propiedad **BETWEEN** nos sirve para arrojar registros que estén en el medio de dos. Por ejemplo los registros con id entre 20 y 30.
```sql 
SELECT * 
FROM post
Where id BETwEEN 16 AND 95
```
Este comando podría traducirse como:
```sql 
SELECT * 
FROM post
Where id>16 AND id<95
```

Util para rangos de fechas o precios.

### Observacion!

- Cabe mencionar que los operadores LIKE y BETWEEN AND, pueden ser negados con el comando **NOT**  
	NOT LIKE  ->  Buscara aquellos elementos que no contengan cierta cadena
	NOT BETWEEEN – AND – -> Buscara aquellos elementos fuera del rango seleccionado 
	
  Estos operadores hacen los contrario a sus versiones originales

- En el caso del dato **Date año-mes-dia** puedo usar el Where solo para discriminar solo uno de estos elementos
```sql 
SELECT * 
FROM post
Where YEAR(fechaNacimiento) = '2005';
```
Tambien puedo usar la isntruccion BETWEEN
```sql 
SELECT * 
FROM post
Where MONTH(fechaNacimiento) BEWTEEN '02' AND '06' ;
```
Debo usar '' para aclarar la fecha.

- En las consultas Where puedo usar los **operadores logicos AND y OR**  

```sql
SELECT *
from provedores
where ciudad='Paris' and situcion>20
```


# Group By


La sentencias GROUP BY tiene que ver con agrupación. Indica a la base de datos qué criterios debe tener en cuenta para agrupar.
###### Ejemplo simple
```sql
select status, count(*) AS postnumber
from post
```
Esto nos trairia todo los post agrupados juntos en la tabla, el resultado sera:

|status|postnumber|
|------|-------------|
|active| 15|
Nos dararia todo los post sin importar si son Active o no active, __aqui entra la sentencia group by para agrupar los datos de alguna manera__, yo le digo a través de que campo quiero que se agrupe. 

```sql
select status, count(*) AS postnumber
from post
group by status
```

Aqui le digo que me agrupe de acuardo a los status de mi tabla, en este caso mi status son solamente 2:

|status|postnumber|
|------|-------------|
|active| 10|
| no active| 5 |

##### Ejemplo Complejo
Supongase que quiero saber cuantos post se hicieron por año:

```sql
select YEAR(fecha_de_publicacion) AS post_year, count(*) AS postnumber
from post
group by post_year;
```

En mi primera linea le dije que seleccione los años de la fecha de publicacion y lo renombre como post_year o año de posteo _esto seria un subconjunto_. El segundo select es el count. 
Lo intersante aqui es que _agrupe mi conteo de acuerdo al subconjunto que cree previamente_ 

|post_year|postnumber|
|------|-------------|
|2021| 101|
| 2022| 550 |
aqui tengo el desglose de cuantos post se hicieron por año.

**Este tipo de aplicaciones se pueden hacer para cualquier tiempo, o agrupacion.**

##### Ejemplo Complejo con Doble agrupacion
Supongamos que ahora queremos saber la cantidad de post activos y no activos por año.Tendre que realizar una **doble agrupacion** para esto mi sentencia select deber contar con 3 elementos, los dos elemenots que quiera agrupar y el contador:

```sql
select status ,YEAR(fecha_de_publicacion) AS post_year, count(*) AS postnumber
from post
group by status, post_year;


```

El group by tambien tendre que poner la diferenciacion por status  y año

|post_year|active| no active|postnumber|
|---------|------|----------|----------|
|2021     | 2      | 4      |       6  | 
| 2022    | 5      |  2     |         7|

**El GROUP BY puede ser decisivo para que una queries anidada funcione correctamente** ejemplo ![[Trabajos Practicos BD-Practica#3. Obtener nombre y apellido de aquellos clientes que hayan alquilado la misma película al menos en dos ocasiones. El resultado de la consulta debe incluir ademas el titulo de la película y la cantidad de veces que fue alquilada por dicho cliente.]]

# Order By 



**Order by** Tiene que ver con el ordenamiento de los datos en base de un criterio seleccionado por nosotros. 
```sql
select *
from posts
order by titulo desc
```
Existen 2 formas de ordenar de forma ascendente **ASC**(empiza de menos a mas) y de manera descendente **DESC**(empiza de mayor a menor). 
Otra forma de ordenar es _ordenar por cadena de carcateres_ cuando yo tenga un atributo de tipo de dato char el order by va ser de caracter alfabetico (ABC en ASC) y de la Z a la A en DESC.

**Limit** se usa con el group by y _limita por medio un rango_. Muy utilizado para los tops tipo tops 10, tops 5.

```sql
select *
from posts
order by titulo ASC
limit 5;
```

Me treaira los Primero 5 titulos de peliculas.

##### Obs:
Si yo al order by no le coloco nada (ASC o DESC) actuara como un Group by osea los agrupara de acuerdo al criterio que elijamos pero de manera aleatoria.


**_importante!!! En el ORDER BY siempre usar el id ya que el titulo, o cualquier otro atributo conviene_**
Ej.[[Trabajos Practicos BD-Practica#3.7 Obtener el titulo de las 10 películas que más ganancia hayan generado a la empresa y la ganancia total acumulada por alquileres de cada una de ellas. Ordenar los resultados de mayor a menor.]]




# Having
**HAVING** tiene una similitud muy grande con **WHERE**, sin embargo el uso de ellos depende del orden. Cuando se quiere seleccionar tuplas **agrupadas** únicamente se puede hacer con **HAVING**. Por lo general si yo tengo un conjunto de entidades agrupados por un Group by y un contador no podre usar un WHERE (ya que esta sentencia va estrictamente después del FROM todavía no a captado el agrupamiento o el campo dinámico creado) ante estas situaciones tendré que usar un HAVING

```sql
select monthname(fecha_publicacion) as mesdepublicacion, status, count(*) as cantidadpost
from post
group by status, mesdepublicacion
having cantidadpost > 5
order by mesdepublicacion;
```

En este ejemplo creo una tabla que acomoda la post por mes y stautus _pero solo mostrara aquellos meses que hayan tenido mas de 5 post_.

*No neseriamente por usar el having no tengo que usar el Where es mas hay casos en donde deber usar los 2*

![[Trabajos Practicos BD-Practica#6. Obtener mediante una única consulta todos los actores que hayan actuado en más de 3 películas de la categoría ‘Comedy’. Mostrar los actores que hayan actuado en más películas de este género primero.]]





# Operaciones con Conjuntos:
Las operaciones con conjuntos operan sobre queries generadas previamentes y corresponden a las operaciones sobre el algebra relacional. Estas utilizan los operadores **union, intersect y except**, ejemplos:

- Obtener los proveedores y partes que se encuentran en “Paris”
 ```sql
 (select snombre from proveedores where ciudad = “Paris”)
union
(select pnombre from partes where ciudad = “Paris”)
 ``` 

- Obtener todos los proveedores que viven en una ciudad donde no hay partes. 	
```sql
(select ciudad from proveedores) 
except
(select ciudad from partes)
```

- Obtener todas las ciudades donde hay proveedores y partes.
 ```sql
(select ciudad from proveedores) 
intersect
(select ciudad from partes) 
 ```
Cada una de las operaciones antes citadas eliminan duplicados automaticamente para dejar los duplicados se utilizan las versiones **union all, intersect all y except all** 

Las operaciones con conjunto ya no se usan tanto porque tengo los JOIN.

