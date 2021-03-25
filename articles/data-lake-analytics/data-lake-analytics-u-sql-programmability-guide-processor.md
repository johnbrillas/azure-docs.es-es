---
title: Guía de programación de procesadores definidos por el usuario de U-SQL para Azure Data Lake
description: Obtenga información sobre los procesadores definidos por el usuario en la guía de programación de UDO de con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 6ff45c577e94a8c63bd7cb1e6603e4d5519af5c6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512494"
---
# <a name="use-user-defined-processor"></a>Uso de procesador definido por el usuario

## <a name="u-sql-udo-user-defined-processor"></a>U-SQL UDO: procesador definido por el usuario
El procesador definido por el usuario o UDP es un tipo de UDO de U-SQL que permite procesar las filas entrantes mediante la aplicación de características de programación. UDP permite combinar columnas, modificar valores y agregar nuevas columnas si es necesario. Básicamente le ayuda a procesar un conjunto de filas para generar elementos de datos necesarios.

## <a name="how-to-define-and-use-user-defined-processor"></a>Definición y uso de un procesador definido por el usuario
Para definir un UDP, necesitamos crear una interfaz `IProcessor` con el atributo `SqlUserDefinedProcessor`, que es opcional para UDP.

Esta interfaz debe contener la definición de la invalidación del conjunto de filas de interfaz `IRow`, como se muestra en el ejemplo siguiente:

```csharp
[SqlUserDefinedProcessor]
public class MyProcessor: IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
        …
 }
}
```

**SqlUserDefinedProcessor** indica que el tipo se debe registrar como un procesador definido por el usuario. No se puede heredar esta clase.

El atributo SqlUserDefinedProcessor es **opcional** para la definición de UDP.

Los objetos de programación principales son **input** y **output**. El objeto Input se utiliza para enumerar las columnas de entrada y Output para establecer datos de salida como resultado de la actividad del procesador.

Para la enumeración de las columnas de entrada, usamos el método `input.Get`.

```csharp
string column_name = input.Get<string>("column_name");
```

El parámetro para el método `input.Get` es una columna que se pasa como parte de la cláusula `PRODUCE` de la instrucción `PROCESS` del script base U-SQL. Es necesario usar aquí el tipo de datos correcto.

Para la salida, use el método `output.Set`.

Es importante comprender que el productor personalizado solo genera columnas y valores que se definen con la llamada al método `output.Set`.

```csharp
output.Set<string>("mycolumn", mycolumn);
```

La salida del procesador real se desencadena mediante una llamada a `return output.AsReadOnly();`.

Este es un ejemplo de procesador:

```csharp
[SqlUserDefinedProcessor]
public class FullDescriptionProcessor : IProcessor
{
public override IRow Process(IRow input, IUpdatableRow output)
 {
     string user = input.Get<string>("user");
     string des = input.Get<string>("des");
     string full_description = user.ToUpper() + "=>" + des;
     output.Set<string>("dt", input.Get<string>("dt"));
     output.Set<string>("full_description", full_description);
     output.Set<Guid>("new_guid", Guid.NewGuid());
     output.Set<Guid>("guid", input.Get<Guid>("guid"));
     return output.AsReadOnly();
 }
}
```

En este escenario de caso de uso, el procesador genera una nueva columna denominada "full_description" combinando las existentes (en este caso, “user” en mayúsculas y “des”). También vuelve a generar un GUID y devuelve los valores de GUID originales y nuevos.

Como puede ver en el ejemplo anterior, puede llamar a métodos de C# durante la llamada al método `output.Set`.

Este es un ejemplo de script U-SQL base que utiliza un procesador personalizado:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file USING Extractors.Tsv();

@rs1 =
     PROCESS @rs0
     PRODUCE dt String,
             full_description String,
             guid Guid,
             new_guid Guid
     USING new USQL_Programmability.FullDescriptionProcessor();

OUTPUT @rs1 TO @output_file USING Outputters.Text();
```


## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)