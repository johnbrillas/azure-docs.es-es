---
title: Guía de programación de aplicadores definidos por el usuario de U-SQL para Azure Data Lake
description: Obtenga información sobre los programadores definidos por el usuario en la guía de programación de UDO con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 0842a2cfa021ef8ea45c19ec885c7dec371730de
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512465"
---
# <a name="use-user-defined-applier"></a>Uso de aplicador definido por el usuario 

## <a name="u-sql-udo-user-defined-applier"></a>UDO de U-SQL: aplicador definido por el usuario
Un aplicador definido por el usuario U-SQL permite invocar una función de C# personalizada para cada fila devuelta por la expresión de tabla externa de una consulta. La entrada de la derecha se evalúa para cada fila de la entrada izquierda y las filas producidas se combinan en la salida final. La lista de las columnas producidas por el operador APPLY es la combinación del conjunto de columnas de la entrada izquierda y derecha.


## <a name="how-to-define-and-use-user-defined-applier"></a>Definición y uso de un aplicador definido por el usuario
El aplicador definido por el usuario se invoca como parte de la expresión USQL SELECT.

La llamada típica al aplicador definido por el usuario tiene un aspecto parecido al siguiente:

```usql
SELECT …
FROM …
CROSS APPLYis used to pass parameters
new MyScript.MyApplier(param1, param2) AS alias(output_param1 string, …);
```

Consulte [U-SQL SELECT Selecting from CROSS APPLY and OUTER APPLY](/u-sql/statements-and-expressions/select/from/select-selecting-from-cross-apply-and-outer-apply) (U-SQL SELECT, selección de CROSS APPLY y OUTER APPLY) para más información sobre cómo utilizar aplicadores en una expresión SELECT.

La definición de clase base de aplicador definida por el usuario es la siguiente:

```csharp
public abstract class IApplier : IUserDefinedOperator
{
protected IApplier();

public abstract IEnumerable<IRow> Apply(IRow input, IUpdatableRow output);
}
```

Para definir un aplicador definido por el usuario, es necesario crear la interfaz `IApplier` con el atributo [`SqlUserDefinedApplier`], que es opcional para la definición de este aplicador.

```csharp
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
    public ParserApplier()
    {
        …
    }

    public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
    {
        …
    }
}
```

* Se llama a Apply para cada fila de la tabla externa. Devuelve el conjunto de filas de salida `IUpdatableRow`.
* La clase Constructor se utiliza para pasar parámetros al aplicador definido por el usuario.

**SqlUserDefinedApplier** indica que el tipo se debe registrar como un aplicador definido por el usuario. No se puede heredar esta clase.

**SqlUserDefinedApplier** es **opcional** para la definición del aplicador definido por el usuario.


Los objetos de programación principales son los siguientes:

```csharp
public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
```

Los conjuntos de filas de entrada se pasan como entrada `IRow`. Las filas de salida se generan como interfaz de salida `IUpdatableRow`.

Los nombres de columna individuales se pueden determinar mediante una llamada al método Schema `IRow`.

