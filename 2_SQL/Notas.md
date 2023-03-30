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






# Escritura de subcontuldas en T-SQL 

Transact-SQL admite la creación de *subcontulda*, en las que una conutla interna devuelve su resultado a una consulta externa.


## Trabajar con subconsultas
- Una subconsulta es una instrucción SELECT anidada o incrustada en otra consulta.
- El propósito de una subconsulta es devolver resultados a la consulta externa. La forma de los resultados determinará si la subconsulta es una subconsulta escalar o multivalor:
  - Las escalres devuelven un solo valor, las externas. La consulta externas deben procesar un único resultado.
  - Las multivalor devuelven un resultado muy similar a una tabla de una sola columna. esetas deben poder procesar varios valores.

Además d ela elcción entre subconsultas escalres y multivalor, las subconsultas pueden ser independientes o pueden correlacionarse con la consula externa:
  - Las subconsultas correacionadas haen referencia a una o varias columnas de la consulta externa y, por tanto, dependen de ella las subconsuoltas correlacionadas no se pueden ejecutar por separado desde la ocnsulta externa.


## Usar subconsultas escalars o multivalor 

### Suconsultas escalares

```sql
SELECT MAX(SalesOrderID)
FROM Sales.SalesOrderHeader
```

Esta consulta devuelve un valor único que indica el valor más alto de **OrderID** de la tabla **SalesOrderHeader**.

Para obtener más detalles del peeido entonces hay que filtrar mediante **SalesOrderDatails** en funcón del valor devuelto por la consulta anterior. Puede realizar esta tarea anidando la consulta para recuperar el valor máximo de **SalesOrderID** dentro de la cláusula WHERE  de una consulta que recupera los detalles del pedido.


```sql
SELECT SalesOrderID, ProductID, OrderQty
FROM Sales.SalesOrderDetail
WHERE SalesOrderID = 
   (SELECT MAX(SalesOrderID)
    FROM Sales.SalesOrderHeader);
```

se compone de una cláusula SELECT, una cláusula FROM, una cláusula WHERE y una subconsulta.

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se seleccionan las columnas SalesOrderID, ProductID y OrderQty de la tabla "SalesOrderDetail" de la base de datos "Sales".

La cláusula FROM especifica la tabla "SalesOrderDetail" de la que se seleccionarán los datos.

La cláusula WHERE filtra las filas de la tabla "SalesOrderDetail" en función de una condición. En este caso, la condición es SalesOrderID = (SELECT MAX(SalesOrderID) FROM Sales.SalesOrderHeader). Esto significa que solo se seleccionarán las filas en las que la columna SalesOrderID tenga el valor que se devuelve de la subconsulta. En otras palabras, solo se seleccionarán los detalles del pedido correspondientes al último pedido realizado en la tabla de encabezados de pedidos "SalesOrderHeader".

La subconsulta (SELECT MAX(SalesOrderID) FROM Sales.SalesOrderHeader) se ejecuta primero. Esta subconsulta selecciona el valor máximo de la columna SalesOrderID de la tabla de encabezados de pedidos "SalesOrderHeader". Este valor máximo es el ID del último pedido realizado en la tabla de encabezados de pedidos.

La consulta principal se ejecuta a continuación, utilizando el valor máximo de la columna SalesOrderID devuelto por la subconsulta como el valor que se debe comparar con la columna SalesOrderID en la tabla "SalesOrderDetail". Solo las filas de la tabla "SalesOrderDetail" que corresponden al último pedido realizado se seleccionan y se muestran en el resultado de la consulta.

En resumen, esta consulta devuelve los detalles del pedido correspondientes al último pedido realizado en la tabla de encabezados de pedidos "SalesOrderHeader".

#### Subconsultas multivalor

Una subconsulta multivalor es adecuada para devolver resultados mediante el operaod IN. En el ejemplo hipotético siguiente se devuelven los valores **CustomerID** y **SalesOrderID** de todos los pedidos realizaod por los clientes de Canaá.

```sql
SELECT CustomerID, SalesOrderID
FROM Sales.SalesOrderHeader
WHERE CustomerID IN (
    SELECT CustomerID
    FROM Sales.Customer
    WHERE CountryRegion = 'Canada');
```

se compone de una cláusula SELECT, una cláusula FROM, una cláusula WHERE y una subconsulta.

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se seleccionan las columnas CustomerID y SalesOrderID de la tabla "SalesOrderHeader" de la base de datos "Sales".

