---
title: Creación de una consulta básica
titleSuffix: Azure Cognitive Search
description: Obtenga información sobre cómo construir una solicitud de consulta de Cognitive Search, qué herramientas y API usar para realizar pruebas y codificaciones, y cómo las decisiones de consulta comienzan con el diseño de índices.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: db36a77d93735b151ad893b7e25ba86f104e7b90
ms.sourcegitcommit: 63d0621404375d4ac64055f1df4177dfad3d6de6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2020
ms.locfileid: "97510471"
---
# <a name="create-a-query-in-azure-cognitive-search"></a>Creación de una consulta en Azure Cognitive Search

Si va a compilar una consulta por primera vez, en este artículo se describen las herramientas y las API que necesitará, los métodos que se usan para crear una consulta y el modo en que la estructura y el contenido del índice pueden afectar a los resultados de la consulta. Para obtener una aproximación de la apariencia de una solicitud de consulta, consulte [Tipos y composiciones de consultas](search-query-overview.md).

## <a name="choose-tools-and-apis"></a>Elección de herramientas y API

Necesitará una herramienta o API para crear una consulta. Cualquiera de las siguientes sugerencias le serán útiles para las cargas de trabajo de prueba y producción.

| Metodología | Descripción |
|-------------|-------------|
| Portal| [El explorador de búsqueda (portal)](search-explorer.md) es una interfaz de consulta de Azure Portal que ejecuta consultas en índices del servicio de búsqueda subyacente. El portal realiza llamadas a la API de REST en segundo plano para la operación [Buscar documentos](/rest/api/searchservice/search-documents), pero no puede invocar las funciones de autocompletar, de sugerencias o de búsqueda de documentos.<br/><br/> Puede seleccionar cualquier versión de índice y API de REST, incluida la versión preliminar. Una cadena de consulta puede utilizar una sintaxis simple o completa, con compatibilidad para todos los parámetros de consulta (filter, select, searchFields, etc.). Cuando abre un índice en el portal, puede trabajar con el explorador de búsqueda junto con la definición JSON del índice en las pestañas en paralelo, para facilitar el acceso a los atributos de campo. Compruebe los campos que se pueden buscar, ordenar, filtrar y clasificar mientras se prueban las consultas. <br/>Esto es recomendable para realizar exploraciones, pruebas y validaciones tempranas. [Más información.](search-explorer.md) |
| Herramientas para pruebas de web| [Postman o Visual Studio Code](search-get-started-rest.md) son buenas opciones para formular una solicitud de [búsqueda de documentos](/rest/api/searchservice/search-documents) y cualquier otra solicitud en REST. Las API de REST admiten todas las posibles operaciones de programación en Azure Cognitive Search y, cuando se usa una herramienta como Postman o Visual Studio Code, puede emitir solicitudes de forma interactiva para comprender cómo funciona la característica antes de invertir en el código. Una herramienta para pruebas de web es una buena opción si no dispone de derechos administrativos o de colaborador en Azure Portal. Siempre que tenga una dirección URL de búsqueda y una clave de API de consulta, puede usar las herramientas para ejecutar consultas en un índice existente. |
| SDK de Azure | Cuando esté listo para escribir código, puede usar las bibliotecas de cliente Azure.Search.Document en los SDK de Azure para .NET, Python, JavaScript o Java. Cada SDK tiene su propia programación de versión, pero puede crear y consultar índices en todos ellos. <br/><br/>[SearchClient (.NET)](/dotnet/api/azure.search.documents.searchclient) se puede usar para consultar un índice de búsqueda en C#.  [Más información.](search-howto-dotnet-sdk.md)<br/><br/>[SearchClient (Python)](/dotnet/api/azure.search.documents.searchclient) se puede usar para consultar un índice de búsqueda en Python. [Más información.](search-get-started-python.md)<br/><br/>[SearchClient (JavaScript)](/dotnet/api/azure.search.documents.searchclient) se puede usar para consultar un índice de búsqueda en JavaScript. [Más información.](search-get-started-javascript.md) |

## <a name="set-up-a-search-client"></a>Configuración de un cliente de búsqueda

Un cliente de búsqueda se autentica en el servicio de búsqueda, envía solicitudes y controla las respuestas. Independientemente de la herramienta o API que use, un cliente de búsqueda debe tener lo siguiente:

| Propiedades | Descripción |
|------------|-------------|
| Punto de conexión | Un servicio de búsqueda se puede usar con direcciones URL si tiene este formato: `https://[service-name].search.windows.net`. |
| Clave de acceso de la API (administrador o consulta) | Autentica la solicitud en el servicio de búsqueda. |
| Nombre del índice | Las consultas siempre se dirigen a la colección de documentos de un índice único. No es posible combinar índices ni crear estructuras de datos personalizadas o temporales como destino de la consulta. |
| Versión de API | Las llamadas de REST requieren explícitamente el valor `api-version` en la solicitud. Por el contrario, las bibliotecas de cliente del SDK de Azure obtienen sus versiones en función de una versión específica de la API de REST. En el caso de los SDK, el valor `api-version` está implícito. |

