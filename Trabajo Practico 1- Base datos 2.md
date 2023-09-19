

### Ejercicio 1

1. Tiene muchos dominios repetidos, lo que indica que no esta normalizada.
2. Hay muchas entidades con sus respectivos atributos en una misma tabla. Lo que implica que no hay normalizacion en la tabla
3. En la tabla tengo atributos multivaluados que deberian ir en su respectiva tabla. (Opcional)

Tabla Clientes

| CodCLi | NomCli | Suc-NroFac |
|---------|--------|---------|
|1 | Alvarez |1-500|
|107| Castro | 1-501|
| 110 | Liz | 2-500| 




**Tabla Articulos**

| CodigoArt | Nom ARt | PuUART |
|------------|----------|-----------|

**Tabla Factura**

| Suc-Nfac | FecFac | ForPag  |Total Factura |
|-----------|---------|------|------------|

**Tabla Detalle**

|Suc-naf| Subtotal |




### Ejercicio 2

```sql
--Creo Base de Datos acad

Create database acad;

--Entidad ALumnp

CREATE TABLE Alumno(
    id_Alumno integer PRIMARY KEY AUTO_INCREMENT NOT null,
    Nombre varchar(150) NOT null 
);

--Entidad Carrera
CREATE TABLE Carrera(
id_Carrera integer PRIMARY KEY AUTO_INCREMENT not null,
nombre varchar(150) NOT null,
titulo varchar(150) NOT null);    
--Entidad Materia
CREATE TABLE Materia(
id_materia integer PRIMARY KEY AUTO_INCREMENT NOT null,
nombre varchar(150) not null    
);   

--entidad Parcial
CREATE TABLE Parcial(
id_Parcial integer PRIMARY KEY AUTO_INCREMENT not null,
id_Materia int NOT null,    
fecha_parcial date null,
FOREIGN KEY (id_Materia) REFERENCES Materia(id_Materia));

--Fecha de matriculacionALumnoxCarrera

--Profe es buena pratica, que tenga su propio id, si quisiera podria luego hacer un alter table y poner ambos como claves como PK 


 CREATE TABLE alumno_carrera(
alumno_carrera int PRIMARY KEY AUTO_INCREMENT NOT null,
id_Alumno int NOT null,
id_carrera int not null,    
fecha_matriculacion date NOT null,
FOREIGN KEY (id_Alumno) REFERENCES Alumno(id_Alumno),
FOREIGN KEY (id_Carrera) REFERENCES Carrera(id_Carrera)
);  


--Tabla pasarela carrera_materia

create table carrera_materia(
carrera_materia int primary key auto_increment not null,
id_Carrera int not null,
id_Materia int not null, 
año_cursada date not null,
cuatrimestre int not null,
foreign key(id_carrera) references Carrera(id_Carrera),
foreign key (id_Materia) references Materia(id_Materia)
);

--Tabla Pasarela Materia Alumno

create table alumno_materia(
alumno_materia int primary key auto_increment not null,
id_Alumno int not null,
id_Materia int not null,
fecha_insc date not null,
estado_incripcion char(50) not null,
fecha_estado date not null,
foreign key(id_Alumno) references Alumno(id_Alumno),
foreign key(id_Materia) references Materia(id_Materia)
);

--Tabla Pasarela Parciales y Alumnos

create table parcial_alumno(
parcial_alumno int primary key auto_increment not null,
id_Parcial int not null,
id_Alumno int not null,
nota int check(nota>=0 AND nota<=10),
foreign key (id_Parcial) references Parcial(id_Parcial),
foreign key (id_Alumno) references Alumno(id_Alumno)
);

--Examen final 

create table examen_final(
id_examen int primary key auto_increment not null,
id_Materia int not null,
fecha date null,
foreign key (id_Materia) references Materia(id_Materia)
);

--Examen Final y Alumnos

create table examen_alumno(
examen_alumno int primary key auto_increment not null,
id_examen int not null,
id_Alumno int not null,
fecha_insc date not null,
asistencia char(50),
nota int null check(nota>=0 AND nota<=10),
foreign key (id_examen) references examen_final(id_examen),
foreign key (id_Alumno) references Alumno(id_Alumno)
);


--examen final y carrera

create table examen_carrera(
examen_carrera int primary key auto_increment not null,
id_examen int not null,
id_Carrera int not null,
foreign key (id_examen) references examen_final(id_examen),
foreign key (id_Carrera) references Carrera(id_Carrera)
);

```