La cláusula FROM especifica la tabla "SalesOrderHeader" de la que se seleccionarán los datos.

La cláusula WHERE filtra las filas de la tabla "SalesOrderHeader" en función de una condición. En este caso, la condición es CustomerID IN (SELECT CustomerID FROM Sales.Customer WHERE CountryRegion = 'Canada'). Esto significa que solo se seleccionarán las filas en las que la columna CustomerID esté presente en la lista de identificadores de clientes que se devuelven de la subconsulta. En otras palabras, solo se seleccionarán los encabezados de pedidos realizados por clientes que residan en Canadá.

La subconsulta (SELECT CustomerID FROM Sales.Customer WHERE CountryRegion = 'Canada') se ejecuta primero. Esta subconsulta selecciona todos los identificadores de clientes de la tabla "Customer" de la base de datos "Sales" que residen en Canadá.

La consulta principal se ejecuta a continuación, utilizando la lista de identificadores de clientes que se devuelve de la subconsulta como la lista de clientes a seleccionar. Solo se seleccionan los encabezados de pedidos que corresponden a los clientes de Canadá, y se muestran los identificadores de cliente y los identificadores de pedido de cada pedido realizado por estos clientes.

En resumen, esta consulta devuelve los identificadores de cliente y los identificadores de pedido de todos los pedidos realizados por clientes que residan en Canadá.

- La consulta anterior se puede elaborar de una **manera más simple**
```sql
SELECT c.CustomerID, o.SalesOrderID
FROM Sales.Customer AS c
JOIN Sales.SalesOrderHeader AS o
    ON c.CustomerID = o.CustomerID
WHERE c. CountryRegion = 'Canada';
```

**¿Cómo se decide si se escribe una consulta que implica varias tablas como JOIN  o con una subconsulta?**
- A veces, solo depende de con qué se siente más cómodo. La mayoría de las consultas anidadas que se convierten fácilmente eon JOIN  realmente se convertirán en JOIN  de forma interna. En el caso de estas consultas, no hay ninguna diferencia real al escribir la consulat de una manera frete a otra. 
- Una restricción que debe tener en cuenta es que cuando se usea una consulta anidada los resultados devueltos al cliente solo pueden incluir columnas de la consulta externa. Por lo tanto, si tiene que devoler columnas de ambas tablas, debe escribir la consulta mediante JOIN. 
- Por último, hay situaciones en las que la consulta interna necesita realizar operaciones mucho más complicadas que las recuperaciones simples de nuestros ejemplos. La reescritura se subconsultas complejas mediante JOIN  puede ser difícil. Para muchos desarroladores de SQL, las subconsultas funcionan mejor para un proceamiento complicado, ya que permite dividir el procesamiento en pasos más pequeños. 


### Usar subconsultas independientes o correlacionadas

#### Trabajar con subconsultas correlacionadas

EStas igual son instrucciones SELECT anidads dentro de una consulta **externa**. 
- Consideraciones especiales cuando se usa subconsultas correlacionadas:
  - No se pueden ejecutar por separado desde la consulta externa. Esta restricción complica las pruebas y la derpuración.
  - A diferencia de las subconsultas independientes, que se procesan una vez, las subconsultas correlaciondas se ajecutarán varias veces. Lógicamente, la consula extenra se ejecutar primer y, para cada fila devuelta se preocesa la consutla interna.

```sql
SELECT SalesOrderID, CustomerID, OrderDate
FROM SalesLT.SalesOrderHeader AS o1
WHERE SalesOrderID =
    (SELECT MAX(SalesOrderID)
     FROM SalesLT.SalesOrderHeader AS o2
     WHERE o2.CustomerID = o1.CustomerID)
ORDER BY CustomerID, OrderDate;
```

Esta consulta se compone de una cláusula SELECT, una cláusula FROM, una cláusula WHERE, una subconsulta y una cláusula ORDER BY.

La cláusula SELECT especifica las columnas que se mostrarán en el resultado de la consulta. En este caso, se seleccionan las columnas SalesOrderID, CustomerID y OrderDate de la tabla "SalesOrderHeader" de la base de datos "SalesLT".

La cláusula FROM especifica la tabla "SalesOrderHeader" de la que se seleccionarán los datos, y se le da un alias de o1.

