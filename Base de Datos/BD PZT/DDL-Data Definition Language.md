## DDL (Data Definition Language) 
Es aquel que nos ayudara a crear, borrar y alterar nuestros objetos  de mi base de datos. 
Este tiene 3 comandos principales

__Create__:Nos ayuda a crear bases de datos, tablas,vistas e indices.[RDB_SQL_DDL_DDM](#Create) 

__Drop__: Borra las tablas (precaución al usarla, ya que puede borrar toda la base de datos)[RDB_SQL_DDL_DDM](#Alter) 

__Alter__:Modifica u altera estas entidades.  

## Objetos del DDL

Ademas tenemos 3 grandes grupos de objetos que vamos a manipular con el leguaje DDL.

Las __DATABASE__ son el repositorio de datos que vamos a usar en nuestros proyectos.

Las __Table__ o tablas son la proyección o traducción a SQL de la entidades que tengo en mis diagramas.

Las __View__ o vistas es la proyección de la tabla en la BD para que sea entendible para mi usuario. Las view tienen otras características (ya hare un apartado sobre estas).


# Create

### Crear base de Datos y usar BD
```sql 

CREATE DATABASE test_db;

CREATE DATABASE IF NOT EXIST test_db;

--Si quiero aclarar los caracteres a utilizar 
create database test_db default character set utf8;


--Aveces aparece Schema en ciertos ejecutadores de base de datos
CREATE SCHEMA DATABASE test_db;

--Para indicar cual base de dato vamos a usar. 

USE DATABASE test_db; 

USE test_db; 

```

El  _CREATE DATABESA_ genera mi base de datos y luego va el nombre de la base de datos que quiero usar.

El _USE DATABASE_ esto es para aclarar que voy a usar la base de datos que acabo de crear, también es utilizable cuando recién entro a la consola para aclarar cual es la base de datos que voy a estar utilizando en la terminal. 



### Crear Tabla
```sql 

CREATE TABLE people (
 person_id int,
 last_name varchar(255),
 first_name varchar (255),
 city varchar(255),
 birthdate date,
 
);

```
Crear una tabla es mas complejo ya que cada elemento de mi tabla tiene campos (tipos de datos) y estos campos pueden tener sus restricciones. Una buena practica es nombrar las tablas con el plural del nombre que queremos que tenga por ej. si tengo la tabla persona conviene que sea personas, tambien conviene que sea en ingles.

##### En Workbench

En _Workbench_ debo hacer click en la base de datos en la que quiera trabajar y luego click derecho en tablas "create tables" abrira un panel
![[Pasted image 20230810082336.png]]

El campo **PK** indica clave primaria, marcar esto no solo lo dejara como clave primaria sino que le indicara al gestor las características de una.

El **campo A.I** significa **auto incremeto** util para las Primary Key ya que se va auto incrementado cada vez que creo una.

El campo **NN me indica not null** es decir si podrá ser nulo o no. Si lo marco le estoy diciendo al gestor que no puede ser nulo. 

Una vez tenga todo lo que necesite le doy a Aplicar me lanzara la ventana con la sentencia SQL

```sql
CREATE TABLE `test_franco`.`Persona` (
  `id_Persona` INT NOT NULL AUTO_INCREMENT,
  `last_name` VARCHAR(255) NULL,
  `first_name` VARCHAR(255) NULL,
  `Address` VARCHAR(255) NULL,
  `City` VARCHAR(255) NULL,
  PRIMARY KEY (`id_Persona`));


```

Si quiero ver en workbench la tabla, hago click derecho en la misma "select row, limit 100"


##### En phpMyAdmin

En _phpMyAdmin_ primero en nueva tabla nos va a pedir el numero de columnas y una vez puesto relleno los campos

![[Pasted image 20230510091602.png]]


El campo **indice** nos permite colocar si elemento no puede ser nulo o en este caso llave primaria. 

El campo Nulo funciona asi: Si lo marco **dejara que los atributos puedan ser nulos** y si **no lo marco** hara que mis atributos sean **NOTNULL es decir no los dejara ser nulos**.

![[Pasted image 20230510092026.png]]



#### Ej Sentencia  DDL

Otro ejemplo de crear tabla:
Debes crear una tabla de datos que permita almacenar información sobre personas, llamada `people`. La tabla tendrá cinco campos: person_id, last_name, first_name, address, y city.

-   La columna `person_id` debe ser de tipo entero y debe la llave primaria de la tabla y debe ser autoincremental y recuerda no permitir valores NULOS.
-   La columna `last_name` debe ser de tipo texto y debe tener un tamaño máximo de 255 caracteres y permita valores NULOS.
-   La columna `first_name` debe ser de tipo texto y debe tener un tamaño máximo de 255 caracteres y permita valores NULOS.
-   La columna `address` debe ser de tipo texto y debe tener un tamaño máximo de 255 caracteres y permita valores NULOS.
-   La columna `city` debe ser de tipo texto y debe tener un tamaño máximo de 255 caracteres y permita valores NULOS.

En consola debería escribirlo: 

```sql 

Inserta tu sentencia aqui

CREATE TABLE IF NOT EXISTS people (

person_id INTEGER PRIMARY KEY AUTO_INCREMENT NOT NULL,

last_name VARCHAR(255) NULL,

first_name VARCHAR(255) NULL,

address VARCHAR(255) NULL,

city VARCHAR(255) NULL

);


--Si no agregue la PK
Alter table people
ADD COnstraint pk_people Primary key (person_id)


```

A la hora de añadir tablas y colocar __AUTO_INCREMENT__ deberé tener en cuenta, que si borro el registro el manejador de BD no se da cuenta por ej si yo tenia mi ultimo id 4 y lo borro por defecto el proximo sera 5, en mi tabla vería id 3 a 5.

El __UNSIGNED__ no guarda en mi base de datos el bit negativo del signo esto nos ahorra memoria y _restringe_ a solo usar valores positivos (util por ejemplo en la logica del negocio).

Otro elemento en la creación de tablas para poder

```sql
price DOUBLE(6,2) NOT NULL DEFAULT 10.0,
```

Aparte del not null para que no pueda poner datos nulos puedo usar, el elemento __DEFAULT__ en caso de que no se agregue el campo tengo uno por defecto que ira siempre(util en la logica del negocio). 

### CREACIÓN DE VISTAS O VIEWS

![[Pasted image 20230510154529.png]]

Las vistas tiene que ver con los select y proyecciones. Las vistas toman datos en las BD y los represntan de forma presentable. El comando view tiene 2 comandos prinicpales **CREATE VIEW name AS** siendo el name el nombre de la vista.

La vista nos sirve para **presentar la informacion de [[SELECT-Consultas SQL-Queries SQL]]** aqui me es de mucha utilidad ya que una vez echa la consulta el resultado es temporal pero si lo guardo en una **view** queda en memoria. 

**Por lo tanto dentro de las vista voy a poder guardar la informacion de las consultas que yo realice y presentarlas, ya que las vistas se mantienen en memoria sin consultar en memoria cada vez.** 

_Ejemplo: Considerando mi BD "pruebaBlog" y una tabla "people" voy a crear una vista llamada "gente_Blog" :

```sql 

USE `pruebaBlog`;
CREATE OR REPLACE VIEW `gente_Blog` AS 
SELECT * FROM pruebaBlog.people;


```

-->La consulta va despues del **AS** . 
-->REPLACE es por si la view ya existe es para remplazarla.

En este ejemplo la tabla y la view seran iguales ya que yo la cree asi .

![[Pasted image 20230512081157.png]]


# Alter
## Alter Table

**Alter es el comando que me permite alterar o modificar las tablas**.  
Entonces el ALTER TABLE  se utiliza para modificar la estructura de una tabla (entidad) existente.  

###### ADD:
si quiero agregar otro atributo a la tabla voy a poner **ADD**


```sql 

ALTER TABLE people
ADD direccion VARCHAR (255);

--Opcional--
ADD direccion VARCHAR (255) NULL AFTER city;
--Como opcional para colocar despues de cierta entidad--

```

 Tambien puedo agragar restricciones con (ADD restricción)
 
 **ALTER TABLE/COLUMN**
 Me permite modificar el tipo de dato de mi atributo en la tabla, para eso voy usar el alter table para indicar donde haré el cambio y luego el alter column para indicar que atributo modificare junto al nuevo tipo de dato que quiera colocar

```sql 

ALTER TABLE people
ALTER COLUMN direccion VARCHAR (500);

```

**DROP COLUMN** 
si quiero Borrar el tipo de dato de mi entidad en la tabla voy a poner: 

```sql 

ALTER TABLE pruebaBlog.people
DROP COLUMN direccion;

```

Con esto voy a borrar unicamente el atributo.

También puedo borrar alguna restricción de tabla (DROP CONSTRAINT restricción).

## Drop
Esta sentencia borra todo y por eso es peligrosa, ya que una ves borrada no habrá forma de recuperar eso perdido

 Borrar Tabla:
 ```sql
 DROP TABLE people
```

Borrar la Base de Datos 
```sql
DROP TABLE base_datos
```

![[Pasted image 20230707090406.png]]




Las **sentencias DDL se van a usar muy pocas veces**. Generalmente al inicio de un proyecto, para darle mantenimiento de forma puntual , ect.