### Ejercicio 3

```sql
create table Producto(
codPro int primary key auto_increment not null,
nompro char(150) not null,
precio_unidad int,
origen char(150) check (origen in ('importado','nacional'))
);

create table Cliente(
codCli  int primary key auto_increment not null,
nombCli char(150),
provincia char(150) not null
);


create table ventas(
venta_id int primary key auto_increment not null,
codCli int not null,
codPro int not null,
fecha date,
cantidad int,
total int,
FOREIGN KEY (codCli) REFERENCES Cliente(codCli),
FOREIGN KEY (codPro) REFERENCES Producto(codPro)
);

```

Consultas:

```sql

--1

select *
from Cliente;

--2

select *
from Producto;

--3
select *
from Cliente
where provincia="Salta";

--4

select nombCli
from Cliente
where provincia="Cordoba";


--5

select *
from Producto
where precio_unidad=10;

--6

select *
from ventas
where codCli=1;

--7
select *
from ventas
where DAY(fecha)>05;

--8
select *
from Producto
order by nompro asc;

--9
select *
from ventas
where DAY(fecha)>05 and codCli=2;


--10
select *
from ventas
order by fecha desc;

--11
select count(distinct codCli) as Cantidad_CLientes
from Cliente 

--12: ¿Cuántas ventas se realizaron al cliente 3?
select count(distinct venta_id) as Cantidad_Ventas
from ventas
where codCli=3;


--13: ¿Cuántos clientes de Córdoba existen en la base?
select count(distinct codCli) as ClienteCba
from Cliente
where provincia="Cordoba";

--14
select count(*) as ventas
from ventas
group by ventas.codCli;


```



## Ejercicio 4

Creación de vistas


