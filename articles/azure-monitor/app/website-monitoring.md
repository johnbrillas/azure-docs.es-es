---
title: 'Inicio rápido: Supervisión de sitios web con Azure Monitor Application Insights'
description: En este inicio rápido aprenderá a configurar la supervisión de sitios web en el lado cliente/explorador con Azure Monitor Application Insights.
ms.topic: quickstart
ms.date: 08/19/2020
ms.custom: mvc
ms.openlocfilehash: 225ba3642a9f66b1645565552620584bff0cb1c8
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726098"
---
# <a name="quickstart-start-monitoring-your-website-with-azure-monitor-application-insights"></a>Inicio rápido: Empezar a supervisar un sitio web con Azure Monitor Application Insights

En este inicio rápido, aprenderá a agregar el SDK de JavaScript para Application Insights de código abierto a un sitio web. También aprenderá a conocer mejor la experiencia de lado cliente/explorador de los visitantes de su sitio web.

Con Application Insights de Azure Monitor puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del sitio web. También puede identificar y diagnosticar errores en la aplicación rápidamente sin tener que esperar a que un usuario informe de ellos. Application Insights proporciona funcionalidades de supervisión tanto del lado servidor como del lado cliente/explorador.

## <a name="prerequisites"></a>Prerrequisitos

* Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* Un sitio web al que puede agregar el SDK de JavaScript para Application Insights.

## <a name="enable-application-insights"></a>Habilitación de Application Insights

Application Insights puede recopilar datos de telemetría desde cualquier aplicación conectada a Internet que se ejecute tanto en la nube como en un entorno local. Para ver estos datos, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Seleccione **Crear un recurso** > **Herramientas de administración** > **Application Insights**.

   > [!NOTE]
   >Si es la primera vez que crea un recurso de Application Insights, consulte [Creación de recursos en Application Insights](./create-new-resource.md).
1. Cuando aparezca el cuadro de configuración, use la tabla siguiente para completar los campos de entrada:

    | Configuración        | Value           | Descripción  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | El nombre que identifica la aplicación que está supervisando. |
   | **Grupo de recursos**     | myResourceGroup      | El nombre del nuevo grupo de recursos que hospeda los datos de Application Insights. puede crear un grupo de recursos o use uno existente. |
   | **Ubicación** | Este de EE. UU. | Elija una ubicación cerca de usted o de donde se hospeda la aplicación. |
1. Seleccione **Crear**.

## <a name="create-an-html-file"></a>Creación de un archivo HTML

1. En el equipo local, cree un archivo llamado ``hello_world.html``. En este ejemplo, cree el archivo en la raíz de la unidad C, así que debe ser algo como ``C:\hello_world.html``.
1. Copie y pegue el siguiente script en ``hello_world.html``:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations, visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-application-insights-sdk"></a>Configuración del SDK de Application Insights

1. Seleccione **Información general** > **Información esencial** y copie la **clave de instrumentación** de la aplicación.

   ![Formulario Nuevo recurso de Application Insights](media/website-monitoring/instrumentation-key-001.png)

