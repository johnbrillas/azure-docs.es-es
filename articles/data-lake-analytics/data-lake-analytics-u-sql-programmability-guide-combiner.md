---
title: Guía de programación de combinadores definidos por el usuario con U-SQL para Azure Data Lake
description: Obtenga información sobre los combinadores definidos por el usuario en la guía de programación de UDO con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: a6c560cf4ec11197183711656d69024591e7008c
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512495"
---
# <a name="use-user-defined-combiner"></a>Uso de combinador definido por el usuario

## <a name="u-sql-udo-user-defined-combiner"></a>UDO de U-SQL: combinador definido por el usuario
El combinador definido por el usuario o UDC permite combinar filas de conjuntos de filas izquierdos y derechos, en función de una lógica personalizada. El combinador definido por el usuario se utiliza con la expresión de COMBINE.

## <a name="how-to-define-and-use-user-defined-combiner"></a>Definición y uso de combinadores definidos por el usuario

Se invoca a un combinador con la expresión COMBINE que proporciona la información necesaria sobre los conjuntos de filas de entrada, las columnas de agrupamiento, el esquema de resultados esperado e información adicional.

Para llamar a un combinador en un script base U-SQL, se usa la sintaxis siguiente:

```usql
Combine_Expression :=
    'COMBINE' Combine_Input
    'WITH' Combine_Input
    Join_On_Clause
    Produce_Clause
    [Readonly_Clause]
    [Required_Clause]
    USING_Clause.
```

Para más información, vea [COMBINE Expression (U-SQL)](/u-sql/statements-and-expressions/combine-expression) (Expresión COMBINE (U-SQL)).

Para definir un combinador definido por el usuario, es necesario crear la interfaz `ICombiner` con el atributo [`SqlUserDefinedCombiner`], que es opcional para la definición de dicho combinador.

Definición de clase base `ICombiner`:

```csharp
public abstract class ICombiner : IUserDefinedOperator
{
protected ICombiner();
public virtual IEnumerable<IRow> Combine(List<IRowset> inputs,
       IUpdatableRow output);
public abstract IEnumerable<IRow> Combine(IRowset left, IRowset right,
       IUpdatableRow output);
}
```

La implementación personalizada de una interfaz `ICombiner` debe contener la definición de una invalidación Combine `IEnumerable<IRow>`.

```csharp
[SqlUserDefinedCombiner]
public class MyCombiner : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    …
}
}
```

El atributo **SqlUserDefinedCombiner** indica que el tipo se debe registrar como un combinador definido por el usuario. No se puede heredar esta clase.

**SqlUserDefinedCombiner** se utiliza para definir la propiedad del modo de combinador. Es un atributo opcional para la definición de un combinador definido por el usuario.

Modo CombinerMode

La enumeración de CombinerMode puede tomar los siguientes valores:

* Full (0) Cada fila de salida depende potencialmente de todas las filas de entrada desde la izquierda y derecha con el mismo valor de clave.

* Left (1) Cada fila de salida depende de una sola fila de entrada de la izquierda (y potencialmente todas las filas de la derecha con el mismo valor de clave).

* Right (2) Cada fila de salida depende de una sola fila de entrada de la derecha (y potencialmente todas las filas de la izquierda con el mismo valor de clave).

* Inner (3) Cada fila de salida depende de una sola fila de entrada de la izquierda y derecha con el mismo valor.

Ejemplo:    [`SqlUserDefinedCombiner(Mode=CombinerMode.Left)`]


Los objetos de programación principales son:

```csharp
    public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
        IUpdatableRow output
```

Los conjuntos de filas de entrada se pasan como el tipo de interfaz `IRowset` **left** y **right**. Ambos conjuntos de filas deben enumerarse para su procesamiento. Solo puede enumerar una vez cada interfaz, por lo que se debe enumerar y almacenar en caché si es necesario.

Para el almacenamiento en caché, se puede crear un tipo de estructura de memoria List\<T\> como resultado de la ejecución de la consulta LINQ y, más concretamente, List<`IRow`>. El tipo de datos anónimo también se puede utilizar durante la enumeración.

