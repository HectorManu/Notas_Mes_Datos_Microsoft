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



# Introducción a las consultas con Trannsact-SQL

## Limitación de los resultados ordenados
- TOP es una extensión propiedad de Microsoft de la cáusula SELECT. La cláusula TOP le permitirá especificar cuátas filas se van a devlover, ya sea como un entero positivo o como un porcentaje de todas las filas calificadas. El número de filas se puede especificar como una constante o como una expresión. La cláusula TOP se usa con más frecuencia con ORDER BY, pero se puede usar con datos no ordenados.

```sql
SELECT TOP (N) <column_list>
FROM <table_source>
WHERE <search_condition>
ORDER BY <order list> [ASC|DESC];
```




## Combinación de varias tablas con JOIN en T-SQL

En este módulo, se considerará principlamente parte de la cláusula FROM. En este módulo se describirá cómo la clásusula FROM  de una instrucción SELECT de T-SQL  **crea tablas virtuales intermedias que se consumirán en fases psteriores de la consulta.**

### La cláusula FOM y las tablas virtuales


### Uso de combinacioens internas

INNER JOIN. Las convinaciones internas se usasn para resolver muchos problemas empresariales comunes, especialmente en entornos de base de datos muy normalizados. Para recuperar datos que se han almacenado en varias tablas, a menudo tend´ra que ombinarlo mediante consultas INNER JOIN.INNER JOIN  comienza su fase de procesmiento lógico como un producto cartesianod y después se filtra para quitar las filas que no coinciden con el predicado. 

#### Procesamiento de INNER JOIN  

- La consulta SQL selecciona dos columnas de dos tablas diferentes (Employee y SalesOrder) de dos bases de datos diferentes (HR y Sales) y las une utilizando un INNER JOIN basado en la igualdad de las columnas "EmployeeID". La consulta es la siguiente:

```sql
SELECT emp.FirstName, ord.Amount
FROM HR.Employee AS emp
INNER JOIN Sales.SalesOrder AS ord
    ON emp.EmployeeID = ord.EmployeeID;
```

Aquí está lo que sucede paso a paso:

La cláusula SELECT se utiliza para especificar qué columnas se desean recuperar de las tablas.

emp.FirstName selecciona la columna "FirstName" de la tabla "Employee" (alias "emp").

ord.Amount selecciona la columna "Amount" de la tabla "SalesOrder" (alias "ord").

La cláusula FROM especifica las tablas involucradas en la consulta.

HR.Employee AS emp especifica la tabla "Employee" de la base de datos "HR" y la asigna un alias "emp".
La cláusula INNER JOIN combina las filas de dos tablas basadas en una condición de igualdad.

Sales.SalesOrder AS ord especifica la tabla "SalesOrder" de la base de datos "Sales" y la asigna un alias "ord".

ON emp.EmployeeID = ord.EmployeeID especifica la condición de igualdad para unir las dos tablas. En este caso, la columna "EmployeeID" de la tabla "Employee" es igual a la columna "EmployeeID" de la tabla "SalesOrder".

El resultado es una tabla que muestra los valores correspondientes de las columnas "FirstName" de la tabla "Employee" y "Amount" de la tabla "SalesOrder" para todas las filas donde las columnas "EmployeeID" son iguales en ambas tablas.

En resumen, la consulta selecciona el nombre del empleado y el monto de venta de dos tablas diferentes, Employee y SalesOrder, en dos bases de datos diferentes, HR y Sales, y las une en función de la columna "EmployeeID" utilizando un INNER JOIN. El resultado es una tabla que muestra los valores correspondientes de las columnas "FirstName" y "Amount" para todas las filas que cumplen la condición de igualdad.


La consulta SQL que proporcionas:

```sql
SELECT emp.FirstName, ord.Amount
FROM HR.Employee AS emp
LEFT JOIN Sales.SalesOrder AS ord
    ON emp.EmployeeID = ord.EmployeeID;
```

se compone de una cláusula SELECT y una cláusula LEFT JOIN.

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se selecciona el nombre del empleado (FirstName) de la tabla "Employee" de la base de datos "HR" y el monto de la venta (Amount) de la tabla "SalesOrder" de la base de datos "Sales".

La cláusula LEFT JOIN combina las filas de dos tablas, "Employee" y "SalesOrder", en función de una condición de igualdad. En este caso, se usa la condición ON emp.EmployeeID = ord.EmployeeID, que une las filas de las dos tablas en función del valor de la columna "EmployeeID".

La diferencia entre LEFT JOIN y INNER JOIN es que LEFT JOIN devuelve todas las filas de la tabla de la izquierda ("Employee") y las filas de la tabla de la derecha ("SalesOrder") que cumplan la condición de unión. Si una fila de la tabla de la izquierda no tiene una fila correspondiente en la tabla de la derecha, el resultado de la consulta para la tabla de la derecha será NULL. En otras palabras, LEFT JOIN conserva todas las filas de la tabla de la izquierda, incluso si no hay una fila correspondiente en la tabla de la derecha.

