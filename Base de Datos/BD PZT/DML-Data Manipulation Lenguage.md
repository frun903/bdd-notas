Tiene que ver con el contenido, actualización (manipular-recuperar datos) y consulta de las base de datos. Tiene ciertos comandos principales 
- insert
- update
- delete
- select ==> [[SELECT-Consultas SQL-Queries SQL]]

Estas se usan muy seguido en el manejo de bases de datos. Con el lenguaje DML puede llavar la BD a un estado inicial a uno próximo. Por lo tanto 3 actualizan generan cambios en la base y el _select no hace cambios pero es el encargado de la recuperacion_

#### Insert
Agrega un nuevo registro a nuestra tabla de base dato. Su estructura consiste en primero seleccionar en que tabla lo vamos a insertar **into** (tabla la cual le vamos a agragar datos) y luego entre paréntesis **value** que son los valores que quiera colocar.
```sql
insert into people(las_name,first_name,address, city)
values ('Ranma', 'Laura', 'Calle 15', 'CBA')
```

En value no necesariamente irán todos los valores, en el caso de no poner alguno se llenaran con el valor default que puede NULL o alguno que yo haya definido con el constrain.  Es muy importante _mantener el orden de los paréntesis del into y el values_ para que la correspondencia de elementos se mantenga.  

#### Update
Permite actualizar o modificar los datos _que ya tenemos_ (si el dato no existe no lo va a modificar). Existen diferentes tipos de Update dependiendo su alcance.

```sql
update people
set last_name= 'Chavez', city='Merida'
where person_id= 1234;
```

En update colocare la tabla que quiera modificar, por otro lado en SET irán los cambios que quiero hacer y por ultimo el where es que me indica a que elemento de esa tabla haré la modificación. Este Update es el **mas especifico** y el que mas utilizare.

otro ejemplo:

```sql
update people
set age=45
where dni=2099345 AND country='Argentina'
```

Luego tengo los update **masivos** donde literalmente modifco columnas (atributos) enteras.

![[Pasted image 20230606101806.png]]

En el 1er ejemplo modifico a todas las personas que vivan en merida y la segunda a todas las personas. 
Los masivos pueden llegar a ser peligrosos si no se utilizan correctamente. 


##### Los SET admiten formulas es decir yo puedo actualizar todos los elementos de la tabla en base al valor antiguo con una formula

```SQL

 update programas
 set Precio= (Precio*20)/100;

```

Osea actualizao en base a los viejos valores de la BD con una formula. 


#### Delete
Esta borra el contenido de una tabla y como el Update tiene **alcances** por ende puede borrar desde un solo casillero a toda la tabla.

```sql
Delete from people 
```
Seria un Delete masivo borrando toda la tabla (_Tratar de NO USAR_)

Suponiendo la siguiente tabla 

| ID  | nombre | apellido |
|----|---------|----------|
|12 | juan     | Degio    |
|23| juli| cesar |
|28| Tomi | Osan| 
```sql
Delete from people 
```

| ID  | nombre | apellido |
|----|---------|----------|
Delete masivo elimino todas las tuplas de una tabla



```sql
delete from people
Where id=23;
```


| ID  | nombre | apellido |
|----|---------|----------|
|12 | juan     | Degio    |
|28| Tomi | Osan| 

Borre **toda la fila del id 23 con todos sus datos**.   


###### Yo: En SQL DML, puedo usar set en el Delete para ser mas especifico con el comando? ChatGPT responde No

ChatGPT: No, en SQL DML (Data Manipulation Language), específicamente en el comando DELETE, no se utiliza la cláusula SET. 

La sintaxis básica del comando DELETE es la siguiente:

```sql

`DELETE FROM tabla WHERE condición;`
```

La cláusula WHERE se utiliza para especificar la condición que deben cumplir las filas que se desean eliminar. Si deseas ser más específico con el comando DELETE, puedes utilizar condiciones más complejas en la cláusula WHERE, utilizando operadores lógicos como AND y OR, así como funciones y comparadores para comparar los valores de las columnas.

Por ejemplo:

```sql

`DELETE FROM tabla WHERE columna1 = valor1 AND columna2 > valor2;`

```

Esto eliminará las filas de la tabla que cumplan las condiciones especificadas. Recuerda tener cuidado al utilizar el comando DELETE, ya que elimina datos de manera permanente de la tabla. Asegúrate de tener una copia de seguridad de los datos importantes antes de ejecutar el comando.

_**Por lo tanto en Delete solo se puede usar Where y elimino un resgisto! no un solo elemento**_  