```sql
CREATE VIEW `V_contrato_region` AS 
SELECT id_contrato,provincias.nombre,regiones.nomreg
from contratos inner join provincias on contratos.id_provincia= provincias.id_provincia
inner join regiones on regiones.id_region=provincias.id_region;

---

CREATE VIEW V_contrato_plan_servicio AS
SELECT contratos.id_contrato,contratos.fecha, planes.descplan, servicios.descripcion
from contratos INNER JOIN planes on contratos.id_plan=planes.id_plan INNER join servicios on planes.id_servicio=servicios.id_servicio;

---
CREATE VIEW V_reclamos_ultimos_20_dias AS
SELECT registros.id_registro, contratos.id_contrato,registros.tiempo_atencion, soluciones.descripcion
FROM registros INNER join contratos on registros.id_contrato=contratos.id_contrato INNER join soluciones on soluciones.id_solucion=registros.id_solucion; 

---

CREATE VIEW V_registros_solucionados AS
SELECT registros.id_registro, registros.fecha,operadores.nombre, operadores.apellido
FROM registros INNER JOIN operadores on registros.id_operador=operadores.id_operador;



----

CREATE VIEW V_calificacion_servicio AS
SELECT registros.id_registro, registros.fecha, encuestas.descripcion
FROM registros INNER JOIN encuestas on registros.id_encuesta=encuestas.id_encuesta;

----

CREATE VIEW V_lugar AS
SELECT provincias.nombre, regiones.nomreg
FROM provincias
JOIN regiones ON provincias.id_region = regiones.id_region

----

CREATE VIEW V_planes as 
SELECT servicios.descripcion,planes.descplan, planes.costo
from servicios INNER join planes on servicios.id_servicio=planes.id_servicio;

---

CREATE VIEW V_contrato AS
SELECT contratos.id_contrato, contratos.fecha, planes.descplan, provincias.nombre, regiones.nomreg
FROM contratos INNER JOIN planes on contratos.id_plan=planes.id_plan INNER join provincias ON provincias.id_provincia=contratos.id_provincia INNER join regiones on regiones.id_region=provincias.id_region;


----

CREATE VIEW V_registros AS
SELECT registros.id_registro, registros.tiempo_atencion, soluciones.descripcion as "Desc Solucion", encuestas.descripcion as "Desc Encues", operadores.nombre, operadores.apellido
FROM registros INNER JOIN soluciones on registros.id_solucion=soluciones.id_solucion 
INNER join operadores on registros.id_operador=operadores.id_operador
INNER join encuestas on registros.id_encuesta=encuestas.id_encuesta;


----

CREATE VIEW V_contrato_reclamos AS
SELECT contratos.id_contrato, contratos.fecha, planes.descplan as "Plan", regiones.nomreg as "Region", soluciones.descripcion as "Solucion", encuestas.descripcion as "Calificacion", planes.costo as "Costo Reclamo", operadores.id_operador
FROM contratos INNER JOIN planes on contratos.id_plan=planes.id_plan
INNER JOIN provincias on contratos.id_provincia=provincias.id_provincia
INNER JOIN regiones on provincias.id_region=regiones.id_region
INNER JOIN registros on registros.id_contrato=contratos.id_contrato
INNER JOIN encuestas on encuestas.id_encuesta=registros.id_encuesta
INNER join soluciones on soluciones.id_solucion= registros.id_solucion
INNER JOIN operadores on operadores.id_operador=registros.id_operador;

----11

CREATE VIEW Cantidad_Reclamos_Region AS
SELECT regiones.nomreg as "Region", COUNT( registros.id_registro) as "Cantidad Reclamos"
FROM registros INNER JOIN contratos on registros.id_contrato= contratos.id_contrato
INNER JOIN provincias on contratos.id_provincia=provincias.id_provincia
INNER JOIN regiones on regiones.id_region=provincias.id_region
GROUP by regiones.id_region;

----12

CREATE VIEW Reclamos_Solucionados AS
SELECT COUNT(registros.id_registro) AS "Reclamos Solucionados"
FROM registros INNER JOIN soluciones on soluciones.id_solucion=registros.id_solucion
WHERE soluciones.descripcion="SI";

----13

CREATE VIEW Ranking_Operadores_solucion AS
SELECT operadores.id_operador, operadores.nombre,operadores.apellido, COUNT(registros.id_registro) AS Reclamos_Solucionados
FROM registros INNER JOIN soluciones on soluciones.id_solucion=registros.id_solucion
INNER join operadores on operadores.id_operador=registros.id_operador
WHERE soluciones.descripcion="SI"
GROUP BY operadores.id_operador
ORDER BY Reclamos_Solucionados DESC;


---14

CREATE VIEW operadores_peor_desempenio AS
SELECT operadores.id_operador, operadores.nombre,operadores.apellido, COUNT(registros.id_registro) AS Reclamos_Solucionados
FROM registros INNER JOIN soluciones on soluciones.id_solucion=registros.id_solucion
INNER join operadores on operadores.id_operador=registros.id_operador
WHERE soluciones.descripcion="SI"
GROUP BY operadores.id_operador
ORDER BY Reclamos_Solucionados ASC LIMIT 1;


---15
Create view Reclamo_no_solucionado as 
SELECT registros.fecha, COUNT(registros.id_registro) AS Reclamos_NO_Solucionados
FROM registros INNER JOIN soluciones on soluciones.id_solucion=registros.id_solucion
WHERE soluciones.descripcion="NO"
GROUP BY registros.fecha;

```

**Detalle segun nacho es _todo_ para que se entiendan
las vistas