### <a name="in-the-portal"></a>En el portal

El explorador de búsqueda y otras herramientas del portal tienen una conexión de cliente integrada en el servicio, y cuenta con índices y otros objetos de acceso directo desde las páginas del portal. Es necesario tener un rol de colaborador o superior en el servicio para obtener acceso a herramientas, asistentes y objetos. 

### <a name="using-rest"></a>Uso de REST

En el caso de las llamadas de REST, puede usar [Postman o herramientas similares](search-get-started-rest.md) como el cliente, para especificar una solicitud de [Búsqueda de documentos](/rest/api/searchservice/search-documents). Tenga en cuenta que cada solicitud es independiente, por lo que debe proporcionar el punto de conexión, el nombre del índice y la versión de la API en cada solicitud. Otras propiedades, el tipo de contenido y la clave de API, se pasan en el encabezado de solicitud. 

Puede usar POST o GET para consultar un índice. De todos modos, teniendo en cuenta los parámetros especificados en el cuerpo de la solicitud, es más fácil de trabajar con POST. Si usa POST, asegúrese de incluir `docs/search` en la dirección URL:

```http
POST https://myservice.search.windows.net/indexes/hotels-sample-index/docs/search?api-version=2020-06-30
{
    "count": true,
    "queryType": "simple",
    "search": "*"
}
```

### <a name="using-azure-sdks"></a>Uso de los SDK de Azure

Si usa un SDK de Azure, creará el cliente en el código. Todos los SDK proporcionan clientes de búsqueda que pueden conservar el estado, lo que le permite reutilizar la conexión. En las operaciones de consulta, creará una instancia de un valor **`SearchClient`** y proporcionará valores para las siguientes propiedades: Punto de conexión, clave, índice. A continuación, puede llamar al valor **`Search method`** para pasar la cadena de consulta. 

