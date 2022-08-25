# SQL-Practico-Platzi

# Introduccion a SQL
## Clase 1: Breve historia de SQL

SQL (Structured Query Language - Lenguaje de consulta estructurada) es un lenguaje que se basó en 2 principios fundamentales:

Teoría de conjuntos.
Álgebra relacional de Edgar Codd (científico informático inglés).
⠀
SQL fue creada en 1974 por IBM.
Originalmente fue llamado SEQUEL, posteriormente se cambió el nombre por problemas de derechos de autor.
⠀⠀⠀⠀⠀⠀⠀⠀⠀
Relation Company (actualmente con el nombre Oracle) creó el software Oracle V2 en 1979.

Mas adelante SQL se convertiría en un lenguaje estándar que unifica todo dentro de las bases de datos relacionales, se convierte en una norma ANSI o ISO.

## Clase 2: Álgebra relacional

El álgebra relacional estudia basicamente las operaciones que se pueden realizar entre diversos conjuntos de datos.
⠀
No confundir las relaciones del álgebra relacional con las relaciones de una base de datos relacional.

Las relaciones de una base de datos es cuando unes dos tablas.
Las relaciones en álgebra relacional se refiere a una tabla.
- La diferencia es conceptual: Las tablas pueden tener tuplas repetidas pero en el álgebra relacional cada relación no tiene un cuerpo, no tiene un primer ni último row.
⠀
Tipos de operadores
Operadores unarios.- Requiere una relación o tabla para funcionar.
- Proyección (π): Equivale al comando Select. Saca un número de columnas o atributos sin necesidad de hacer una unión con una segunda tabla.
π<Nombre, Apellido, Email>(Tabla_Alumno)
⠀
- Selección (σ): Equivale al comando Where. Consiste en el filtrado de de tuplas.
σ<Suscripción=Expert>(Tabla_Alumno)
⠀
Operadores binarios.- Requiere más de una tabla para operar.
- Producto cartesiano (x): Toma todos los elementos de una tabla y los combina con los elementos de la segunda tabla.
Docentes_Quinto_A x Alumnos_Quinto_A
⠀
- Unión (∪): Obtiene los elementos que existen en una de las tablas o en la otra de las tablas.
Alumnos_Quinto_A x Alumnos_Quinto_B
⠀
- Diferencia (-): Obtiene los elementos que existe en una de las tablas pero que no corresponde a la otra tabla.
Alumnos_planExpertPlus - Alumnos_planFree

## Clase 3: Instalacion Postgre

## Clase 4: Qué es una proyección (SELECT)

Proyección significa elegir QUE columnas (o expresiones) la consulta debe retornar.
Selección significa CUALES SON las columnas retornadas.

Supongamos la siguiente consulta:  

select *Columna1, Columna2, Columna3* from *table_1* where ***n=3***;
La proyección sería: Columna1, Columna2 y Columna3 y la parte de Selección es n*=3,* debido a esto es que denominamos a la clausula SELECT como la proyección y los filtros aplicados en la clausula WHERE como Selección.

La Consulta más Básica sería

SELECT * FROM Tabla1;
El astrisco (*) es un comodín que indica que queremos traer una proyección completa (Todos los campos o columnas existentes en la tabla)

Alias
Un alias (..AS…), es otra forma de llamar a una tabla o a una columna, y se utiliza para simplificar las sentencias SQL cuando los nombres de tablas o columnas son largos o complicados.

SELECT Columna1 AS Alias1 FROM Tabla1; (Para referirnos a la Columna1, podremos denominarla como Alias1)

…
.

Funciones de agregación
Las funciones de agregación en SQL nos permiten efectuar operaciones sobre un conjunto de resultados, pero devolviendo un único valor agregado para todos ellos.
Es decir, nos permiten obtener medias, máximos, etc… sobre un conjunto de valores.

Las funciones de agregación básicas que soportan todos los gestores de datos son las siguientes:

