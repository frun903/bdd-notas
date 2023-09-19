## Modelo Relacional

El modelo relacional es un modelo de datos y es por ello que debe cumplir con 3 características fundamentales sobre los datos:
-  **La estructura**: que debe permitir representar la información que nos interesa del mundo real.
- **La manipulación:** para que permita operaciones de actualización y consulta sobre los datos.
- **La integridad:** que es facilitada mediante el establecimiento de reglas de integridad o condiciones que los datos deben cumplir si o si según el contexto que se está modelando con la base de datos.

Los principios del modelo de datos relacional fueron establecidos por E.F. Codd, estos principios tienen el objetivo el percebimiento de la estructura de mi base de datos, la idea de que mi BD este bien _normalizada_ es conseguir un alto grado de independencia entre los datos y  con ese propósito, todos los valores de datos se consideran atómicos (no es posible descomponerlos).

### Estructura de datos:

La estructura de los datos del modelo relacional se basa en el concepto de _relación_.

El nombre de la **entidad**(o relacion R) en la relación puede ser descripto de la siguiente forma:  

ALUMNO(nombre,apellido,legajo,dni)

Que el esquema de la relación está conformado por el nombre de la **entidad**(relación) y el conjunto de atributos que la describen. Y su **grado** es la cantidad de atributos en este caso seria de grado 4.


### Primary Key, Clave candidata, clave primaria y clave alternativa de las relaciones

Toda tupla (registro) que contiene una base de datos relacional _debe poderse identificar de manera única_, es por esto que es _necesario definir los distintos conceptos de claves_ que pueden existir en una relación. 

También podemos garantizar que toda entidad tiene como mínimo una clave candidata.
Por ejemplo: En la relación de esquema EMPLEADOS(DNI, LEG, nombre, apellido, teléfono), sólo hay dos claves candidatas: {DNI} y {LEG}.

Por lo general una de las claves candidatas de una relación se designa como clave primaria de la relación. La clave primaria es la clave candidata cuyos valores se utilizarán para identificar las tuplas de la relación. Las claves candidatas no elegidas como primaria se denominan claves alternativas. Es responsabilidad del diseñador de la base de datos el seleccionar a la clave primaria de entre las claves candidatas.

Es posible que una clave candidata o _una clave primaria este conformada por más de un atributo_. Por ejemplo, el esquema SUCURSAL(edificio, número, superficie), la clave primaria está formada por los atributos edificio y número. En este caso, podrá ocurrir que dos sucursales diferentes estén en el mismo edificio, o bien que tengan el mismo número, pero nunca pasará que tengan la misma combinación de valores para edificio y número. Se aclara aquí que los atributos edificio y número están subrayados porque son considerados parte de la clave primaria.

### Claves Foráneas 
Las bases de datos relacionales proporcionan un mecanismo que permite _conectar tuplas de tablas diferentes y dicho mecanismo es lo que se denomina como clave foránea_ de las relaciones.

Para hacer la conexión, una clave foránea tiene el conjunto de atributos de una relación que referencian la clave primaria de otra relación. Incluso puede ser necesario reflejar lazos entre tuplas que pertenecen a una misma relación, tienen por objetivo establecer una conexión con la clave primaria que referencian.

## Operaciones del modelo:
Las operaciones del modelo relacional deben permitir manipular datos almacenados en una base de datos relacional. La manipulación de datos hace referencia a dos aspectos centrales: la actualización y la consulta.

La _actualización de los datos_ consiste en hacer que los cambios que se producen en la realidad queden reflejados en las relaciones de la base de datos. Existen tres operaciones básicas de actualización:

- 1 _Inserción_, que sirve para añadir una o más tuplas a una relación.
- 2 _Borrado_, que sirve para eliminar una o más tuplas de una relación.
- 3 _Modificación_, que sirve para alterar los valores que tienen una o más tuplas de una relación para uno o más de sus atributos.

La _consulta de los datos_ consiste en la obtención de datos deducibles a partir de las relaciones que contiene la base de datos. La obtención de los datos que responden a una consulta puede requerir el análisis y la extracción de datos de una o más de las relaciones que mantiene la base de datos.


## Reglas de Integridad / Restricciones 

En las bases de datos relacionales la extensión de las relaciones (es decir, las tuplas que contienen las relaciones) deben tener valores que reflejen la realidad correctamente. Denominamos _integridad o restricciones la propiedad de los datos de corresponder a representaciones posibles (correctas) del mundo real_. Por ejemplo que en sueldos no haya valores negativos o que Primary Key sea unica!

Existen en si 2 tipos de restricciones o integridad **las restricciones o integridad del modelo** y **la restricciones de integridad de usuarios** estas ultimas son las menos importante aquellas que deben cumplir en una base de datos particular con unos usuarios concretos, pero que no son necesariamente relevantes en otra base de datos. Por otro lado las **restricciones de integridad** del modelo son  propias de un modelo de datos, y se deben cumplir en toda base de datos que siga dicho modelo.

- **Regla de integridad de unicidad de la clave primaria**: Establece que toda clave primaria que se elija para una relación no debe tener valores repetidos.
- **Regla de integridad de entidad de la clave primaria**: La regla de integridad de entidad de la clave primaria dispone que los atributos de la clave primaria de una relación no puedan tener valores nulos
- **Regla de integridad referencial**: Esta regla está relacionada con el concepto de clave foránea. Concretamente, determina que todos los valores que toma una clave foránea deben ser valores nulos o valores que existen en la clave primaria a la que hace referencia.










