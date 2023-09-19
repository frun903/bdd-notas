Estas son las bases de datos basados en

- Clave- Valor
- Basadas en documnetos JSON
- Basadas en Grafos
- Basadas en memorias
- Optimizadas para busquedas

Responden a diferentes necesidades que nacieron de problemas en SQL.  

## Clave-valor
Echas para almacenar datos de manera rápida y soy capaz de extraer estos datos en base a una clave.  Esta guarda los datos en base a un id. No es bueno para hacer consultas complejas ya que para c/d datos tendré los datos.Manejan los diccionarios de manera excepcional. Ejemplos: DynamoDB, Cassandra.

## Basadas en documentos
Los documentos son de tipo JSON y están guardados como clave-valor pero tiene una estructura un poco mas definidas. Son mas utilizadas fuera del standar SQL. Dos grandes ejemplos son MongoDB y FireStore y nos ayudan mucho a guardar el estado actual de mi aplicación. Estos datos o estados de una aplicacion web se guardan en documentos (los Json), para que no serian buenas para las consultas.

## Basadas en Grafos
Basadas en teoría de grafos, sirven para entidades que se encuentran ínter conectadas por múltiples relaciones. Estos nodos están relacionados entre todos. Ideales para almacenar relaciones complejas como inteligencia artificial con relaciones independientes y complejas. Ejemplos: **neo4j, TITAN**.


## En Memoria
 **En memoria:** Pueden ser de estructura variada, pero su ventaja radica en la velocidad, ya que al vivir en memoria la extracción de datos es casi inmediata. Están son de memoria volátil, si yo manejo un servidor en claud si el servidor se reinicia deberé volver a cargar todo. Ejemplos: **Memcached, Redis**.

## Optimizadas para búsquedas
Pueden ser de diversas estructuras, su ventaja radica en que se pueden hacer queries-consultas y búsquedas complejas de manera sencilla. Ejemplos: BigQuery, Elasticsearch. Son muy utilizadas en buisness intelignece y machine learning.



