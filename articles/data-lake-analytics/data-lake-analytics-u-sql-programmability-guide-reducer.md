---
title: Guía de programación de reductores definidos por el usuario de U-SQL para Azure Data Lake
description: Obtenga información sobre los reductores definidos por el usuario en la guía de programación de UDO con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 52d44685678c3e89dc820042a7925d284500cef8
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512461"
---
# <a name="use-user-defined-reducer"></a>Uso de reductor definido por el usuario

## <a name="u-sql-udo-user-defined-reducer"></a>UDO de U-SQL: reductor definido por el usuario

U-SQL le permite escribir reductores de conjuntos de filas personalizados en C# mediante el marco de extensibilidad de operadores definidos por el usuario mediante la implementación de una interfaz IReducer.

El reductor definido por el usuario o UDR puede usarse para eliminar las filas innecesarias durante la extracción de datos (importar). También se puede utilizar para manipular y evaluar las filas y columnas. En función de la lógica de programación, puede definir también qué filas se deben extraer.

## <a name="how-to-define-and-use-user-defined-reducer"></a>Definición y uso de un reductor definido por el usuario
Para definir una clase UDR, necesitamos crear una interfaz `IReducer` con el atributo `SqlUserDefinedReducer` opcional.

Esta interfaz de clase debe contener una definición de la invalidación del conjunto de filas de interfaz `IEnumerable`.

```csharp
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
        …
    }

}
```

El atributo **SqlUserDefinedReducer** indica que el tipo se debe registrar como un reductor definido por el usuario. No se puede heredar esta clase.
**SqlUserDefinedReducer** es un atributo opcional para la definición del reductor definido por el usuario. Se utiliza para definir la propiedad IsRecursive.

* bool     IsRecursive    
* **true** = indica si esta reductor es asociativo y conmutativo

Los objetos de programación principales son **input** y **output**. El objeto input se utiliza para enumerar las filas de entrada. Output se usa para establecer filas de salida como resultado de la actividad de reducción.

Para la enumeración de filas de entrada, usamos el método `Row.Get`.

```csharp
foreach (IRow row in input.Rows)
{
    row.Get<string>("mycolumn");
}
```

El parámetro para el método `Row.Get` es una columna que se pasa como parte de la clase `PRODUCE` de la instrucción `REDUCE` del script base U-SQL. También es necesario usar aquí el tipo de datos correcto.

Para la salida, use el método `output.Set`.

Es importante comprender que el reductor personalizado solo dará como resultado valores definidos con la llamada al método `output.Set`.

```csharp
output.Set<string>("mycolumn", guid);
```

La salida del reductor real se desencadena mediante una llamada a `yield return output.AsReadOnly();`.

Este es un ejemplo de reductor:

```csharp
[SqlUserDefinedReducer]
public class EmptyUserReducer : IReducer
{

    public override IEnumerable<IRow> Reduce(IRowset input, IUpdatableRow output)
    {
    string guid;
    DateTime dt;
    string user;
    string des;

    foreach (IRow row in input.Rows)
    {
        guid = row.Get<string>("guid");
        dt = row.Get<DateTime>("dt");
        user = row.Get<string>("user");
        des = row.Get<string>("des");

        if (user.Length > 0)
        {
        output.Set<string>("guid", guid);
        output.Set<DateTime>("dt", dt);
        output.Set<string>("user", user);
        output.Set<string>("des", des);

        yield return output.AsReadOnly();
        }
    }
    }

}
```

En este escenario de caso de uso, el reductor omite filas con un nombre de usuario vacío. Para cada fila del conjunto de filas lee cada columna necesaria, después evalúa la longitud del nombre de usuario. Y genera la fila real solo si la longitud del valor del nombre de usuario es mayor que 0.

Este es un script base U-SQL que utiliza un reductor personalizado:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file_reducer.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid string,
        dt DateTime,
            user String,
            des String
    FROM @input_file 
    USING Extractors.Tsv();

@rs1 =
    REDUCE @rs0 PRESORT guid
    ON guid  
    PRODUCE guid string, dt DateTime, user String, des String
    USING new USQL_Programmability.EmptyUserReducer();

@rs2 =
    SELECT guid AS start_id,
           dt AS start_time,
           DateTime.Now.ToString("M/d/yyyy") AS Nowdate,
           USQL_Programmability.CustomFunctions.GetFiscalPeriodWithCustomType(dt).ToString() AS start_fiscalperiod,
           user,
           des
    FROM @rs1;

OUTPUT @rs2 
    TO @output_file 
    USING Outputters.Text();
```

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)