
Son siglas de relational DataBase Management System o Sistema Manejador de Bases de datos relacionales. Son un programas que nos ayudan a llevar nuestros modelos de clases o E-R a una base de datos en nuestra PC, estas  toman un lenguaje base, pero cada uno lo apropia, imponiéndole diferentes reglas y características.
No me sirve tener una base de datos, si no puedo obtener información del ella para eso estan **RDBMS**

## SQL **S**tructured **Q**uery **L**anguage 

**SQL** significa **S**tructured **Q**uery **L**anguage y es un que lenguaje tiene una estructura clara y fija. Su objetivo es hacer un solo lenguaje para consultar cualquier manejador de bases de datos volviéndose un gran estándar. Existe un amplio uso de este lenguaje y se crea cuando se necesito un estadar par hacer un solo lenguaje para cualquier manejador de BD.

Este esta estructurado y c/d ejecución tiene una propia estructura. Que puede ser **hasta aplicada la hora de modelar mi base de datos**. Por ejemplo puedo pensar en la consultas para formular mi BD y elimino un par de relaciones pensado en las queries.    

SQL tiene 2 grandes sublenguas 

[[DDL-Data Definition Language]]

[[DML-Data Manipulation Lenguage]]

[[TCL Sentencias Lenguaje de Control de Transaccion]] 




## Administrador de Base de Datos Relacionales
¿Qué es RDB y RDBMS? Son siglas de relational DataBase Management System o Sistema Manejador de Bases de datos relacionales. Son un programas que nos ayudan a llevar nuestros modelos de clases o E-R a una base de datos en nuestra PC, estas  toman un lenguaje base, pero cada uno lo apropia, imponiéndole diferentes reglas y características.
No me sirve tener una base de datos, si no puedo obtener información del ella para eso están **RDBMS**.

Interfaz del RDBMS MySQLWorkmech ==> [[MySQLWorkbench-ClienteGrafico]]

La interfaces gráficas se conectan a mi servidor de base datos y a partir de ahí nos permiten visualizar y modificar los datos. La **consola o terminal** es mas especifica y mas rápida (es decir que te ahorra tiempos de respuestas) ya que me conecto directamente al servidor MySQL. 

Entrar a la terminal

```
mysql -u root -h localhost -p
```


- _-u_ me indica el usuario utilizare actualmente root
- _-h_ en que servidor IP o dominio esta mi base de datos. Si no lo colocamos por defecto nos llevara a la localhost.
- _-p_ significa que le voy a mandar la contraseña  yo puedo escribirla seguida de la "p" pero esto resultaría poco seguro. Es mejor esperar y que me la pida luego con el click.  

Una vez colocada, entrara a mi terminal de mysql. Dentro tengo ciertos comandos que me ayudan a navegar en la misma, la gran mayoría puedo verlos desde "help".

- ctrl + L limpia la pantalla

- ctrl  + c  Para salir de la terminal  

- **show databases;** -> lista las bases de datos que tiene el servidor o usuario.

- **use name_database;**-> selecciona o se conecta a la base de datos a trabajar

- **show tables;** -> muestra las tablas que contiene la base de datos

Si no se en que base de dato estoy puedo usar el comando. 

- **select database();** -> muestra cual es la base de datos que tenemos seleccionada o en la que se esta trabajando.

- **Show warnings** me muestra el de detalle de la advertencia. 


Todos los comandos deben de terminar con **“;”**