La cláusula WHERE filtra las filas de la tabla "SalesOrderHeader" en función de una condición. En este caso, la condición es SalesOrderID = (SELECT MAX(SalesOrderID) FROM SalesLT.SalesOrderHeader AS o2 WHERE o2.CustomerID = o1.CustomerID). Esto significa que solo se seleccionarán las filas en las que el valor de la columna SalesOrderID sea igual al valor máximo de SalesOrderID para el cliente correspondiente. La subconsulta se utiliza para obtener el valor máximo de SalesOrderID para cada cliente, y la condición o2.CustomerID = o1.CustomerID se utiliza para comparar el valor del cliente en la subconsulta con el valor del cliente en la tabla principal.

La subconsulta (SELECT MAX(SalesOrderID) FROM SalesLT.SalesOrderHeader AS o2 WHERE o2.CustomerID = o1.CustomerID) se ejecuta primero para cada fila de la tabla principal. Esta subconsulta selecciona el valor máximo de SalesOrderID de la tabla "SalesOrderHeader" de la base de datos "SalesLT" para el cliente correspondiente.

La consulta principal se ejecuta a continuación, utilizando el valor máximo de SalesOrderID devuelto por la subconsulta como la condición para seleccionar la fila correspondiente de la tabla principal. Solo se seleccionan los encabezados de pedidos que corresponden al valor máximo de SalesOrderID para cada cliente.

La cláusula ORDER BY se utiliza para ordenar los resultados por el valor de la columna CustomerID, y luego por el valor de la columna OrderDate en orden ascendente.

En resumen, esta consulta devuelve los identificadores de pedido, identificadores de cliente y fechas de pedido para el pedido más reciente realizado por cada cliente en la tabla "SalesOrderHeader" de la base de datos "SalesLT", ordenados por el identificador de cliente y luego por la fecha de pedido.






##### Escritura de subconsultas correlacionadas
Para escribir subconsultas correlacionadas, tenga en cuenta las siguientes directrices:

Escriba la consulta externa para aceptar el resultado devuelto adecuado de la consulta interna. Si la consulta interna es escalar, puede usar operadores de igualdad y comparación, como =, <, > y <>, en la cláusula WHERE. Si la consulta interna puede devolver varios valores, use un predicado IN. Cree un plan para controlar los resultados NULL.
Identifique la columna de la consulta externa a la que hará referencia la subconsulta correlacionada. Declare un alias para la tabla que es el origen de la columna en la consulta externa.
Identifique la columna de la tabla interna que se comparará con la columna de la tabla externa. Cree un alias para la tabla de origen, como hizo para la consulta externa.
Escriba la consulta interna para recuperar valores de su origen, en función del valor de entrada de la consulta externa. Por ejemplo, use la columna externa en la cláusula WHERE de la consulta interna.
La correlación entre las consultas interna y externa se produce cuando la consulta interna hace referencia al valor externo para su comparación. Es esta correlación la que proporciona a la subconsulta su nombre.


##### Comprobación de conocimiento:

1. Una consulta con una subconsulta en la cláusula WHERE devuelve el siguiente error: mensaje 512, nivel 16, estado 1, línea 17 La subconsulta ha devuelto más de 1 valor, lo que no es correcto cuando va a continuación de =, !=, <, <= , >, >= o cuando se usa como una expresión. ¿Qué se puede hacer para solucionar este error?

Asegúrese de que la subconsulta no usa SELECT * en la lista SELECT.

Cambie el operador que presenta la subconsulta a IN o NOT IN
Correcto. El error indica que la subconsulta devuelve más de un valor y no se puede comparar una lista de valores con operadores de comparación.


Agregar DISTINCT a la lista SELECT
2. ¿Cuál de las afirmaciones siguientes es correcta con respecto a las subconsultas correlacionadas?

Una subconsulta correlacionada devuelve un único valor escalar

Una subconsulta correlacionada devuelve varias columnas y filas

Una subconsulta correlacionada hace referencia a un valor en la consulta externa
Correcto. Una subconsulta correlacionada hace referencia a un valor en la consulta externa.




# Uso de funciones integradas y GROUP BY en Transact-AQL

## Introducción 

Al recueprar datos de tabas de una base de datos, suele ser útil poder manipular valores de datos mediante *funciones*; para dar formato, convertir, agregar o modificar de cualquier otro modo de salida de consulta. Además, al agregar datos, a menudo querrá *agrupar* los resultados y mostara agregaciones para cada groupo; por ejempllo, para ver los valores totales por categoría. 


## categorizar funciones integradas
Transact-SQL  incluye muchas funciones integradas, desde funciones que convierten tipos de datos hasta funcinoes que agregan y analizan grupos de filas. 