| Idioma | Cliente | Ejemplo |
|----------|--------|---------|
| C# y .NET | [SearchClient](/dotnet/api/azure.search.documents.searchclient) | [Envíe la primera consulta de búsqueda en C#](/dotnet/api/overview/azure/search.documents-readme#send-your-first-search-query) |
| Python      | [SearchClient](/python/api/azure-search-documents/azure.search.documents.searchclient) | [Envíe la primera consulta de búsqueda en Python](/python/api/overview/azure/search-documents-readme#send-your-first-search-request) |
| Java        | [SearchClient](/java/api/com.azure.search.documents.searchclient) | [Envíe la primera consulta de búsqueda en Java](/java/api/overview/azure/search-documents-readme#send-your-first-search-query)  |
| JavaScript  | [SearchClient](/javascript/api/@azure/search-documents/searchclient) | [Envíe la primera consulta de búsqueda en JavaScript](/javascript/api/overview/azure/search-documents-readme#send-your-first-search-query)  |

## <a name="choose-a-parser-simple--full"></a>Elección del analizador: básico | completo

Si la consulta es una búsqueda de texto completo, se usará un analizador para procesar el contenido del parámetro de búsqueda. Azure Cognitive Search ofrece dos analizadores de consultas. El analizador simple entiende la [sintaxis de consulta simple](query-simple-syntax.md). Este analizador se seleccionó como valor predeterminado, debido a su velocidad y eficacia en las consultas de texto de forma libre. La sintaxis admite operadores de búsqueda comunes (AND, OR, NOT) en las búsquedas de términos y frases y en la búsqueda de prefijos (`*`), como "sea*" para Seattle y Seaside. Como recomendación general, pruebe primero el analizador simple y, a continuación, pase al analizador completo si los requisitos de la aplicación llaman a consultas más complejas.

La [sintaxis de consulta completa de Lucene](query-Lucene-syntax.md#bkmk_syntax) que se habilita al agregar `queryType=full` a la solicitud, se basa en el [analizador Apache Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html).

La sintaxis completa y la simple se superponen hasta el punto en que ambas admiten el mismo prefijo y las mismas operaciones booleanas, pero la sintaxis completa proporciona más operadores. En su totalidad, hay más operadores para las expresiones booleanas y más operadores para consultas avanzadas como la búsqueda aproximada, la búsqueda con caracteres comodín, la búsqueda de proximidad y las expresiones regulares.

## <a name="choose-query-methods"></a>Elección de los métodos de consulta

La búsqueda es fundamentalmente un ejercicio controlado por el usuario, donde los términos o frases se recopilan de un cuadro de búsqueda o de los eventos de clic de una página. En la tabla siguiente se resumen los mecanismos mediante los cuales puede recopilar las entradas de usuario y la experiencia de búsqueda esperada.

| Entrada | Experiencia |
|-------|---------|
| [Método de búsqueda](/rest/api/searchservice/search-documents) | El usuario escribe términos o frases en un cuadro de búsqueda, con o sin operadores, y hace clic en Buscar para enviar la solicitud. La búsqueda se puede usar con filtros en la misma solicitud, pero no con la opción de autocompletar o con sugerencias. |
| [Método de autocompletar](/rest/api/searchservice/autocomplete) | El usuario escribe unos pocos caracteres y las consultas se inician después de escribir cada carácter nuevo. La respuesta es una cadena completada del índice. Si la cadena proporcionada es válida, el usuario hace clic en Buscar para enviar la consulta al servicio. |
| [Método de sugerencias](/rest/api/searchservice/suggestions) | Igual que sucede con la opción de autocompletar, el usuario escribe unos pocos caracteres y se generan consultas incrementales. La respuesta es una lista desplegable de documentos coincidentes, que se representa normalmente con algunos campos únicos o descriptivos. Si alguna de las selecciones es válida, el usuario hace clic en una y se devuelve el documento coincidente. |
| [Navegación por facetas](/rest/api/searchservice/search-documents#query-parameters) | Una página muestra vínculos de navegación en los que se puede hacer clic o rutas de navegación que restringen el ámbito de la búsqueda. Una estructura de navegación por facetas se compone de forma dinámica en función de una consulta inicial. Por ejemplo, el valor `search=*` se usa para rellenar un árbol de navegación por facetas que se compone de cada categoría posible. Una estructura de navegación por facetas se crea a partir de una respuesta de consulta, pero también es un mecanismo para expresar la consulta siguiente. En una referencia de la API de REST, el valor `facets` se documenta como un parámetro de consulta de una operación de búsqueda de documentos, pero se puede usar sin el parámetro `search`.|
| [Método de filtro](/rest/api/searchservice/search-documents#query-parameters) | Los filtros se usan con las facetas para restringir los resultados. Asimismo, también puede implementar un filtro en el segundo plano de la página, para inicializar, por ejemplo, la página con campos específicos del idioma. En una referencia de la API de REST, el valor `$filter` se documenta como un parámetro de consulta de una operación de búsqueda de documentos, pero se puede usar sin el parámetro `search`.|

## <a name="know-your-field-attributes"></a>Conozca los atributos de campo

Si ya ha revisado los [aspectos básicos de una solicitud de consulta](search-query-overview.md), puede que recuerde que los parámetros de la solicitud de consulta dependen de cómo se atribuyen los campos en un índice. Por ejemplo, si quiere usar un campo en una consulta, un filtro o un criterio de ordenación, este se debe poder *buscar*, *filtrar* y *ordenar*. Del mismo modo, solo pueden aparecer en los resultados los campos marcados como *recuperables*. Cuando empiece a especificar los parámetros `search`, `filter` y `orderby` en la solicitud, asegúrese de comprobar los atributos a medida que avance, para evitar resultados inesperados.

En la captura de pantalla siguiente del portal que muestra el [índice de ejemplo Hotels](search-get-started-portal.md), solo se pueden usar los dos últimos campos "LastRenovationDate" y "Rating" en una sola cláusula `"$orderby"`.

![Definición del índice para el ejemplo del hotel](./media/search-query-overview/hotel-sample-index-definition.png "Definición del índice para el ejemplo del hotel")

Para obtener una descripción de los atributos de campo, consulte [Creación del índice (API de REST)](/rest/api/searchservice/create-index).

## <a name="know-your-tokens"></a>Conozca los tokens

Durante la indexación, el motor de consultas usa un analizador para realizar el análisis de texto en las cadenas, maximizando así la posibilidad de encontrar coincidencias en el momento de la consulta. Como mínimo, las cadenas están en minúsculas, pero también pueden someterse a una lematización y detener la eliminación de palabras. Las cadenas o palabras compuestas más amplias suelen dividirse con espacios en blanco, guiones o rayas, y se indexan como tokens independientes. 

Lo importante aquí es que lo que cree que contiene su índice, y lo que realmente contiene, puede ser diferente. Si las consultas no devuelven los resultados esperados, puede inspeccionar los tokens que haya creado el analizador a través de la opción [Analizar texto (API de REST)](/rest/api/searchservice/test-analyzer). Para obtener más información acerca de la tokenización y el impacto en las consultas, consulte [Búsqueda de términos y patrones parciales con caracteres especiales](search-query-partial-matching.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que conoce mejor cómo se construye una solicitud de consulta, consulte las siguientes guías de inicio rápido para obtener experiencia práctica.

+ [Explorador de búsqueda](search-explorer.md)
+ [Consultas con REST](search-get-started-rest.md)
+ [Consultas con .NET](search-get-started-dotnet.md)
+ [Consultas con Python](search-get-started-python.md)
+ [Consultas con JavaScript](search-get-started-javascript.md)