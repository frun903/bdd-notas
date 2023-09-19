Temiendo la llegada un parcial a mano la idea es reacer cada TP pero con una escritura de una sola vez. Como el parcial va ser asi me parece una buena practica de [[SELECT-Consultas SQL-Queries SQL]] y de paso pruebo el metodo de [[BD "Mira lo que hago yo es" By Juan]]  y con la informacion del profe.


### Trabajo practico 1 consignas:
1.  Obtener una lista de todos los actores.
2.  Obtener una lista de todos los apellidos de actores, sin repetir.
3.  Obtener una lista de todas las ciudades de Argentina.
4.  Obtener nombre, apellido, direccion y telefono de todos los clientes de Argentina que tengan domicilio en Buenos Aires.
5.  Obtener el nombre de todos los clientes que hayan realizado un alquiler cuyo monto total de pago supere los 10 dolares. Presentar estos datos ordenados de mayor a menos e incluir ademas como resultado de la consulta el monto total abonado.
6.  Obtener el número de inventario de todas aquellas películas alquiladas que aún no han sido devueltas.

**Resolucion**

###### 1.1 Obtener una lista de todos los actores.

```sql
select *
from actor;
```

###### 1.2  Obtener una lista de todos los apellidos de actores, sin repetir.

```sql
Select distinct A.last_name
from actor as A;
```

###### 1.3 Obtener una lista de todas las ciudades de Argentina.
```sql
select city.city
from city as Cy, contry as pais
Where Cy.country_id=pais.country_id And pais.country='Argentina';
```

###### 1.4 Obtener nombre, apellido, direccion y telefono de todos los clientes de Argentina que tengan domicilio en Buenos Aires.
"De este me acuerdo un poco Buenos aires no esta en City como uno naturalmente pensaría, si mal lo recuerdo estaba en district"

```sql
select customer.first_name, customer.last_name, address.address, address.phone
from customer, address, city , country 
--Debo tratar de en el Where hacer las conexiones de estos elementos 
Where  country.country_id=city.country_id and city.city_id=address.city_id and address.address_id=customer.address_id and country='Argentina' And district='Buenos Aires';
```


###### 1.5  Obtener el nombre de todos los clientes que hayan realizado un alquiler cuyo monto total de pago supere los 10 dolares. Presentar estos datos ordenados de mayor a menos e incluir ademas como resultado de la consulta el monto total abonado.

```sql
select customer.first_name
from customer, payment, rental
Where rental.rental_id=payment.rental_id


```

### Trabajo Practico 2 (pre-parcial)
1. Obtener una lista de todas las películas que fueron alquiladas al menos una vez

```sql
select distinct film.title
from film inner join inventory on film.film_id=inventory.film_id
inner join rental on inventory.inventory_id=rental.inventory_id;
```
tuplas=958
```sql
select distinct film.title
from film, inventory, rental
where film.film_id=inventory.film_id and inventory.inventory_id=rental.inventory_id;
```
tuplas= 958

2. Obtener una lista de todas las películas en inventario que no registren ningún alquiler.
```sql
select film.title
from film INNER JOIN inventory on inventory.film_id=film.film_id LEFT JOIN rental on rental.inventory_id=inventory.inventory_id
WHERE rental.rental_id is null;
```

##### Uso de subqueries para resolucion
**Solución del profe el planteamiento del problema es genial y es con una _subconsulta_ de esta forma es mas facil separar los subconjuntos en mi mente.** 

```sql
select film.title
from film
where film.film_id in (select DISTINCT i.film_id from inventory as i left join rental on i.inventory_id=rental.inventory_id
WHERE rental.rental_id is null);
```

**Ojo para destacar elemento null usar _is**.

Es decir revisa con el **in** si dentro de la subconsulta (tabla dentro de tabla) si hay elementos en coincidencia. 