Las funcioens de T-SQL se puden clasificar de la siguiente manera:

**CAtegoría de la función**  | **Descripción**
--- | ---
Escalar | Opera sobre un solo valor y devuelve otro valor. 
Lógicos | Compara varios valores para determinar una única salida. 
Clasificación |  Opera sobre una partición conjunto de filas
Conjunot de filas | Devuelve una tabla virtual que se puede usar en una cláusula Fom de una instrucción T-SQL.
Agregado | Toma uno o vaiors valors de entrada y devuelve un único valor de resumen.


## Usar funciones escalares
Las funcioes escalares deuelven un valor único y normalmente funcionan en una sola fila. El númeor de valores de entrada que toman puede ser cero, uno, o varios. Dado que las funcinoes escalares **siempre devuelven un solo valor**, *se pueden usar en cualquier lugar en el que se necesite un único valor*. SE usar normalmente en cláusulas SELECT y predicaod de cláusula  WHERE. También se pueden usar en clásulas SET  de una isntrucción UPDATE.
 las funcioens escalare sintegradas se puden organizar en muchas categorías, como cadena, conversión, lógica matemática y otras. En este módulo se explican algunas funcinoes escalares comunes.
 Esetas son algunas de las ocnsideraciones que hay que tener en cuenta al usar funciones escalares


- Determinismo: si la funcioens devuelve el mismo valor para el mismo estado de entrada y base de datos cada vez que se llama, se dice que es I determinista .
- Intecalación: cuando se usan fucinoes que manipulan datos de carcteres, 'qué intercalación se usará? Aguans funcies usea la intercalación del valor de entrada, otros usan la intercalación de la base d edatos si no se proporciona ninguna intrcalación de entrada.
  

### Ejemplos de funcioens escalares

Se enumeraón más de 200 funciones escalares que abarcan varias categorías, entre la que incluyen las siguites:
1. Funcinoes de configuración 
2. de conversión 
3. cursores
4. fecha y hora
5. matemáticas
6. metadatos
7. seguridad
8. cadena
9. sistema
10. estadísticas del sistema
11. de texto y de imagen

#### Ejemplo hipotético siguiente se usan **varias funciones de fecha y hora**
La consulta tiene como objetivo mostrar una serie de detalles relacionados con la fecha de los pedidos de venta almacenados en la tabla SalesOrderHeader de la base de datos Sales. Cada columna seleccionada proporciona un detalle específico relacionado con la fecha de pedido.

```sql
SELECT  SalesOrderID,
    OrderDate,
        YEAR(OrderDate) AS OrderYear,
        DATENAME(mm, OrderDate) AS OrderMonth,
        DAY(OrderDate) AS OrderDay,
        DATENAME(dw, OrderDate) AS OrderWeekDay,
        DATEDIFF(yy,OrderDate, GETDATE()) AS YearsSinceOrder
FROM Sales.SalesOrderHeader;
```

A continuación, se explican los pasos que realiza esta consulta:

SELECT: La consulta comienza con una instrucción SELECT que indica las columnas que se desean mostrar en el resultado final. En este caso, la consulta selecciona las columnas SalesOrderID y OrderDate, así como varias columnas que se generan a partir de la fecha de pedido.

FROM: La cláusula FROM especifica la tabla SalesOrderHeader de la base de datos Sales.

YEAR: La función YEAR extrae el año de la fecha del pedido en la columna OrderDate. El resultado se almacena en una nueva columna llamada OrderYear.

DATENAME: La función DATENAME se utiliza dos veces en esta consulta. La primera instancia se utiliza para extraer el nombre del mes de la fecha del pedido. El resultado se almacena en una nueva columna llamada OrderMonth. La segunda instancia se utiliza para extraer el nombre del día de la semana de la fecha del pedido. El resultado se almacena en una nueva columna llamada OrderWeekDay.

DAY: La función DAY se utiliza para extraer el día del mes de la fecha del pedido en la columna OrderDate. El resultado se almacena en una nueva columna llamada OrderDay.

DATEDIFF: La función DATEDIFF se utiliza para calcular la cantidad de años que han pasado desde el pedido. La consulta utiliza la fecha actual GETDATE() como segundo argumento de la función, lo que permite calcular la diferencia en años entre la fecha del pedido y la fecha actual. El resultado se almacena en una nueva columna llamada YearsSinceOrder.