1. Agregue el siguiente script al archivo ``hello_world.html`` antes de la etiqueta de cierre ``</head>``:

    ```html
    <script type="text/javascript">
    !function(T,l,y){var S=T.location,k="script",D="instrumentationKey",C="ingestionendpoint",I="disableExceptionTracking",E="ai.device.",b="toLowerCase",w="crossOrigin",N="POST",e="appInsightsSDK",t=y.name||"appInsights";(y.name||T[e])&&(T[e]=t);var n=T[t]||function(d){var g=!1,f=!1,m={initialize:!0,queue:[],sv:"5",version:2,config:d};function v(e,t){var n={},a="Browser";return n[E+"id"]=a[b](),n[E+"type"]=a,n["ai.operation.name"]=S&&S.pathname||"_unknown_",n["ai.internal.sdkVersion"]="javascript:snippet_"+(m.sv||m.version),{time:function(){var e=new Date;function t(e){var t=""+e;return 1===t.length&&(t="0"+t),t}return e.getUTCFullYear()+"-"+t(1+e.getUTCMonth())+"-"+t(e.getUTCDate())+"T"+t(e.getUTCHours())+":"+t(e.getUTCMinutes())+":"+t(e.getUTCSeconds())+"."+((e.getUTCMilliseconds()/1e3).toFixed(3)+"").slice(2,5)+"Z"}(),iKey:e,name:"Microsoft.ApplicationInsights."+e.replace(/-/g,"")+"."+t,sampleRate:100,tags:n,data:{baseData:{ver:2}}}}var h=d.url||y.src;if(h){function a(e){var t,n,a,i,r,o,s,c,u,p,l;g=!0,m.queue=[],f||(f=!0,t=h,s=function(){var e={},t=d.connectionString;if(t)for(var n=t.split(";"),a=0;a<n.length;a++){var i=n[a].split("=");2===i.length&&(e[i[0][b]()]=i[1])}if(!e[C]){var r=e.endpointsuffix,o=r?e.location:null;e[C]="https://"+(o?o+".":"")+"dc."+(r||"services.visualstudio.com")}return e}(),c=s[D]||d[D]||"",u=s[C],p=u?u+"/v2/track":d.endpointUrl,(l=[]).push((n="SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details)",a=t,i=p,(o=(r=v(c,"Exception")).data).baseType="ExceptionData",o.baseData.exceptions=[{typeName:"SDKLoadFailed",message:n.replace(/\./g,"-"),hasFullStack:!1,stack:n+"\nSnippet failed to load ["+a+"] -- Telemetry is disabled\nHelp Link: https://go.microsoft.com/fwlink/?linkid=2128109\nHost: "+(S&&S.pathname||"_unknown_")+"\nEndpoint: "+i,parsedStack:[]}],r)),l.push(function(e,t,n,a){var i=v(c,"Message"),r=i.data;r.baseType="MessageData";var o=r.baseData;return o.message='AI (Internal): 99 message:"'+("SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details) ("+n+")").replace(/\"/g,"")+'"',o.properties={endpoint:a},i}(0,0,t,p)),function(e,t){if(JSON){var n=T.fetch;if(n&&!y.useXhr)n(t,{method:N,body:JSON.stringify(e),mode:"cors"});else if(XMLHttpRequest){var a=new XMLHttpRequest;a.open(N,t),a.setRequestHeader("Content-type","application/json"),a.send(JSON.stringify(e))}}}(l,p))}function i(e,t){f||setTimeout(function(){!t&&m.core||a()},500)}var e=function(){var n=l.createElement(k);n.src=h;var e=y[w];return!e&&""!==e||"undefined"==n[w]||(n[w]=e),n.onload=i,n.onerror=a,n.onreadystatechange=function(e,t){"loaded"!==n.readyState&&"complete"!==n.readyState||i(0,t)},n}();y.ld<0?l.getElementsByTagName("head")[0].appendChild(e):setTimeout(function(){l.getElementsByTagName(k)[0].parentNode.appendChild(e)},y.ld||0)}try{m.cookie=l.cookie}catch(p){}function t(e){for(;e.length;)!function(t){m[t]=function(){var e=arguments;g||m.queue.push(function(){m[t].apply(m,e)})}}(e.pop())}var n="track",r="TrackPage",o="TrackEvent";t([n+"Event",n+"PageView",n+"Exception",n+"Trace",n+"DependencyData",n+"Metric",n+"PageViewPerformance","start"+r,"stop"+r,"start"+o,"stop"+o,"addTelemetryInitializer","setAuthenticatedUserContext","clearAuthenticatedUserContext","flush"]),m.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4};var s=(d.extensionConfig||{}).ApplicationInsightsAnalytics||{};if(!0!==d[I]&&!0!==s[I]){var c="onerror";t(["_"+c]);var u=T[c];T[c]=function(e,t,n,a,i){var r=u&&u(e,t,n,a,i);return!0!==r&&m["_"+c]({message:e,url:t,lineNumber:n,columnNumber:a,error:i}),r},d.autoExceptionInstrumented=!0}return m}(y.cfg);function a(){y.onInit&&y.onInit(n)}(T[t]=n).queue&&0===n.queue.length?(n.queue.push(a),n.trackPageView({})):a()}(window,document,{nConfig||{}).ApplicationInsightsAnalytics||{};if(!0!==d[C]&&!0!==s[C]){method="onerror",t(["_"+method]);var c=T[method];T[method]=function(e,t,n,a,i){var r=c&&c(e,t,n,a,i);return!0!==r&&m["_"+method]({message:e,url:t,lineNumber:n,columnNumber:a,error:i}),r},d.autoExceptionInstrumented=!0}return m}(y.cfg);(T[t]=n).queue&&0===n.queue.length&&n.trackPageView({})}(window,document,{
    src: "https://js.monitor.azure.com/scripts/b/ai.2.min.js", // The SDK URL Source
    // name: "appInsights", // Global SDK Instance name defaults to "appInsights" when not supplied
    // ld: 0, // Defines the load delay (in ms) before attempting to load the sdk. -1 = block page load and add to head. (default) = 0ms load after timeout,
    // useXhr: 1, // Use XHR instead of fetch to report failures (if available),
    crossOrigin: "anonymous", // When supplied this will add the provided value as the cross origin attribute on the script tag
    // onInit: null, // Once the application insights instance has loaded and initialized this callback function will be called with 1 argument -- the sdk instance (DO NOT ADD anything to the sdk.queue -- As they won't get called)
    cfg: { // Application Insights Configuration
        instrumentationKey: "YOUR_INSTRUMENTATION_KEY_GOES_HERE"
        /* ...Other Configuration Options... */
    }});
    </script>
    ```

    > [!NOTE]
    > El fragmento de código actual (que se muestra a continuación) es la versión "5"; la versión se codifica en el fragmento de código como sv:"#", y los [detalles de configuración y versión actual están disponibles en GitHub](https://go.microsoft.com/fwlink/?linkid=2156318).
   
1. Modifique ``hello_world.html`` y agregue la clave de instrumentación.

1. Abra ``hello_world.html`` en una sesión de explorador local. Esta acción crea una vista de página individual. Puede actualizar el explorador para generar varias vistas de páginas de prueba.

## <a name="monitor-your-website-in-the-azure-portal"></a>Supervisión de un sitio web en Azure Portal

1. Vuelva a abrir la página de **información general** de Application Insights en Azure Portal para ver los detalles de la aplicación en ejecución. La página de **información general** es donde ha recuperado la clave de instrumentación.

   Los cuatro gráficos predeterminados de la página de introducción se extienden a los datos de la aplicación del lado servidor. Dado que vamos a instrumentar las interacciones del lado cliente/explorador con el SDK de JavaScript, esta vista concreta no se aplica, salvo que también tengamos instalado un SDK del lado servidor.

1. Seleccione **Analytics** ![Icono de mapa de aplicación](media/website-monitoring/006.png).  Esta acción abre **Analytics**, que proporciona un lenguaje de consulta avanzado para analizar todos los datos recopilados por Application Insights. Para ver los datos relacionados con las solicitudes de explorador del lado cliente, ejecute la siguiente consulta:

    ```kusto
    // average pageView duration by name
    let timeGrain=1s;
    let dataset=pageViews
    // additional filters can be applied here
    | where timestamp > ago(15m)
    | where client_Type == "Browser" ;
    // calculate average pageView duration for all pageViews
    dataset
    | summarize avg(duration) by bin(timestamp, timeGrain)
    | extend pageView='Overall'
    // render result in a chart
    | render timechart
    ```

   ![Gráfico de Analytics con las solicitudes de usuario durante un período de tiempo](./media/website-monitoring/analytics-query.png)

1. Vuelva a la página **Introducción**. En el encabezado **Investigar**, seleccione **Explorador** y, después, seleccione **Rendimiento**.  Aparecen las métricas relacionadas con el rendimiento de su sitio web. Hay una vista correspondiente para analizar los errores y excepciones del sitio web. Puede seleccionar **Ejemplos** para acceder a los [detalles de la transacción de un extremo a otro](./transaction-diagnostics.md).

   ![Gráfico de las métricas del servidor](./media/website-monitoring/browser-performance.png)

1. En el menú principal de Application Insights, en el encabezado **Uso**, seleccione [**Usuarios**](./usage-segmentation.md) para empezar a explorar las [herramientas de análisis del comportamiento del usuario](./usage-overview.md). Dado que las pruebas se realizan desde una sola máquina, solo se verán los datos de un usuario. En el caso de un sitio web en vivo, la distribución de los usuarios podría ser como esta:

     ![Gráfico de usuario](./media/website-monitoring/usage-users.png)

1. En el caso de un sitio web más complejo con varias páginas, puede usar la herramienta [**Flujos de usuario**](./usage-flows.md) para realizar el seguimiento del recorrido que realizan los visitantes en las distintas partes del sitio web.

   ![Visualización de flujos de usuario](./media/website-monitoring/user-flows.png)

Para más información acerca de configuraciones avanzadas para supervisar sitios web, consulte la [referencia a JavaScript SDK API](./javascript.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si planea seguir trabajando con otros inicios rápidos o tutoriales, no limpie los recursos creados en este inicio rápido. De lo contrario, realice los siguientes pasos para eliminarlos en Azure Portal.

> [!NOTE]
> Si ha usado un grupo de recursos existente, las siguientes instrucciones no funcionarán. En su lugar, puede eliminar el recurso de Application Insights individual. Tenga esto en cuenta que cuando se elimina un grupo de recursos, se eliminan también todos los recursos subyacentes que sean miembros del grupo.

1. En el menú de la izquierda de Azure Portal, seleccione **Grupos de recursos** y luego **myResourceGroup**, o el nombre de un grupo de recursos temporal.
1. En la página del grupo de recursos, seleccione **Eliminar**, escriba **myResourceGroup** en el cuadro de texto y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Búsqueda y diagnóstico de problemas de rendimiento](../logs/log-query-overview.md)

