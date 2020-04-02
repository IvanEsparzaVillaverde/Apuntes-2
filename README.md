# Indice
1-[Sublenguajes SQL](#Sublenguajes-SQL)
2-[Dominios](#Dominios)
3-[DDL](#DDL)
  3.1-[CREATE](#CREATE)
# Sublenguajes SQL

En el lenguajes SQL existen 6 sublenguajes que son:

 - DQL (Data Query Languaje): SELECT
 
 - DML (Data Manipulation Languaje): INSERT, UPDATE, DELETE

>Estos dos sublenguajes operan sobre los datos.

 - DDL (Data Definition Languaje): CREATE, ALTER, DROP
 
> Esta sublenguaje opera sobre los objetos de la BD.
> DQL, DML y DDL son el **núcleo central de SQL**.

 - TCL (Transaction Control Languaje): COMMIT, ROLLBACK, SAVEPOINT
 
 - DCL(Data Control Languaje):GRANT, REVOKE, AUDIT, COMMENT, ANALYZE
 
 - SCL(Session Control Languaje): ALERT SESSION, SET ROLL

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

DEFINICIÓN DE DOMINIOS:
```sql
CREATE DOMAIN Nome_Válido VARCHAR(30);
CREATE DOMAIN Tipo_Código CHAR(5);
CREATE DOMAIN Tipo_DNI    CHAR(9);
```

# DDL

## CREATE

En DDL, para crear BD o tablas utilizamos **CREATE**.

Para crear una BD utilizaremos la siguiente estructura:

```sql
CREATE (SCHEMA | DATABASE)
    [IF NOT EXISTS]		<nombre de BD>
    [CHARACTER SET]		<nombre de charset>;

```
EJEMPLO:
```sql
CREATE DATABASE ProyectoDeInvestigacion;
```
    
Para crear tablas utilizaremos una estructura similar: 

```sql
    CREATE TABLE nombreTabla (
    Columnas  Dominios
    );
```
EJEMPLO:
```sql
CREATE TABLE Departamento (
  Nome_Departamento Nome_Válido,
  Teléfono          CHAR(9)      NOT NULL,
  Director          Tipo_DNI
);
```
## CONSTRAINT

En las tablas de una BD debemos indicar claves primarias y claves ajenas. Para indicar claves primarias utilizaremos **PRIMARY KEY** justo despues del tipo de dominio.
```sql
    CREATE TABLE nombreTabla (
    <atributo>  <Dominio> PRIMARY KEY
    );
```
EJEMPLO:
```sql
CREATE TABLE Departamento (
  Nome_Departamento Nome_Válido PRIMARY KEY,
  Teléfono          CHAR(9)      NOT NULL,
  Director          Tipo_DNI
);
```
>Si existen mas de una clave primaria ponemos **PRIMARY KEY** y entre parentesis los nombres de las claves.
```sql
    CREATE TABLE nombreTabla (
    <atributo1>  <Dominio1>,
    <atributo2>  <Dominio2>,
    PRIMARY KEY (atributo1,atributo2)
    );
```
EJEMPLO:
```sql
CREATE TABLE Departamento (
  Nome_Departamento Nome_Válido,
  Teléfono          CHAR(9)      NOT NULL,
  Director          Tipo_DNI
  PRIMARY KEY (Nome_Departamento,Director)
);
```
Para las claves ajenas, las indicamos con **FOREGIN KEY (atributos) REFERENCES (tabla que se referencia)**. A continuación utilizamos **ON DELETE** y **ON UPDATE**. 
```sql
FOREIGN KEY (<atributos>) REFERENCES nombreTabla (<atributosReferencia>)
[ON DELETE NO ACTION | CASCADE | SET DEFAULT | SET NULL]
[ON UPDATE NO ACTION | CASCADE | SET DEFAULT | SET NULL]
```
EJEMPLO:
```sql
CREATE TABLE Grupo (
  Nome_Grupo        Nome_Válido,
  Nome_Departamento Nome_Válido,
  Área              Nome_Válido NOT NULL,
  Lider             Tipo_DNI,
  PRIMARY KEY (Nome_Grupo, Nome_Departamento)
  FOREIGN KEY (Nome_Departamento) REFERENCES Departamento
    ON DELETE CASCADE 
    ON UPDATE CASCADE
);
```
>No es recomendable utilizar en **ON DELETE** **CASCADE** ya que se borraría todo lo que esta antes de lo que quremos eliminar es mejor utilizar **SET NULL**.

>En **ON UPDATE** al contrario de **ON DELETE** deberiamos utilizar **CASCADE** ya que así podriamos actualizar todos los datos de la tabla.

Para hacer predicados se utiliza **CHECK**.
```sql
CHECK predicado (atributos)
[(NOT) DEFERRABLE ]
[INITIALLY INMEDIATE | DEFERABLE]
```
EJEMPLO:
```sql
CREATE TABLE Proxecto (
  Código_Proxecto Tipo_Código  PRIMARY KEY,
  Nome_Proxecto   Nome_Válido  NOT NULL,
  Orzamento       MONEY        NOT NULL,
  Data_Inicio     DATE         NOT NULL,
  Data_Fin        DATE,
  CONSTRAINT Check_Dates
    CHECK (Data_Inicio < Data_Fin)
);
```
> **CHECK** tiene 2 valores que son **NOT DEFERRABLE INITIALLY INMEDIATE**, indiaca que el predicado no es aplazable y se debe ejecutar de inmediato,y **DEFERRABLE INITIALLY DEFERRABLE**, que indica que el predicado es aplazabley no es necesario ejecutarlo en ese momento.

**UNIQUE** lo utilizamos en claves secundarias. 
```sql
    CREATE TABLE nombreTabla (
    <atributo1>  <Dominio1> PRIMARY KEY,
    <atributo2>  <Dominio2> UNIQUE,
    <atributo2>  <Dominio2>,
    );
```
EJEMPLO:
```sql
CREATE TABLE Proxecto (
  Código_Proxecto Tipo_Código  PRIMARY KEY,
  Nome_Proxecto   Nome_Válido  NOT NULL,
  UNIQUE (Nome_Proxecto)
);
```
## DROP
**DROP** lo utilizamos para borrar BD y tablas.
> Para borrar una Base de Datos, utilizaremos **DROP SCHEMA** o **DROP DATABASE**. Puede llevar **IF EXISTS**. Lo que hace **IF EXISTS** es borrar la tabla si existe.Si ponemos un nombre erroneo o de una tabla que no exixte no borrará ninguna tabla.
```sql
    DROP ( SCHEMA | DATABASE)
    [IF EXISTS]      <nombre da BD>;
```
>Para borrar una tabla utilizamos **DROP TABLE**. AL igual que **DROP SCHEMA** o **DROP DATABASE** puede tener el valor **IF EXISTS**, **CASCADE** y **RESTRICT**. 
```sql
    DROP TABLE 
    [IF EXISTS]   <nombreTabla>
    [CASCADE | RESTRICT];
```
>**CASCADE** eliminará toda la tabla.

>**RESTRICT** si la tabla tiene dependencia de otra esa tabla no se eliminará.

## ALTER
**ALTER** sirve para modificar una columna o tabla.
>Tiene dos valores **ADD** (añadir) y **DROP** (borrar).
```sql
ALTER TABLE nombreTabla 
DROP COLUMN <atributo> [CASCADE | RESTRICT],
ADD COLUMN <atributo> <Dominio> NOT NULL ...                        
```
EJEMPLO:
```sql
ALTER TABLE Financia
  ADD CONSTRAINT FK_Proxecto_Financia
    FOREIGN KEY (Código_Proxecto)
    REFERENCES Proxecto
    ON DELETE Cascade
    ON UPDATE Cascade;
```
Si queremos modificar una columna debemos poner **ALTER TABLE (tabla) DROP|ADD COLUMN...**

# DML

## INSERT

Sirve para insertar nuevas tuplas en una tabla.
```sql
INSERT INTO <nome-da-tabla> [(<atributo1>,<atributo2>...)]
VALUES (<valor1>,<valor2>,...) | SELECT... ;
```
>En **SELECT** tiene que indicar el mismo número de columnas o elmismo número de dominios (DATETIME, NCHAR, INTEGER).

## VALUES
```sql
(<valor1A>,<valor2A>,...),
(<valor1A>,<valor2A>,...),
...),
(<valor1N>,<valor2N>,...), | SELECT
```
## UPDATE
Se utiliza para actualizar las tuplas de una columna. 
``` sql
 UPDATE <nombre tabla>
   SET <atributo1> = <valor1>,
	<atributo2> = <valor2>,
	...
    [WHERE <predicado>];
```
EJEMPLO:
``` sql
 UPDATE world
   SET name = 'España',
	continent= 'Africa',
	...
    WHERE name='Spain';
```
## DELETE 

Sirve para borrar tuplasde una tabla.

Al igual que en **UPDATE** podemos utilizar **WHERE** para especificar las tuplas que queremos borrar.
```sql
DELETE FROM <nome tabla>
 [WHERE <predicado>];
```
EJEMPLO:
```sql
DELETE FROM world
 WHERE population>100 000 000;
```
