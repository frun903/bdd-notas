Tienen un fuerte soporte matemático. Relaciones estructuras matemáticas y la tablas serian los elementos o Variable o Ctes.  

# Los Sistemas de Gestión de Base de Datos (SGBD)
Los DBMS (Database Management System), son **programas de software que sirven para poder manipular una base de datos por parte de un usuario o de una aplicación informática.** Es decir que actúan como interfaz entre los mismos.

Mediante los mismos se puede manipular, extraer y configurar la información almacenada en una base de datos. Los usuarios pueden acceder a la información usando herramientas específicas de consulta y de generación de informes o bien mediante aplicaciones al efecto.

Un DBMS tiene 5 funciones principales:

1. Conceder a múltiples usuarios **acceso simultáneo** a una única base de datos.
2. Establecer y mantener **normas de seguridad y derechos de acceso** de los usuarios.
3. Hacer **respaldos de los datos** de forma habitual y recuperarlos rápidamente en caso de que se produzca una ruptura.
4. Establecer reglas y normas de bases de datos para **proteger la integridad de los datos**.
5. Proporcionar **definiciones y descripciones** de “diccionario" de los datos disponibles.

Las empresas de hoy en dia necesitan tener una vision global para el trabajo interrelaciones y junto con la facilidad que dan las redes para la intercomunicación entre ordenadores, ha conducido a **los SGBD actuales, que permiten que un programa pueda trabajar con diferentes BD como si se tratase de una sola.** Es lo que se conoce como ***base de datos distribuida***.

![[Pasted image 20230705080644.png]]

##### Razones Básicas para tener una base de Datos Distribuida:

- **Coste:** **Una BD distribuida puede reducir el coste**. En el caso de un sistema centralizado, todos los equipos usuarios, que pueden estar distribuidos por distintas y lejanas áreas geográficas, están conectados al sistema central por medio de líneas de comunicación. El coste total de las comunicaciones se puede reducir haciendo que un usuario tenga más cerca los datos que utiliza con mayor frecuencia: por ejemplo, en un ordenador de su propia oficina o, incluso, en su ordenador personal.
- **Disponibilidad:** **La disponibilidad de un sistema con una BD distribuida puede ser más alta**, porque si queda fuera de servicio uno de los sistemas, los demás seguirán funcionando. Si los datos residentes en el sistema no disponible están replicados en otro sistema, continuarán estando disponibles. En caso contrario, sólo estarán disponibles los datos de los demás sistemas.


# Arquitectura de los SGBD

### Esquemas y Niveles
Un esquema en una BD es un componente central de la arquitectura de un SGBD que permite independizar el SGBD de la BD ya que se puede cambiar el diseño de la BD (su esquema) sin tener que hacer ningún cambio en el SGBD. Hablando de un programa como tal tenemos mySQL donde se pueden hacer cambios la estructura logica del diagrama y si bien afecto la BD los datos se mantienen.

### Nivel Lógico
Por otro lado, **cuando se habla de nivel lógico se hace referencia a cómo se ocultan ciertos detalles sobre el almacenamiento y manipulación física de los datos.** Es por ello que en este nivel sólo se habla de entidades, atributos y reglas de integridad. Serian mis modelos de entidad-relacion y el de clases.

### Nivel Físico
En lo referido al rendimiento es interesante describir elementos de nivel físico como, por ejemplo, qué índices tendremos y qué características presentarán, cómo y  dónde (en qué  espacio físico)  queremos que se agrupen físicamente los registros, de qué tamaño deben ser las páginas, etc.

## Arquitectura ANSI/SPARC
De acuerdo con la arquitectura ANSI/SPARC, debe haber tres niveles de esquemas (tres niveles de abstracción). La idea básica de ANSI/SPARC consistía en descomponer el nivel lógico en dos: el nivel externo y el nivel conceptual. El  nivel interno es el denominado nivel físico.
![[Pasted image 20230705082008.png]]

- En el nivel externo se sitúan las diferentes visiones lógicas que los procesos usuarios (programas de aplicación y usuarios directos) tendrán de las partes de la BD que utilizarán. Estas visiones se denominan esquemas externos. **Serian mis diagrama de entidad-relacion**.

- En el nivel conceptual hay una sola descripción lógica básica, única y global, que denominamos esquema conceptual, y que sirve de referencia para el resto de los esquemas. **Los que serian los Diagrama de Clases**.
- En el nivel físico hay una sola descripción física, que denominamos esquema interno.**Son los Squemas de programas como SQL.**

