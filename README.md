# SQL Notes
#### First of all we need to create a database

`create database database-name;`

#### This command shows all databases created
`show databases;`

#### Comments
`-- This is a comment;`

#### Data types
Like in any other language, there are some data types like the following ones:

- int = `0, 1, 25, 6000`
- float = `61.23` (Decimals)
- varchar = `string`

#### Before start working we need to specify with wich database we are going to work with

`use database-name;`

#### Just after that, we can create a table with ther columns and wich one of the columns includes the _primary key_
Every row of a table is called _register_, and the primary key is the register's id. Also we need to specify the data type.

```
CREATE TABLE database-name (
id int,
tipo varchar(255), -- BETWEEN PARENTESIS GOES THE MAXIMUM NUMBER OF CHARACTERS
estado varchar(255),
PRIMARY KEY (id)
);
```
This will create the following table

| id  |  Tipo | Estado  |
|:---:|:--------:|:-------:|
 |     |          |


#### Insert data into the table
When we insert data into the table 
INSERT INTO animales (tipo, estado) VALUES ('chanchito', 'feliz');
-- USAR SOLAMENTE ESTO VA A TIRAR ERROR PORQUE NUNCA LE DIJIMOS A LA TABLA QUE EL ID DEBE SER AUTO
-- INCREMENTAL POR LO QUE AHORA VAMOS A TENER QUE MODIFICARLA

-- MODIFICAR UNA TABLA PARA INDICARLE QUE LOS ID DEBEN SER AUTO INCREMENTALES
ALTER TABLE animales MODIFY COLUMN id int auto_increment;

-- PARA CREAR LA TABLA CON COLUMNA AUTO INCREMENTAL DESDE EL PRINCIPIO ES ASI
CREATE TABLE nombre (
id int NOT NULL AUTO_INCREMENT,
tipo varchar(255) DEFAULT NULL, -- ENTRE PARENTESIS VA LA CANTIDAD MAXIMA DE CARACTERES
estado varchar(255) DEFAULT NULL,
PRIMARY KEY (id)
);

-- CON SELECT UNO INDICA CUALES COLUMNAS QUIERE VER DE UNA TABLA, CON * SE VEN TODAS
-- PRIMERO LAS COLUMNAS Y LUEGO LA TABLA
SELECT * FROM animales;

SELECT * FROM animales WHERE id = 1;

SELECT * FROM animales WHERE estado = 'FELIZ' AND tipo = 'felipe';

-- ACTUALIZAR REGISTROS QUE YA SE ENCUENTRAN EN LA DB
UPDATE animales SET estado = 'feliz' WHERE id = 3;

-- BORRAR REGISTROS DE LA DB
DELETE FROM animales WHERE id = 4;

-- RENOMBRAR UNA COLUMNA DE UNA TABLA
ALTER TABLE user RENAME COLUMN edad TO age;

-- RENOMBRAR UNA TABLA
ALTER TABLE usuarios RENAME TO user;
-- EN EL VIDEO USA ESTA OTRA
RENAME TABLE products TO product;

-- TRAER LOS PRIMEROS RECURSOS DE UNA TABLA
SELECT * FROM nombreTabla limit cantidad;

-- TRAER CON CONDICIONES
SELECT * FROM user WHERE age > 15;
SELECT * FROM user WHERE age > 20 or email = 'layla@gmail.com';
SELECT * From  user WHERE email != 'layla@gmail.com';
SELECT * FROM user WHERE age BETWEEN 15 AND 30;
-- BUSCA DENTRO DE LA COLUMNA mail ALGUN REGISTRO QUE DENTRO TENGA LA CADENA SIN IMPORTAR QUE HAY
-- ANTES O DESPUES DE LA PALABRA
SELECT * FROM user WHERE email LIKE '%gmail%';
-- ACA LO MISMO, PERO SOLO SIN IMPORTAR EL PRINCIPIO, TIENE QUE TERMINAR EN GMAIL
SELECT * FROM user WHERE email LIKE '%gmail';
-- ACA LO MISMO, PERO SOLO SIN IMPORTAR lo que haya despues de la palabra
SELECT * FROM user WHERE email LIKE 'oscar%';

-- HACER UNA CONSULTA CON ORDEN ASCENDENTE
SELECT * FROM user ORDER BY age ASC;
-- HACER UNA CONSULTA CON ORDEN DESCENDENTE
SELECT * FROM user ORDER BY age DESC;

-- TRAER EL REGISTRO QUE TENGA LA MAYOR EDAD
SELECT MAX(age) AS MAYOR FROM USER;
-- TRAER EL REGISTRO QUE TENGA LA MENOR EDAD
SELECT MIN(age) AS MENOR FROM USER;

-- TRAER MAS DE UNA COLUMNA
SELECT id, name FROM user;
-- TRAER COLUMNAS CON ALIAS
SELECT id, name AS nombre FROM user;


-- JOINS
-- CON ESTO CREO UNA TABLA DE PRODUCTOS, DONDE ESPECIFICO EL USUARIO DE LA TABLA DE USUARIOS QUE
-- CREO EL PRODUCTO
CREATE TABLE products (
id INT NOT NULL AUTO_INCREMENT,
name VARCHAR(50) NOT NULL,
created_by INT NOT NULL,
marca VARCHAR(50) NOT NULL,
PRIMARY KEY(id),
FOREIGN KEY(created_by) REFERENCES user(id)
);

-- EN EL VIDEO USA ESTA OTRA
RENAME TABLE products TO product;

