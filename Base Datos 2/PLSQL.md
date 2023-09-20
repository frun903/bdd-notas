
Es una lógica del servidor donde todos los clientes vana ejecutar la misma lógica. Atra vez de estos puedo acceder a los datos en las BD. La lógica a nivel de servidor nos permite mejorar la seguridad y mejorar la perfonramnce de la BD.  Estas al almacenarse en el servidor nos permiten hacer procesos en forma local significa que no hay trasferencia a través de la red lo que lleva a tener menos trafico de datos.

Las reglas que puedo fijar en el PLSQL son _muy básicas_, si la reglas es muy compleja es difícil depurar (probar). Si hago mucha lógica de forma secuencial voy a bajar el rendimiento o porformance del servidor o Bd. 
Se pueden utilizar DML no DDL (aunque algunos motores de BD no).

Fijar cierta logica del negocio desde la base permitimos que solo veamos lo que qeramos que vean y funcione como queramos nosotros, y con las _transacciones cuidamos la base de datos._ Si  debemos controlar como se actualiza la base debemos velar por los datos (estos son del cliente) deben ser consistentes y seguros (no deben sufrir perdida o modificaciones no indicadas). Ese _control debera tener un esquemas de soluciones por medio de **procedimientos almacenados** y para la seguridad o consistencia (tanto si se debe borrar)_ 


Si hay una actualizacion de la base se debe hacer una **auditoria de la base**, esa actualizacion debe ser detectada por el sistema, tipo si yo modifico la tabla X que quede guardado en algun lugar registrada la modificacion y que se modifico, esta actualizacion deber ser detectable para eso estan los _trigger o Disparador_ [[Trigger o disipador]]    


---


# Procedimientos  Almacenados

Un procedimiento almacenado es un conjunto de instrucciones, que se almacenan bajo un nombre

_Ventajas:_
- Concentran lógica de negocio.
- Evitan acceso directo a las tablas.
- Reducen el tráﬁco de red.

_Desventajas:_
- Se aplican lógicas de negocios, pero no muy complejas.
- Difíciles de depurar.
- Ejecución de un procedimiento almacenado: Se invoca por su nombre
- Se pueden reverenciar tablas, vistas, funciones y procedimientos almacenados.
- Se pueden utilizar instrucciones DML no DDL.



Características de Proc. Almc. para _almacenar como procedimiento un bloque de sentencias SQL (consulta o otros) para hacer una actividad especifica bajo un nombre_. Estos se delimitan con **//** dentro de este bloque el ; de las sentencias no acabaran en el llegaran hasta el **delimiter;** 

```sql
delimiter//

create procedure pa_programas_lista()
begin
select *
from progrmas;
END;
//

delimiter;

--Ejecucion de proc. 

call pa_progrmas_listas();

```
 
En los procedimientos almacenados dentro del paréntesis _puede haber parámetros_ que pueden ser usados sobre el procedimiento. 
```sql
-- Parámetros
delimiter //
CREATE PROCEDURE pa_programas_buscar(parametro_id integer)
BEGIN
SELECT *
FROM programas
WHERE id = parametro_id;
END;
//
delimiter ;
```

 
En los procedimientos almacenados pueden tener o crear variables locales.
```sql
DELIMITER // -- Cambia Delimitadores
CREATE PROCEDURE pa_programas_cantidad2()
BEGIN
-- Declara variable local al procedimiento
DECLARE vprogramas INT;
SELECT COUNT(*)
FROM programas
INTO vprogramas; -- Almacena resultado consulta en variable

SELECT vprogramas; -- Muestra variable
END;
//
DELIMITER ; -- Recompone Delimitadores

```

Estas variables _pueden guardar variables_ por medio del comando **INTO**. 
estas variables locales funcionan dentro del bloque si quisiera usarlas afuera deberé invocar el procedimiento _y que devuelva esa variable para usarla dentro de otro procedimiento_, pero si quiero usarlas deber invocar si o si el procedimiento!.  

```sql
-- Calcular cantidad de programas.
DELIMITER //

CREATE PROCEDURE pa_programas_cantidad_tipo
(IN var_tipo VARCHAR(20), OUT var_programas INT)

BEGIN
SELECT COUNT(*)
INTO var_programas
FROM programas
WHERE tipo =var_tipo;
END;
//
DELIMITER

```

Los parámetros entraran cuando llame al proce. 

---

# Funciones Almacenadas

Permiten retornar **solo un valor** sin embargo simplifican mucho el desarrollo de las consultas. Sin embargo en algunos motores disminuye la performance. Estas funciones se denominan de lineas ya que van en linea con mi consulta sql. Acepta parámetros y debemos especificar en la cabecera que  tipo de dato se retorna. La función se utiliza como elemento complementario para una consulta select para desarrollo del mismo, inclusive puedo utilizar elementos dentro del mismo select como parámetros.

