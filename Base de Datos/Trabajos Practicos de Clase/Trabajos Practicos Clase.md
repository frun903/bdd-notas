La idea es hacer consultas por medio de Queries[[SELECT-Consultas SQL-Queries SQL]]
###  Trabajo práctico 1:
1. Obtener una lista de todos los actores.
```sql
select *
from actor;
```
2.  Obtener una lista de todos los apellidos de actores, sin repetir.
```sql
SELECT DISTINCT actor.last_name
FROM actor;
```
3. Obtener una lista de todas las ciudades de Argentina.

```sql
SELECT *
FROM city
WHERE city.country_id=6;
```
Lo que no me gusta de esta es que, no siempre sabre el id de argentina.
Esta me gusta mas en el Where le digo _Donde pais=Argentina y idpais de ciudad sea igual al id pais de pais_

```sql
SELECT city.city
FROM city, country
WHERE country.country='Argentina' AND city.country_id=country.country_id;
```
4.  Obtener nombre, apellido, direccion y telefono de todos los clientes de Argentina que tengan domicilio en Buenos Aires.

_recorda que no siempre los datos van a estar en el lugar que deben. Como buenos aires **no esta en City se me ocurre usar su codigo postal para poder discriminarlos adecuadamente**. Sin embargo al tener tantos codigos postales no seria practico_. También esta **distrito** hago una consulta y encuentro a buenos Aires.

```sql
SELECT address.address, customer.first_name, customer.last_name, address.phone
FROM address, customer
WHERE address.district='Buenos Aires' AND address.address_id=customer.address_id;
```

![[Pasted image 20230527181411.png]]

5.  Obtener el nombre de todos los clientes que hayan realizado un alquiler cuyo monto total de pago supere los 10 dolares. Presentar estos datos ordenados de mayor a menos e incluir ademas como resultado de la consulta el monto total abonado.

```sql
SELECT DISTINCT customer.first_name, payment.amount
FROM customer, payment
WHERE payment.amount>10
ORDER BY payment.amount DESC;
```

6. Obtener el número de inventario de todas aquellas películas alquiladas que aún no han sido devueltas

```sql
SELECT DISTINCT film.title
FROM  inventory, film
WHERE inventory.film_id=inventory.inventory_id IN (SELECT rental.inventory_id FROM rental WHERE rental.return_date IS NULL);
```

### Trabajo práctico 2
1.  Obtener una lista de todas las películas que fueron alquiladas al menos una vez.
```sql
SELECT DISTINCT film.title
FROM film, inventory, rental
WHERE film.film_id=inventory.film_id AND rental.inventory_id=inventory.inventory_id;
```

2.  Obtener una lista de todas las películas en inventario que no registren ningún alquiler.
```sql
SELECT DISTINCT film.title
FROM film, inventory
WHERE film.film_id=inventory.film_id NOT IN (SELECT DISTINCT inventory.inventory_id FROM inventory, rental WHERE inventory.inventory_id=rental.inventory_id);
```

##### Consultar al profe ej2

3.  Obtener la cantidad de veces que fue alquilada cada película. Presentar los datos como “titulo”, “cantidad_alquileres”.
```sql
SELECT DISTINCT film.title as titulo, COUNT(*) as cantidad_alquileres
FROM film, inventory,rental
WHERE film.film_id=inventory.film_id AND rental.inventory_id=inventory.inventory_id
GROUP BY titulo;
```

4.  ¿Cuáles son las 10 películas más alquiladas por los clientes del videoclub?

```sql
SELECT DISTINCT film.title as titulo, COUNT(*) as cantidad_alquileres
FROM film, inventory,rental
WHERE film.film_id=inventory.film_id AND rental.inventory_id=inventory.inventory_id
GROUP BY titulo
ORDER BY cantidad_alquileres DESC
LIMIT 10;
```

##### Consultar al profe ej4 si yo tengo varias películas con la misma calificación que entrarian en el top 10 como lo solucionaría?  En mi caso tengo varias con calificación 31 que quedan fuera del top por poner solo 10.

