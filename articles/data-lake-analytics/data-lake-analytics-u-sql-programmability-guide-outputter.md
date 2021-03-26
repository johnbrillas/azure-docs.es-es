---
title: Guía de programación de outputters definidos por el usuario de U-SQL para Azure Data Lake
description: Obtenga información sobre los outputters definidos por el usuario en la guía de programación de UDO con U-SQL.
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.topic: how-to
ms.date: 06/30/2017
ms.openlocfilehash: 56b104b5cc8f8923445455c71fe2418e39539b8e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96512462"
---
# <a name="use-user-defined-outputter"></a>Uso de outputter definido por el usuario

## <a name="u-sql-udo-user-defined-outputter"></a>UDO de U-SQL: outputter definido por el usuario
Outputter definido por el usuario es otro UDO de U-SQL que permite extender una funcionalidad integrada de U-SQL. Al igual que el extractor, hay varios outputters integrados.

* *Outputters.Text()* : escribe datos en archivos de texto delimitado con diferentes codificaciones.
* *Outputters.Csv()* : escribe datos en archivos de valores separados por comas (CSV) con diferentes codificaciones.
* *Outputters.Tsv()* : escribe datos en archivos de valores separados por tabulaciones (TSV) con diferentes codificaciones.

El outputter personalizado le permite escribir datos en un formato definido personalizado. Esto puede resultar útil para las siguientes tareas:

* Escribir datos en archivos semiestructurados o no estructurados.
* Escribir datos en codificaciones no admitidas.
* Modificar datos de salida o agregar atributos personalizados.

## <a name="how-to-define-and-use-user-defined-outputter"></a>Definición y uso de un outputter definido por el usuario
Para definir el outputter definido por el usuario, necesitamos crear la interfaz `IOutputter`.

Esta es la implementación de la clase base `IOutputter`:

```csharp
public abstract class IOutputter : IUserDefinedOperator
{
    protected IOutputter();

    public virtual void Close();
    public abstract void Output(IRow input, IUnstructuredWriter output);
}
```

Todos los parámetros de entrada al outputter, como delimitadores de columna o fila, codificación, etc., deben definirse en el constructor de la clase. La interfaz `IOutputter` también debe contener una definición de la invalidación `void Output`. El atributo `[SqlUserDefinedOutputter(AtomicFileProcessing = true)`, opcionalmente, se puede establecer para el procesamiento de archivos atómico. Para más información, consulte los siguientes detalles.

```csharp
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class MyOutputter : IOutputter
{

    public MyOutputter(myparam1, myparam2)
    {
      …
    }

    public override void Close()
    {
      …
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
      …
    }
}
```

* `Output` se llama para cada fila de entrada. Devuelve el conjunto de filas `IUnstructuredWriter output`.
* La clase Constructor se utiliza para pasar parámetros al outputter definido por el usuario.
* `Close` se usa para invalidar opcionalmente para liberar el estado costoso o saber cuándo se ha escrito la última fila.

El atributo **SqlUserDefinedOutputter** indica que el tipo se debe registrar como un outputter definido por el usuario. No se puede heredar esta clase.

SqlUserDefinedOutputter es un atributo opcional para la definición del outputter definido por el usuario. Se utiliza para definir la propiedad AtomicFileProcessing.

* bool     AtomicFileProcessing   

* **true** = Indica que este outputter requiere archivos de salida atómicos (JSON, XML...)
* **false** = Indica que este outputter puede tratar con archivos divididos o distribuidos (CSV, SEQ...)

Los objetos de programación principales son **row** y **output**. El objeto **fila** se usa para enumerar los datos de salida como la interfaz `IRow`. **Salida** se usa para establecer los datos de salida al archivo de destino.

Se tiene acceso a los datos de entrada a través de la interfaz `IRow`. Los datos de salida pasan una fila en cada momento.

Los valores individuales se enumeran mediante una llamada al método Get de la interfaz IRow:

```csharp
row.Get<string>("column_name")
```

Los nombres de columna individuales se pueden determinar mediante una llamada a `row.Schema`:

```csharp
ISchema schema = row.Schema;
var col = schema[i];
string val = row.Get<string>(col.Name)
```

Este enfoque permite crear un outputter flexible para cualquier esquema de metadatos.

Los datos de salida se escriben en el archivo mediante `System.IO.StreamWriter`. El parámetro de flujo se establece en `output.BaseStream` como parte de `IUnstructuredWriter output`.

Tenga en cuenta que esto es importante para vaciar el búfer de datos en el archivo después de cada iteración de fila. Además, el objeto `StreamWriter` debe utilizarse con el atributo Disposable habilitado (valor predeterminado) y con la palabra clave **using**:

```csharp
using (StreamWriter streamWriter = new StreamWriter(output.BaseStream, this._encoding))
{
…
}
```

De lo contrario, llame al método Flush() explícitamente después de cada iteración. Esto se muestra en el ejemplo siguiente.

### <a name="set-headers-and-footers-for-user-defined-outputter"></a>Establecimiento de encabezados y pies de página para el outputter definido por el usuario
Para establecer un encabezado, use el flujo de ejecución de iteración único.

