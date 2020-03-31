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

    CREATE (SCHEMA | DATABASE)
    [IF NOT EXISTS]		<nombre de BD>
    [CHARACTER SET]		<nombre de charset>;
    
Para crear tablas utilizaremos una estructura similar: 

    CREATE TABLE "nombre de la tabla" (
    Columnas  Dominios
    );