###### 5.  Obtener todas las películas en las que actué el señor Fred Costner.
```sql
SELECT DISTINCT film.title
FROM  film, actor
WHERE actor.first_name='Fred' AND actor.last_name='Costner';


SELECT DISTINCT film.title
FROM film_actor,film,actor
WHERE actor.first_name='Fred' AND actor.last_name='Costner'AND actor.actor_id=16;



SELECT DISTINCT film.title
FROM film_actor, film
WHERE film_actor.actor_id IN (SELECT actor.actor_id FROM actor WHERE actor.first_name= 'Fred' AND actor.last_name='Costner');


--El tema aqui es Fred Costner solo sale en 27 peliculas segun su id 16 pero si uso cualquiera de las 3 opciones da 1000 debere revisar nuevamente cual seria la correcta.

--Bien vamos mejor por medio de Where pude conseguir las 27 peliculas pero por medio de un ID, 

SELECT DISTINCT film.title
FROM film,film_actor,actor
WHERE film.film_id IN(SELECT DISTINCT film_actor.film_id
FROM film_actor,film,actor
WHERE film_actor.actor_id=16);

--creo que puedo corregirlo si modifico dentro del Where hago una relacion para que obentga el id de la peliculas de costner

SELECT DISTINCT film.title
FROM film
WHERE film.film_id IN
(SELECT DISTINCT film_actor.film_id
FROM film_actor
WHERE film_actor.actor_id IN
(SELECT actor.actor_id
 FROM actor
 WHERE actor.first_name='Fred' AND actor.last_name='Costner'))


--Tube que hacer 3 consultas anidada la mas interna me obtiene el id de fred costner
SELECT actor.actor_id
 FROM actor
 WHERE actor.first_name='Fred' AND actor.last_name='Costner'))
 --La siguiente es la de la tabla pasarela de film_actor para encontrar las peliculas que correspondan al id del del actor. 
 SELECT DISTINCT film_actor.film_id
FROM film_actor
WHERE film_actor.actor_id IN(...)

--La ultima consulta utiliza los id de peliculas correspondientes a Fred costner para obtener los titulos de las peliculas  

SELECT DISTINCT film.title
FROM film
WHERE film.film_id IN(...)

```
En otras palabras para tablas pasarles primero busco el id correspiende, luego busco su par y por ultimo el elemento del id que busco. 



6.  Obtener el monto total de ingresos adquiridos históricos del videoclub.

```sql
SELECT SUM(payment.amount) as 'Monto Historico'
FROM payment
```

7. Que monto total de ingresos supone para el videoclub el alquiler de películas en las que el sr. Costner actúa?

Bien considerando que un monto total voy a volver a utilizar SUM() con mas o menos la misma iteracion que hice para buscar las peliculas donde sale el señor costner, con la diferencia que 1ro voy a conseguir el id de film coinicdirlo con el de inventary.id para conseguir el rental



```sql
SELECT DISTINCT SUM(payment.amount) AS 'Ingreso de peliculas de Costner'
FROM payment
WHERE payment.rental_id IN(
SELECT DISTINCT rental.inventory_id
FROM rental
WHERE rental.inventory_id IN(
SELECT DISTINCT inventory.inventory_id
FROM inventory 
WHERE inventory.film_id IN(
SELECT DISTINCT film.film_id
FROM film
WHERE film.film_id IN
(SELECT DISTINCT film_actor.film_id
FROM film_actor
WHERE film_actor.actor_id IN
(SELECT actor.actor_id
 FROM actor
 WHERE actor.first_name='Fred' AND actor.last_name='Costner')))));
```


### Trabajo Practico 3:

La idea es usar JOIN [[SELECT-Consultas SQL-Queries SQL]]]


1.   Obtener una lista completa de las películas que no se encuentren en stock.

```sql 
SELECT film.title
FROM film LEFT JOIN inventory ON film.film_id=inventory.film_id
WHERE inventory.film_id IS NULL;
```

2.  A partir de una única consulta responder: ¿Existen películas que no estén vinculadas con ningún actor?: Si 
_El 2 es  mas sencillo sus claves foráneas son mas claras al tratarse de una tabla pasarela_
```sql
SELECT DISTINCT film.title
FROM film LEFT JOIN film_actor ON film.film_id=film_actor.film_id
WHERE film_actor.actor_id IS NULL;
```

3.   Obtener nombre y apellido de aquellos clientes que hayan alquilado la misma película al menos en dos ocasiones. El resultado de la consulta debe incluir ademas el titulo de la película y la cantidad de veces que fue alquilada por dicho cliente.
-
```sql
SELECT c.first_name, c.last_name, f.title, COUNT(*) AS cantidad_alquileres

FROM customer c

JOIN rental r ON c.customer_id = r.customer_id

JOIN inventory i ON r.inventory_id = i.inventory_id

JOIN film f ON i.film_id = f.film_id

GROUP BY c.customer_id, f.film_id

HAVING cantidad_alquileres >= 2;

```

