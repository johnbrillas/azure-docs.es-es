---
title: Guía de programación de extractores definidos por el usuario de U-SQL para Azure Data Lake
description: Obtenga información sobre los extractores definidos por el usuario en la guía de programación de UDO con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: a3613c2b95f13e6bbaaf570f522ad1f7b7edd756
ms.sourcegitcommit: df66dff4e34a0b7780cba503bb141d6b72335a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96512492"
---
# <a name="use-user-defined-extractor"></a>Uso de extractor definido por el usuario

## <a name="u-sql-udo-user-defined-extractor"></a>UDO de U-SQL: extractor definido por el usuario
U-SQL permite importar datos externos utilizando una instrucción EXTRACT. La instrucción EXTRACT puede utilizar extractores UDO integrados:  

* *Extractors.Text()* : proporciona extracción de archivos de texto delimitado con diferentes codificaciones.

* *Extractors.Csv()* : proporciona extracción de archivos de valores separados por comas (CSV) con diferentes codificaciones.

* *Extractors.Tsv()* : proporciona extracción de archivos de valores separados por tabulaciones (TSV) con diferentes codificaciones.

Puede ser útil desarrollar un extractor personalizado. Esto puede resultar útil durante la importación de datos si se desea realizar alguna de las siguientes tareas:

* Modificar datos de entrada mediante la división de columnas y modificar valores individuales. La funcionalidad PROCESSOR es mejor para combinar las columnas.
* Analizar datos no estructurados, como páginas web o correos electrónicos o datos parcialmente no estructurados, como XML o JSON.
* Analizar datos en codificación no compatible.

## <a name="how-to-define-and-use-user-defined-extractor"></a>Definición y uso de un extractor definido por el usuario
Para definir un extractor definido por el usuario o UDE, necesitamos crear una interfaz `IExtractor`. Todos los parámetros de entrada al extractor, como delimitadores de columna o fila, codificación, etc., deben definirse en el constructor de la clase. La interfaz `IExtractor` también debe contener la definición de la invalidación `IEnumerable<IRow>` como se indica a continuación:

```csharp
[SqlUserDefinedExtractor]
public class SampleExtractor : IExtractor
{
     public SampleExtractor(string row_delimiter, char col_delimiter)
     { … }

     public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
     { … }
}
```

El atributo **SqlUserDefinedExtractor** indica que el tipo se debe registrar como un extractor definido por el usuario. No se puede heredar esta clase.

SqlUserDefinedExtractor es un atributo opcional para la definición de UDE. Se utiliza para definir la propiedad AtomicFileProcessing para el objeto UDE.

* bool     AtomicFileProcessing   

* **true** = Indica que este extractor requiere archivos de entrada atómicos (JSON, XML...)
* **false** = Indica que este extractor puede tratar con archivos divididos o distribuidos (CSV, SEQ...)

Los objetos de programación UDE principales son **input** y **output**. El objeto de entrada se utiliza para enumerar los datos de entrada como `IUnstructuredReader`. El objeto de salida se utiliza para establecer datos de salida como resultado de la actividad del extractor.

Se tiene acceso a los datos de entrada a través de `System.IO.Stream` y `System.IO.StreamReader`.

Para la enumeración de columnas de entrada, primero dividiremos el flujo de entrada mediante un delimitador de filas.

```csharp
foreach (Stream current in input.Split(my_row_delimiter))
{
…
}
```

Después, dividiremos aún más la fila de entrada en partes de la columna.

```csharp
foreach (Stream current in input.Split(my_row_delimiter))
{
…
    string[] parts = line.Split(my_column_delimiter);
    foreach (string part in parts)
    { … }
}
```

Para establecer datos de salida, usamos el método `output.Set`.

Es importante comprender que el extractor personalizado solo genera columnas y valores que se definen con la salida. Establezca la llamada del método.

```csharp
output.Set<string>(count, part);
```

La salida del extractor real se desencadena mediante una llamada a `yield return output.AsReadOnly();`.

Este es el ejemplo de extractor:

```csharp
[SqlUserDefinedExtractor(AtomicFileProcessing = true)]
public class FullDescriptionExtractor : IExtractor
{
     private Encoding _encoding;
     private byte[] _row_delim;
     private char _col_delim;

    public FullDescriptionExtractor(Encoding encoding, string row_delim = "\r\n", char col_delim = '\t')
    {
         this._encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
         this._row_delim = this._encoding.GetBytes(row_delim);
         this._col_delim = col_delim;

    }

    public override IEnumerable<IRow> Extract(IUnstructuredReader input, IUpdatableRow output)
    {
         string line;
         //Read the input line by line
         foreach (Stream current in input.Split(_encoding.GetBytes("\r\n")))
         {
        using (System.IO.StreamReader streamReader = new StreamReader(current, this._encoding))
         {
             line = streamReader.ReadToEnd().Trim();
             //Split the input by the column delimiter
             string[] parts = line.Split(this._col_delim);
             int count = 0; // start with first column
             foreach (string part in parts)
             {
    if (count == 0)
             {  // for column “guid”, re-generated guid
                 Guid new_guid = Guid.NewGuid();
                 output.Set<Guid>(count, new_guid);
             }
             else if (count == 2)
             {
                 // for column “user”, convert to UPPER case
                 output.Set<string>(count, part.ToUpper());

             }
             else
             {
                 // keep the rest of the columns as-is
                 output.Set<string>(count, part);
             }
             count += 1;
             }

         }
         yield return output.AsReadOnly();
         }
         yield break;
     }
}
```

En este escenario de caso de uso, el extractor vuelve a generar un GUID para la columna “guid” y convierte los valores de la columna “user” en mayúsculas. Los extractores personalizados pueden producir resultados más complicados mediante el análisis de datos de entrada y la manipulación de estos.

Este es un script base U-SQL que utiliza un extractor personalizado:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.tsv";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
    FROM @input_file
        USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 TO @output_file USING Outputters.Text();
```


## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)