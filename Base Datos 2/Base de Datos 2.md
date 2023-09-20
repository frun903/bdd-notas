Dentro de una organización tengo un sistema corporativo que tiene una _aplicativo_ y _base de datos_ la lógica del negocio puede ir en uno o en otro o en el medio. Sin embargo hay ciertas cuestiones que son solo de la base de datos. Los datos dentro de la BD sean _datos calidad_ sin eso tanto la lógica como la api van a fallar, esto incluye la transferencia, inserción de datos de manera correcta para tener la calidad de los datos requiero ciertas tareas de administración (eso me da seguridad en los datos) y el mantenimiento (que brinda la optimisacion de los mismos).  Estos datos como organización me va a permitir tomar decisiones para el desarrollo de la misma, si los datos no son buenos no podre hacer buenas estrategias para mi negocio. Los datos de calidad son las que dan soporte para tomar decisiones de mi negocio ya que son los echos por eso la importancia de cuidarlos y trabajarlos correctamente.
Hoy en día esos datos no solo sirven para el pasado sino también para ver que podría pasar en el futuro. 

En la materia de BD2  vamos a armar un data were  hause para poder dar soporte de decisiones a mis organizaciones. 

Cuando pleantemaos un problema necesitamos una solución pero esa solución requiere de tener un **objetivo claro** de los que debo trabajar.

_Entonces a partir de un objetivo voy a diseñar el sistema del data were hause para poder dar soporte a la organizacion_. La importancia del objetivo es tan grande que si es mal planteado que perderíamos la direccion del proyecto. 


##### Propuesta en curso:

Sera un proyecto para platear un estudio debere decidir sobre que elemento del proyecto o organizacion por ejemplo quiero saber _las ventas_, el consumo de mis equipos, ect. Es decir yo debere tener bien claro que elemento voy a estudar ante que efectos. 

##### A nivel Software 
Para esta materia sobre todo para la parte de Were hause:

- El motor de la base datos los que voy a utilizar es MySql/;MariaDB --> Trae una intefaz de cliente terminal.
- Herramienta Grafica Workbench/HeidiSQL
- Pentaho: que permite hacer los procesos de extraccion y tranferencia de datos sobre la BD y me permite a hacer los "cubos" a partir la cual vamos a hacer la consulta. Dentro de esto necesitare Kettle

##### Evaluacion
Respetar un poco la asistencia
1 Parcial Teorico- Practico
El Proyecto

Examen final es mostrar el proyecto y una "puequeña parte teorica o practica".


## Clase practica:


|Cliente|--------> |Servidor  |
 							 |  Servidor|------->| Base de Datos |


Una _Base de datos_ es una colección de datos _organizados_. Es importante que esten almacenados de manera organizada para una correcta recuperación. Yo los coleccionos en tablas respetando un _modelo relacional_, este modelo se carecterisa por tener relaciones 

|tab1| tab2| tab3 |
|----|-----|-----|
|-|-|-|
- Relacion   Tablas (para la implematacion)
- Tuplas filas Reguistros(impl.)
- Atributos columnas columnas o campos

Una tabla en el modelo puro se llama relacion. Las bases de datos son unos o mas tablas por lo tanto si tiene mas tablas implica mas relaciones. Las relaciones se referencian entre si (e cuendo tengo atributos coicidentes la llaves foraneas). 

Para que el dato tenga un dominio debe ser de un tipo de datos. Los datos ya estan organizados otra propiedad del modelo relacional esta definida por el algebra relacional o calculo relacional esto indica como ir haciendo las operaciones. El algbra o calculo ralcional permiten que existan o equivalen al SQL.

Todos los motroes manejan el mismo estandar, mas alla de esta c/d motor tendra su propias caracteristicas. 

MySql y SQLsever se sostienen en el modelo relacional 

#### BD

Los datos en la base se organizan/responde en base al _modelo relacional_ y que me permite trabajar con las _tablas_. El modelo de hace con el modelo de entidad-relacion y sobre todo materializar esas _ralciones_ hecho el diseño llevo el modelo al motor de mi base de datos. 

(Cuaderno Santi)

La materialización de la BD voy a necesitar el lenguaje de SQL para poder construirlo. 



SQL es un conjunto de sentencias que se seleccionan en base a los que debo realizar. _Durante el cursado vamos a tener que escribir SQL_. Para la escritura el profe evaluara que significa c/d sentencia. 

¿Diferencia Clave primaria e indicie? La PK es la que distingue los elementos de la tablas. la ventaja de tener indice me ayuda en búsqueda o en el proceso de recuperación de datos, pero la dev. lo hace mas pesado ya que aumenta el tamaño de la misma y la carga de datos sera mas compleja ya que la actualizacion o agragacion de los datos los otros indices deberán actualizarse c/d vez que haga una de estas operaciones por lo tanto ante una actualizacion hay una degradacion en la performans del sistema.
_Los indices_ (cualquiera que no sea clave primaria) pueden llegar admitir repetidos pero se puede _declarar_ que NO admita repetidos.