```sql
DELIMITER //

CREATE FUNCTION fa_programas_cantidad()
RETURNS INT -- Especiﬁcamos parámetro de salida
BEGIN

DECLARE vcantidad INT;
SELECT COUNT(*)
INTO vcantidad
FROM programas;
RETURN vcantidad; -- Retornar el valor obtenido

END;
//
DELIMITER;
```

Una función, además contiene la cláusula "return". Una función acepta parámetros, se invoca con su nombre y retorna un valor. Puede ser invocada en una consulta de la siguiente forma:

```sql
select nompro, origen, precio,f_incremento(precio,10)
from productos;
```

 _Por lo tanto estas funciones  no hacen cálculos en si sino que aportan algún dato (que es aportado por ej al select)._ 


#### Yo Probando Funciones!

```sql
DELIMITER //

CREATE FUNCTION Promedio_Precios()
RETURNS INT
BEGIN
    DECLARE Promedio_Peli INT;
    
    SELECT AVG(Precio)
    INTO Promedio_Peli
    FROM programas;
    
    RETURN Promedio_Peli; 
END;

//

DELIMITER ;

```

Al probar la funcion me lanza "ERROR 1418 (HY000): This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you might want to use the less safe log_bin_trust_function_creators variable)". Segun ChatGPT
_l error que estás viendo se debe a que estás creando una función almacenada sin especificar si es `DETERMINISTIC`, `NO SQL` o `READS SQL DATA` en su declaración. Además, si la replicación binaria (binary logging) está habilitada en tu servidor MySQL, es posible que necesites ajustar la variable `log_bin_trust_function_creators` para permitir la creación de funciones._

Ante esto nos da la siguiente solucion:
![[Pasted image 20230831082943.png]]
![[Pasted image 20230831083027.png]]


Solo con **colocar la variable global**
```sql
SET GLOBAL log_bin_trust_function_creators = 1;
```
**Ya mis funciones funcionan correctamente.**

Pruebo llamar mi función:

```sql
select * from programas where Precio>Promedio_Precios();
```

![[Pasted image 20230831082748.png]]

--- 

# Proc. Almc. Control de Flujo

- sentencia _set_ le da valor constante a mi variable
- Puedo establecer mi condicional como en cualquier lenguaje (if/end if;)
- Puedo usar _case_ como el switch 
- **Ciclos** como el _while, Do while_ 
- loops tipo _for_ 
- Puedo usar _nsert into lista values(v);_ para agregar en la lista.

--- 

## Ej:  Procedimientos Almacenados 

![[Pasted image 20230817155805.png]]


#### Para borrar procedimiento
Uso la sentencia  `drop procedure nombre_Precedimiento;` 

![[Pasted image 20230817161107.png]]



##### Ejemplos

![[Pasted image 20230817161551.png]]


![[Pasted image 20230817170450.png]]


![[Pasted image 20230817171924.png]]

**Observacion cancat(varible,'%')**

![[Pasted image 20230817173456.png]]


![[Pasted image 20230817175201.png]]


#### Idea del Profe!
La idea es crear un _procedimiento almacenados que como parametro acepte los valores a insertar en la tabla._ Dentro del procedimiento almacenados vamos a usar DML

```sql

delimiter//
create procedure insertar_Datos(titulo_ex char(50), estudio_ex char(50),anio_ex int, tipo_ex char(50) )
begin

insert into programas(titulo,estudio,anio, tipo)
values (titulo_ex, estudio_ex, anio_ex,  tipo_ex);

END;
//
delimiter;


```

![[Pasted image 20230824123028.png]]

![[Pasted image 20230824123039.png]]


```sql
DELIMITER //

CREATE PROCEDURE insertarDatos(titulo_ex VARCHAR(50), estudio_ex VARCHAR(50), anio_ex INT, precio_ex INT, tipo_ex VARCHAR(50))
BEGIN
    INSERT INTO programas (titulo, estudio, anio, Precio, tipo)
    VALUES (titulo_ex, estudio_ex, anio_ex, precio_ex, tipo_ex);
END;

//

DELIMITER ;


--Llamar al procedimineto

CALL insertarDatos('Logan', 'Fox', 2016, 65, 'Pelicula');
CALL insertarDatos('DeadPool', 'Fox', 2016, 70, 'Pelicula');
CALL insertarDatos('Animales', 'Warner', 2012, 35, 'documental');

```


### Observacion Rara pero necesaria despues del END tanto en procedimentos almacenados como en funciones poner ; End;


