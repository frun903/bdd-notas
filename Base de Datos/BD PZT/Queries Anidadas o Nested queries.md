Significan que dentro de un query [[SELECT-Consultas SQL-Queries SQL]] podemos hacer otro query. Esto sirve para hacer join de tablas, estando una en memoria. También teniendo un query como condicional del otro.  En otras palabras genero una tabla temporal en memoria que usara otras tablas como referencia para armarse a si misma. 
Las consultas anidadas son la mejor opción cuando los valores dependen de otras tablas, y estas no se encuentran relacionadas entre si.
SQL proporciona este mecanismo de **subconsultas anidadas**, una subconsulta es una expresion select-from-where que se anida dentro de otra consulta:

```sql
--Ejemplo:Obtener todos los nombres de proveedores que han realizado al menos un envío de productos.
select distinct snmobre
	from proeedores
	where snum in (select snum from provvedores);

```

Notese como se usa la notacion **in** para denotar que se trata de una consulta anidada. Ademas podemos usar el **not in** para excluir cierta seleccion.
```sql
---Obtener todos los proveedores que no realizaron ningún envío de mercadería
select *
from proveedores
where snum not in (select snum from envios);
```


Un uso común de subconsultas es llevar a cabo comprobaciones sobre pertenencia a conjuntos, comparación de conjuntos y cardinalidad de conjuntos. Inclusive puedo hacer mas especifica la subconsulta para obtener el resultado que necesito:

```sql
--ejemplo: Obtener todos los proveedores de ‘Paris’ que hayan realizado el envío de un ‘Perno’
select *
from provvedores
where provvedroes.ciudad = 'paris' and (snum) in(select snum from partes as P, envios as E where p.nun= e.pnum and p.nombre='Perno');
```


**Desventaja**:
Este proceso puede ser tan profundo como quieras, teniendo infinitos queries anidados. Lo cual puede ser negativo ya que por cada query tengo otro anidado _llevando a que el procesamiento de la consulta sea de caracter **exponencial**_ lo que llevaría que ante muchos datos el procesamiento sea pesado y lento a esto se le conoce como un **producto cartesiano** ya que se multiplican todos los registros de una tabla con todos los del nuevo query. 



Ejemplos del trabajo practico:

**Sin Join**:

![[Trabajos Practicos Clase#5. Obtener todas las películas en las que actué el señor Fred Costner.]]


**Con el JOIN**:
![[Trabajos Practicos Clase#6. Obtener mediante una única consulta todos los actores que hayan actuado en más de 3 películas de la categoría ‘Comedy’. Mostrar los actores que hayan actuado en más películas de este género primero.]]