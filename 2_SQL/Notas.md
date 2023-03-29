# Notas

- **SQL** es un acrónimo de Lenguaje de consulta estructurado.
- **SELECT** de SQL  se usa para consultar la base de datos y devlover un conjunto de filas de datos.
- Ejemplos de administración de bases de datos relacionaels habituales 
  - SQL Server, MySQL, PostgreSQL, MariaDB y Orable.


## Transact-SQL 

Instrucciónes SQL básicas:
  - SELECT
  - INSERT
  - UPDATE
  - DELETE
- Los sistemas de base de datos de Microsoft, como SQL Server, Azure SQL, Database, Azure Synapse Analytics y otros, usan un dialecto de SQL denominado Transct-SQL o T-SQL, TSLQ incluye extensiones de lenguaje para escribir procedimientos almacenados y funciones, que son código de aplicación almacenando en la base de datos, y administrar cuentas de usuario.

## SQL es un lenguaje declarativo
- SQL admite cierta sintaxis de procedimientos, pero la consulta de datos con SQL suele seguir la smántica declarativa. SQL se usa para escribir los resultados que desea y procesaor de contultas dle motor de base de datos desarrolla un *plan* de consultas para recuperarlos. El procesaor de consultas use estadísticas sobre los datos de la base de datos y los índice que se definen en las tablas para elaborar un buen plan de consulta.

# Instrucción SELECT 
SELECT, que, con diferencia, es la instrucción DML  que tiene mayor número de opcinoes y variaciones.

```sql
SELECT OrderDate, COUNT(OrderID) AS Orders
FROM Sales.SalesOrder
WHERE Status = 'Shipped'
GROUP BY OrderDate
HAVING COUNT(OrderID) > 1
ORDER BY OrderDate DESC;
```
## Explicación de consulta:
  - SELECT esta compuesta de varias *cláusulas*, pero primeramente aparece **OrderDAte** y después el recuento de valores **OrderID**, a los que se asigna el nombre ***AS Orders***.
  - FROM identifica qué tabla es el origen de las filas de la consulta. **FROM Sales.SalerOrder**.
  - WHERE filtra las filas de los resultados, manteniendo solo las filas que cumplen la condición especificada; en este caso, los pedidos que tiene el estado **"Shipped"**.
  - GROUP BY toma las filas que cumplan la condición del filtro y las agrupa por **OrderDate**, de mod que todas las filas con el mismo valor **OrderDate**  se consideren como un único grupo y se devuelva una fila para cada grupo 
  - HAVING filtar los grupos en función de su propio predicado. Solo las fechas con más de un pedido se incluirán en los resultados:
  - ORDER BY, ORDENA LA SALIDA EN ORDEN DESCENDENTE DE **oRDERdaTE**


## Slección de todas la columnas
- SELECT * es adecuado para una prueba rápida, debe evitar usarlo en el trabajo de producción por los siguiente motivos:
  *   Los cambios en la tabla que agregan o reorganizan columas se reflejarán en los resultaods d ela consulta, lo que puede dar lugar auna salida inesperada para las aplicaciones o informes que usan la consulta.
  *   La devolución de datos que no son necesarios puede relentizar las consulas y provocar problemas d erendimiento si la tabla de origen contiene un gran número de filas.
  *   Ejemplo:
```sql
SELECT * FROM Production.Product;
```
este da un output de todas las columnas

## Slección de columnas específicas

```sql
SELECT ProductID, Name, ListPrice, StandardCost
FROM Production.Product;
```

## Selección de expresiones

```sql
SELECT ProductID,
      Name + '(' + ProductNumber + ')',
  ListPrice - StandardCost
FROM Production.Product;
```

- En esta consulta carecemos de encabzado de columna y esta en blanco porlo que indica *"sin nombre de columna"* o un nombre predeterminado como **column1**.*Más adelante vermoes cómo especificar un **alias** para el nombre de columna en la consulta*.
- 


## Especificación de alias de columnas

Puede especificar un *alias* para cada columna que la contua SELECT devuelve, ya sea como alternativa al nombre de la columna de origen o poara asignar un nonbre a la salida de un a espresión.

Por ejemplo, esta es la mima consulta que antes pero con alias especificado para cada una de las columnas:

```sql
SELECT ProductID AS ID,
      Name + '(' + ProductNumber + ')' AS ProductName,
  ListPrice - StandardCost AS Markup
FROM Production.Product;
```

# T-SQL  incluye funciones que le ayudan a convertis explícitamente entre tipos de datos

## CAST Y TRY_CAST
La función CAST convierte un valor en un tipo de datos especificado si dicho valro es compatible con el tipo de daot de destino. Se devolverá un eror si no es compatible.

Por ejemplo, la consulta siguiente usa CAST  parea convertis lor valores *integer* de la colmuna **ProductID** en valores *varchar* (con un máximo de 4 caracteres) para concatenarlos con otro valor basado en caracteres:

```sql
SELECT CAST(ProductID AS varchar(4)) + ': ' + Name AS ProductName
FROM Production.Product;
```