Resultado: El resultado final es una tabla con seis columnas: SalesOrderID, OrderDate, OrderYear, OrderMonth, OrderDay y OrderWeekDay, YearsSinceOrder. Cada fila muestra información detallada sobre un pedido de venta, incluyendo su fecha, año, mes, día del mes, día de la semana y años transcurridos desde que se realizó el pedido.

```sql
SELECT TaxAmt,
       ROUND(TaxAmt, 0) AS Rounded,
       FLOOR(TaxAmt) AS Floor,
       CEILING(TaxAmt) AS Ceiling,
       SQUARE(TaxAmt) AS Squared,
       SQRT(TaxAmt) AS Root,
       LOG(TaxAmt) AS Log,
       TaxAmt * RAND() AS Randomized
FROM Sales.SalesOrderHeader;
```


La consulta selecciona varias columnas de la tabla SalesOrderHeader y realiza una serie de cálculos sobre la columna TaxAmt:

TaxAmt es la columna que almacena el importe de impuestos de cada orden de venta.
ROUND(TaxAmt, 0) redondea el valor de TaxAmt al número entero más cercano.
FLOOR(TaxAmt) devuelve el valor entero más grande que sea menor o igual que TaxAmt.
CEILING(TaxAmt) devuelve el valor entero más pequeño que sea mayor o igual que TaxAmt.
SQUARE(TaxAmt) devuelve el cuadrado de TaxAmt.
SQRT(TaxAmt) devuelve la raíz cuadrada de TaxAmt.
LOG(TaxAmt) devuelve el logaritmo natural de TaxAmt.
TaxAmt * RAND() devuelve un valor aleatorio entre 0 y 1, multiplicado por TaxAmt.
En resumen, la consulta selecciona información sobre los impuestos de las órdenes de venta y realiza una serie de cálculos matemáticos sobre los valores de impuestos.


```sql
SELECT  CompanyName,
        UPPER(CompanyName) AS UpperCase,
        LOWER(CompanyName) AS LowerCase,
        LEN(CompanyName) AS Length,
        REVERSE(CompanyName) AS Reversed,
        CHARINDEX(' ', CompanyName) AS FirstSpace,
        LEFT(CompanyName, CHARINDEX(' ', CompanyName)) AS FirstWord,
        SUBSTRING(CompanyName, CHARINDEX(' ', CompanyName) + 1, LEN(CompanyName)) AS RestOfName
FROM Sales.Customer
```


Esta consulta realiza varias operaciones en la columna "CompanyName" de la tabla "Sales.Customer". Cada operación se describe a continuación:

La función "UPPER" convierte el valor de la columna "CompanyName" en mayúsculas y se asigna a la columna "UpperCase".

La función "LOWER" convierte el valor de la columna "CompanyName" en minúsculas y se asigna a la columna "LowerCase".

La función "LEN" devuelve la longitud del valor en la columna "CompanyName" y se asigna a la columna "Length".

La función "REVERSE" invierte el orden de los caracteres en el valor de la columna "CompanyName" y se asigna a la columna "Reversed".

La función "CHARINDEX" devuelve la posición del primer espacio en blanco en el valor de la columna "CompanyName" y se asigna a la columna "FirstSpace". Si no hay un espacio en blanco, devuelve 0.

La función "LEFT" devuelve la parte izquierda del valor en la columna "CompanyName" hasta el primer espacio en blanco y se asigna a la columna "FirstWord".

La función "SUBSTRING" devuelve la parte del valor en la columna "CompanyName" que sigue al primer espacio en blanco y se asigna a la columna "RestOfName".

Finalmente, la consulta selecciona todas las columnas que se han calculado y las devuelve en una única tabla.


##### Funciones lógicas

###### IF

```sql
SELECT AddressType,
      IIF(AddressType = 'Main Office', 'Billing', 'Mailing') AS UseAddressFor
FROM Sales.CustomerAddress;
```
Esta consulta devuelve dos columnas de la tabla "Sales.CustomerAddress":

"AddressType": el tipo de dirección.

"UseAddressFor": una nueva columna que se calcula utilizando la función "IIF".

La función "IIF" es una función condicional que devuelve un valor según una expresión booleana. En este caso, la expresión es "AddressType = 'Main Office'", lo que significa que si el valor de "AddressType" es "Main Office", entonces el valor de "UseAddressFor" será "Billing". De lo contrario, el valor de "UseAddressFor" será "Mailing".