Como recomendacion para las Queries anidadas [[Queries Anidadas o Nested queries]] usar el **as** para distinguir los elemento internos de la tabla con los externos esto es muy util para las comparciones ya que diferencia un mismo atributo externo con uno interno.  

_Otra opcion:_
Este seria otro caso donde digo que me des la películas que no estén dentro (not in) de aquellas peliculas del inventario y tengan coincidencia con rental. 
```sql
select film.title
from film
where film.film_id not in (select DISTINCT i.film_id from inventory as i inner join rental on i.inventory_id=rental.inventory_id);
```


###### 3. Obtener la cantidad de veces que fue alquilada cada película. Presentar los datos como “titulo”, “cantidad_alquileres”.

```sql
select film.title as titulo, count (rental.rental_id) as cantidad_alquileres
from film, inventory, rental
where film.film_id=inventory.film_id and inventory.inventory_id=rental.inventory_id
group by titulo;

/*Como lo haria con Joins*/
select film.title as titulo, count(rental.rental_id) as cantidad_alquileres
from film inner join inventory on film.film_id=inventory.film_id
inner join rental on inventory.inventory_id=rental.inventory_id
group by titulo;
```

###### 4. ¿Cuáles son las 10 películas más alquiladas por los clientes del videoclub?
```sql
select film.title as titulo, count (rental.rental_id) as cantidad_alquileres
from film, inventory, rental
where film.film_id=inventory.film_id and inventory.inventory_id=rental.inventory_id
group by titulo
order by cantidad_alquileres Desc limit 10;

```


###### 5. Obtener todas las películas en las que actué el señor Fred Costner.
```sql
select film.title
from film inner join film_actor on film.film_id= film_actor.film_id 
inner join actor on film_actor.actor_id=actor.actor_id
where actor.first_name='Fred' and actor.last_name='Costner'
```

###### 6. Obtener el monto total de ingresos adquiridos históricos del videoclub. 
```sql
select sum(payment.amount) as ingreso_historico
from payment 

```

###### 7.¿Que monto total de ingresos supone para el videoclub el alquiler de películas en las que el sr. Costner actúa?
```sql
select sum(payment.amount) as ingreso_SrCostner
from payment inner join rental on payment.rental_id=rental.rental_id
inner join inventory on rental.inventory_id=inventory.inventory_id
inner join film on film.film_id=inventory.film_id
inner join film_actor on film.film_id= film_actor.film_id 
inner join actor on film_actor.actor_id=actor.actor_id
where actor.first_name='Fred' and actor.last_name='Costner'
```

![[Pasted image 20230615175452.png]]

### Trabajo Practico 3  (pre-parcial)
###### 1. Obtener una lista completa de las películas que no se encuentren en stock.

```sql
select film.title
from film left join inventory on inventory.film_id= film.film_id
where inventory.inventory_id is null
```


###### 2. A partir de una única consulta responder: ¿Existen películas que no estén vinculadas con ningún actor?

```sql
select film.title 
from film inner join film_actor on film.film_id=film_actor.film_id
where film_actor.actor_id is null 

```

No existe pelicula sin actores


###### 3. Obtener nombre y apellido de aquellos clientes que hayan alquilado la misma película al menos en dos ocasiones. El resultado de la consulta debe incluir ademas el titulo de la película y la cantidad de veces que fue alquilada por dicho cliente.



```sql
select customer.first_name, customer.last_name, COUNT(rental.rental_id) as alquileres , film.title as titulo_pelicula
from customer inner join rental on rental.customer_id=customer.customer_id
inner join inventory on inventory.inventory_id= rental.inventory_id
inner join film on inventory.film_id=film.film_id
group by titulo_pelicula, customer.first_name, customer.last_name
having alquileres>=2
```