-- CARGAR VARIOS REGISTROS A LA VEZ
INSERT INTO product (name, created_by, marca)
values
('ipad', 1, 'apple'),
('iphone', 1, 'apple'),
('watch', 2, 'apple'),
('macbook', 1, 'apple'),
('imac', 3, 'apple'),
('ipad mini', 2, 'apple');

-- LEFT JOIN
-- CON LEFT JOIN LO QUE HACEMOS ES TRAER TODOS LOS REGISTROS DE LA TABLA DE USUARIOS Y SI ES QUE HAY
-- REGISTROS EN LA TABLA DE PRODUCT CREADOS POR USUARIOS LOS TRAE TAMBIEN

-- TRAE TODOS LOS REGISTROS DE LA TABLA DE USUARIOS QUE CORRESPONDEN CON LA CONSULTA QUE ESTOY EJECUTANDO
-- Y TRAE LOS PRODUCTOS SOLO SI ESTAN ASOCIADOS A ALGUN REGISTRO DE LA TABLA DE USUARIOS
-- U Y P SON ALIAS
-- ESTA CONSULTA DEVUELVE LA TABLA DE USUARIOS COMO PRINCIPAL
SELECT u.id, u.email, p.name FROM user u LEFT JOIN product p ON u.id = p.created_by;

-- RIGHT JOIN
-- FUNCIONA IGUAL QUE EL LEFT JOIN PERO DEVOLVIENDO COMO PROTAGONISTA LA TABLA DE PRODUCTOS
SELECT u.id, u.email, p.name FROM user u RIGHT JOIN product p ON u.id = p.created_by;

-- INNER JOIN
-- VIMOS QUE EN EL CASO DE LEFT JOIN NOS TRAE TODOS LOS USUARIOS Y EN EL CASO DE ENCONTRAR ALGUN PRODUCTO
-- ASOCIADO TAMBIEN NOS VA A TRAER ESE PRODUCTO.
-- EN EL CASO DE RIGHT JOIN VIMOS QUE VA A TRAER TODOS LOS PRODUCTOS Y EN EL CASO QUEE EXISTA NOS VA A TRAER
-- UN USUARIO ASOCIADO
-- INNER JOIN LO QUE HACE ES TRAER LA INTERSECCION ENTRE AMBAS TABLAS
-- TANTO USUARIOS COMO PRODUCTOS SIEMPRE Y CUANDO ESTOS ESTEN ASOCIADOS
SELECT u.id, u.email, p.name FROM user u INNER JOIN product p ON u.id = p.created_by;

-- CROSS JOIN
-- ENTREGA EL PRODUCTO CARTESIANO ENTRE AMBAS TABLAS
-- JUNTA CADA UNO DE LOS REGISTROS DE UNA TABLA, CON LOS REGISTROS DE OTRA TABLA
-- POR EJ, SI TENEMOS UNA PRIMER TABLA CON 3 REGISTROS Y OTRA CON 4, LOS RESULTADOS SERIAN 1*1, 1*2, 1*3, 1*4
-- 2*1, 2*2, 2*3, 2*4, 3*1, 3*2 , 3*3, 3*4
-- ESTO PUEDE SER MUY PEGRILOSO PORQUE LA CANTIDAD DE REGISTROS DE LA CONSULTA PUEDE SER POTENCIALMENTE GIGANTE
-- NO HACE FALTA ESPECIFICAR DONDE SE JUNTAN LAS TABLAS
SELECT u.id, u.name, p.id, p.name FROM user u CROSS JOIN product p;

-- GROUP BY
-- ESTO AGRUPA TOMANDO EN CUENTA EL PARAMETRO QUE SE LE PASA, SI PASAMOS POR EJEMPLO LA MARCA COMO PARAMETRO
-- NOS VA A DEVOLVER 6 PORQUE SON 6 LOS PRODUCTOS REGISTRADOS DE LA MARCA APPLE
SELECT COUNT(id), marca FROM product GROUP BY marca;

-- VAMOS A HACER UN LEFT JOIN CON LA TABLA DE USUARIO, EN ESTE CASO LA TABLA PRINCIPAL SERIA LA TABLA DE PRODUCTO
-- ESTO DEVUELVE LA CANTIDAD DE PRODUCTOS QUE CREO CADA UNO DE LOS REGISTROS DE LA TABLA DE USUARIO
SELECT COUNT(p.id), u.name FROM product p LEFT JOIN user u ON u.id = p.created_by GROUP BY p.created_by;

-- HAVING
-- SE LE PUEDEN PASAR PARAMETROS PARA QUE NOS TRAIGA LO QUE NOS INTERESA, POR EJEMPLO LOS USUARIOS QUE
-- CARGARON MAS DE DOS PRODUCTOS
SELECT COUNT(p.id), u.name FROM product p LEFT JOIN user u
ON u.id = p.created_by GROUP BY p.created_by
HAVING COUNT(P.ID) >= 2;

-- ESTA ES LA ULTIMA INSTRUCCION MAS IMPORTANTES Y USADAS RECURRENTEMENTE
-- AHORA VAMOS A VER LA ULTIMA INSTUCCION ANTES DE REALIZAR MODELAMIENTO DE BASES DE DATOS
-- DROP TABLE
DROP TABLE product;


-- CARDINALIDAD
-- SE CREA UNA TABLA INTERMEDIA QUE TIENE RELACION 1-n CON LAS OTRAS TABLAS DE LOS EXTREMOS

-- DIAGRAMAS DE ENTIDAD RELACION