Por lo tanto, para cada fila en la tabla "Sales.CustomerAddress", la función "IIF" evalúa si el valor de "AddressType" es "Main Office" o no y devuelve el valor correspondiente en la columna "UseAddressFor".

La consulta devuelve ambas columnas para todas las filas de la tabla "Sales.CustomerAddress".

###### CHOOSE

```sql
SELECT SalesOrderID, Status,
CHOOSE(Status, 'Ordered', 'Shipped', 'Delivered') AS OrderStatus
FROM Sales.SalesOrderHeader;
```
Esta consulta utiliza la función "CHOOSE" para devolver una cadena de caracteres basada en el valor de la columna "Status" en la tabla "Sales.SalesOrderHeader". La consulta se describe a continuación:

La consulta selecciona las columnas "SalesOrderID" y "Status" de la tabla "Sales.SalesOrderHeader".

La función "CHOOSE" toma la columna "Status" como su primer argumento y devuelve una cadena de caracteres basada en su valor.

Si el valor de "Status" es 1, la función "CHOOSE" devuelve 'Ordered'. Si es 2, devuelve 'Shipped'. Si es 3, devuelve 'Delivered'.

El resultado de la función "CHOOSE" se asigna a una columna llamada "OrderStatus".

La consulta devuelve las columnas "SalesOrderID", "Status" y "OrderStatus" en una única tabla.

En resumen, esta consulta crea una nueva columna llamada "OrderStatus" que contiene el estado de la orden ("Ordered", "Shipped" o "Delivered") basado en el valor de la columna "Status".



## Utilizar funcinoes de clasificación y conjunto de filas


- Las vuncinoes de clasificación y conjunto de filas no son funcinoes escalares porque no devuelven un solo valor. Estas funcnioes aceptan un conjunto de filas como entrada y devuelven un conjunto de filas como salida.

### Funcnieos de categoría

- Las funcnoies de clasificación permiten realizar cálculos en un conjunot de filas definido por el usuario. Estas funcnioes incluyen funcnieos de clasificación, deplzamiento, agregao y distribución.

En este ejemplo se usa la función RANK  para calcular una clasificación basada en ListPrice, con el precino más alto clasificado en 1:


```sql
SELECT TOP 100 ProductID, Name, ListPrice,
RANK() OVER(ORDER BY ListPrice DESC) AS RankByPrice
FROM Production.Product AS p
ORDER BY RankByPrice;
```
La siguiente consulta selecciona los primeros 100 productos de la tabla "Production.Product" y devuelve las columnas "ProductID", "Name" y "ListPrice". Además, utiliza la función RANK() para asignar una clasificación a cada producto en función de su precio en orden descendente. Cada operación se describe a continuación:

La cláusula "SELECT" selecciona las columnas "ProductID", "Name" y "ListPrice".

La función "RANK()" devuelve la clasificación de cada producto en función de su precio en orden descendente. La cláusula "OVER" indica que se aplicará la función RANK() a todo el conjunto de resultados y la cláusula "ORDER BY ListPrice DESC" indica que se ordenará el conjunto de resultados en orden descendente según el precio.

La cláusula "AS" asigna el alias "RankByPrice" a la clasificación obtenida.

La cláusula "FROM" indica que se seleccionarán los datos de la tabla "Production.Product" y se le asigna el alias "p".

La cláusula "ORDER BY" ordena los resultados por la columna "RankByPrice".

La cláusula "TOP 100" indica que se seleccionarán los primeros 100 productos de la tabla "Production.Product".

En resumen, la consulta selecciona los primeros 100 productos de la tabla "Production.Product" y devuelve su "ProductID", "Name" y "ListPrice". Además, utiliza la función RANK() para asignar una clasificación a cada producto en función de su precio en orden descendente, y los resultados se ordenan según la clasificación obtenida.

### Over
Puede usar la cláusla OVER para definir particiones o agroupaciones dentro de los datos. Por ejemplo, la consulta siguiente amplía el ejemplo anterior para calcular las clasificaciones basadas en precios de los productos dentro de cada caategoría.


```sql
SELECT c.Name AS Category, p.Name AS Product, ListPrice,
  RANK() OVER(PARTITION BY c.Name ORDER BY ListPrice DESC) AS RankByPrice
FROM Production.Product AS p
JOIN Production.ProductCategory AS c
ON p.ProductCategoryID = c.ProductcategoryID
ORDER BY Category, RankByPrice;
```