Por lo tanto, en esta consulta, se seleccionan los nombres de todos los empleados de la tabla "Employee" y, para cada empleado, se muestra el monto de la venta correspondiente si se encuentra en la tabla "SalesOrder". Si un empleado no ha realizado ninguna venta, el resultado para el monto de la venta será NULL.


### Uso de combinaciones cruzadas

**Para crear de forma explícita un producto cartesiano, use el operador  CROSS JOIN**.

1. Esta operación crea un conjunot de resultados con todas las combinaciones posible de las filas de entrada: 

```sql
SELECT <select_list>
FROM table1 AS t1
CROSS JOIN table2 AS t2;
```

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. La lista de selección puede incluir nombres de columna, expresiones y funciones. La sintaxis general de la cláusula SELECT es SELECT column1, column2, ... FROM table_name.

La cláusula CROSS JOIN combina todas las filas de dos tablas, "table1" y "table2", produciendo un resultado que es el producto cartesiano de ambas tablas. En otras palabras, cada fila de la tabla "table1" se combina con cada fila de la tabla "table2", generando un resultado que tiene el número de filas igual al número de filas en "table1" multiplicado por el número de filas en "table2".

La cláusula CROSS JOIN no requiere una condición de unión, ya que todas las filas de cada tabla se combinan entre sí.

Por lo tanto, en esta consulta, se seleccionan las columnas especificadas en <select_list> de las tablas "table1" y "table2", y se produce un resultado que tiene el número de filas igual al número de filas en "table1" multiplicado por el número de filas en "table2", en el que cada fila de la tabla "table1" se combina con cada fila de la tabla "table2".


Aunque este resultado no suele ser una salida deseada, hay algunas aplicaciones prácticas para escribir una operación CROSS JOIN explícita:

Crear una tabla de números, con una fila para cada valor posible de un intervalo.
Generar grandes volúmenes de datos para pruebas. Cuando se le aplica una combinación cruzada a sí misma, una tabla con tan solo 100 filas puede generar fácilmente 10 000 filas de salida.

#### Ejemplo del uso de CROSS JOIN 
 para crear todas las combinacinos de empleados y productos: 

 ```sql
 SELECT emp.FirstName, prd.Name
FROM HR.Employee AS emp
CROSS JOIN Production.Product AS prd;
 ```

 La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se selecciona el nombre del empleado (FirstName) de la tabla "Employee" de la base de datos "HR" y el nombre del producto (Name) de la tabla "Product" de la base de datos "Production".

La cláusula CROSS JOIN combina todas las filas de dos tablas, "Employee" y "Product", sin una condición de unión. En otras palabras, CROSS JOIN combina todas las filas de la tabla de la izquierda con todas las filas de la tabla de la derecha.

Por lo tanto, en esta consulta, se seleccionan los nombres de todos los empleados de la tabla "Employee" y el nombre de todos los productos de la tabla "Product". Luego, se combinan todas las filas de la tabla "Employee" con todas las filas de la tabla "Product" para generar un conjunto de resultados que contiene todas las combinaciones posibles de empleados y productos. Por ejemplo, si la tabla "Employee" tiene 10 filas y la tabla "Product" tiene 20 filas, el resultado de la consulta tendría 200 filas (10 x 20).


#### Uso de autocombinaciones

```sql
SELECT emp.FirstName AS Employee, 
       mgr.FirstName AS Manager
FROM HR.Employee AS emp
LEFT OUTER JOIN HR.Employee AS mgr
  ON emp.ManagerID = mgr.EmployeeID;
```

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se selecciona el nombre del empleado (FirstName) de la tabla "Employee" de la base de datos "HR" y el nombre del gerente del empleado (FirstName de nuevo) de la misma tabla.

Los alias de columna Employee y Manager se asignan a las columnas de la tabla para hacer que los nombres de las columnas en la salida de la consulta sean más legibles.

La cláusula LEFT OUTER JOIN combina las filas de dos tablas, "Employee" y "Employee" (renombrada como "mgr"), en función de una condición de igualdad. En este caso, se usa la condición ON emp.ManagerID = mgr.EmployeeID, que une las filas de las dos tablas en función del valor de la columna "ManagerID" en la tabla "Employee" y la columna "EmployeeID" en la tabla "mgr".

La diferencia entre LEFT OUTER JOIN y INNER JOIN es que LEFT OUTER JOIN devuelve todas las filas de la tabla de la izquierda ("Employee") y las filas de la tabla de la derecha ("mgr") que cumplan la condición de unión. Si una fila de la tabla de la izquierda no tiene una fila correspondiente en la tabla de la derecha, el resultado de la consulta para la tabla de la derecha será NULL. En otras palabras, LEFT OUTER JOIN conserva todas las filas de la tabla de la izquierda, incluso si no hay una fila correspondiente en la tabla de la derecha.

Por lo tanto, en esta consulta, se seleccionan los nombres de todos los empleados de la tabla "Employee" de la base de datos "HR" y el nombre de su gerente, si tienen uno. Si un empleado no tiene un gerente asignado (es decir, ManagerID es NULL), el resultado de la consulta para el nombre del gerente será NULL. La consulta muestra todos los empleados, incluso aquellos que no tienen un gerente asignado.