**_Este es muy interesante_** ya que es toda la consulta la que hace que funcione. Por un lado el salect trae todos los elementos incluyendo el count(). El count(rental.rental_id) funciona porque _puse el group by_ que le dijo a mi count que que cuente las **peliculas** (title y el profe lo mejora con id film )alquiladas por tales **cliente**(nombre, apellido y la version del profe _incluye el id lo que lo mejora ya que puede haber clientes con el mismo nombre y apellido_). Por ultimo el having hace que sea mayor o igual a 2 la cantidad de aquileres 
![[Pasted image 20230615192710.png]]


Si en el group by solo hubiera puesto el film.title me lanzaba todo pero el _count_ estaba en funcion de pelicula unicamente es como si le preguntara cuentas veces fue alquilada tal pelicula.

###### 4. Obtener todos los datos de las películas cuyo stock sea de exactamente 2 ejemplares.

```sql
select *, count(inventory.film_id) as stock
from film inner join inventory on inventory.film_id=film.film_id
group by film.film_title /*Lo mejor seria usar id por si el nombre es el mismo*/ 
having stock=2;

```


###### 5. Obtener una tabla a través de la cual se pueda responder cuántos alquileres fueron realizados en cada sucursal del videoclub y cual es el total recaudado en cada de ellas. Presentar los datos solicitados como “cant_alquileres” y “ganancia” respectivamente.
```sql
select store.store_id, count(rental.rental_id) as cant_alquileres, sum(payment.amount) as ganancia
from rental inner join payment on rental.rental_id=payment.rental_id
inner join staff on staff.staff_id=payment.staff_id
inner join store on store.store_id=staff.store_id
group by store.store_id;

```


###### 6. Obtener mediante una única consulta todos los actores que hayan actuado en más de 3 películas de la categoría ‘Comedy’. Mostrar los actores que hayan actuado en más películas de este género primero.

```sql
select actor.actor_id, actor.first_name, actor.last_name, count(film.film_id) as cantidad_pelicula
from actor inner join film_actor on film_actor.actor_id=actor.actor_id
inner join film on film.film_id= film_actor.film_id
inner join film_category on film.film_id= film_category.film_id
inner join category on category.category_id=film_category.category_id
Where category.name='Comedy'
group by actor.actor_id
having cantidad_pelicula>3
order by cantidad_pelicula desc;

```

###### 7. Obtener el titulo de las 10 películas que más ganancia hayan generado a la empresa y la ganancia total acumulada por alquileres de cada una de ellas. Ordenar los resultados de mayor a menor.
```sql
select film.title, Sum(payment.amount) as ganancia
from film inner join inventory on inventory.film_id= film.film_id
inner join rental on rental.inventory_id= inventory.inventory_id
inner join payment on rental.rental_id= payment.rental_id
group by film.title
order by ganancia desc limit 10;
```

**Ojo pide ganancia= SUM() y no olvidar del limite de 10**

###### 8. Obtener el titulo de las películas cuya ganancia promedio sea menor al promedio de ganancia de todas las película
```sql
/**Requiere sub consulta**/

select film.title, AVG(payment.amount) as ganancia_por_peli
from film inner join inventory on film.film_id=invetory.film_id
inner join rental on rental.inventory_id= inventory.inventory_id
inner join payment on rental.rental_id= payment.rental_id
group by film.title
having ganancia_por_peli<(select AVG(payment.amount) as ganancia_promedio
from film inner join inventory on film.film_id=invetory.film_id
inner join rental on rental.inventory_id= inventory.inventory_id
inner join payment on rental.rental_id= payment.rental_id)

```


### Trabajo Practico 3

###### 3.4  

###### 3.5   Obtener una tabla a través de la cual se pueda responder cuántos alquileres fueron realizados en cada sucursal del videoclub y cual es el total recaudado en cada de ellas. Presentar los datos solicitados como “cant_alquileres” y “ganancia” respectivamente.
¿Que me pide?
La cantidad de Alquileres y la ganancia histórica de cada sucursal o tienda store