Consulte [Introducción a las consultas LINQ (C#)](/dotnet/csharp/programming-guide/concepts/linq/introduction-to-linq-queries) para más información sobre las consultas LINQ e [Interfaz IEnumerable\<T\>](/dotnet/api/system.collections.generic.ienumerable-1) para más información sobre la interfaz IEnumerable\<T\>.

Para obtener los valores de datos reales a partir de `IRowset` de entrada, utilizamos el método Get() de la interfaz `IRow`.

```csharp
mycolumn = row.Get<int>("mycolumn")
```

Los nombres de columna individuales se pueden determinar mediante una llamada al método Schema `IRow`.

```csharp
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

O bien, se utiliza el nombre de columna de esquema:

```csharp
c# row.Get<int>(row.Schema[0].Name)
```

La enumeración general con LINQ tiene el siguiente aspecto:

```csharp
var myRowset =
            (from row in left.Rows
                          select new
                          {
                              Mycolumn = row.Get<int>("mycolumn"),
                          }).ToList();
```

Después de enumerar ambos conjuntos de filas, vamos a crear un bucle a través de todas las filas. Para cada fila del conjunto de filas izquierdo, vamos a buscar todas las filas que satisfagan la condición de nuestro combinador.

Los valores de salida se deben establecer con la salida `IUpdatableRow`.

```csharp
output.Set<int>("mycolumn", mycolumn)
```

La salida real se desencadena mediante una llamada a `yield return output.AsReadOnly();`.

Este es un ejemplo de combinador:

```csharp
[SqlUserDefinedCombiner]
public class CombineSales : ICombiner
{

public override IEnumerable<IRow> Combine(IRowset left, IRowset right,
    IUpdatableRow output)
{
    var internetSales =
    (from row in left.Rows
          select new
          {
              ProductKey = row.Get<int>("ProductKey"),
              OrderDateKey = row.Get<int>("OrderDateKey"),
              SalesAmount = row.Get<decimal>("SalesAmount"),
              TaxAmt = row.Get<decimal>("TaxAmt")
          }).ToList();

    var resellerSales =
    (from row in right.Rows
     select new
     {
     ProductKey = row.Get<int>("ProductKey"),
     OrderDateKey = row.Get<int>("OrderDateKey"),
     SalesAmount = row.Get<decimal>("SalesAmount"),
     TaxAmt = row.Get<decimal>("TaxAmt")
     }).ToList();

    foreach (var row_i in internetSales)
    {
    foreach (var row_r in resellerSales)
    {

        if (
        row_i.OrderDateKey > 0
        && row_i.OrderDateKey < row_r.OrderDateKey
        && row_i.OrderDateKey == 20010701
        && (row_r.SalesAmount + row_r.TaxAmt) > 20000)
        {
        output.Set<int>("OrderDateKey", row_i.OrderDateKey);
        output.Set<int>("ProductKey", row_i.ProductKey);
        output.Set<decimal>("Internet_Sales_Amount", row_i.SalesAmount + row_i.TaxAmt);
        output.Set<decimal>("Reseller_Sales_Amount", row_r.SalesAmount + row_r.TaxAmt);
        }

    }
    }
    yield return output.AsReadOnly();
}
}
```

En este escenario de caso de uso, vamos a crear un informe de análisis para cada distribuidor. El objetivo es encontrar todos los productos cuyo costo es superior a 20 000 USD y que se venden a través del sitio web de Internet más rápido que mediante el distribuidor convencional dentro de un determinado período de tiempo.

Aquí está el script base U-SQL: Puede comparar la lógica entre JOIN convencional y un combinador:

```sql
DECLARE @LocalURI string = @"\usql-programmability\";

DECLARE @input_file_internet_sales string = @LocalURI+"FactInternetSales.txt";
DECLARE @input_file_reseller_sales string = @LocalURI+"FactResellerSales.txt";
DECLARE @output_file1 string = @LocalURI+"output_file1.tsv";
DECLARE @output_file2 string = @LocalURI+"output_file2.tsv";

@fact_internet_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    CustomerKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_internet_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@fact_reseller_sales =
EXTRACT
    ProductKey int ,
    OrderDateKey int ,
    DueDateKey int ,
    ShipDateKey int ,
    ResellerKey int ,
    EmployeeKey int ,
    PromotionKey int ,
    CurrencyKey int ,
    SalesTerritoryKey int ,
    SalesOrderNumber String ,
    SalesOrderLineNumber  int ,
    RevisionNumber int ,
    OrderQuantity int ,
    UnitPrice decimal ,
    ExtendedAmount decimal,
    UnitPriceDiscountPct float ,
    DiscountAmount float ,
    ProductStandardCost decimal ,
    TotalProductCost decimal ,
    SalesAmount decimal ,
    TaxAmt decimal ,
    Freight decimal ,
    CarrierTrackingNumber String,
    CustomerPONumber String
FROM @input_file_reseller_sales
USING Extractors.Text(delimiter:'|', encoding: Encoding.Unicode);

@rs1 =
SELECT
    fis.OrderDateKey,
    fis.ProductKey,
    fis.SalesAmount+fis.TaxAmt AS Internet_Sales_Amount,
    frs.SalesAmount+frs.TaxAmt AS Reseller_Sales_Amount
FROM @fact_internet_sales AS fis
     INNER JOIN @fact_reseller_sales AS frs
     ON fis.ProductKey == frs.ProductKey
WHERE
    fis.OrderDateKey < frs.OrderDateKey
    AND fis.OrderDateKey == 20010701
    AND frs.SalesAmount+frs.TaxAmt > 20000;

@rs2 =
COMBINE @fact_internet_sales AS fis
WITH @fact_reseller_sales AS frs
ON fis.ProductKey == frs.ProductKey
PRODUCE OrderDateKey int,
        ProductKey int,
        Internet_Sales_Amount decimal,
        Reseller_Sales_Amount decimal
USING new USQL_Programmability.CombineSales();

OUTPUT @rs1 TO @output_file1 USING Outputters.Tsv();
OUTPUT @rs2 TO @output_file2 USING Outputters.Tsv();
```

Se puede llamar al combinador definido por el usuario como una nueva instancia del objeto applier:

```csharp
USING new MyNameSpace.MyCombiner();
```


O con la invocación de un método de fábrica de contenedor:

```csharp
USING MyNameSpace.MyCombiner();
```

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)