4.   Obtener todos los datos de las películas cuyo stock sea de exactamente 2 ejemplares.

_Arrojan la misma cantidad de resultado y el mismo resultado ambas formas:_
```sql
SELECT *, COUNT(*) as Stock
FROM film INNER JOIN inventory ON film.film_id=inventory.film_id
GROUP BY inventory.film_id
HAVING Stock=2;
```


```sql
SELECT *, COUNT(inventory.film_id) as Stock
FROM film INNER JOIN inventory ON film.film_id=inventory.film_id
GROUP BY film.film_id
HAVING Stock=2;
```


5. Obtener una tabla a través de la cual se pueda responder cuántos alquileres fueron realizados en cada sucursal del videoclub y cual es el total recaudado en cada de ellas. Presentar los datos solicitados como “cant_alquileres” y “ganancia” respectivamente

```sql
SELECT payment.staff_id,COUNT(payment.rental_id) AS cant_alquileres, SUM(payment.amount) AS ganancia
FROM payment 
GROUP BY payment.staff_id;

--

SELECT staff.staff_id,COUNT(payment.rental_id) AS cant_alquileres, SUM(payment.amount) AS ganancia
FROM payment INNER JOIN staff ON payment.staff_id=staff.staff_id
GROUP BY payment.staff_id;

```

![[Pasted image 20230530092923.png]]

_Sin embargo estos casos no son prácticos ya que puede que en un futuro haya mas empleados en cada tienda_

_Otra opción pero anidando los JOIN_

```sql
SELECT 'tienda', COUNT(payment.rental_id) AS cant_alquileres, SUM(payment.amount) AS ganancia
FROM payment INNER JOIN 
(SELECT store.manager_staff_id
FROM store LEFT JOIN staff ON store.manager_staff_id=staff.staff_id) AS tienda
ON tienda.manager_staff_id=payment.staff_id
GROUP BY tienda.manager_staff_id;
```

![[Pasted image 20230530094142.png]]
El tema con este es que no se cual tienda es.

###### 6. Obtener mediante una única consulta todos los actores que hayan actuado en más de 3 películas de la categoría ‘Comedy’. Mostrar los actores que hayan actuado en más películas de este género primero.

//Primero voy a discriminar a aquellas peliculas que sean de comedia. Como es una tabla pasarela antes debere discriminar aquell 

```sql
SELECT film.film_id
FROM film LEFT JOIN (SELECT film_category.film_id FROM film_category INNER JOIN category ON category.category_id=film_category.category_id WHERE category.name='Comedy') as Comedias ON film.film_id=Comedia.film_id;


--Hasta aqui encontre las comedias 

SELECT film_category.film_id 
FROM film_category INNER JOIN category ON category.category_id=film_category.category_id WHERE category.name='Comedy'

--Creo que desde aqui puedo ir directo a actor ya que tengo los id de comedias

SELECT film_actor.actor_id ,COUNT(film_actor.film_id)
FROM film_actor INNER JOIN 
(SELECT film_category.film_id 
FROM film_category INNER JOIN category ON category.category_id=film_category.category_id WHERE category.name='Comedy') AS Comedias ON film_actor.film_id=Comedias.film_id
GROUP BY film_actor.actor_id
Having COUNT(film_actor.film_id)>3;

```

**Use el inner en ambos casos, ya que solo me interesaba la intersección del conjunto**. Ahí me salen los actores en base al id de los mismo, si los quisiera por nombre debería hacer otra anidacion pero con actors.


7. Obtener el titulo de las 10 películas que más ganancia hayan generado a la empresa y la ganancia total acumulada por alquileres de cada una de ellas. Ordenar los resultados de mayor a menor.

```sql
SELECT film.title, COUNT(inventory.inventory_id)
FROM film INNER JOIN inventory ON film.film_id=inventory.film_id
GROUP BY film.title;

o

SELECT film.film_id, COUNT(inventory.inventory_id)
FROM film INNER JOIN inventory ON film.film_id=inventory.film_id
GROUP BY film.film_id;

--Bien, con esto logre tener las peliculas y su canitidad de inventario.
--Ahora obetuve el valor de la suma d

SELECT rental.rental_id, SUM(payment.amount)
FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY rental.rental_id;


---Bueno se logro que me tire por id el monto acumulado, ahora haria falta conectarlo con peliculas 


SELECT inventory.inventory_id, SUM(Cosas)
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.inventory_id;


---Aqui le hice unas mejoras para que me tire el monto de acuerdo al film id ya con esto puedo conectarlo a otro join

SELECT inventory.film_id, SUM(Cosas)
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id;


```