#### SELECT
Las consultas del select nos permiten la extraccion de datos.
El SQL con el select nos brinda un **Conjunto de datos Resultados**. Lapalabra SElect me indica qe quiero visualizar y la palabra from el origen de los datos que quiero visualizar. 


El asterisco nos trae todo ¿Es conveniente utilizarlo? Este trae un tiempo extra de ejecucion ya que debe ordenar en la BD del sistema, lo ideal es escribir la lista de campos para que mi sist. no deba ordenarlos y solo traerlos.  

BD sistema
Dentro de mi base de datos esta compuesta por la que arme nosotros y otra del sistema (formado por metadatos que definen mi BD). Aparte de Sistema tengo otra BD es el TEMPDB persisten cuando nos da el resultado del SELECT pero luego es eliminda (como una RAM)


La delicadeza del SELECT es clave ya que escribir una consulta compleja traera mayores tiempos de ejecución.


Para el SELECT debe haber un _plan de ejecución_ debe ser optimo lo desarrolla el motor pero debemos escribir bien las consultas sobre todo en los JOIN en las distintas tablas. El plan de ejecución se base en los JOIN y los indices de mi tabla (estos favorecen la ejec.).


#### Funciones del DML

Como todo lenguaje podemos crear nuestras propias funciones y tengo algunas ya definas pero no todas se hacen en todos los casos. Generalmente las mas usadas son las de _agrupamiento o agregacion (SUM,AVG,COUNT)_ sobre todo en la manipulación y entontecen de información por medio de los datos. Existen distintas posibilidades de crear funciones de agregación.


#### Vistas
.Si tengo que repetir muchas veces una consulta no es practico ante esto las vistas que tienen su origen en las consultas 


Tablas---------|**Vista** |-------

![[Pasted image 20230810151714.png]]

Luego de crear la vista esta se comporta como una _tabla nueva_ y la puedo utilizar para realizar operaciones con ella (para otra búsquedas) y sobre ella.
Ademas estas agregan un **grado de seguridad** para la base de datos, ya que los usuarios tendrán diferentes vistas, ya que restringen el acceso y lo que podra ver c/d user.

Por lo atnato las vistas nos permiten almacenar consultas de forma permanente y no debo rescribirlas varias veces y la otra venataja consiste en la restrcion de accesos a los datos (esta restriccion es filas o columnas) si yo solo doy accseso a las vistas brindo seguridad  en una BD.

#### PLSQL
El lenguaje PLSQL ejecuta como motor de BD herimientas o procesos agregando _Funciones, triggers y procesos de almacenamiento._ [[PLSQL]] 


#### Transacciones
Sirve para implementar sistemas que sean útiles. En la arquitectura [[Programacion Servidor-Cliente]] muchos clientes pueden solicitar pedidos al servidor, este servidor debe responder a todos, esto significa que el servidor debe soportar un acceso **concurrente** es poder responder varias solicitudes en el mismo momento.
El tema es que si la accion es basado en consulta _no hay modificacion de datos_ por lo cual atenderlos sera mas rapido, sin embargo dependera _de los recursos_ (si hay muchos clientes sera complicado). Donde el servidor tendra problamas _es cuando la accion es la actualizacion y consulta de datos_ estas actualizaciones o cambios de estados de la base pueden alterar las consultas-repuestas para el cliente, 

El motor debe estar preparado y tiene herramientas para evitar incosistencias en mi negocio. 
Las transacciones cuando hago un update hago una transaccion, una trans. es cualquier operacion que haga para actualizar la bd. Lo ideal es hacer todas las transacciones en un conjunto que se consideren en una sola actualizacion (ej 3 sentencias de update)

[[Transacciones]] 

[[Trabajo Practico 1- Base datos 2]] 


## Sistema de Soporte de Decisiones

Nosotros almacenamos datos para conocer el desarrollo del ámbito donde me desempeño, en general la BD son muy importantes. Los datos que yo guardo deben ser **datos de calidad** es el dato correcto y que responde a las reglas del negocio. 
Ponele si es NOTA del 0 a 10 no puede haber un 14 eso rompe las reglas del negocios. Mis datos ven el comportamiento de mi organización, todos los elementos (transacción,dominios,ect) es para que mis datos sean de calidad pero esto no es suficiente _si yo no hago un análisis correcto_, el **dato como un aparato mas de análisis del cual me voy a apoyar para tomar decisiones**.    

