
## Arquitectura
La arquitectura es de que manera vamos a desarrollar nuestro sistema. Esxiten 2 tippos de arquitecturas bien diferencias 



#### Arq. Centralizada: 
Tengo un equipo central al cual se conectan muchas compus. 
C/d uno de los departemanetos de la organizacion van a tener un servidor propio sirviendo a un grupos de computadores, este servidor es es cual central del cual dependen las otras para trabajar. A su vez puede ser que cada servidor pertenece a c/d departamente esto trae una desventaje y es que entre los Dptos no se pueden tranferir informacion entre si (requerire de herrmientas extrenas)

Una limitacion de esta arquitectura es que dependen de la capacidad de c/d servidor, por lo tanto necesita ser buenos para que los terminales esclavos cuando le soliciten el servidor puedan visualizar lo solicitados 

Para mejorar la teconogia debere cambiar mi elemento central. 
Terminales solo sirven para visualizar datos del central _no pueden_ procesar informacion.
El termianl central es el unico que tiene capacidad de procasmiento



Todos los temrinales esclabos estan concetados al central, los elementos como impresoras (denominados recursos) tambien deben estar conectoados al central

Esta Arq. es muy poco utiliada hoy en dia.  

Termino terminal indica que ese elemento no tiene capacidad de procesamientos.

##### Desventajas

#### Arq. Distribuida

Aqui todos los equipos deberan ser algo de manera tal que no todo queda en solo equipo. Dajando todo la informacion en diversos equipos permitiendo el entre cruze de informacion. Apartir de esos elementos puedo dar informacion a diversos computadores.   

C/d computador prosecasa y tiene la capicidsd de procezamiento por si mismo.
C/d equipamiento puede tener sus propios recursos

Con esto devidimos el procesamiento de varias terminales. 

Para organizaciones muy grandes conviene una Arquitectura distribuida 

##### Desvenajas

Nunca puedo desentralizar todo por ejemplo la base da datos 
Complejidad: es de mayor coste 


## Intro Arq Cliente-Servidor

Se reconcen 3 elementos fundamentales 

Clientes>--------Red-------->Servidor

Los clientes se los puede clasificar en _liviano, pesado e hibrido_ esta clasificacion es  por capacidad de procesamiento.
 - Por ejemplo un terminal seria liviano, por lo tanto si _no_ procesa es liviano
- Si tengo un compotador a o aplicacion es cuando la logica del negocio esta dado en la aplicacion, es decir la logica (validacion,solicitud y mantemiento) da datos es realizado por el. Diriamos que es pesado.


|Cliente|--->----Pedido--->---Servidor(Brinda Servicios en general)

|Cliente|---<----Respuesta-<--Servidor

Algunos servicios del servidor pueden ser archivos y _base de datos_.


Los servidros se clasifican por funcion

### Esquema Cliente Servidpr

3 componentes por los que van **solicitudes y respuestas**, el cliente hace la solicitud y el servidor la respuesta, la ida y vuelta de c/d eleemento es atravez de la red.

(foto)

Por lo tanto siempre hay alguien que solicita y alguien que responda.

### Esquema Fisico y Logico
El esquema desde el pto. de vista logico me indica **dentro de un servidor fisisco pueden ejecutarse 1 o mas procesos servidores**. Cuando hablo de servidor no es solo el equipo fisico qe es la maquina y que logicamnete puede desepañar **mas de un procesos servidores** o servicios. 

Proceso servidor es quel que atiende al cliente, si el cliente quieere mail se activara el proceso de mail.  

(foto)

Distintos clientes pueden solicitar el mismo proceso, es mas se puede preparar el servidor para cuantos clientes necesite para c/d area. Mi servidor requeria de la potencia suficuente para poder desempeñar c/d proceso o servicio.

Un servidor es donde puedo montar multiples servicios. Estos procesos-servidor deben estar en permaente ejecucion o activo para poder responder en cualquier momento cuando el cliente lo solicite.   


## Clasificacion Modelos Clientes-Servidor


### Distribucion de Funciones de Capas


## Desventajas arq. cliente servidor

- ... Muchos elementos de distinta naturaleza deben poder integrarse entre si. (Ej Apis de C++, JV o Py deben unirse a una BD).
- ... La complejidad de la arq.hace referencia a grandes problemas de seguridad debido a los multiples ventanas o visualizaciones de c/d cliente a partir de un mismo servidor,
- ...  La gran solicitud de datos puede _congestionar la red_ haciendo que decaiga la calidad del servidor 