**NOTA**: Observe que varias filas tienen el mismo valor de clasificación y que han omitido algunos valores. Esto de debe a que solo estamos usando RANK. En función del requisito, es posible que quiera evitar vínculos con el mismo valor de clasificaicón. Puede controlar el valor de clasificación con otras funcnioes, DENSE_RANK, NTILE y ROW_NUMBER, según sea necesario. Para obtener más información sobre estas funcineos, consulte la documentación de Transct-SQL.

La consulta realiza lo siguiente:

Selecciona las columnas "c.Name" (alias "Category"), "p.Name" (alias "Product"), "ListPrice" y una nueva columna llamada "RankByPrice".

La tabla principal de la consulta es "Production.Product", que se une con "Production.ProductCategory" usando la cláusula JOIN, uniendo las columnas "ProductCategoryID" y "ProductcategoryID".

La función de clasificación "RANK" se usa para asignar un rango a cada producto dentro de su categoría según su precio de lista ("ListPrice"). La cláusula "OVER" especifica que la clasificación se realizará para cada categoría por separado (PARTITION BY c.Name), y que los productos se ordenarán de mayor a menor precio (ORDER BY ListPrice DESC).

El resultado de la función de clasificación se almacena en la columna "RankByPrice".

La consulta ordena los resultados por "Category" y "RankByPrice".

En resumen, la consulta devuelve una lista de productos junto con su precio de lista, su categoría correspondiente y el rango que ocupan en términos de precio dentro de su categoría. Los resultados se ordenan primero por categoría y luego por rango de precio.



### Funciones de conjuntos de filas

Las funcnies de conjunto de filas devuelven una tabla virtual que se puede usar en la cláusula FROM  como origdne de datos. Esstas funcioes toiman parámetros específicos de la propia función de conjunto de filas. Incluyen OPENDATASROUCE, OPENQUERY OPENWORSET, OPENXML y OPERNJSON.
las fucnioes IOPENDATASOURCE, OPENQUERY y OPEROWSET permiten pasar una consulta a un servidor de base de datos remoto. A continuación, el servidor remoto devolverá un conjunto de finals de resultados. Por ejemplo, la consulta siguietne usar OPENROWSET para obtener los resultados d euna ocnsulta de una instalncia SQL Server llamada SALSEDEB.

```sql
SELECT a.*
FROM OPENROWSET('SQLNCLI', 'Server=SalesDB;Trusted_Connection=yes;',
    'SELECT Name, ListPrice
    FROM AdventureWorks.Production.Product') AS a;
```

La consulta utiliza la función OPENROWSET para obtener los datos de la tabla "Production.Product" de la base de datos "AdventureWorks" y devolverlos en una única tabla.

Los pasos que realiza la consulta son los siguientes:

La función "OPENROWSET" se utiliza para acceder a los datos de una fuente externa y devuelve una tabla.

Se especifica el proveedor OLE DB 'SQLNCLI' para conectarse a la base de datos. Se establece la conexión con el servidor 'SalesDB' y se indica que se debe utilizar la autenticación de Windows ('Trusted_Connection=yes').

Se define la consulta que se utilizará para obtener los datos de la tabla "Production.Product" en la base de datos "AdventureWorks". La consulta selecciona las columnas "Name" y "ListPrice" de la tabla.

La consulta se utiliza como una subconsulta dentro de la función "OPENROWSET" y se asigna el alias "a" a la tabla resultante.

Se utiliza la cláusula "SELECT" para seleccionar todas las columnas de la tabla "a" y se devuelve el resultado de la consulta en una única tabla.

En resumen, la consulta utiliza la función "OPENROWSET" para obtener los datos de una tabla externa y devuelve los resultados en una única tabla que contiene las columnas "Name" y "ListPrice" de la tabla "Production.Product" de la base de datos "AdventureWorks".

Para usar servidores remotos, debe habilitar algunas opciones avanzadas en la instancia de SQL Server en la que se ejecuta la consulta.

Las funciones OPENXML y OPENJSON permiten consultar datos estructurados en formato XML o JSON y extraer valores en un conjunto de filas tabular.


## Uso de funciones de agregado

T-SQL  proporciona funcnieos de agregado como SUM, MAX y AVG para realizar cálculos que toman varios valores y devuelven un único resultado.

### Uso de funcinoes de agregado

Al trabajar con funciones de agregado, debe tener en cuenta los siguientes puntos:

Las funciones de agregado devuelven un único valor (escalar) y se pueden usar en instrucciones SELECT casi en cualquier lugar en el que se pueda usar un solo valor. Por ejemplo, estas funciones se pueden usar en las cláusulas SELECT, HAVING y ORDER BY. Sin embargo, no se pueden usar en la cláusula WHERE.
Las funciones de agregado omiten los valores NULL, excepto cuando se usa COUNT(*).
Las funciones de agregado de una lista SELECT no tienen un encabezado de columna, a menos que proporcione un alias mediante AS.
Las funciones de agregado de una lista SELECT funcionan en todas las filas que se pasan a la operación SELECT. Si no hay ninguna cláusula GROUP BY, se resumirán todas las filas que cumplan cualquier filtro de la cláusula WHERE. Obtendrá más información sobre GROUP BY en la unidad siguiente.
A menos que use GROUP BY, no debe combinar funciones de agregado con columnas no incluidas en las funciones de la misma lista SELECT.


### funcnioes de agregado integradas

- Como se mencionnó, Transact-SQL proporciona muchas funcnioes de agregado integradas. Estas son algunas d elas más comunes 

**Nombre de la función** | **Sintaxis** | **Descripción**
--- | --- |---
SUM | SUM( *expression*) | Suma todos los valores numéricos no  NULL  de una columna.
MEDIA | AVG(*expression*) | Promedia todos los valores numéricos no NULL  de una columna (sima/recuento).
MIN | MIN(expression) | Devuelve el número más pequeño, la fecha y ho ra más tempranas o la cadne aque se produce por primera vez.
MAX | MAX(expression) | Devuelve le número más grand, la fecha y hora más reciente o la última cadena.
COUNT o COUNT_BIG | COUNT(*) o COUNT(*expression*) | Con (*), se eucnta todas las filas, incluidas las filas con valores NULL. Cuando se especifica una columna como Iespresión devuelve el recuneto de filas que no son NULL para esa columna. COUNT  devuelve un valor int; COUNT_BIG devuelve un valor big_int.


Comprobación de conocimiento
Completado
200 XP
5 minutos
1. Ejecute la consulta siguiente: SELECT OrderNo, CHOOSE(Status, 'Ordered', 'Shipped', 'Delivered') AS OrderState FROM Sales.Order; Which OrderState. ¿Qué valor OrderState se devuelve para las filas con un valor status de 2?

Enviado
Correcto. CHOOSE devuelve el valor en función de su posición ordinal basada en 1.


Delivered (Entregado)

NULL
2. Debe devolver el número de clientes de cada ciudad. ¿Qué consulta debo usar?

SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer;

SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer GROUP BY City;
Correcto. Use GROUP BY para agregar por grupos.


SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer ORDER BY City;
Incorrecto. Use GROUP BY para agregar por grupos.

3. Debe devolver una fila para cada categoría con un precio medio superior a 10,00. ¿Qué consulta debo usar?

SELECT Category, AVG(Price) FROM Store.Product WHERE AVG(Price) > 10.00;

SELECT Category, AVG(Price) FROM Store.Product GROUP BY Category WHERE AVG(Price) > 10.00;
Incorrecto. Use una cláusula HAVING para filtrar grupos.


SELECT Category, AVG(Price) FROM Store.Product GROUP BY Category HAVING AVG(Price) > 10.00;
Correcto. Use una cláusula HAVING para filtrar grupos.
3


1. Ejecute la consulta siguiente: SELECT OrderNo, CHOOSE(Status, 'Ordered', 'Shipped', 'Delivered') AS OrderState FROM Sales.Order; Which OrderState. ¿Qué valor OrderState se devuelve para las filas con un valor status de 2?

**Enviado**
Correcto. CHOOSE devuelve el valor en función de su posición ordinal basada en 1.


Delivered (Entregado)

NULL
2. Debe devolver el número de clientes de cada ciudad. ¿Qué consulta debo usar?

SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer;

SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer GROUP BY City;
Correcto. Use GROUP BY para agregar por grupos.


SELECT City, COUNT(*) AS CustomerCount FROM Sales.Customer ORDER BY City;
3. Debe devolver una fila para cada categoría con un precio medio superior a 10,00. ¿Qué consulta debo usar?

SELECT Category, AVG(Price) FROM Store.Product WHERE AVG(Price) > 10.00;

SELECT Category, AVG(Price) FROM Store.Product GROUP BY Category WHERE AVG(Price) > 10.00;

SELECT Category, AVG(Price) FROM Store.Product GROUP BY Category HAVING AVG(Price) > 10.00;
Correcto. Use una cláusula HAVING para filtrar grupos.