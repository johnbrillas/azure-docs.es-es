---
title: Tutorial de C# sobre la ordenación de los resultados
titleSuffix: Azure Cognitive Search
description: En este tutorial de C# se muestra cómo ordenar los resultados de la búsqueda. Se basa en un proyecto de hoteles anterior, que se ordena por propiedad principal y propiedad secundaria, y que incluye un perfil de puntuación para agregar criterios de mejora.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 01/26/2021
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: 1f8100dd6340383eadec5d10b7f23db59ba0ebdf
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98786391"
---
# <a name="tutorial-order-search-results-using-the-net-sdk"></a>Tutorial: Ordenación de los resultados de búsqueda mediante el SDK de .NET

En esta serie de tutoriales, los resultados se devuelven y muestran en un [orden predeterminado](index-add-scoring-profiles.md#what-is-default-scoring). En este tutorial, agregará criterios de ordenación principal y secundario. Como alternativa a la ordenación basada en valores numéricos, en el último ejemplo se muestra cómo clasificar los resultados en función de un perfil de puntuación personalizado. También describiremos un poco más la visualización de _tipos complejos_.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Ordenar los resultados según una propiedad
> * Ordenar los resultados de orden según varias propiedades
> * Ordenar los resultados según un perfil de puntuación

## <a name="overview"></a>Información general

En este tutorial se amplía el proyecto de desplazamiento infinito creado en el tutorial [Incorporación de la paginación a los resultados de búsqueda](tutorial-csharp-paging.md) tutorial.

En el proyecto siguiente se puede encontrar una versión finalizada del código de este tutorial:

* [5-order-results (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/5-order-results)

## <a name="prerequisites"></a>Prerrequisitos

* La solución [2b-add-infinite-scroll (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/2b-add-infinite-scroll). Este proyecto puede ser su propia versión, que se creó a partir del tutorial anterior o de una copia de GitHub.

Este tutorial se ha actualizado para usar el paquete [Azure.Search.Documents (versión 11)](https://www.nuget.org/packages/Azure.Search.Documents/). Para obtener una versión anterior del SDK de .NET, consulte el [código de ejemplo de Microsoft.Azure.Search (versión 10)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10).

## <a name="order-results-based-on-one-property"></a>Ordenar los resultados según una propiedad

Al ordenar los resultados por una propiedad, como la clasificación del hotel, no solo queremos los resultados ordenados, sino que también queremos la confirmación de que el orden es correcto. La incorporación del campo de clasificación a los resultados nos permite confirmar que los resultados están ordenados correctamente.

En este ejercicio, también se van a agregar algunos elementos más a la visualización de los resultados (la tarifa de habitación más baja y la más alta) de todos los hoteles.

No hace falta modificar ninguno de los modelos para habilitar el ordenamiento. La vista y el controlador son los únicos elementos que requieren actualizaciones. Para comenzar, abra el controlador principal.

### <a name="add-the-orderby-property-to-the-search-parameters"></a>Agregar la propiedad OrderBy a los parámetros de búsqueda

1. En HomeController.cs, agregue la opción **OrderBy** e incluya la propiedad Rating con un criterio de ordenación descendente. En el siguiente método **Index(SearchData model)** , agregue la siguiente línea a los parámetros de búsqueda.

    ```cs
    options.OrderBy.Add("Rating desc");
    ```

    >[!Note]
    > El orden predeterminado es ascendente, aunque puede agregar `asc` a la propiedad para aclarar este punto. El orden descendente se especifica mediante la adición de `desc`.

1. Ahora ejecute la aplicación y escriba un término de búsqueda común. Los resultados pueden o no estar en el orden correcto, ya que ni el desarrollador ni el usuario tienen una forma sencilla de comprobar los resultados.

1. Confirmemos que los resultados se ordenan según la clasificación. En primer lugar, reemplace las clases **box1** y **box2** del archivo hotels.css por las siguientes clases (estas clases son todas las nuevas que necesitamos para este tutorial).

    ```html
    textarea.box1A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box1B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 14pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box2A {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        padding-left: 5px;
        text-align: left;
    }

    textarea.box2B {
        width: 324px;
        height: 32px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        color: blue;
        text-align: right;
        padding-right: 5px;
    }

    textarea.box3 {
        width: 648px;
        height: 100px;
        border: none;
        background-color: azure;
        font-size: 12pt;
        padding-left: 5px;
        margin-bottom: 24px;
    }
    ```

    > [!Tip]
    > Los exploradores suelen almacenar archivos css en caché, lo que puede resultar en el uso de un archivo css antiguo y la omisión de las modificaciones. Una buena manera de evitar esto es agregar al vínculo una cadena de consulta con un parámetro de versión. Por ejemplo:
    >
    >```html
    >   <link rel="stylesheet" href="~/css/hotels.css?v1.1" />
    >```
    >
    > Actualice el número de versión solamente si cree que el explorador está usando un archivo css antiguo.

1. Agregue la propiedad **Rating** al parámetro **Select** en el método **Index(SearchData model)** para que los resultados incluyan los tres campos siguientes:

    ```cs
    options.Select.Add("HotelName");
    options.Select.Add("Description");
    options.Select.Add("Rating");
    ```

1. Abra la vista (index.cshtml) y reemplace el bucle de representación ( **&lt;!-- Show the hotel data. --&gt;** ) por el código siguiente.

    ```cs
    <!-- Show the hotel data. -->
    @for (var i = 0; i < result.Count; i++)
    {
        var ratingText = $"Rating: {result[i].Document.Rating}";

        // Display the hotel details.
        @Html.TextArea($"name{i}", result[i].Document.HotelName, new { @class = "box1A" })
        @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
        @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
    }
    ```

1. La clasificación debe estar disponible tanto la primera página que se muestra como en las páginas subsiguientes a las que se llaman mediante el desplazamiento infinito. En la segunda de estas dos situaciones, es necesario actualizar tanto la acción **Next** del controlador como la función **scrolled** de la vista. Comenzando por el controlador, cambie el método **Next** al código siguiente. Este código crea y comunica el texto de la clasificación.

    ```cs
    public async Task<ActionResult> Next(SearchData model)
    {
        // Set the next page setting, and call the Index(model) action.
        model.paging = "next";
        await Index(model);

        // Create an empty list.
        var nextHotels = new List<string>();

        // Add a hotel details to the list.
        await foreach (var result in model.resultList.GetResultsAsync())
        {
            var ratingText = $"Rating: {result.Document.Rating}";
            var rateText = $"Rates from ${result.Document.cheapest} to ${result.Document.expensive}";
    
            string fullDescription = result.Document.Description;
    
            // Add strings to the list.
            nextHotels.Add(result.Document.HotelName);
            nextHotels.Add(ratingText);
            nextHotels.Add(fullDescription);
        }

        // Rather than return a view, return the list of data.
        return new JsonResult(nextHotels);
    }
    ```

1. Ahora, actualice la función **scrolled** en la vista para mostrar el texto de la clasificación.

    ```javascript
    <script>
        function scrolled() {
            if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                $.getJSON("/Home/Next", function (data) {
                    var div = document.getElementById('myDiv');

                    // Append the returned data to the current list of hotels.
                    for (var i = 0; i < data.length; i += 3) {
                        div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box3">' + data[i + 2] + '</textarea>';
                    }
                });
            }
        }
    </script>
    ```

1. Ahora, vuelva a ejecutar la aplicación. Busque cualquier término común, como "wifi", y compruebe que los resultados se ordenan en orden descendente de clasificación de hotel.

    ![Ordenamiento basado en la clasificación](./media/tutorial-csharp-create-first-app/azure-search-orders-rating.png)

    Observará que varios hoteles tienen una clasificación idéntica, por lo que su visualización en pantalla es de nuevo en el orden en el que se encuentran los datos, que es arbitrario.

    Antes de agregar un segundo nivel de ordenamiento, agreguemos algo de código para mostrar el rango de tarifas de habitación. Agregamos este código a para mostrar los datos de extracción de un _tipo complejo_ y también para que podamos analizar los resultados de ordenamiento en función del precio (por ejemplo, el precio más bajo primero).

### <a name="add-the-range-of-room-rates-to-the-view"></a>Agregar un rango de tarifas de habitación a la vista

1. Agregue las propiedades que contengan la tarifa de habitación más económica y la más cara al modeloHotel.cs.

    ```cs
    // Room rate range
    public double cheapest { get; set; }
    public double expensive { get; set; }
    ```

1. Calcule las tarifas de habitación al final de la acción **Index(SearchData model)** , en el controlador principal. Agregue los cálculos después de almacenar los datos temporales.

    ```cs
    // Ensure TempData is stored for the next call.
    TempData["page"] = page;
    TempData["searchfor"] = model.searchText;

    // Calculate the room rate ranges.
    await foreach (var result in model.resultList.GetResultsAsync())
    {
        var cheapest = 0d;
        var expensive = 0d;

        foreach (var room in result.Document.Rooms)
        {
            var rate = room.BaseRate;
            if (rate < cheapest || cheapest == 0)
            {
                cheapest = (double)rate;
            }
            if (rate > expensive)
            {
                expensive = (double)rate;
            }
        }
        model.resultList.Results[n].Document.cheapest = cheapest;
        model.resultList.Results[n].Document.expensive = expensive;
    }
    ```

1. Agregue la propiedad **Rooms** al parámetro **Select** en el método de acción **Index(SearchData model)** del controlador.

    ```cs
    options.Select.Add("Rooms");
    ```

1. Cambie el bucle de representación en la vista para mostrar el rango de tarifas para la primera página de resultados.

    ```cs
    <!-- Show the hotel data. -->
    @for (var i = 0; i < result.Count; i++)
    {
        var rateText = $"Rates from ${result[i].Document.cheapest} to ${result[i].Document.expensive}";
        var ratingText = $"Rating: {result[i].Document.Rating}";

        string fullDescription = result[i].Document.Description;

        // Display the hotel details.
        @Html.TextArea($"name{i}", result[i].Document.HotelName, new { @class = "box1A" })
        @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
        @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
        @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
    }
    ```

1. Cambie el método **Next** en el controlador principal para comunicar el rango de tarifas para las páginas de resultados subsiguientes.

    ```cs
    public async Task<ActionResult> Next(SearchData model)
    {
        // Set the next page setting, and call the Index(model) action.
        model.paging = "next";
        await Index(model);

        // Create an empty list.
        var nextHotels = new List<string>();

        // Add a hotel details to the list.
        await foreach (var result in model.resultList.GetResultsAsync())
        {
            var ratingText = $"Rating: {result.Document.Rating}";
            var rateText = $"Rates from ${result.Document.cheapest} to ${result.Document.expensive}";

            string fullDescription = result.Document.Description;

            // Add strings to the list.
            nextHotels.Add(result.Document.HotelName);
            nextHotels.Add(ratingText);
            nextHotels.Add(rateText);
            nextHotels.Add(fullDescription);
        }

        // Rather than return a view, return the list of data.
        return new JsonResult(nextHotels);
    }
    ```

1. Actualice la función **scrolled** en la vista para controlar el texto de tarifas de habitación.

    ```javascript
    <script>
        function scrolled() {
            if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                $.getJSON("/Home/Next", function (data) {
                    var div = document.getElementById('myDiv');

                    // Append the returned data to the current list of hotels.
                    for (var i = 0; i < data.length; i += 4) {
                        div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                    }
                });
            }
        }
    </script>
    ```

1. Ejecute la aplicación y compruebe que se muestran los rangos de precios de habitación.

    ![Mostrar rangos de precios de habitación](./media/tutorial-csharp-create-first-app/azure-search-orders-rooms.png)

La propiedad **OrderBy** de los parámetros de búsqueda no aceptará una entrada como, por ejemplo, **Rooms.BaseRate**, para proporcionar la tarifa de habitación más económica, incluso si las habitaciones ya estaban ordenadas según la tarifa. En este caso, las habitaciones no se ordenan según la tarifa. Para mostrar hoteles en el conjunto de datos de ejemplo, ordenados según la tarifa de habitación, tendría que ordenar los resultados en el controlador principal y enviar estos a la vista en el orden deseado.

## <a name="order-results-based-on-multiple-values"></a>Ordenar los resultados según varios valores

Ahora la pregunta es cómo diferenciar entre los hoteles con la misma clasificación. Un enfoque podría ser una ordenación secundaria basada en la última vez que se renovó el hotel, de modo que los hoteles renovados más recientemente aparezcan más arriba en los resultados.

1. Para agregar un segundo nivel de ordenación, agregue **LastRenovationDate** a los resultados de la búsqueda y a **OrderBy** en el método **Index(SearchData model)** .

    ```cs
    options.Select.Add("LastRenovationDate");

    options.OrderBy.Add("LastRenovationDate desc");
    ```

    >[!Tip]
    > Puede especificar cualquier número de propiedades en la lista **OrderBy**. Si los hoteles tuvieran la misma clasificación y fecha de reforma, se puede especificar una tercera propiedad para diferenciar entre ellos.

1. Nuevamente, es necesario ver la fecha de reforma en la vista, simplemente para estar seguro de que el ordenamiento es correcto. Para algo como una renovación, es probable que solo se necesite el año. Cambie el bucle de representación en la vista por el código siguiente.

    ```cs
    <!-- Show the hotel data. -->
    @for (var i = 0; i < result.Count; i++)
    {
        var rateText = $"Rates from ${result[i].Document.cheapest} to ${result[i].Document.expensive}";
        var lastRenovatedText = $"Last renovated: { result[i].Document.LastRenovationDate.Value.Year}";
        var ratingText = $"Rating: {result[i].Document.Rating}";

        string fullDescription = result[i].Document.Description;

        // Display the hotel details.
        @Html.TextArea($"name{i}", result[i].Document.HotelName, new { @class = "box1A" })
        @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
        @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
        @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
        @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
    }
    ```

1. Cambie el método **Next** en el controlador principal para reenviar el componente de año de la última fecha de reforma.

    ```cs
        public async Task<ActionResult> Next(SearchData model)
        {
            // Set the next page setting, and call the Index(model) action.
            model.paging = "next";
            await Index(model);

            // Create an empty list.
            var nextHotels = new List<string>();

            // Add a hotel details to the list.
            await foreach (var result in model.resultList.GetResultsAsync())
            {
                var ratingText = $"Rating: {result.Document.Rating}";
                var rateText = $"Rates from ${result.Document.cheapest} to ${result.Document.expensive}";
                var lastRenovatedText = $"Last renovated: {result.Document.LastRenovationDate.Value.Year}";

                string fullDescription = result.Document.Description;

                // Add strings to the list.
                nextHotels.Add(result.Document.HotelName);
                nextHotels.Add(ratingText);
                nextHotels.Add(rateText);
                nextHotels.Add(lastRenovatedText);
                nextHotels.Add(fullDescription);
            }

            // Rather than return a view, return the list of data.
            return new JsonResult(nextHotels);
        }
    ```

1. Cambie la función **scrolled** en la vista para mostrar el texto de reforma.

    ```javascript
    <script>
        function scrolled() {
            if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                $.getJSON("/Home/Next", function (data) {
                    var div = document.getElementById('myDiv');

                    // Append the returned data to the current list of hotels.
                    for (var i = 0; i < data.length; i += 5) {
                        div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box1B">' + data[i + 1] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box2B">' + data[i + 3] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                    }
                });
            }
        }
    </script>
    ```

1. Ejecute la aplicación. Busque un término común, por ejemplo "pool" o "view", y compruebe que los hoteles con la misma clasificación ahora se muestran en orden descendente según la fecha de reforma.

    ![Ordenar según la fecha de reforma](./media/tutorial-csharp-create-first-app/azure-search-orders-renovation.png)

## <a name="order-results-based-on-a-scoring-profile"></a>Ordenar los resultados según un perfil de puntuación

En los ejemplos que se proporcionan en el tutorial hasta ahora se muestra cómo ordenar según valores numéricos (clasificación y fecha de renovación), lo que proporciona una secuencia _exacta_ de ordenación. Sin embargo, algunas búsquedas y datos no se prestan a una comparación tan fácil entre dos elementos de datos. En el caso de las consultas de búsqueda de texto completo, Cognitive Search incluye el concepto de _clasificación_. Se pueden especificar _perfiles de puntuación_ para influir en el modo en que se clasifican los resultados, lo que proporciona comparaciones cualitativas y más complejas.

Los [perfiles de puntuación](index-add-scoring-profiles.md) se definen en el esquema de índices. Se han configurado varios perfiles de puntuación en los datos de hoteles. Echemos un vistazo a cómo se define un perfil de puntuación y luego intentemos escribir código para buscar en ellos.

### <a name="how-scoring-profiles-are-defined"></a>Definición de perfiles de puntuación

Los perfiles de puntuación se definen en un índice de búsqueda en tiempo de diseño. El índice de hoteles de solo lectura hospedado por Microsoft tiene tres perfiles de puntuación. En esta sección se exploran los perfiles de puntuación y se muestra cómo usarlos en el código.

1. A continuación se muestra el perfil de puntuación predeterminado para el conjunto de datos de hoteles, que se usa cuando no se especifica ningún parámetro **OrderBy** o **ScoringProfile**. Este perfil aumenta la _puntuación_ para un hotel si el texto de búsqueda está presente en el nombre del hotel, la descripción o la lista de etiquetas (servicios). Observe cómo las ponderaciones de la puntuación favorecen determinados campos. Si el texto de búsqueda aparece en otro campo, que no se muestra abajo, tendrá una ponderación de 1. Obviamente, cuanto más alta sea la puntuación, más arriba en la vista aparecerá un resultado.

     ```cs
    {
        "name": "boostByField",
        "text": {
            "weights": {
                "Tags": 3,
                "HotelName": 2,
                "Description": 1.5,
                "Description_fr": 1.5,
            }
        }
    }
    ```

1. El siguiente perfil de puntuación alternativo aumenta la puntuación significativamente si un parámetro proporcionado incluye uno o varios elementos de la lista de etiquetas (que llamaremos "servicios"). El punto clave de este perfil es que _debe_ proporcionarse un parámetro que contenga texto. Si el parámetro está vacío o no se proporciona, se lanzará un error.

    ```cs
    {
        "name":"boostAmenities",
        "functions":[
            {
            "fieldName":"Tags",
            "freshness":null,
            "interpolation":"linear",
            "magnitude":null,
            "distance":null,
            "tag":{
                "tagsParameter":"amenities"
            },
            "type":"tag",
            "boost":5
            }
        ],
        "functionAggregation":0
    },
    ```

1. En este tercer perfil, la clasificación del hotel proporciona un aumento significativo a la puntuación. La fecha de la última reforma también aumentará la puntuación, pero solo si esos datos se encuentran dentro de los 730 días (2 años) desde la fecha actual.

    ```cs
    {
        "name":"renovatedAndHighlyRated",
        "functions":[
            {
            "fieldName":"Rating",
            "freshness":null,
            "interpolation":"linear",
            "magnitude":{
                "boostingRangeStart":0,
                "boostingRangeEnd":5,
                "constantBoostBeyondRange":false
            },
            "distance":null,
            "tag":null,
            "type":"magnitude",
            "boost":20
            },
            {
            "fieldName":"LastRenovationDate",
            "freshness":{
                "boostingDuration":"P730D"
            },
            "interpolation":"quadratic",
            "magnitude":null,
            "distance":null,
            "tag":null,
            "type":"freshness",
            "boost":10
            }
        ],
        "functionAggregation":0
    }
    ```

    Ahora, veamos si estos perfiles funcionan según teníamos previsto.

### <a name="add-code-to-the-view-to-compare-profiles"></a>Agregar código a la vista para comparar perfiles

1. Abra el archivo index.cshtml y reemplace la sección &lt;body&gt; por el código siguiente.

    ```html
    <body>

    @using (Html.BeginForm("Index", "Home", FormMethod.Post))
    {
        <table>
            <tr>
                <td></td>
                <td>
                    <h1 class="sampleTitle">
                        <img src="~/images/azure-logo.png" width="80" />
                        Hotels Search - Order Results
                    </h1>
                </td>
            </tr>
            <tr>
                <td></td>
                <td>
                    <!-- Display the search text box, with the search icon to the right of it. -->
                    <div class="searchBoxForm">
                        @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input class="searchBoxSubmit" type="submit" value="">
                    </div>

                    <div class="searchBoxForm">
                        <b>&nbsp;Order:&nbsp;</b>
                        @Html.RadioButtonFor(m => m.scoring, "Default") Default&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "RatingRenovation") By numerical Rating&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "boostAmenities") By Amenities&nbsp;&nbsp;
                        @Html.RadioButtonFor(m => m.scoring, "renovatedAndHighlyRated") By Renovated date/Rating profile&nbsp;&nbsp;
                    </div>
                </td>
            </tr>

            <tr>
                <td valign="top">
                    <div id="facetplace" class="facetchecks">

                        @if (Model != null && Model.facetText != null)
                        {
                            <h5 class="facetheader">Amenities:</h5>
                            <ul class="facetlist">
                                @for (var c = 0; c < Model.facetText.Length; c++)
                                {
                                    <li> @Html.CheckBoxFor(m => m.facetOn[c], new { @id = "check" + c.ToString() }) @Model.facetText[c] </li>
                                }

                            </ul>
                        }
                    </div>
                </td>
                <td>
                    @if (Model != null && Model.resultList != null)
                    {
                        // Show the total result count.
                        <p class="sampleText">
                            @Html.DisplayFor(m => m.resultList.Count) Results <br />
                        </p>

                        <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

                            <!-- Show the hotel data. -->
                            @for (var i = 0; i < Model.resultList.Results.Count; i++)
                            {
                                var rateText = $"Rates from ${Model.resultList.Results[i].Document.cheapest} to ${Model.resultList.Results[i].Document.expensive}";
                                var lastRenovatedText = $"Last renovated: { Model.resultList.Results[i].Document.LastRenovationDate.Value.Year}";
                                var ratingText = $"Rating: {Model.resultList.Results[i].Document.Rating}";

                                string amenities = string.Join(", ", Model.resultList.Results[i].Document.Tags);
                                string fullDescription = Model.resultList.Results[i].Document.Description;
                                fullDescription += $"\nAmenities: {amenities}";

                                // Display the hotel details.
                                @Html.TextArea($"name{i}", Model.resultList.Results[i].Document.HotelName, new { @class = "box1A" })
                                @Html.TextArea($"rating{i}", ratingText, new { @class = "box1B" })
                                @Html.TextArea($"rates{i}", rateText, new { @class = "box2A" })
                                @Html.TextArea($"renovation{i}", lastRenovatedText, new { @class = "box2B" })
                                @Html.TextArea($"desc{i}", fullDescription, new { @class = "box3" })
                            }
                        </div>

                        <script>
                            function scrolled() {
                                if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                                    $.getJSON("/Home/Next", function (data) {
                                        var div = document.getElementById('myDiv');

                                        // Append the returned data to the current list of hotels.
                                        for (var i = 0; i < data.length; i += 5) {
                                            div.innerHTML += '\n<textarea class="box1A">' + data[i] + '</textarea>';
                                            div.innerHTML += '<textarea class="box1B">' + data[i + 1] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box2A">' + data[i + 2] + '</textarea>';
                                            div.innerHTML += '<textarea class="box2B">' + data[i + 3] + '</textarea>';
                                            div.innerHTML += '\n<textarea class="box3">' + data[i + 4] + '</textarea>';
                                        }
                                    });
                                }
                            }
                        </script>
                    }
                </td>
            </tr>
        </table>
    }
    </body>
    ```

1. Abra el archivo SearchData.cs y reemplace la clase **SearchData** por el código siguiente.

    ```cs
    public class SearchData
    {
        public SearchData()
        {
        }

        // Constructor to initialize the list of facets sent from the controller.
        public SearchData(List<string> facets)
        {
            facetText = new string[facets.Count];

            for (int i = 0; i < facets.Count; i++)
            {
                facetText[i] = facets[i];
            }
        }

        // Array to hold the text for each amenity.
        public string[] facetText { get; set; }

        // Array to hold the setting for each amenitity.
        public bool[] facetOn { get; set; }

        // The text to search for.
        public string searchText { get; set; }

        // Record if the next page is requested.
        public string paging { get; set; }

        // The list of results.
        public DocumentSearchResult<Hotel> resultList;

        public string scoring { get; set; }       
    }
    ```

1. Abra el archivo hotels.css y agregue las siguientes clases HTML.

    ```html
    .facetlist {
        list-style: none;
    }
    
    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;
    }
    ```

### <a name="add-code-to-the-controller-to-specify-a-scoring-profile"></a>Agregar código al controlador para especificar un perfil de puntuación

1. Abra el archivo de controlador principal. Agregue la siguiente instrucción **using** (para ayudar con la creación de listas).

    ```cs
    using System.Linq;
    ```

1. En este ejemplo, necesitamos que la llamada inicial a **Index** haga algo más que simplemente devolver la vista inicial. El método ahora busca hasta 20 servicios que se mostrarán en la vista.

    ```cs
        public async Task<ActionResult> Index()
        {
            InitSearch();

            // Set up the facets call in the search parameters.
            SearchOptions options = new SearchOptions();
            // Search for up to 20 amenities.
            options.Facets.Add("Tags,count:20");

            SearchResults<Hotel> searchResult = await _searchClient.SearchAsync<Hotel>("*", options);

            // Convert the results to a list that can be displayed in the client.
            List<string> facets = searchResult.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

            // Initiate a model with a list of facets for the first view.
            SearchData model = new SearchData(facets);

            // Save the facet text for the next view.
            SaveFacets(model, false);

            // Render the view including the facets.
            return View(model);
        }
    ```

1. Necesitamos dos métodos privados para guardar las facetas en el almacenamiento temporal y para recuperarlas del almacenamiento temporal y rellenar un modelo.

    ```cs
        // Save the facet text to temporary storage, optionally saving the state of the check boxes.
        private void SaveFacets(SearchData model, bool saveChecks = false)
        {
            for (int i = 0; i < model.facetText.Length; i++)
            {
                TempData["facet" + i.ToString()] = model.facetText[i];
                if (saveChecks)
                {
                    TempData["faceton" + i.ToString()] = model.facetOn[i];
                }
            }
            TempData["facetcount"] = model.facetText.Length;
        }

        // Recover the facet text to a model, optionally recoving the state of the check boxes.
        private void RecoverFacets(SearchData model, bool recoverChecks = false)
        {
            // Create arrays of the appropriate length.
            model.facetText = new string[(int)TempData["facetcount"]];
            if (recoverChecks)
            {
                model.facetOn = new bool[(int)TempData["facetcount"]];
            }

            for (int i = 0; i < (int)TempData["facetcount"]; i++)
            {
                model.facetText[i] = TempData["facet" + i.ToString()].ToString();
                if (recoverChecks)
                {
                    model.facetOn[i] = (bool)TempData["faceton" + i.ToString()];
                }
            }
        }
    ```

1. Es necesario establecer los parámetros **OrderBy** y **ScoringProfile** según corresponda. Reemplace el método **Index(SearchData model)** existente por lo siguiente.

    ```cs
    public async Task<ActionResult> Index(SearchData model)
    {
        try
        {
            InitSearch();

            int page;

            if (model.paging != null && model.paging == "next")
            {
                // Recover the facet text, and the facet check box settings.
                RecoverFacets(model, true);

                // Increment the page.
                page = (int)TempData["page"] + 1;

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();
            }
            else
            {
                // First search with text. 
                // Recover the facet text, but ignore the check box settings, and use the current model settings.
                RecoverFacets(model, false);

                // First call. Check for valid text input, and valid scoring profile.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }
                if (model.scoring == null)
                {
                    model.scoring = "Default";
                }
                page = 0;
            }

            // Setup the search parameters.
            var options = new SearchOptions
            {
                SearchMode = SearchMode.All,

                // Skip past results that have already been returned.
                Skip = page * GlobalVariables.ResultsPerPage,

                // Take only the next page worth of results.
                Size = GlobalVariables.ResultsPerPage,

                // Include the total number of results.
                IncludeTotalCount = true,
            };
            // Select the data properties to be returned.
            options.Select.Add("HotelName");
            options.Select.Add("Description");
            options.Select.Add("Tags");
            options.Select.Add("Rooms");
            options.Select.Add("Rating");
            options.Select.Add("LastRenovationDate");

            List<string> parameters = new List<string>();
            // Set the ordering based on the user's radio button selection.
            switch (model.scoring)
            {
                case "RatingRenovation":
                    // Set the ordering/scoring parameters.
                    options.OrderBy.Add("Rating desc");
                    options.OrderBy.Add("LastRenovationDate desc");
                    break;

                case "boostAmenities":
                    {
                        options.ScoringProfile = model.scoring;

                        // Create a string list of amenities that have been clicked.
                        for (int a = 0; a < model.facetOn.Length; a++)
                        {
                            if (model.facetOn[a])
                            {
                                parameters.Add(model.facetText[a]);
                            }
                        }

                        if (parameters.Count > 0)
                        {
                            options.ScoringParameters.Add($"amenities-{ string.Join(',', parameters)}");
                        }
                        else
                        {
                            // No amenities selected, so set profile back to default.
                            options.ScoringProfile = "";
                        }
                    }
                    break;

                case "renovatedAndHighlyRated":
                    options.ScoringProfile = model.scoring;
                    break;

                default:
                    break;
            }

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _searchClient.SearchAsync<Hotel>(model.searchText, options);

            // Ensure TempData is stored for the next call.
            TempData["page"] = page;
            TempData["searchfor"] = model.searchText;
            TempData["scoring"] = model.scoring;
            SaveFacets(model, true);

            // Calculate the room rate ranges.
            await foreach (var result in model.resultList.GetResultsAsync())
            {
                var cheapest = 0d;
                var expensive = 0d;

                foreach (var room in result.Document.Rooms)
                {
                    var rate = room.BaseRate;
                    if (rate < cheapest || cheapest == 0)
                    {
                        cheapest = (double)rate;
                    }
                    if (rate > expensive)
                    {
                        expensive = (double)rate;
                    }
                }

                result.Document.cheapest = cheapest;
                result.Document.expensive = expensive;
            }
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "1" });
        }

        return View("Index", model);
    }
    ```

    Lea los comentarios para cada una de las selecciones **switch**.

1. Si completó el código adicional para la sección anterior sobre el ordenamiento basado en varias propiedades, no es necesario hacer cambios en la acción **Next**.

### <a name="run-and-test-the-app"></a>Ejecutar y probar la aplicación

1. Ejecute la aplicación. Debería ver un conjunto completo de servicios en la vista.

1. Para el ordenamiento, si selecciona "By numerical Rating", obtendrá el ordenamiento que ya se implementó en este tutorial, con la fecha de reforma que decide entre los hoteles de clasificación igual.

   ![Ordenar "beach" según la clasificación](./media/tutorial-csharp-create-first-app/azure-search-orders-beach.png)

1. Ahora pruebe el perfil "By amenities". Haga varias selecciones de servicios y compruebe que en la lista de resultados se ascienden los hoteles con dichos servicios.

   ![Ordenar "beach" según el perfil](./media/tutorial-csharp-create-first-app/azure-search-orders-beach-profile.png)

1. Pruebe el perfil "By Renovated date/Rating" para ver si obtiene los resultados esperados. Solo los hoteles reformados recientemente deberían obtener un aumento tipo _freshness_.

### <a name="resources"></a>Recursos

Para más información, consulte el siguiente tema [Incorporación de perfiles de puntuación a un índice de Azure Cognitive Search](./index-add-scoring-profiles.md).

## <a name="takeaways"></a>Puntos clave

Tenga en cuenta las siguientes conclusiones de este proyecto:

* Los usuarios esperarán que los resultados de búsqueda estén ordenados, con los más relevantes en primer lugar.
* Los datos deben estar estructurados para facilitar el ordenamiento. No pudimos ordenar fácilmente primero por "más económico", ya que los datos no están estructurados para habilitar el ordenamiento sin código adicional.
* Puede haber varios niveles en ordenamiento para diferenciar entre los resultados que tienen el mismo valor en un nivel superior de ordenamiento.
* Es natural que algunos resultados se ordenen en orden ascendente (por ejemplo, la distancia desde un punto determinado) y algunos en orden descendente (por ejemplo, la clasificación de huésped).
* Los perfiles de puntuación se pueden definir cuando no haya comparaciones numéricas disponibles, o estas no sean suficientemente inteligentes, para un conjunto de datos. La puntuación de cada resultado ayudará a ordenar y mostrar los resultados de forma inteligente.

## <a name="next-steps"></a>Pasos siguientes

Ha completado esta serie de tutoriales de C#, y ahora dispone de conocimientos valiosos sobre las API de Azure Cognitive Search.

Si necesita otras referencias y tutoriales, puede explorar [Microsoft Learn](/learn/browse/?products=azure) o los demás tutoriales en la [documentación de Azure Cognitive Search](./index.yml).