![[Pasted image 20230530123022.png]]

```sql
SELECT film.title, SUM(Ganancia) as 'GananciaTotal'
FROM film INNER JOIN 
(SELECT inventory.film_id, SUM(Cosas)AS 'Ganancia'
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id) AS inventarios ON film.film_id=inventarios.film_id
GROUP BY film.title
ORDER BY GananciaTotal DESC
LIMIT 10;

```

![[Pasted image 20230530124858.png]]


8.   Obtener el titulo de las películas cuya ganancia promedio sea menor al promedio de ganancia de todas las películas.


ganancia promedio 

Obtener el titulo de las películas sea menor al promedio de ganancia de todas las películas





GanaciaPromedia: 70.361754

```sql
--Calculando la ganacia promedio con
SELECT  AVG(Ganancia) as 'GananciaPromedioDelTotal'
FROM film INNER JOIN 
(SELECT inventory.film_id, SUM(Cosas)AS 'Ganancia'
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id) AS inventarios ON film.film_id=inventarios.film_id;

--Calculo los elementos

SELECT film.title, AVG(Ganancia) as 'GananciaPromedioPorPelicula'
FROM film INNER JOIN 
(SELECT inventory.film_id, SUM(Cosas)AS 'Ganancia'
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id) AS inventarios ON film.film_id=inventarios.film_id
GROUP by film.title
HAVING GananciaPromedioPorPelicula< 70.361754;
```

**Probé poner la primera consulta en la 2da y funciono**

```sql
SELECT film.title, AVG(Ganancia) as 'GananciaPromedioPorPelicula'
FROM film INNER JOIN 
(SELECT inventory.film_id, SUM(Cosas)AS 'Ganancia'
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id) AS inventarios ON film.film_id=inventarios.film_id
GROUP by film.title
HAVING GananciaPromedioPorPelicula< (SELECT  AVG(Ganancia) as 'GananciaPromedioDelTotal'
FROM film INNER JOIN 
(SELECT inventory.film_id, SUM(Cosas)AS 'Ganancia'
FROM inventory INNER JOIN (SELECT rental.inventory_id, SUM(payment.amount) AS 'Cosas' FROM rental LEFT JOIN payment ON rental.rental_id=payment.rental_id GROUP BY rental.rental_id) AS rentas ON inventory.inventory_id=rentas.inventory_id
GROUP BY inventory.film_id) AS inventarios ON film.film_id=inventarios.film_id);
```


https://drive.google.com/file/d/1fK7DUpvE1mI9A54W8xooVIWyiypOOjJF/view?usp=sharing

https://drive.google.com/file/d/1rUYw8_xxhB8iwzi5DdHFjhUz-v_6ntVk/view?usp=sharing

###    Trabajo práctico 4

###### 1.
Crear en la base de datos una nueva tabla llamada "film_review" que posea los campos:
-   film_review_id SMALLINT, permite solamente números positivos, no puede ser nulo y es auto incremental. Asignar como clave primaria.
-   film_id SMALLINT, no puede ser nulo ni puede ser negativo.
-   review VARCHAR de máximo 200 caracteres.

```sql
CREATE TABLE IF NOT EXISTS film_review (
film_review_id SMALLINT PRIMARY KEY AUTO_INCREMENT NOT NULL,
review VARCHAR(200) NULL,
film_id smallint NOT NULL
);
```

1.1 Bis: suponiedo que me olvide de crear un atributo de mi tabla deberia colocar

```sql
Alter table film_review
add film_id smallint;
```

El comando ALTER TABLE se utiliza para modificar la estructura de una tabla existente, y en este caso, estás agregando un nuevo atributo llamado 'film_id' de tipo smallint a la tabla 'film_review'.


Si yo quisiera conectar tablas por medio de llaves foráneas. La tabla 'film' con la tabla 'film_review' ¿Cual seria la sentica SQL correcta para conectar la llave primeria 'film_id' de la tabla 'film' con el 'film_id' de la tabla 'film_review'?

