La **consola o** [[Terminal MySQL]] es mas especifica y mas rápida (es decir que te ahorra tiempos de respuestas) ya que me conecto directamente al servidor MySQL. 

Entrar a la terminal

```
sudo mysql -u root -h localhost -p
```

```
sudo mysql -u root -p
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