COUNT: devuelve el número total de filas seleccionadas por la consulta, como particularidad se puede usar COUNT()* donde contará todos los registros de la tabla incluyendo nulos.
MIN: devuelve el valor mínimo del campo que especifiquemos.
MAX: devuelve el valor máximo del campo que especifiquemos.
SUM: suma los valores del campo que especifiquemos. Sólo se puede utilizar en columnas numéricas.
AVG: devuelve el valor promedio del campo que especifiquemos. Sólo se puede utilizar en columnas numéricas.
.
Función IF()
Función que evalúa una sola expresión y retorna lo que se le especifica en el caso que sea Verdadera o Falsa

IF (expresion, resultado_true, resultado_else)
Función CASE()
Sirve para evaluar una lista de condiciones y retornar uno o múltiples posibles resultados.

Comienza con la sentencia CASE, luego evalua expresiones comenzando con WHEN y en caso que sea verdadera, devolverá el resultado especificado para esa condición luego del THEN…
.
La sentencia ELSE es opcional y devolverá este valor en caso que todas las condiciones WHEN anteriores sean Falsas.

Si todas las condiciones son falsas, y no existe la clausula ELSE, se devolverá NULL.

CASE 
   WHEN eval_1 THEN resultado_1
   WHEN eval_2 THEN resultado_2
      ...
   WHEN value_n THEN resultado_n
   ELSE resultado
END AS ValorResultado;


## Clase 5: Origen(From)

Con SELECT se especifica que columnas queremos obtener de una tabla determinada y con FROM se indica de donde se va a obtener la información que se va proyectar con SELECT. FROM va después de SELECT

SELECT *
FROM base_de_datos.tabla
En la sentencia anterior el manejador de base de datos (DBMS) va al esquema y proyecta lo solicitado.


Las sentencias SQL no son sensibles a minúsculas o mayúsculas pero se recomienda escribir las palabras claves en MAÝUSCULAS y el resto en minúsculas

JOIN es un complemento de FROM.

También se puede obtener la información de una base de datos remota, es decir que el esquema de donde que queremos obtener información se encuentra en otro DBMS.


Para obtener información de una tabla que se encuentra remotamente se utiliza la función dblink, dicha función recibe dos parámetros:

configuración de conexión al DBMS remoto
consulta SQL

Ejemplo de dblink

```
SELECT *
FROM dblink('
	dbname=somedb
	port=5432 host=someserver
	user=someuser
	password=somepwd',
	'SELECT gid, area, parimeter,
			state, country,
			tract, blockgroup,
			block, the_geom
	FROM massgis.cens2000blocks')
	AS blockgroups
 
``` 
Cúales son las TOP 10 carreras con mayor cantidad de alumnos que aun siguen vigentes?  

```
SELECT carrera, count(alumnos.id) as "Total de alumnos", vigente
FROM platzi.alumnos 
	JOIN platzi.carreras as carr
		ON platzi.alumnos.carrera_id = carr.id
WHERE vigente = true
GROUP BY carrera, vigente
ORDER BY "Total de alumnos" DESC

```
## Clase 6: Productos cartesianos (JOIN)
El producto cartesiano es una operación de la teoría de conjuntos en la que dos o más conjuntos se combinan entre sí. En el modelo de base de datos relacional se utiliza el producto cartesiano para interconectar conjuntos de tuplas en la forma de una tabla. El resultado de esta operación es otro conjunto de tuplas ordenadas, donde cada tupla está compuesta por un elemento de cada conjunto inicial.