```csharp
public override void Output(IRow row, IUnstructuredWriter output)
{
 …
if (isHeaderRow)
{
    …                
}

 …
if (isHeaderRow)
{
    isHeaderRow = false;
}
 …
}
}
```

El código del primer bloque `if (isHeaderRow)` se ejecuta solo una vez.

Para el pie de página, use la referencia a la instancia del objeto `System.IO.Stream` (`output.BaseStream`). Escriba el pie de página en el método Close() de la interfaz `IOutputter`.  Para más información, consulte el siguiente ejemplo.

Este es un ejemplo de un outputter definido por el usuario:

```csharp
[SqlUserDefinedOutputter(AtomicFileProcessing = true)]
public class HTMLOutputter : IOutputter
{
    // Local variables initialization
    private string row_delimiter;
    private char col_delimiter;
    private bool isHeaderRow;
    private Encoding encoding;
    private bool IsTableHeader = true;
    private Stream g_writer;

    // Parameters definition            
    public HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    this.isHeaderRow = isHeader;
    this.encoding = ((encoding == null) ? Encoding.UTF8 : encoding);
    }

    // The Close method is used to write the footer to the file. It's executed only once, after all rows
    public override void Close()
    {
    //Reference to IO.Stream object - g_writer
    StreamWriter streamWriter = new StreamWriter(g_writer, this.encoding);
    streamWriter.Write("</table>");
    streamWriter.Flush();
    streamWriter.Close();
    }

    public override void Output(IRow row, IUnstructuredWriter output)
    {
    System.IO.StreamWriter streamWriter = new StreamWriter(output.BaseStream, this.encoding);

    // Metadata schema initialization to enumerate column names
    ISchema schema = row.Schema;

    // This is a data-independent header--HTML table definition
    if (IsTableHeader)
    {
        streamWriter.Write("<table border=1>");
        IsTableHeader = false;
    }

    // HTML table attributes
    string header_wrapper_on = "<th>";
    string header_wrapper_off = "</th>";
    string data_wrapper_on = "<td>";
    string data_wrapper_off = "</td>";

    // Header row output--runs only once
    if (isHeaderRow)
    {
        streamWriter.Write("<tr>");
        for (int i = 0; i < schema.Count(); i++)
        {
        var col = schema[i];
        streamWriter.Write(header_wrapper_on + col.Name + header_wrapper_off);
        }
        streamWriter.Write("</tr>");
    }

    // Data row output
    streamWriter.Write("<tr>");                
    for (int i = 0; i < schema.Count(); i++)
    {
        var col = schema[i];
        string val = "";
        try
        {
        // Data type enumeration--required to match the distinct list of types from OUTPUT statement
        switch (col.Type.Name.ToString().ToLower())
        {
            case "string": val = row.Get<string>(col.Name).ToString(); break;
            case "guid": val = row.Get<Guid>(col.Name).ToString(); break;
            default: break;
        }
        }
        // Handling NULL values--keeping them empty
        catch (System.NullReferenceException)
        {
        }
        streamWriter.Write(data_wrapper_on + val + data_wrapper_off);
    }
    streamWriter.Write("</tr>");

    if (isHeaderRow)
    {
        isHeaderRow = false;
    }
    // Reference to the instance of the IO.Stream object for footer generation
    g_writer = output.BaseStream;
    streamWriter.Flush();
    }
}

// Define the factory classes
public static class Factory
{
    public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
    {
    return new HTMLOutputter(isHeader, encoding);
    }
}
```

Y el script base U-SQL:

```usql
DECLARE @input_file string = @"\usql-programmability\input_file.tsv";
DECLARE @output_file string = @"\usql-programmability\output_file.html";

@rs0 =
    EXTRACT
            guid Guid,
        dt String,
            user String,
            des String
         FROM @input_file
         USING new USQL_Programmability.FullDescriptionExtractor(Encoding.UTF8);

OUTPUT @rs0 
    TO @output_file 
    USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Se trata de un outputter HTML que crea un archivo HTML con los datos de la tabla.

### <a name="call-outputter-from-u-sql-base-script"></a>Llamada a un outputter desde el script base U-SQL
Para llamar a un outputter personalizado desde el script base U-SQL, se debe crear la nueva instancia del objeto outputter.

```sql
OUTPUT @rs0 TO @output_file USING new USQL_Programmability.HTMLOutputter(isHeader: true);
```

Para evitar la creación de una instancia del objeto en el script base, podemos crear un contenedor de función como se indicó en el ejemplo anterior:

```csharp
        // Define the factory classes
        public static class Factory
        {
            public static HTMLOutputter HTMLOutputter(bool isHeader = false, Encoding encoding = null)
            {
                return new HTMLOutputter(isHeader, encoding);
            }
        }
```

En este caso, la llamada original es similar a la siguiente:

```usql
OUTPUT @rs0 
TO @output_file 
USING USQL_Programmability.Factory.HTMLOutputter(isHeader: true);
```

## <a name="next-steps"></a>Pasos siguientes
* [Introducción a la guía de programación de U-SQL](data-lake-analytics-u-sql-programmability-guide.md)
* [Guía de programación de U-SQL: UDT y UDAGG](data-lake-analytics-u-sql-programmability-guide-UDT-AGG.md)