Cuando trabajo con un sist de soporte de decisiones toma los datos y me ayuda a tomar una decision. Por otro lado un **sistema de información** _es el sistema que puedo tomar para elevorar una base que me permita respaladar mis decisiones_, si me baso en datos mi decision es menos subjetivo, no asegura el éxito pero con elementos que reflejan de forma objetiva los echos tengo un poco mas de probabilidad exiito. Abra toma de decisiones a diferentes niveles (dentro de mi organización).  Hoy en dia de las decisiones debeido a la gran concetividad depende de una buena interpretacion de los datos-informacion en mi BD 

Los sistemas de soporte de decision SSD hay de manueles y automatiados estos me proponen o guiarme para tomar una decision. Mi base de datos sirve para resolver un problema pero ademas es la que termina ayudado a mi SSD

El SSD debe ser 
- Rapido: debe ser rapido ya que si debo hacer un plan de accion este se debe poder acceder de manera rapida para una accion rapida
- Debe integrar ls areas en mi organiazcion, si pienso la organizacion como areas independdientes c/d una tendra sus necesidades.
- Debe ver las tendencias historicas de los datos

SSD Componentes
- Base de datos
- Modelo Contexto y criterio Como proceso los datos para tomar decisisones
- Interfaz: Como convienen _verlo_ para tomar decisiones

Tipos de SSD Exsiten muchos y mis SSD pasa
- Pasivo: Me da la informacion pero no toma la decision por mi, yo en base la info tomo la la decision 
-  Activas: Me da la informacion y posibles alternativas de decision
- Coperativo: Tomo la decision decido y el sistema de informacion toma mis decisiones pasadas es decir que hay un aprendizaje permaente, es mas activo ya que da sugerencias 

SSD Elementos
Basados en Documentos: mis decisiones peuden ser tomadas en base de do documentos (encuestas,ect)
Coperaticion: 

### SSD y BI
Ahora el SSD debe hacer inteligiencia del negocios, con la inteligencia del negocio quiero que el mismo sistema aprenda y descubra cuestiones del negocio que yo no puedo ver de manera clásica, cuando yo no veo ciertas cuestiones y se que algo ocurre para eso tengo mi sistemas que aprenden e intentan ver porque ocurren esa situación. Esa explicacion aveces no esta dentro de nuestros datos sino que esta fuera de nuestros horizontes por ejemplo las redes, esa informacion es externa pero afecta nuestra organizacion y por ende debere tenerla en cuenta. 
Por eso nuestro SSB ahora usara multiples api, integrara arquitecturas, ect. Los datos de mi organizacion se recopilan pero se mezclan con los datos externos BI (inteligencia de negocios buisnes).

Cuando yo armo un sistema para que me ayude a tomar decisiones debo tener claro el _porque_, el que busco (ej mejorar esquema comercial, mejorar productividad, ect). 

#### La inteligencia de Negocio BI
Es mas que solo el SSD ya que incluye mi informacion externa y debe tener un objetivo.
Exsiten distintos niveles
El mas complicado para definir decisiones es el estrategico ya que es aquel que define planes para el menajo de mi organizacion 

Info de valor Centralizada: informacion qe tiene valor y me permite definir cuales son las tendencias, pero para eso debo tener claro mi objetivo.

#### Data warehause
Los data tienen la peculiaridad de tratar de almacenar toda la informacion de valor para poder elevaorar mis decisones, lo que ocurre aqui es que de mi data warehause depende el SSD, este esquema nace como una base de datos transacciones (SQL) normalizada pero leugo a estos datos _debere desnormalizarlos_. Existen muchos aparte el warehause pero se sigue usando ya _que es un gran almacen de datos donde los voy a integrar (osea integro distittntas fuentes de datos)_ todas centralizadas en el warehause. 
Como programo mi Warehause? Sabiendo que c/d area de mi organizacion tiene su problematica y requiere distinta info tendre
DataMArt-> Warehause para un area especifica de mi organizacion
Si tomo todos los dataMart formo el warehause de mi organizacion 


Origen de fuentes: **OLTP**: Sistema transaccional onlien. Son otras fuentes de informacion 
El warehause se completa con _extraccion selectiva_ osea tomo los datos de valor que me intersan 


Mis fuentes de datos se procesan y se guardan por medio del proceso de ETL

_ETL= extraccion transofmracion y carga de datos_

Desventajas del Warehause
- Son caros de implementar por el costos de relaciar los ETL
- No manejan con datos no estructurados (osea no maneje datos asi nomas sin tablas), actualmente se manejan


Nuesta organizacion se hacen BD y los datos deben ser de calidad y transiosacioneales (que respeten las reglas del negocio). Esto porqur en los diferentes nivles de mi organizacion deben tomar decisiones y por medio del ETL (Con datos externos e internos) conformando mi almacen de datos . La calidad (Q) de los datos externos e internos, nos ayudaran a integrar la org.   
![[Pasted image 20230914152354.png]]


Con SQL puedo hacer el ETL

**Se deben evitar datos null** 

Hacer ETL con procedimientos almacenados.
