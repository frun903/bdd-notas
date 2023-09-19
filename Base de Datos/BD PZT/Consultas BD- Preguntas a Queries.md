De pregunta a Query:

- **SELECT:** Lo que quieres mostrar
- **FROM:** De dónde voy a tomar los datos
- **WHERE:** Los filtros de los datos que quieres mostrar
- **GROUP BY:** Los rubros por los que me interesa agrupar la información, esto incluye los elementos agrupados en las Funciones de agregación 
- **ORDER BY:** El orden en que quiero presentar mi información
- **HAVING:** Los filtros que quiero que mis datos agrupados tengan ya mi Group by
- **LIMIT**: La cantidad de registros que quiero


**GROUP_CONCAT( )**: Toma el resultado de la consuta y lo agrupa en base al campo. Ejemplo:
```sql
select posts.titulo, group_concat(nombre_etiqueta)
from post 
inner join post_etiqueta on posts.id=posts_etiquetas.post_id
inner join etiquetas on etiquetas.id = posts_etiquetas.etiquetas_id
group by posts.id
```

_Nos dara de c/d post cual es su etiqueta asociada_

mencionar que la función GROUP_CONCAT si bien acepta un solo parámetro, se puede personalizar al gusto mediante estas tres opciones
DISTINCT, ORDER BY, SEPARATOR

```sql
SELECT posts.titulo, GROUP_CONCAT(DISTINCT etiquetas.nombre ORDER BY etiquetas.nombre SEPARATOR " - "), posts.id, COUNT(*) AS num_etiquetas FROM posts INNER JOIN posts_etiquetas ON posts.id = posts_etiquetas.post_id INNER JOIN etiquetas ON etiquetas.id = posts_etiquetas.etiqueta_id GROUP BY posts.id ORDER BY num_etiquetas;
```





Ejerció la consulta pide que retorne la etiqueta que no tenga ningun post asociado.

Profe:
```sql
select *
from etiquetas
Left join post_etiquetas on etiquetas.id= posts_etiquetas.etiqueta_id
Where posts_etiquetas.etiqueta_id is null;
```

Yo hubiese utilizado una sub consulta.

```sql
select *
from etiquetas
where etiquetas.id not in
 (select etiquetas.id from etiquetas
 inner join posts_etiquetas on
 etiquetas.id= posts_etiquetas.etiqueta_id);
```

Dan el mismo resultado!


Ejercisio cantidad de post por categoria:
yo:
```sql
select categorias.nombre_categoria, count(posts.id) as cant  from categorias, posts 
where categorias.id=posts.categoria_id
group by
categorias.id
order by cant desc limit 1 ;
```

profe:
![[Pasted image 20230903183900.png]]

Yo:

![[Pasted image 20230903184115.png]]


Ejercicio: Cuantos Posts han echo mis usuarios y de que categorias los han echo?

```sql
 select usuarios.nickname, count(posts.id) as posteos, group_concat(nombre_categoria) as Categorias
from posts inner join usuarios on posts.usuario_id=usuarios.id inner join categorias on categorias.id=posts.categoria_id
group by usuarios.nickname
```


```
mysql> select usuarios.nickname, count(posts.id) as posteos, group_concat(nombre_categoria) as Categorias
    -> from posts inner join usuarios on posts.usuario_id=usuarios.id inner join categorias on categorias.id=posts.categoria_id
    -> group by usuarios.nickname;
+----------+---------+-------------------------------------------------------------------------------------+
| nickname | posteos | Categorias                                                                          |
+----------+---------+-------------------------------------------------------------------------------------+
| Ed       |       4 | Espectáculos,Espectáculos,Espectáculos,Espectáculos                                 |
| Israel   |       6 | Economía,Tecnología,Tecnología,Tecnología,Tecnología,Deportes                       |
| Lau      |       2 | Ciencia,Ciencia                                                                     |
| Moni     |       9 | Deportes,Economía,Economía,Deportes,Deportes,Deportes,Tecnología,Ciencia,Ciencia    |
+----------+---------+-------------------------------------------------------------------------------------+
4 rows in set (0,00 sec)
```