```csharp
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Para obtener los valores de datos reales a partir de `IRow` de entrada, utilizamos el método Get() de la interfaz `IRow`.

```csharp
mycolumn = row.Get<int>("mycolumn")
```

O bien, se utiliza el nombre de columna de esquema:

```csharp
row.Get<int>(row.Schema[0].Name)
```

Los valores de salida se deben establecer con la salida `IUpdatableRow`:

```csharp
output.Set<int>("mycolumn", mycolumn)
```

Es importante comprender que los aplicadores personalizados solo darán como resultado columnas y valores definidos con la llamada al método `output.Set`.

La salida real se desencadena mediante una llamada a `yield return output.AsReadOnly();`.

Los parámetros del aplicador definido por el usuario se pueden pasar al constructor. El aplicador puede devolver un número variable de columnas que debe definirse durante la llamada al aplicador en el script base U-SQL.

```csharp
new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);
```

Este es el ejemplo del aplicador definido por el usuario:

```csharp
[SqlUserDefinedApplier]
public class ParserApplier : IApplier
{
private string parsingPart;

public ParserApplier(string parsingPart)
{
    if (parsingPart.ToUpper().Contains("ALL")
    || parsingPart.ToUpper().Contains("MAKE")
    || parsingPart.ToUpper().Contains("MODEL")
    || parsingPart.ToUpper().Contains("YEAR")
    || parsingPart.ToUpper().Contains("TYPE")
    || parsingPart.ToUpper().Contains("MILLAGE")
    )
    {
    this.parsingPart = parsingPart;
    }
    else
    {
    throw new ArgumentException("Incorrect parameter. Please use: 'ALL[MAKE|MODEL|TYPE|MILLAGE]'");
    }
}

public override IEnumerable<IRow> Apply(IRow input, IUpdatableRow output)
{

    string[] properties = input.Get<string>("properties").Split(',');

    //  only process with correct number of properties
    if (properties.Count() == 5)
    {

    string make = properties[0];
    string model = properties[1];
    string year = properties[2];
    string type = properties[3];
    int millage = -1;

    // Only return millage if it is number, otherwise, -1
    if (!int.TryParse(properties[4], out millage))
    {
        millage = -1;
    }

    if (parsingPart.ToUpper().Contains("MAKE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("make", make);
    if (parsingPart.ToUpper().Contains("MODEL") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("model", model);
    if (parsingPart.ToUpper().Contains("YEAR") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("year", year);
    if (parsingPart.ToUpper().Contains("TYPE") || parsingPart.ToUpper().Contains("ALL")) output.Set<string>("type", type);
    if (parsingPart.ToUpper().Contains("MILLAGE") || parsingPart.ToUpper().Contains("ALL")) output.Set<int>("millage", millage);
    }
    yield return output.AsReadOnly();            
}
}
```

Este es el script base U-SQL para este aplicador definido por el usuario:

```usql
DECLARE @input_file string = @"c:\usql-programmability\car_fleet.tsv";
DECLARE @output_file string = @"c:\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
        stocknumber int,
        vin String,
        properties String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
    SELECT
        r.stocknumber,
        r.vin,
        properties.make,
        properties.model,
        properties.year,
        properties.type,
        properties.millage
    FROM @rs0 AS r
    CROSS APPLY
    new USQL_Programmability.ParserApplier ("all") AS properties(make string, model string, year string, type string, millage int);

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```

En este escenario de caso de uso, el aplicador definido por el usuario actúa como un analizador de valores delimitados por comas para las propiedades de flota de automóviles. Las filas del archivo de entrada tienen el siguiente aspecto:

```text
103 Z1AB2CD123XY45889   Ford,Explorer,2005,SUV,152345
303 Y0AB2CD34XY458890   Chevrolet,Cruise,2010,4Dr,32455
210 X5AB2CD45XY458893   Nissan,Altima,2011,4Dr,74000
```

Es un archivo TSV delimitado por tabulaciones típico con la columna de propiedades que contiene las propiedades de los automóviles, como la marca y el modelo. Esas propiedades deben analizarse con las columnas de tabla. El aplicador proporcionado también permite generar un número dinámico de propiedades en el conjunto de filas de resultados, según el parámetro que se pasa. Puede generar todas las propiedades o solo un conjunto específico de propiedades.

```text
...USQL_Programmability.ParserApplier ("all")
...USQL_Programmability.ParserApplier ("make")
...USQL_Programmability.ParserApplier ("make&model")
```

Se puede llamar al aplicador definido por el usuario como una nueva instancia del objeto applier:

```usql
CROSS APPLY new MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```

O con la invocación de un método de fábrica de contenedor:

```csharp
    CROSS APPLY MyNameSpace.MyApplier (parameter: "value") AS alias([columns types]…);
```


## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)