![image](https://user-images.githubusercontent.com/90301902/185771146-e53ebc36-a354-4fe7-b59a-cd65f1a0f0d6.png)

![image](https://user-images.githubusercontent.com/90301902/185771190-9162f24a-3f2e-4c72-9c76-0d4132e070d7.png)

## Clase 7 Seleccion(Where)

![image](https://user-images.githubusercontent.com/90301902/185812399-850eb727-d136-4449-9c41-b1327ab84e71.png)


WHERE es usado para filtrar registros.
WHERE es cuando para extraer solamente las condiciones que cumplen con esa condición.

```
SELECT *
FROM tabla_diaria
WHERE id=1;

SELECT *
FROM tabla_diaria
WHERE cantidad>10;

SELECT *
FROM tabla_diaria
WHERE cantidad<100;

```
Este puede ser combinado con AND ,OR y NOT.

AND y OR son usados para filtrar registros de más de una condición.

AND muestra un registro si todas las condiciones separadas por AND son TRUE.
OR muestra un registro si alguna de las condiciones separadas por OR son TRUE.

```
SELECT *
FROM tabla_diaria
WHERE cantidad > 10
	AND cantidad < 100;

SELECT *
FROM tabla_diaria
WHERE cantidad BETWEEN 10
	AND cantidad < 100;
BETWEEN puede ser usado para definir límites.
```


La separación por paréntesis es muy importante.

```
SELECT * 
FROM users
WHERE name = "Israel"
	AND (
	lastname = "Vázquez"
	OR
	lastname = "López"
);
```
```
SELECT * 
FROM users
WHERE name = "Israel"
	AND 
	lastname = "Vázquez"
	OR
	lastname = "López";
```
En el primero va a devolver todos los que son Israel Vázquez o Israel López, en el segundo devolverá a todos los que se llaman Israel Vázquez o se apellida López(sólo apellido).

NOT valida que un dato no sea TRUE.
```
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
Para especificar patrones en una columna usamos LIKE. Podemos mostrar diferentes cosas que buscamos.
```
```
SELECT *
FROM users
WHERE name LIKE "Is%";
```
```
SELECT *
FROM users
WHERE name LIKE "Is_ael";
```
```
SELECT *
FROM users
WHERE name NOT LIKE "Is_ael";
```
En el primero, colocamos un % para representar que se va a buscar lo que tenga is% pero que no importa los carácteres después de %.
En el segundo le estamos diciendo con _ que puede haber lo que sea en medio de Is y ael.
Y en el último le decimos que ponga todas las filas que no sean igual a lo que arriba estabamos buscando.
Igual podemos decir que nos traiga registros que estén vacíos o que no lo estén.


```
SELECT * 
FROM users
WHERE name IS NULL;
```
```
SELECT *
FROM users
WHERE name IS NOT NULL;
Y para seleccionar filas con datos específicos, usamos IN.
```
```
SELECT *
FROM users
WHERE name IN ('Israel','Laura','Luis');


```

Ejemplo comentarios
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE colegiatura = 2000;
```
Mayor que
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE colegiatura > 2000;
```
Menor que
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE colegiatura < 4000;

```
Entre
Opcion No. 1
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE colegiatura > 2000
   AND colegiatura < 4000;
```
Entre
Opcion No. 2
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE colegiatura BETWEEN 2000 AND 4000;

```
And y Or
Opcion No. 1
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre = 'Edin'
   AND (apellido = 'Keatch' OR apellido = 'Grishin');
```
And y Or
Opcion No. 2
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre = 'Edin'
   AND apellido = 'Keatch' 
    OR apellido = 'Grishin';
```
Comodin que empieza con Al
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre LIKE 'Al%';
```
Comodin que termina con Al
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre LIKE '%er';
```
Comodin que contiene con er
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre LIKE '%er%';
```
Comodin que no contiene con er
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre NOT LIKE '%er%';
 ```
Comodin que varia la ultima letra
```
SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre LIKE 'Ali_'
```
Null
```
  SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE apellido IS NULL;
```
Not Null
```
  SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE apellido IS NOT NULL;
```
In
```
 SELECT *
  FROM PRUEBAS.ALUMNOS
 WHERE nombre IN ('Wanda', 'Hilde', 'Veriee');
```
## CLASE 8: Ordenamiento (ORDER BY)
Las partes más importantes y más usadas de los queries de SQL son:
-La proyección (con SELECT)
-El origen de los datos (Con FROM y sus JOIN)
-WHERE, que nos ayuda a filtrar las tuplas dependiendo de las condiciones que se cumplan.

Hay otras partes de los queries que son opcionales, que no encontraremos en todos los queries pero que igual juegan un papel muy importantes. Una de ellas es el ordenamiento (ORDER BY)

INDICES:
excelentes para búsquedas y ordenamientos, cuando estamos haciendo queries complejos, cuando estamos tratando de extraer información constantemente, haciendo joins complicados a través de esos campos y nos sirve tener un índice que guarde el orden de ese campo en particular para hacer extracciones muy rápidas.

CUIDAR PARA ALTA TRANSACCIONALIDAD:
la contrapartida de esto es que cuando tienes un índice, la parte de escritura, cuando llegas a hacer un Update de esa columna en particular en la que pusiste un índice, entonces cada escritura tardará un poco más que la anterior porque básicamente agrega un nuevo elemento, revisa todos elementos anteriores y posteriores y vuelve a ordenarlos en el índice y si agregamos otro elemento vuelve a revisar todos y a ordenarlos.

No debemos poner índices por poner sino ser muy cuidadosos de aquellas columnas que hacemos JOIN muy seguido o que tienen muchísimos datos y siempre estamos ordenando por esa columna. Es válido ponerlo pero siempre teniendo en cuenta que cuando tenemos que ingerir muchos datos o hacer muchos INSERT no es bueno tener índices en esas tablas (o tener la menor cantidad de índices posible). Es importante tener en mente cuántas lecturas Vs cuántas escrituras haces en una tabla o en un campo particular para decidir si conviene poner un índice. Si vas a hacer muchas escrituras por segundo no es conveniente poner índices o tratar de mantenerlo al mínimo. En cambio, si realmente escribes muy poco en esa tabla pero haces búsquedas constantes y muchos Joins sobre esa tabla entonces vale la pena usar el índice sobre ese campo en particular donde haces el Join o donde haces el ordenamiento porque es un gran complemento para nuestra cláusula ORDER BY.

Ordenamiento
```
 SELECT *
  FROM PRUEBAS.ALUMNOS
ORDER BY fecha_incorporacion;
```
Ascendente
```
 SELECT *
  FROM PRUEBAS.ALUMNOS
ORDER BY fecha_incorporacion ASC;
```
Descendente
```
 SELECT *
  FROM PRUEBAS.ALUMNOS
ORDER BY fecha_incorporacion DESC;
```
## CLASE 9: Agregación y limitantes (GROUP BY y LIMIT)
GROUP BY es una sentencia que agrupa filas que tienen el mismo valor en columnas con el sumatorio. Como decirle ‘encuentra el número de clientes en cada país’.

Suele usarse frecuentemente con las funciones COUNT MAX MIN MAX SUM AVG a un grupo de una o más columnas.
```
SELECT *
FROM tabla_diaria
GROUP BY marca;
```
```
SELECT *
FROM tabla_diaria;
GROUP BY marca, modelo;
```
```
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
ORDER BY COUNT(CustomerID) DESC;
SELECT TOP es usada para especificar el número de registros que se van a retornar.
```
SELECT TOP es útil para tablas con miles de registros, pues un gran número de registros puede afectar el rendimiento.
```
SELECT TOP number
FROM table1
WHERE condition;
```
```
SELECT TOP 1500
FROM tabla_diaria;
```
# Ejercitando tu SQL
## CLASE 10: El Primero
```
SELECT *
--el row que queremos medir y la totalidad de row en un grupo, 
--si no seleccionas nada es toda la tabla
from (
	--window function row_number
	--trae el numero de registro indiferente al criterio que seponga
	-- over si no pone nada trae toda la tabla es agnostico
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
;
```
Reto: Primeros 5 registros de la tabla
1.
```
SELECT *

from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
limit 5;
```


2.
```
SELECT *
from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
where row_id <= 5;
```

3.
```
SELECT * from platzi.alumnos
limit 5;
```

4.
```
SELECT * from platzi.alumnos
where id <= 5;
```
5.
```
SELECT * from platzi.alumnos
fetch first 5 rows only;
```
6.
```
SELECT *
from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
fetch first 5 row_id only;
```
7.
```
SELECT * from platzi.alumnos
where id BETWEEN 1 and 5;
```
8.
```
SELECT *
from (
	select row_number() over() as row_id, *
	from platzi.alumnos
) as alumnos_with_row_num
where row_id BETWEEN 1 and 5;
```
9.
```
SELECT * from platzi.alumnos
where 1<= id and id <=5;
```
## Clase 11 el segundo mas alto
Ejemplo 1
```
-- distinct trae los valores no repetidos
SELECT DISTINCT colegiatura 
from platzi.alumnos as al
where 2 =(
	select count(distinct colegiatura)
	from platzi.alumnos as al2
	where al.colegiatura<=al2.colegiatura
);
```
```
select distinct colegiatura,tutor_id
from platzi.alumnos 
where tutor_id=20
order by colegiatura DESC
limit 1 offset 1;
```
```
select * 
from platzi.alumnos as datos
inner join 
(
	select distinct colegiatura
	from platzi.alumnos
	where tutor_id=20
	order by colegiatura desc
	limit 1 offset 1
) as segunda_mayor_colegiatura
on datos.colegiatura = segunda_mayor_colegiatura.colegiatura;
```
```
select *
from platzi.alumnos as datos
where colegiatura=(
	select distinct colegiatura
	from platzi.alumnos
	where tutor_id=20
	order by colegiatura DESC
	limit 1 offset 1
)
```
Reto la segundo mitad de la tabla
```
SELECT * 
FROM platzi.alumnos 
OFFSET ( SELECT (COUNT(id)/2) FROM platzi.alumnos )

```
```
SELECT 	*
FROM	platzi.alumnos
WHERE 	id > 500
;

```
```
SELECT * FROM platzi.alumnos 
LIMIT 500 OFFSET 500;
```
```
SELECT *
FROM platzi.alumnos
OFFSET 499;
```

## Clase 12 Seleccion de un set de opciones
Ejemplos:
```
select *
from(
	select row_number() over() as row_id,*
	from platzi.alumnos
)as alumnos_with_row_num
-- valores separados por coma recibe in para que los traiga especificamente
where row_id in (1,5,10,12,15,20);

```
```
select *
from platzi.alumnos
where id in(
	select id
	from platzi.alumnos
	where tutor_id=30	
);
```
Retos
```
select *
from platzi.alumnos
where id not in(
	select id
	from platzi.alumnos
	where tutor_id=30	
);

```
```
select *
from platzi.alumnos
where id in(
	select id
	from platzi.alumnos
	where tutor_id!=30	
);
```
```

select *
from platzi.alumnos
where id in(
	select id
	from platzi.alumnos
	where tutor_id<>30	
);
```

## Clase 13 En mis tiempos
Ejemplos:
```
select extract( year from fecha_incorporacion) as years
from platzi.alumnos;
```
```
select date_part('Year',fecha_incorporacion) as years
from platzi.alumnos;
```
```
select date_part('Year',fecha_incorporacion) as years,
date_part('Month',fecha_incorporacion) as months,
date_part('Days',fecha_incorporacion) as days
from platzi.alumnos;
```
Reto Hora Minuto Segundo
```
select date_part('Hour',fecha_incorporacion) as Hours,
date_part('Minute',fecha_incorporacion) as Minutes,
date_part('Second',fecha_incorporacion) as Seconds
from platzi.alumnos;
```

```
SELECT
extract( HOUR from fecha_incorporacion) as hora,
extract( MINUTE from fecha_incorporacion) as minutos,
extract( SECOND from fecha_incorporacion) as segundos
from platzi.alumnos;
```
## Clase 14 seleccionar por año
Ejemplos
```
select *
from platzi.alumnos
where (
extract(year from fecha_incorporacion))=2019;
```

```
select *
from platzi.alumnos
where ( Date_part('Year',fecha_incorporacion))=2019;
```
```

select *
from (
	select*, 
	  	Date_part('Year',fecha_incorporacion) as years
	from platzi.alumnos
)as alumnos_year
where years=2020
```
Reto
```
select *
from platzi.alumnos
where 
extract(year from fecha_incorporacion)=2018
and
extract(month from fecha_incorporacion)=05;
```
```
SELECT *
FROM (
	SELECT *,
		DATE_PART('YEAR', fecha_incorporacion) AS annio_inicio,
		DATE_PART('MONTH', fecha_incorporacion) AS mes_inicio
	FROM platzi.alumnos
) AS year_alumnos
WHERE annio_inicio = 2018 AND mes_inicio = 5;
```

Ejercicios interesantes
1 Dia 2018

```
--el día de la semana en donde 
--mas se registraron personas para el 2018
SELECT 
case
	when extract(DOW from fecha_incorporacion) = 0 then'domingo'
	when extract(DOW from fecha_incorporacion) = 1 then'lunes'
	when extract(DOW from fecha_incorporacion) = 2 then'martes'
	when extract(DOW from fecha_incorporacion) = 3 then'miercoles'
	when extract(DOW from fecha_incorporacion) = 4 then'jueves'
	when extract(DOW from fecha_incorporacion) = 5 then'viernes'
	when extract(DOW from fecha_incorporacion) = 6 then'sabado'
end dia_semana,
COUNT(*) AS total 
from platzi.alumnos
where extract(YEAR from fecha_incorporacion) = 2018
group by dia_semana
order by total desc;
```

2 dia 2018
```
--el día de la semana en donde 
--mas se registraron personas para el 2018
select extract(dow from fecha_incorporacion) as dia,
count(extract(dow from fecha_incorporacion)) as conteo
FROM platzi.alumnos
where extract(YEAR from fecha_incorporacion) = 2018
GROUP by dia 
order by conteo desc;

```
3 mes 2018
```
--el mes en donde mas se registraron personas para el 2018

select extract(month from fecha_incorporacion) as mes,
count(extract(month from fecha_incorporacion)) as conteo
FROM platzi.alumnos
GROUP by mes 
order by mes asc;
```



## Clase 15 Duplicados
En el ejercicio de encontrar un DUPLICADO, se está haciendo una partición de la tabla de todos los valores en filas de cada una de las variables o columnas de la tabla a excepción del id, el cual no puede ser igual y no se repite al ser una primary key.

Cuando se aplica esta partición en cada valor se hace único los valores, es decir, cada row viene siendo una partición, y al utilizar la función agregada que en este caso es ROW_NUMBER() va a contar los valores de cada una de las particiones haciendo que se reinicie los “números de fila” cuando salta de partición en partición. por eso, cuando encuentra dos valores iguales, enumera los dos valores, dejando como valor en su row un 2.
Ejemplos
```
--seleccionar aquel registro que esta repetido
select *
from platzi.alumnos as ou
where(
	select count(*)
	from platzi.alumnos as inr
	where ou.id=inr.id
)>1;
```


```
--select a todos los campos de la tabla
-- :: es hacer un case, que convierte todos
--los campos en un texto
select(platzi.alumnos.*)::text, count(*)
from platzi.alumnos
group by platzi.alumnos.*
HAVING count(*)>1
```

```
select(platzi.alumnos.nombre,
	   platzi.alumnos.apellido,
	   platzi.alumnos.email,
	   platzi.alumnos.colegiatura,
	   platzi.alumnos.fecha_incorporacion
	  )::text, count(*)
from platzi.alumnos
group by platzi.alumnos.nombre,platzi.alumnos.apellido,
platzi.alumnos.email, platzi.alumnos.colegiatura,
platzi.alumnos.fecha_incorporacion
HAVING count(*)>1

```

```
select*
from(       
	select id,
	row_number() over(
		partition by
			nombre,
			apellido,
			email,
			colegiatura,
			fecha_incorporacion,
			carrera_id,
			tutor_id
		order by id asc
	)AS row,
	* 
	from platzi.alumnos
)AS duplicados
where duplicados.row > 1
```
Reto
```
DELETE FROM platzi.alumnos
WHERE id IN (
SELECT id
FROM (
	SELECT id,
	ROW_NUMBER() over(
		PARTITION BY
			nombre,
			apellido,
			email,
			colegiatura,
			fecha_incorporacion,
			carrera_id,
			tutor_id
		ORDER BY id ASC
	)AS row
	
	FROM platzi.alumnos
)AS duplicados
WHERE duplicados.row > 1)
```
```
```
```
```

## Clase 16 selectores de rango

Los tipos de rango que vienen en PostgreSQL son:

int4range: Que trae un rango de enteros.

int8range: Es un rango de enteros grandes.

numrange: Es un rango numérico.

tsrange: Es un rango del tipo timestamp pero sin la zona horaria.

tstzrange: Es un rango del tipo timestamp con la zona horaria

daterange: Es un rango del tipo fecha.


=>’ Significa ‘devuelve’ en este apunte

‘&&’ si se solapan estos dos rangos

(11.1,22.2) && (20.0,30.0) => TRUE
(11.1,19.9) && (20.0,30.0) => FALSE


* Para seleccionar los elementos de la interseccion entre los dos rangos. SI no hay, marca error

```
SELECT int4ramge(10,20) * int4range(15,25) =>[15,20)
```
@> si el rango se encuentra en el grupo

```
SELECT *
FROM platzi.alumnos
WHERE int4range(10,20) @> tutor_id
```

```
select *
from platzi.alumnos
where tutor_id in (1,2,3,4);
```

```
select *
from platzi.alumnos
where tutor_id >=1
and tutor_id <=10;
```
```
--genera enteros de 10 a 20 @> si pertenece o no
select int4range(10,20)@>3;
```
```
-- genera un rango numerico mas amplio que incluye decimales
select numrange(11.1,22.2) && numrange(20.0,30.0);
```
```
-- upper valor mas alto del rango
select upper (int8range(15,25));
```
```
-- lower valor mas bajo del rango
select lower (int8range(15,25));
```
```
--* nos devuelve la interseccion entre ambos rangos

select int4range(10,20)* int4range(15,25);
```
```
--nos comprueba si el conjunto es vacio o no
select isempty (numrange(1,5));

```
```
select * 
from platzi.alumnos
where int4range(10,20) @> tutor_id;
```
reto
```

select 
int8range(min(tutor_id),max(tutor_id))
* int8range(min(carrera_id),max(carrera_id))
from platzi.alumnos
```
## Clase 17 Eres lo maximo
Ejemplos:
```
select fecha_incorporacion
from platzi.alumnos
order by fecha_incorporacion desc
limit 1;
```

```
--trae todos los registros por que la fecha tiene hasta segundos 
--haciendo diferente cada dato
select carrera_id, fecha_incorporacion
from platzi.alumnos
group by carrera_id, fecha_incorporacion
order by fecha_incorporacion desc;
```
```
--limit  permite solamente de la tabla completa
--max permite agrupar de acuerdo a mas criterios
-- cual es la fecha mas reciente en la que un alumno 
--ingreso a una carrera
select carrera_id, max(fecha_incorporacion)
from platzi.alumnos
group by carrera_id
order by carrera_id;
```
Retos:
```
--minimo nombre
select min(nombre)
from platzi.alumnos
```
```
-- minimo por id de tutor

select  tutor_id,min(nombre)
from platzi.alumnos
group by tutor_id
order by tutor_id;
```


## Clase 18 Egoista(selfish)
SELF JOINS

Personalmente considero que a la hora de hacer self-joins es de especial importancia más que en otros casos el hecho de comentar el código
En este caso aclararía en el código que recurrí a esta técnica, ya que los tutores a la vez son alumnos, de ahí que puedo obtener toda la información de la propia tabla
```
select a.nombre,a.apellido,t.nombre,t.apellido
from platzi.alumnos as a
	inner join platzi.alumnos as t 
	on a.tutor_id = t.id
```


```
-- 10 tutores que tienen mas alumnos
select concat(t.nombre,'-',t.apellido) as tutor ,
	count(*) as alumnos_por_tutor
from platzi.alumnos as a
	inner join platzi.alumnos as t 
	on a.tutor_id = t.id
group by tutor
order by alumnos_por_tutor desc
limit 10;
```
Reto version 1
```
select AVG(alumnos_por_tutor) as promedio_alumnos
from(
select	concat(t.nombre,' ',t.apellido) as tutor ,
	count(*) as alumnos_por_tutor
from platzi.alumnos as a
	--se asume que los tutores son alumnos pero no todos los alumnos son tutores
	inner join platzi.alumnos as t 	on a.tutor_id = t.id
group by tutor
order by alumnos_por_tutor desc)
as promedio;

```
Version 2
```
SELECT AVG(n_alumnos) AS promedio_alumnos
FROM (
     SELECT COUNT(*) AS n_alumnos FROM platzi.alumnos
     GROUP BY tutor_id
         ) AS tabla_num_alumnos_por_tutor;
```
tutores promedio por carrera v1
```
select AVG(prom_tutor.tutor_carrera) as promedio_alumnos
from(
select	concat(t.nombre,' ',t.apellido) as tutor ,
	count(*) as tutor_carrera, a.carrera_id
from platzi.alumnos as a
	inner join platzi.alumnos as t 	on a.tutor_id = t.id
	and a.carrera_id=t.tutor_id
group by tutor,a.carrera_id
order by tutor_carrera desc)
as prom_tutor;
```


## Clase 19 Resolviendo Diferencias
Ejemplos
```
select carrera_id, count(*) as cuenta
from platzi.alumnos
group by carrera_id
order by cuenta desc;

```
De la tabla carrera Borrar id entre 30-40
```
delete from platzi.carreras
where id between 30 and 40;

```
left join exclusive
```
select concat(a.nombre,'-',a.apellido) as alumno,
		a.carrera_id,
		c.id,
		c.carrera		
from platzi.alumnos as a
--left join nos interesa la tabla a la izquierda
-- que es la primera tabla que ponemos
--en el from
	left join platzi.carreras as c
	on a.carrera_id = c.id
-- solo los datos tabla alumnos
-- cuales son los alumnos que se quedaron sin carrera 
where c.id is null
order by a.carrera_id desc;

```
Reto
```
select concat(a.nombre,'-',a.apellido) as alumno,
		a.carrera_id,
		c.id,
		c.carrera
from platzi.alumnos as a
	full outer join platzi.carreras as c
	on a.carrera_id = c.id

order by a.carrera_id desc;
```


## Clase 20 Todas las uniones

![image](https://user-images.githubusercontent.com/90301902/186685496-85ba1c9d-0a0f-4caf-a949-b4c47ab4016d.png)


## Clase 21 Triangulando


## Clase 22 Generando rangos


## Clase 23 Regularizando expresiones
# Conceptos de SQL avanzado

## Clase 24 bases de datos distribuidas

## Clase 25 queries distribuidos

## Clase 26 sharding

## Clase 27 window functions

## Clase 28 particiones y agregacion

## Clase 29 el futuro de sql
