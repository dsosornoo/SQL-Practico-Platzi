# SQL-Practico-Platzi


Clase 1: Breve historia de SQL
SQL (Structured Query Language - Lenguaje de consulta estructurada) es un lenguaje que se basó en 2 principios fundamentales:

Teoría de conjuntos.
Álgebra relacional de Edgar Codd (científico informático inglés).
⠀
SQL fue creada en 1974 por IBM.
Originalmente fue llamado SEQUEL, posteriormente se cambió el nombre por problemas de derechos de autor.
⠀⠀⠀⠀⠀⠀⠀⠀⠀
Relation Company (actualmente con el nombre Oracle) creó el software Oracle V2 en 1979.

Mas adelante SQL se convertiría en un lenguaje estándar que unifica todo dentro de las bases de datos relacionales, se convierte en una norma ANSI o ISO.

Clase 2: Álgebra relacional
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