```sql

---15
SELECT registros.id_registro, contratos.id_contrato, registros.tiempo_atencion, soluciones.descripcion as solucion, encuestas.descripcion as calificacion, operadores.nombre as nombre_operador, operadores.apellido AS apellido_operador, registros.fecha
FROM registros
JOIN contratos ON registros.id_contrato = registros.id_contrato
JOIN soluciones ON registros.id_solucion = soluciones.id_solucion
JOIN encuestas ON registros.id_encuesta = encuestas.id_encuesta
JOIN operadores ON operadores.id_operador = registros.id_operador
WHERE soluciones.descripcion = "NO"



---16

CREATE VIEW Recl_Solu_mayorAlpromedio as
SELECT DISTINCT registros.id_registro, contratos.id_contrato, registros.tiempo_atencion, soluciones.descripcion as solucion, encuestas.descripcion as calificacion, operadores.nombre as nombre_operador, operadores.apellido AS apellido_operador
FROM registros
JOIN contratos ON registros.id_contrato = registros.id_contrato
JOIN soluciones ON registros.id_solucion = soluciones.id_solucion
JOIN encuestas ON registros.id_encuesta = encuestas.id_encuesta
JOIN operadores ON operadores.id_operador = registros.id_operador
WHERE soluciones.descripcion = "SI" AND registros.tiempo_atencion>
(SELECT AVG(tiempo_atencion) FROM registros)
GROUP by registros.id_registro;



---17

CREATE VIEW v_contratos_firmados_por_mes AS
SELECT MONTH(contratos.fecha) AS mes, COUNT(*) AS contratos_firmados
FROM contratos
GROUP BY mes;



---18

CREATE VIEW soluciones_SI_Telefonia AS
SELECT registros.fecha,COUNT(registros.id_registro) as soluciones_SI_Telefonia
FROM registros INNER JOIN soluciones ON registros.id_solucion=soluciones.id_solucion
INNER JOIN contratos on contratos.id_contrato=registros.id_contrato
INNER JOIN planes on planes.id_plan=contratos.id_plan
INNER JOIN servicios on planes.id_servicio=servicios.id_servicio
WHERe soluciones.descripcion = "SI" AND  servicios.descripcion="Telefonia"
GROUP by registros.fecha;


---19

CREATE VIEW v_tiempo_de_atencion_por_operador AS SELECT operadores.nombre,operadores.apellido, SUM(registros.tiempo_atencion) AS "Tiempo_de_atencion"
FROM registros INNER JOIN operadores on operadores.id_operador=registros.id_operador
GROUP BY operadores.id_operador
HAVING Tiempo_de_atencion>100;



---20

CREATE VIEW v_costo_total AS SELECT SUM(planes.costo) AS "Costo total"
FROM registros INNER JOIN soluciones ON registros.id_solucion=soluciones.id_solucion
INNER JOIN contratos on contratos.id_contrato=registros.id_contrato
INNER JOIN planes on planes.id_plan=contratos.id_plan
INNER JOIN servicios on planes.id_servicio=servicios.id_servicio;


---21
CREATE VIEW v_costo_total_operador AS SELECT operadores.nombre,operadores.apellido, SUM(planes.costo) AS "Costo total"
FROM registros INNER JOIN soluciones ON registros.id_solucion=soluciones.id_solucion
INNER JOIN contratos on contratos.id_contrato=registros.id_contrato
INNER JOIN planes on planes.id_plan=contratos.id_plan
INNER JOIN servicios on planes.id_servicio=servicios.id_servicio
JOIN operadores ON operadores.id_operador = registros.id_operador
GROUP BY operadores.id_operador;

---22

CREATE VIEW operadores_menor_desempenio_costos as
SELECT operadores.id_operador,operadores.nombre,operadores.apellido,SUM(planes.costo) as DesempenioMenor
FROM registros INNER JOIN operadores ON operadores.id_operador=registros.id_operador
INNER JOIN contratos on contratos.id_contrato=registros.id_contrato
INNER JOIN planes on planes.id_plan=contratos.id_plan
GROUP by operadores.id_operador
ORDER BY DesempenioMenor LIMIT 1;


--23 Rehacer

SELECT regiones.nomreg as region, MONTH(registros.fecha) AS mes
FROM registros 
JOIN contratos on registros.id_contrato= contratos.id_contrato
JOIN provincias on contratos.id_provincia=provincias.id_provincia
JOIN regiones on regiones.id_region=provincias.id_region
GROUP BY regiones.id_region, mes
ORDER BY COUNT( registros.id_registro) DESC


---24
CREATE VIEW QuienGanaMasQueFlorenciaPasator AS
SELECT operadores.nombre, operadores.apellido, operadores.sueldo
FROM operadores
WHERE operadores.sueldo>(SELECT operadores.sueldo
FROM operadores
WHERE operadores.nombre="Florencia" and operadores.apellido="Pastor")
GROUP BY operadores.id_operador;

---25

CREATE VIEW v_contratos_sin_reclamo AS SELECT id_contrato
FROM contratos 
WHERE id_contrato NOT IN(SELECT contratos.id_contrato
FROM registros 
INNER JOIN contratos on contratos.id_contrato=registros.id_contrato);

---26

CREATE VIEW region_sin_reclamo AS
SELECT regiones.nomreg as Regiones
FROM regiones
WHERE regiones.id_region NOT IN (SELECT regiones.id_region FROM registros INNER JOIN contratos on registros.id_contrato=contratos.id_contrato
INNER JOIN provincias on contratos.id_provincia=provincias.id_provincia
INNER JOIN regiones on provincias.id_region=regiones.id_region
);



