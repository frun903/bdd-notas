Consisten en tomar un bloque de sentencias SQL (de forma individual serian ya una transc.) pero al agruparlas las defino como una _unidad de ejecución_ que se consideran una sola sentencia, este bloque tiene un inicio y un fin, si existiera alguna con error _todo el bloque no se ejecutara_. 

La base de datos tiene estados, si esta en estado 0 y desarrollo una transacción llega a un estado 1.  Si llegara a detectar un error  o haber uno se elimina lo procesado y vuelve al estado 0.  

|estado 0|--------->|estado 1|
|estado 0 |------x  Fallo:vuelve a estado 0

**Las transacciones mantienen la integridad de los datos**

Para que se ejecute c/d transacción todo los elementos de la unidad deben funcionar por eso es _atómica_ eso protege la integridad de los datos. 

Otras características que debe contar las transacciones son __ACID__ que quiere decir:
- **Atómicas**: Esto significa que todas las operaciones tienen éxito o fallan.
- **Consistencia:** Mantener el estado de la base de datos en un estado válido.
- **Aislamiento:** Es el requisito de que otras operaciones no puedan acceder a los datos que se han modificado durante una transacción que aún no se ha completado.
- **Durabilidad:** Es la capacidad del sistema de base de datos para poder recuperarse en caso que se presenten transacciones comprometidas contra cualquier tipo de falla del sistema.



### Aislamiento 
Es el requisito de que otras operaciones no puedan acceder a los datos que se han modificado durante una transacción que aún no se ha completado.

Como voy a tener varias transacciones al mismo tiempo al mismo se generan varios inconvenientes:  
- Lectura Sucia: modificaciones en una tabla mientras la A modifica la tabla y B entra mira pero luego la modificación cancela B se queda  con datos erróneos.
- Lecturas fantasmas: una transacción en un momento lanza una consulta de selección con una condición y recibe en ese momento N filas y posteriormente vuelve a lanzar la misma consulta junto con la misma condición y recibe M filas con M > N .Leo 3 registros y vuelvo leer y de golpe hay 9 registros.
- Lecturas no repetibles:Ocurre cuando una transacción activa vuelve a leer un dato cuyo valor difiere con respecto al de la anterior lectura.

_Ante esto las transacciones se **aíslan** para evitar los errores._ Si dos quieren entrar al mismo tiempo el que lo logro llegar antes genera un _bloqueo_ en el recurso o tabla. Estos bloqueos tienen diferentes niveles. El usuario que comenzó el bloqueo debe _confirmar la transacción_ y si no lo hace no se desbloqueara el recurso.

#### Niveles de Aislamiento
Los niveles de aislamientos los maneja el _servidor_, los mayores niveles de aislamientos limitan o disminuyen la performance del sistema.

SQL tiene el _Read Confirm_, Mysql no estaba preparada para utilizar transacciones, para eso esta el _engine InnoDB_ tecnología que se declara cuando creo la base de datos prepara para hacer transacciones. El _engine va en las tablas_, detalle si no vamos  a usar transacciones  no conviene activarlo ya que INNODB tiende a ser mas lento en las consultas. 

![[Pasted image 20230824153030.png]]

#### Autocommit

MySQL viene con un autocommit es decir que realiza automáticamente  las consultas (DML), esto quiere decir que si yo hago un insert automáticamente se veré reflejado en la tabla. _Para poder hacer uso de las transacciones deber cambiar esta opción._  

**autocommit** viene en 1 ==> que las sentencias SQL son confirmadas por default

Para trabajar con transacciones deberé hacer 

SET autocommit=0; 

y convertir el autocommit a un 0. 

```SQL
SELECT @@autocommit;--Ve en que estado esta el autocommit
|1|

SET autocommit=0; --Cambia el estado 

```


## Ejercicios 

#### Ejercicio de Ejemplo!
- crear BD
- crear tabla de 1 columna A(entero)
- iniciar transaccion
- insertar 2 filas 
- Abrir nueva conexion (abrir una nueva terminal) consultar Tabla 
- Confirmar 


![[Pasted image 20230824155015.png]]

Este sirve para observar como el 

##### Problema 1
Considere una transferencia entre dos cuentas. Para lograr esto tienes que escribir sentencias SQL que hagan lo siguiente: Verifique la disponibilidad del monto solicitado en la primera cuenta. Deducir cantidad solicitada de la primera cuenta Depositario en segunda cuenta Si alguien falla este proceso, el conjunto debe revertirse a su estado anterior.

Hecho por Tomi O.

```sql

--funcion ver saldo
delimiter //
CREATE PROCEDURE transferencia( user1 int ,user2 int , monto int)
BEGIN 
	
start transaction;

if ( select clientes.saldo 
from clientes  
where  clientes.id = user1 ) >= monto then
update  clientes  set saldo = clientes.saldo-monto where clientes.id = user1 ;
update  clientes  set saldo = clientes.saldo+monto where clientes.id = user2 ;

commit;
else
rollback;
end if;
end;
 //
 delimiter;

```







#### ejercicio2

Problema 2: Se tiene la siguiente tabla de datos: Datos (id, apellido, nombre, saldo, mail) Crear un procedimiento almacenado que permita insertar datos. Crear un procedimiento almacenado que permita actualizar los datos, apellido, nombre o saldo. Crear un procedimiento almacenado que permita borrar datos. Cada operación debe quedar registrada en la la tabla log_datos (id_cambio,id, apellido, nombre, saldo, mail, operacion) En operación debe quedar registrado el tipo de operación que generó el registro. (I, U, D)

Procedimiento Para insertar datos:

```sql

DELIMITER //

CREATE PROCEDURE insertarDatos( ape_ex varchar(50), nom_ex varchar(50), saldo_ex int, mail_ex varchar(50))
BEGIN
declare IdCambios int;
INSERT INTO datos( apeliddo,nombre,saldo,mail)
VALUES (ape_ex, nom_ex , saldo_ex , mail_ex);

select id into IdCambios from datos
order by id desc
limit 1;


INSERT INTO log_datos( id, apeliddo,nombre,saldo,mail, operacion)
VALUES (IdCambios,ape_ex, nom_ex , saldo_ex , mail_ex, 'I');

END;

//

DELIMITER ;
```


![[Pasted image 20230831170755.png]]

Procedimiento para cambiar  todos los datos Upadtes

```sql
DELIMITER //

CREATE PROCEDURE UpdateDatos( id_ex int ,ape_ex varchar(50), nom_ex varchar(50), saldo_ex int, mail_ex varchar(50))
BEGIN

update datos
SET apeliddo=ape_ex,nombre=nom_ex,saldo=saldo_ex,mail=mail_ex
where id=id_ex;

INSERT INTO log_datos( id, apeliddo,nombre,saldo,mail, operacion)
VALUES (id_ex,ape_ex, nom_ex , saldo_ex , mail_ex, 'U');

END;

//

DELIMITER ;

```


![[Pasted image 20230831172126.png]]


Eliminar el dato:

```sql
DELIMITER //

CREATE PROCEDURE EliDatos(id_ex int)
BEGIN

delete from datos
where id=id_ex;

INSERT INTO log_datos( id, operacion)
VALUES (id_ex, 'D');

END;

//

DELIMITER ;

```

![[Pasted image 20230831172832.png]]



