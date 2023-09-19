Un trigger, en mi tabla de datos se ralizan actualiaciones (InserUpdateDelete) el propio sistema hara una accion si se hacen alguna de estas tres acciones de manera automatica para que esto ocurra y sea independiente. A c/d tabla se le anexara un pequeño scritp codigo, ese procedimiento es el que se conoce como trigger y va tener la capacidad de que si produce alguna de sesas acciones haran ese bloque de codigo SQL.  Para que esto ocurra el trigger debe estar si o si asociado a la tabla y debe ocurrir un evente previo incluyedo procedimeots almacendos qque inclyan esas 3 acciones.
Estos trigger nos permiten establecer ciertas logica de negocio. Antes de la Forengi key la relacion o regla de gestion eran echas por los triggers. Para qu elos triggers pueden fuencionar mi tabla tendra asiadas 2 tablas auxiliares new (tendra el nuevo estado actualizadp) y old (el viejo estado antes de la actualizacion), el trigger actuara en base a estas dos tablas pero por si solo no puede acceder por eso nosotros   

![[Trigger o disipador.excalidraw]]

su estrucutra es un primero asociarlo a la tabla tal despues de que se produzca a una accion. Debemos acarar que los elementos nuevos al trigger vienen de una tabla new que contiene el registro. 

Los triggers se desencadenan luego de alguna acción, la lógica nuevo de un trigger debe ser simple (código corto)

Todo el nuevo registro esta en el new y el viejo estara en el old por si quiero acceder a los datos viejos. 


Para una actualizacion de la base ded atos de cualquier tipo puedo hacer un trigger qe tiene 2 tablas auxiliares que se crean apenas esta accion del trigger se produzca. 


#### Tipica aplicacion de un trigger

En si se puede hacer cualquier control, pero _cualquier actualizacion que se produzaca dispara un trigger_, suponiendo un saldo si se esa una modificacion eso debe quedar guardado _aqui el trigger inserta un detalle en una tabla nueva cual fue el registro que se cambio_.

# Control de Flujo y cursores 

Son elementos que se se generan por control de flujo son los cursores _que hacen trataminetos registros por registros_ pero las bases de datos no estan preparados para trabajar de esa forma lo que implica una caida en el rendimiento.
Si se puede evitar el uso del cursos es mejor usar las otras herrminetas.

Este consiste en hacer una estrucutra cursor de tipo cursor, con una consulta, el resultado de esa consulta se trabajara dato por dato del resultado obtenido 

Para poder disponer de los datos debere hacer un ciclo colocar c/d dato y de ahi puedo hacer lo que quiera.

El problema es como qe las consultas pueden implicar varias tablas y sumado a la transacciones eso generara un bloqueo muy grnade de las bases de datos haciendo caer el procsacamiento de la base de datos. Por lo qe los ususarios concurretnes no podran entrar. Si hay muchas tablas se genera un bloqueo masivo! el gestor de base datos no podra llegar a sacar


Por lo general el cursor se utiliza para aplizaciones pequeñas BD o con pocas tablas.

, este tiene una peculiardid Se puede sacar del registro una como vista donde se trbajaran los datos obtendios de la consulta


No olvidar que la misma consulta conllevara un rendimiento, por ejecucion una consulta SQL tendra su propia forma. esta estructura debe tratar de ser minima (en cuanto a cantidad de tablas de que involucre) y clara para bloquear la menor cantidad de datos.  Para eso nuestras consultas deberan ser claras y funcionales (sin llegar a lo absurdo). Tambien debere tener en cuenta el indice (osea si esta organizado en base a las Primary key) si la consulta se hace en base al indice el rendimiento sera muchos mas rapido que si lo hago buscando un elemento especifico o con un where nombre like "juan" por dar un ejemplo




C/d ejercisio vamos a crear
- Base de datos
- Crear Tablas
- La logica del negocio (triggers, proc. alm., ect)
_Todo esto incluye lo que seria la logica del negocio_, estos objetos deben ser justificados. Esos problemas implican la solución completa osea todos los objetos. 