<div style="border: 1px solid #000000; padding: 10px; background-color: #2F4F4F;"> Una idea muy interesante en un múltiple choice es si en el **diagrama Clases** es que elementos tiene:
<li> Entidades
<li>Atributos
<li>Relaciones
<li> Reglas de integridad o restricciones
</div>

----------------
# Modelos de Base de Datos
Una BD es una representación de la realidad de lo que nos interesa en nuestros sistemas de información (SI). Dicho de otro modo, una BD se puede considerar un modelo de la realidad. El componente fundamental utilizado para modelar en un SGBD relacional son las tablas. Sin embargo, en otros tipos de SGBD se utilizan otros componentes.

El conjunto de componentes o herramientas conceptuales que un SGBD proporciona para modelar recibe el nombre de modelo de BD. Los cuatro modelos de BD más utilizados en los SI son: 
- Modelo Relacional
- Modelo Jerarquico
- Modelo en Red
- Modelo Relacional con Objetos
![[Pasted image 20230707073140.png]]
Todo modelo de BD nos **proporciona 3 tipos de Herramientas**:

1. Estructuras de datos con las que se puede construir la BD: tablas, árboles, etc. Seria todo lo lógico que hay detrás del sistema gestor de base de datos, esto no lo vemos ni manipulamos nosotros pero lo hace el sistema gestor.
2. Diferentes tipos de restricciones (o reglas) de integridad que el SGBD tendrá que hacer cumplir a los datos: dominios, claves, etc.
3. Una serie de operaciones para trabajar con los datos. Un ejemplo de ello, en el modelo relacional, es la operación SELECT, que sirve para seleccionar (o leer) las filas y columnas que cumplen alguna condición. Un ejemplo de operación típica del modelo jerárquico y del modelo en red podría ser la que nos dice si un determinado registro tiene “hijos” o no.



# Lenguajes y Usuarios
Para poder comunicarse con el SGBD un usuario (ya sea un programa de aplicación o un usuario directo) debe utilizar un lenguaje determinado.Hay muchos lenguajes diferentes según el tipo de usuarios para los que están pensados.

Hay lenguajes especializados en la escritura de esquemas; es decir, en la descripción de la BD. Se conocen genéricamente como **DDL o data definition language**.
Se conocen como **DML o data management language (lenguaje de manipulación de datos)**. Sin embargo, lo más frecuente es que el mismo lenguaje disponga de construcciones para las dos funciones ya sean DDL y DML.

El lenguaje SQL, que es el más utilizado en las BD relacionales, tiene verbos (instrucciones) de tres tipos diferentes:

  - **Verbos del tipo DML** por ejemplo, SELECT para hacer consultas, e INSERT, UPDATE y DELETE para trabajar sobre la actualización de los datos.

  - **Verbos del tipo DDL** por ejemplo, CREATE TABLE para definir las tablas, sus columnas y las restricciones.
  
 - Además, SQL tiene verbos de control del entorno, como por ejemplo COMMIT y ROLLBACK para delimitar transacciones.


# Administración de BD
Por lo general en las organizaciones existe un tipo específico de usuario de BD que se encarga de llevar a cabo una serie de funciones centralizadas de gestión y administración para asegurar que la explotación (uso) de la BD sea la correcta. Ese usuario es denominado generalmente Administrador de BD y sus tareas típicas son:

- Mantenimiento, administración y control de los esquemas. Comunicación de los cambios a los usuarios.

- Asegurar la máxima disponibilidad de los datos; por ejemplo, haciendo copias (back-ups), administrando diarios (journals o logs), reconstruyendo la BD, etc.

- Resolución de emergencias.

- Vigilancia de la integridad y de la calidad de los datos.

- Diseño físico, estrategia de caminos de acceso y reestructuraciones.

- Control del rendimiento y decisiones relativas a las modificaciones en los esquemas y/o en los parámetros del SGBD y del SO, para mejorarlo.

- Normativa y asesoramiento a los programadores y a los usuarios finales sobre la utilización de la BD.

- Control y administración de la seguridad: autorizaciones, restricciones, etc.


## Normalizacion 

Es llevar a la base datos a una estrucutra mas estable correcta que mantenga la integridad de los datos. Buscamos evitar la redundancia(datos no repetidos) y ambigüedad (que haya datos que identifiquen cada elemento) de los datos. Para normalizar llevo a las **formas normales** 