###### 2.
2. Asigne el campo “film_id” de la tabla como clave foránea utilizando la tabla “film” como referencia.
**ChatGpt me lanzo esto pero no anduvo**  
```sql
alter table film_review
add consttraint film_id
foreign key (film_id) references film(film_id);
```

###### 3
3. Inserte una breve reseña sobre 5 películas de su elección.
ACADEMY DINOSAUR 1
|ALONE TRIP 17
|DOGMA FAMILY|239|
|FLASH WARS| 321
WIND PHANTOM 976

```sql
insert into film_review (review, film_id)
values ('Comedia para toda la familia', 1);

insert into film_review (review, film_id)
values ('Una instropeccion personal', 17);

insert into film_review (review, film_id)
values ('Una pelicula profunda sobre la vida de Jk Rowling', 239);

insert into film_review (review, film_id)
values ('Un reinvencion del clasico Star Wars', 321);

insert into film_review (review, film_id)
values ('Su mirada critica sobre cierto tema es cataclismica', 976);
```

Como me lanzaba error si ponia film:id, se lo pose en la consulta.

![[Pasted image 20230607050452.png]]

###### 4
4.  Elimine de la tabla las tuplas cuyo ID este entre 2 y 4.
```sql
delete from film_review
where film_review_id>1 and film_review_id<5
```

###### 5
5.  Inserte una reseña sobre la película 128. Puede asignarle el ID 3 a esta nueva tupla?

```sql
insert into film_review (film_review_id, review, film_id)
values ( 3 ,'Una apacionante clase', 128);
```

Si pude asignarle el ID 3 a esta nueva tupla por ser insert y haber eliminado el id anterior.
**Distinto seria si ya existiera el id=3** lo probé y dio error:
![[Pasted image 20230607051442.png]]
Si yo quisiera de todas formas poner en el id 3 una reseña para la pelicula 128 debería hacer un update.


###### 6
Elimine todas las tuplas de la tabla.

```sql
delete from film_review;
```

El alcance de esta sentencia afectara todas las tuplas sin afectar la estructura de la tabla. 

###### 7
Agregue una columna “date” de tipo DATE a la tabla film_review.

```sql
alter table film_review
add date DATE;

```

###### 8
Inserte una nueva reseña, pero esta vez utilice la fecha actual como dato para la columna "date".

```sql
insert into film_review (review, film_id, date)
values ('Comedia para toda la familia', 1, '2023-06-07' );
```


**Obervacion!** En las sentencias SQL el tipo de dato _DATE va entre comillas '2023-07-08'_, ademas tiene el formato YYYY-MM-DD año mes dia.


###### 9
Elimine la columna “date”
```sql
alter table Fran.film_review
drop column date;
```

**Observación** Fran es el nombre de la base de datos. 


###### 10
6. Elimine la tabla film_review de la base de datos.

```sql
DROP TABLE film_review;
```


###### 11.
Crear una vista "ventas_2005" en donde se detalle la cantidad de ventas registradas por mes en el año 2005.

cantidad 
ventas 
ordenar

La consulta:
```sql
SELECT DATE_FORMAT(payment_date, '%Y-%m') AS mes, COUNT(payment_id) AS ventas
FROM payment
WHERE YEAR(payment_date) = 2005
GROUP BY mes  
ORDER BY `mes` ASC
```

Creo vista en base a la consulta:

```sql

CREATE OR REPLACE VIEW `ventas_2005` AS 
SELECT DATE_FORMAT(payment_date, '%Y-%m') AS mes, COUNT(payment_id) AS ventas
FROM payment
WHERE YEAR(payment_date) = 2005
GROUP BY mes  
ORDER BY `mes` ASC
;
```


### Trabajo práctico 5

##### 1. Listar el el nombre de la película 6, la cantidad de unidades en inventario y la cantidad de veces que fue alquilada. El formato y los valores del resultado de la consulta deben coincidir con la siguiente tabla:

```sql

select film.title, count(inventory.inventory_id) as 'Unidades en inventario', count(rental.rental_id) as 'Cantidad de alquileres'
from film join inventory on film.film_id=inventory.film_id 
join rental on inventory.inventory_id=rental.inventory_id
WHERE film.film_id=6;


union all 
```

Notose que no daba porque 
```sql
select film.title, inventory.inventory_id, rental.rental_id
from film join inventory on film.film_id=inventory.film_id 
join rental on inventory.inventory_id=rental.inventory_id
WHERE film.film_id=6;
```
AL unir las tablas se me repetia el id de inventary porque esyaba unido a la tabla join y quedaria asi:

![[Pasted image 20230609165501.png]]

Para evitar esto deberiamos usar el **Distinct**
```sql
select film.title, count(DISTINCT inventory.inventory_id) as 'Unidades en inventario', count(DISTINCT rental.rental_id) as 'Cantidad de alquileres'
from film join inventory on film.film_id=inventory.film_id 
join rental on inventory.inventory_id=rental.inventory_id
WHERE film.film_id=6;


(SELECT film.title
FROM film
WHERE film.film_id=6)
UNION
(SELECT COUNT(inventory.inventory_id)
 FROM film JOIN inventory on film.film_id=inventory.film_id
 WHERE film.film_id=6);


---
(SELECT 'nombre',film.title
FROM film
WHERE film.film_id=6) 
UNION 
(SELECT 'unidades de inventario',count(DISTINCT inventory.inventory_id) 
 FROM film JOIN inventory on film.film_id=inventory.film_id
 WHERE film.film_id=6)
 UNION
(select 'cantidad de alquileres',count(DISTINCT rental.rental_id) 
from film join inventory on film.film_id=inventory.film_id 
join rental on inventory.inventory_id=rental.inventory_id
WHERE film.film_id=6);
```

Notese para poner a la derecha el nombre de la tabla uso en el select '', el resto

##### 2. Obtener una lista de los nombres de las categorías juntos a su correspondiente porcentaje del total de alquileres.

```sql
**

SELECT category.name AS nombre_categoria,

COUNT(rental.rental_id) * 100 / (SELECT COUNT(*) 

FROM rental) AS porcentaje_total

FROM category

JOIN film_category ON category.category_id = film_category.category_id

JOIN film ON film_category.film_id = film.film_id

JOIN inventory ON film.film_id = inventory.film_id

JOIN rental ON inventory.inventory_id = rental.inventory_id

GROUP BY category.name;

```

###### 3.

```sql


SELECT category.name AS nombre_categoria, COUNT(rental.rental_id) AS total_alquileres, 

AVG(payment.amount) AS promedio_ganancia, SUM(payment.amount) AS ganancia_total

FROM category

JOIN film_category ON category.category_id = film_category.category_id

JOIN film ON film_category.film_id = film.film_id

JOIN inventory ON film.film_id = inventory.film_id

JOIN rental ON inventory.inventory_id = rental.inventory_id

JOIN payment ON payment.rental_id=rental.rental_id

GROUP BY category.name;

```

##### 4. Obtener nombre y ganancia promedio de todas las películas cuya ganancia promedio supere el promedio de alguna categoría. Ordenar por promedio de mayor a menor.

```sql

SELECT film.title AS nombre_pelicula, AVG(payment.amount) AS ganancia_promedio

FROM film

JOIN inventory ON film.film_id = inventory.film_id

JOIN rental ON inventory.inventory_id = rental.inventory_id

JOIN payment ON rental.rental_id = payment.rental_id

GROUP BY film.title

  

HAVING AVG(payment.amount) > SOME (

  SELECT AVG(payment.amount)

  FROM category

  JOIN film_category ON category.category_id = film_category.category_id

  JOIN film ON film_category.film_id = film.film_id

  JOIN inventory ON film.film_id = inventory.film_id

  JOIN rental ON inventory.inventory_id = rental.inventory_id

  JOIN payment ON rental.rental_id = payment.rental_id

  GROUP BY category.category_id

)

ORDER BY ganancia_promedio DESC

```


#####  5. Obtener nombre y ganancia promedio de todas las películas cuya ganancia promedio supere el promedio de todas las categoría. Ordenar por promedio de mayor a menor.

```sql
SELECT film.title AS nombre_pelicula, AVG(payment.amount) AS ganancia_promedio
FROM film
JOIN inventory ON film.film_id = inventory.film_id
JOIN rental ON inventory.inventory_id = rental.inventory_id
JOIN payment ON rental.rental_id = payment.rental_id
GROUP BY film.title
HAVING AVG(payment.amount) > ALL(
  SELECT AVG(payment.amount)
  FROM category
  JOIN film_category ON category.category_id = film_category.category_id
  JOIN film ON film_category.film_id = film.film_id
  JOIN inventory ON film.film_id = inventory.film_id
  JOIN rental ON inventory.inventory_id = rental.inventory_id
  JOIN payment ON rental.rental_id = payment.rental_id
  GROUP BY category.category_id
)
ORDER BY ganancia_promedio DESC;

```

