# Sublenguajes SQL

En el lenguajes SQL existen 6 sublenguajes que son:
1- DQL (Data Query Languaje): SELECT
2-DML (Data Manipulation Languaje): INSERT, UPDATE, DELETE
>Estos dos sublenguajes operan sobre los datos.

3-DDL (Data Definition Languaje): CREATE, ALTER, DROP
> Esta sublenguaje opera sobre los objetos de la BD.
> DQL, DML y DDL son el **núcleo central de SQL**.

4- TCL (Transaction Control Languaje): COMMIT, ROLLBACK, SAVEPOINT
5-DCL(Data Control Languaje):GRANT, REVOKE, AUDIT, COMMENT, ANALYZE
6-SCL(Session Control Languaje): ALERT SESSION, SET ROLL




## Dominios

Cuando hablamos de dominios nos estamos refiriendo a los tipos de datos que podemos introducir en la BD. Los que usaremos para la creación de BD son :
	
- INTEGER (números enteros)
- DECIMAL (números decimales)
- TEXT (texto)
- VARCHAR (cadena de caracteres)
- CHAR (un numero determinado de caracteres)
- BOOLEAN 
- TIME (hora del día)
- DATE (fecha)
- TIMESTAMP (Es una mezcla entre TIME y DATE, indica la fecha del día y la hora)




# DDL

## CREATE

En DDL, para crear BD o tablas utilizamos **CREATE**.

Para crear una BD utilizaremos la siguiente estructura:

```sql
CREATE (SCHEMA | DATABASE)
    [IF NOT EXISTS]		<nombre de BD>
    [CHARACTER SET]		<nombre de charset>;

```
    
Para crear tablas utilizaremos una estructura similar: 

```sql
    CREATE TABLE nombreTabla (
    Columnas  Dominios
    );
```
## CONSTRAINT

En las tablas de una BD debemos indicar claves primarias y claves ajenas. Para indicar claves primarias utilizaremos **PRIMARY KEY** justo despues del tipo de dominio.

Para las claves ajenas, las indicamos con **FOREGIN KEY (atributos) REFERENCES (tabla que se referencia)**. A continuación utilizamos **ON DELETE** y **ON UPDATE**. 
>No es recomendable utilizar en **ON DELETE** **CASCADE** ya que se borraría todo lo que esta antes de lo que quremos eliminar es mejor utilizar **SET NULL**.

>En **ON UPDATE** al contrario de **ON DELETE** deberiamos utilizar **CASCADE** ya que así podriamos actualizar todos los datos de la tabla.

Para hacer predicados se utiliza **CHECK**.
> **CHECK** tiene 2 valores que son **NOT DEFERRABLE INITIALLY INMEDIATE**, indiaca que el predicado no es aplazable y se debe ejecutar de inmediato,y **DEFERRABLE INITIALLY DEFERRABLE**, que indica que el predicado es aplazabley no es necesario ejecutarlo en ese momento.

**UNIQUE** lo utilizamos en claves secundarias. 

## DROP
**DROP** lo utilizamos para borrar BD y tablas.
> Para borrar una Base de Datos, utilizaremos **DROP SCHEMA** o **DROP DATABASE**. Puede llevar **IF EXISTS**. Lo que hace **IF EXISTS** es borrar la tabla si existe.Si ponemos un nombre erroneo o de una tabla que no exixte no borrará ninguna tabla.

>Para borrar una tabla utilizamos **DROP TABLE**. AL igual que **DROP SCHEMA** o **DROP DATABASE** puede tener el valor **IF EXISTS**, **CASCADE** y **RESTRICT**. 

>**CASCADE** eliminará toda la tabla.

>**RESTRICT** si la tabla tiene dependencia de otra esa tabla no se eliminará.

## ALTER
**ALTER** sirve para modificar una columna o tabla.
>Tiene dos valores **ADD** (añadir) y **DROP** (borrar).

Si queremos modificar una columna debemos poner **ALTER TABLE (tanla) DROP|ADD COLUMN...**

# DML

## INSERT

Sirve para insertar nuevas tuplas en una tabla.

INSERT INTO <nome-da-tabla> [(<atributo1>,<atributo2>...)]
VALUES (<valor1>,<valor2>,...) | SELECT... ;

>En **SELECT** tiene que indicar el mismo número de columnas o elmismo número de dominios (DATETIME, NCHAR, INTEGER).

## VALUES

(<valor1A>,<valor2A>,...),
(<valor1A>,<valor2A>,...),
...),
(<valor1N>,<valor2N>,...), | SELECT

## UPDATE
Se utiliza para actualizar las tuplas de una columna. 
 
 UPDATE <nombre tabla>
   SET <atributo1> = <valor1>,
	<atributo2> = <valor2>,
	...
    [WHERE <predicado>];
		
## DELETE 

Sirve para borrar tuplasde una tabla.

Al igual que en **UPDATE** podemos utilizar **WHERE** para especificar las tuplas que queremos borrar.

DELETE FROM <nome tabla>
 [WHERE <predicado>];