```sql
SELECT store.store_id,COUNT(rental.rental_id) AS 'Cant. Alq' , SUM(payment.amount) AS 'Ganancia'
FROM payment JOIN staff ON payment.staff_id=staff.staff_id 
JOIN rental ON rental.rental_id=payment.rental_id
JOIN inventory on inventory.inventory_id=rental.inventory_id
JOIN store ON inventory.store_id=store.store_id
GROUP BY store.store_id;

```


###### 3.6 Obtener mediante una única consulta todos los actores que hayan actuado en más de 3 películas de la categoría ‘Comedy’. Mostrar los actores que hayan actuado en más películas de este género primero.

¿Que me pide?
	Actores que hayan trabajado en mas de 3 películas de la categoría comedia 

```sql
SELECT actor.first_name, actor.last_name
FROM actor JOIN film_actor ON actor.actor_id=film_actor.actor_id
INNER JOIN film ON film_actor.film_id=film.film_id
INNER JOIN film_category ON film_category.film_id=film.film_id
INNER JOIN category ON film_category.category_id=category.category_id 
WHERE category.name='Comedy' 
GROUP BY actor.actor_id,actor.first_name, actor.last_name
HAVING COUNT(film.film_id)>3
ORDER BY COUNT(film.film_id) DESC;
```

**Observacion**: Dentro de los Count no vuelvo a poner todo el camino ya que seria **redundante**  porque lo definí en el FROM

**Observacion 2!:** Poner siempre en el Group by los elementos del Select **siempre** 

**_Entonces en otras palabras lo obtenido una vez de la conexion queda en la para las operaciones del Where select y AVG,SUM, ect..._

###### 3.7   Obtener el titulo de las 10 películas que más ganancia hayan generado a la empresa y la ganancia total acumulada por alquileres de cada una de ellas. Ordenar los resultados de mayor a menor.

- Me pide la 10 películas que mayor recaudacion hayan tenido y ese monto.
- **Tembien me pide que lo ordene

```sql
SELECT film.title, SUM(payment.amount) AS ganancia
FROM payment INNER JOIN rental ON payment.rental_id=rental.rental_id
INNER JOIN inventory ON inventory.inventory_id=rental.inventory_id
INNER JOIN film ON film.film_id=inventory.film_id
GROUP BY film.film_id
order by ganancia DESC
LIMIT 10;
```

____importante!!! En el ORDER BY siempre usar el id ya que el titulo, o cualquier otro atributo conviene 



###### 3.8 Obtener el titulo de las películas cuya ganancia promedio sea menor al promedio de ganancia de todas las películas.
```sql
SELECT film.title,AVG(payment.amount)
FROM film, rental, inventory, payment
WHERE payment.rental_id=rental.rental_id AND inventory.inventory_id=rental.inventory_id AND film.film_id=inventory.film_id 
GROUP BY film.film_id
HAVING AVG(payment.amount)<(SELECT  AVG(payment.amount)
FROM payment, rental, inventory, film
WHERE payment.rental_id=rental.rental_id AND inventory.inventory_id=rental.inventory_id AND film.film_id=inventory.film_id);


--Observar como en el Having ya estoy medio redundante 

SELECT film.title,AVG(payment.amount)
FROM film, rental, inventory, payment
WHERE payment.rental_id=rental.rental_id AND inventory.inventory_id=rental.inventory_id AND film.film_id=inventory.film_id 
GROUP BY film.film_id
HAVING AVG(payment.amount)<(SELECT  AVG(payment.amount)
FROM payment);


```

5. Obtener nombre y ganancia promedio de todas las películas cuya ganancia promedio supere _el promedio de todas las categoría_. Ordenar por promedio de mayor a menor.

_el promedio de todas las categoría_ indica una subconsulta. Como voy a comparar uno elemento con muchos debemos usar la senencia **SOME** que me indicaba que haga esa comparación con alguno elementos. También para poder comparar uno con muchos puedo usar también **ALL** .


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++




