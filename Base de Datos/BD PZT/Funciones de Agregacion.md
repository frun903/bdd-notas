Son funciones que operan dentro de la Queries [[SELECT-Consultas SQL-Queries SQL]] en el multiconjunto de valores de una columna de una relación y devuelve el valor. Las funciones de agragacion son las siguientes:
- __avg__: valor medio
- __min__:  valor mínimo
- __max__:  valor máximo
- __sum__:  suma de valores
- __count__:  número de valores

Es decir trabajan con los valores de las columnas y devuelven un resultado.
Escritos de manera simple es decir solo la función nos devolverán solo el resultado:
```sql
select avg (presupuesto) as valormedio_de_provedores
from provedores;
```

|valormedio_de_provedores| Vacio |
|---------------------------|-------|
|150                                     |nul| 
otros ejemplos:
![[Pasted image 20230526201509.png]]

Sin embargo aveces nos interesar saber alguna de estas fuciones **cada elemento de la tabla** ante esto entra en juego la instruccion Group by. Por ejemplo cuando vimos Group by utilizamos **count** para saber de cada elemento el valor: 
![[SELECT-Consultas SQL-Queries SQL#Ejemplo simple]]

 Como __observación los atributos de la clausula select fuera de las funciones de agragacion deben aparecer siempre en la lista del group by.__
 En el ejemplo anterior observe que status esta tanto en select como group by. Otro caso seria el siguiente:

```sql
select p.snum, snombre, sum(cant)
from envios as e, proveedores as p
where e.snum =p.snum
group by p.snum, snombre;
```

## Ejemplos de SUM

![[Trabajos Practicos BD-Practica#6. Obtener el monto total de ingresos adquiridos históricos del videoclub.]]

![[Trabajos Practicos BD-Practica#7.¿Que monto total de ingresos supone para el videoclub el alquiler de películas en las que el sr. Costner actúa?]]