Consisten en tomar un bloque de sentencias SQL (de forma individual serian ya una transc.) pero al agruparlas las defino como una _unidad de ejecución_ que se consideran una sola sentencia, este bloque tiene un inicio y un fin, si existiera alguna con error _todo el bloque no se ejecutara_. 

La base de datos esta en estado 0 si desarrollo la transacción llega a un estado 1 pero si llega a detectar un error elimina lo procesado y vuelve al estado 0.  

|est1|---------|est2|
|est1 |------x  Fallo:vuelve a estado 1

**Las transacciones mantienen la integridad de los datos**

Para que se ejecute c/d transaccion todo los elementos de la unidad deben funcionar por eso es _atómica_.

Como voy a tener varias transacciones al mismo tiempo al mismo se generan varios inconvenientes 
- lecutra sucia: modificacioens en una tabla mientras la A modifica la tabla y B entra mira pero luego la modificacion cancela B se queda  con datos erroneos.
- Lecutras fantasmas: Leo 3 regiustros y vuelvo leer leo 9

_Ante esto las transacciones se **aislan** para evitar los errores._ Si dos quieren entrar al mismo tiempo el que lo logro llegar antes genera un _bloqueo_ en el recurso o tabla. Estos bloqueos tienen diferentes niveles. El usuario que comenzó el bloqueo debe _confirmar la transacción_ y si no lo hace no se desbloqueara el recurso.

#### Niveles de Aislamiento
Los niveles de aislamientos los maneja el _servidor_, los mayores niveles de aislamientos limitan o disminuyen la performance del sistema. SQL tiene el _Read Confirm_, mysql no estaba preparada para utilizar transacciones, para eso esta el _engine InnoDB_ tecnología que se declara cuando creo la base de datos prepara para hacer transacciones. El _engine va en las tablas_, detalle si no vamos  a usar transacciones  no conviene activarlo

![[Pasted image 20230824153030.png]]

**autocommit** viene en 1 ==> que las sentencias SQL son confirmadas por default

Para trabajar con transacciones deberé hacer 

SET autocommit=0; 

y convertir el autocommit a un 0. 

## Ejercicios 

#### Ejercicio de Ejemplo!
- crear BD
- crear tabla de 1 columna A(entero)
- iniciar transaccion
- insertar 2 filas 
- Abrir nueva conexion (abrir una nueva terminal) consultar Tabla 
- Confirmar 


![[Pasted image 20230824155015.png]]


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



**ctrl c ==> Para salir**



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