#####  6. Obtener id, nombre y ganancia promedio de todas las películas cuya ganancia promedio supere el promedio de su categoría. Incluir también el nombre de la categoría a la cual pertenece la película y el promedio de dicha categoría como resultado.

_Bien desgranemos lo que nos pide: 
- id, nombre, AVG(payment.aumont), category.name, avg(category)
- cuya ganancia promedio supere el promedio de su categoría. Esto me indicaría que debo hacer un promedio de cada categoría. Y a ese promedio ir comparando con cada película con su categoría correspondiente.  

idea  Juan hacer una sub consulta dentro de la consulta. Como lo hacíamos antes!!!

Entonces en la sub consulta voy a conseguir la ganancia promedio por categoría y _el id de cada una_ esto con el fin de después conectar.


_**No debo especificar el camino nuevamente si ya esta dentro de la subconsulta**_ 

```sql

/*SubConsulta*/

SELECT category.category_id as id_catagory , category.name AS categoria, AVG(payment.amount) as ganancia_categoria 
FROM category JOIN film_category ON category.category_id= film_category.category_id
JOIN film ON film_category.film_id=film.film_id
JOIN inventory ON inventory.film_id=film.film_id
JOIN rental ON inventory.inventory_id=rental.inventory_id
JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY category.name;


```

![[Pasted image 20230609191725.png]]






```sql
SELECT film.film_id, film.title, AVG(payment.amount) as ganancia_por_peli, CATEGORIA.categoria_nombre, CATEGORIA.ganancia_categoria 
FROM (SELECT category.category_id as id_catagory , category.name AS categoria_nombre, AVG(payment.amount) as ganancia_categoria 
FROM category JOIN film_category ON category.category_id= film_category.category_id
JOIN film ON film_category.film_id=film.film_id
JOIN inventory ON inventory.film_id=film.film_id
JOIN rental ON inventory.inventory_id=rental.inventory_id
JOIN payment ON rental.rental_id=payment.rental_id
GROUP BY category.name) AS CATEGORIA
GROUP BY film.film_id,film.title
HAVING ganancia_por_peli>CATEGORIA.ganancia_categoria 

```


```sql
SELECT film.film_id, film.title AS nombre_pelicula, AVG(payment.amount) AS ganancia_promedio
FROM film
JOIN inventory ON film.film_id = inventory.film_id
JOIN rental ON inventory.inventory_id = rental.inventory_id
JOIN payment ON rental.rental_id = payment.rental_id
GROUP BY film.title
HAVING ganancia_promedio> ALL(
  SELECT AVG(payment.amount) as ganancia_categoria 
FROM category JOIN film_category ON category.category_id= film_category.category_id
JOIN film ON film_category.film_id=film.film_id
JOIN inventory ON inventory.film_id=film.film_id
JOIN rental ON inventory.inventory_id=rental.inventory_id
JOIN payment ON rental.rental_id=payment.rental_id

)
ORDER BY ganancia_promedio DESC;
```

_**La idea del profe fue usar la misma estructura del 4 y 5 pero considerando   en el Having ... > (..... WHERE category.category_id=film_category.category_id ) para indicar que la coincidencia de la pelicula con el category id **_

```sql
SELECT film.film_id, film.title AS nombre_pelicula, AVG(payment.amount) AS ganancia_promedio, category.name as categoria 
FROM category JOIN film_category on category.category_id= film_category.category_id 
JOIN film on film.film_id=film_category.film_id
JOIN inventory ON film.film_id = inventory.film_id
JOIN rental ON inventory.inventory_id = rental.inventory_id
JOIN payment ON rental.rental_id = payment.rental_id
GROUP BY film.title
HAVING ganancia_promedio > (
  SELECT AVG(payment.amount) as ganancia_categoria 
FROM category as category_sub JOIN film_category  as film_category_sub  ON category_sub.category_id= film_category_sub.category_id
JOIN film ON film_category_sub.film_id=film.film_id
JOIN inventory ON inventory.film_id=film.film_id
JOIN rental ON inventory.inventory_id=rental.inventory_id
JOIN payment ON rental.rental_id=payment.rental_id
WHERE category.category_id=film_category_sub.category_id
)
ORDER BY ganancia_promedio DESC;

```