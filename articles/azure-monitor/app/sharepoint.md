---
title: Supervisión de un sitio de SharePoint con Application Insights
description: Inicio de la supervisión de una nueva aplicación con una nueva clave de instrumentación
ms.topic: conceptual
ms.date: 09/08/2020
ms.openlocfilehash: 74f0eeecb303d68af8aaa48b5c1806ceeef4d318
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102486376"
---
# <a name="monitor-a-sharepoint-site-with-application-insights"></a>Supervisión de un sitio de SharePoint con Application Insights

Azure Application Insights le permite supervisar la disponibilidad, el rendimiento y el uso de sus aplicaciones. Aquí aprenderá a configurarlo para un sitio de SharePoint.

> [!NOTE]
> Debido a algunos problemas de seguridad, no puede agregar directamente el script que se describe en este artículo a sus páginas web de la experiencia de usuario moderna de SharePoint. También puede usar [el marco de SharePoint (SPFx)](/sharepoint/dev/spfx/extensions/overview-extensions) para crear una extensión personalizada que puede usar para instalar Application Insights en los sitios de SharePoint.

## <a name="create-an-application-insights-resource"></a>Creación de recursos en Application Insights
En el [portal de Azure](https://portal.azure.com), cree un nuevo recurso de Application Insights. Elija ASP.NET como el tipo de aplicación.

![Haga clic en Propiedades, seleccione la clave y presione ctrl + C.](./media/sharepoint/001.png)

En la ventana que se abre podrá ver los datos de uso y rendimiento acerca de la aplicación. Para volver a ella la próxima vez que inicie sesión en Azure, encontrará un icono correspondiente en la pantalla de inicio. También puede hacer clic en Examinar para buscarla.

## <a name="add-the-script-to-your-web-pages"></a>Incorporación del script a sus páginas web

El fragmento de código actual (que se muestra a continuación) es la versión "5"; la versión se codifica en el fragmento de código como sv:"#", y la [versión actual también está disponible en GitHub](https://go.microsoft.com/fwlink/?linkid=2156318).

```HTML
<!-- 
To collect user behavior analytics tools about your application, 
insert the following script into each page you want to track.
Place this code immediately before the closing </head> tag,
and before any other scripts. Your first data will appear 
automatically in just a few seconds.
-->
<script type="text/javascript">
!function(T,l,y){var S=T.location,k="script",D="instrumentationKey",C="ingestionendpoint",I="disableExceptionTracking",E="ai.device.",b="toLowerCase",w="crossOrigin",N="POST",e="appInsightsSDK",t=y.name||"appInsights";(y.name||T[e])&&(T[e]=t);var n=T[t]||function(d){var g=!1,f=!1,m={initialize:!0,queue:[],sv:"5",version:2,config:d};function v(e,t){var n={},a="Browser";return n[E+"id"]=a[b](),n[E+"type"]=a,n["ai.operation.name"]=S&&S.pathname||"_unknown_",n["ai.internal.sdkVersion"]="javascript:snippet_"+(m.sv||m.version),{time:function(){var e=new Date;function t(e){var t=""+e;return 1===t.length&&(t="0"+t),t}return e.getUTCFullYear()+"-"+t(1+e.getUTCMonth())+"-"+t(e.getUTCDate())+"T"+t(e.getUTCHours())+":"+t(e.getUTCMinutes())+":"+t(e.getUTCSeconds())+"."+((e.getUTCMilliseconds()/1e3).toFixed(3)+"").slice(2,5)+"Z"}(),iKey:e,name:"Microsoft.ApplicationInsights."+e.replace(/-/g,"")+"."+t,sampleRate:100,tags:n,data:{baseData:{ver:2}}}}var h=d.url||y.src;if(h){function a(e){var t,n,a,i,r,o,s,c,u,p,l;g=!0,m.queue=[],f||(f=!0,t=h,s=function(){var e={},t=d.connectionString;if(t)for(var n=t.split(";"),a=0;a<n.length;a++){var i=n[a].split("=");2===i.length&&(e[i[0][b]()]=i[1])}if(!e[C]){var r=e.endpointsuffix,o=r?e.location:null;e[C]="https://"+(o?o+".":"")+"dc."+(r||"services.visualstudio.com")}return e}(),c=s[D]||d[D]||"",u=s[C],p=u?u+"/v2/track":d.endpointUrl,(l=[]).push((n="SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details)",a=t,i=p,(o=(r=v(c,"Exception")).data).baseType="ExceptionData",o.baseData.exceptions=[{typeName:"SDKLoadFailed",message:n.replace(/\./g,"-"),hasFullStack:!1,stack:n+"\nSnippet failed to load ["+a+"] -- Telemetry is disabled\nHelp Link: https://go.microsoft.com/fwlink/?linkid=2128109\nHost: "+(S&&S.pathname||"_unknown_")+"\nEndpoint: "+i,parsedStack:[]}],r)),l.push(function(e,t,n,a){var i=v(c,"Message"),r=i.data;r.baseType="MessageData";var o=r.baseData;return o.message='AI (Internal): 99 message:"'+("SDK LOAD Failure: Failed to load Application Insights SDK script (See stack for details) ("+n+")").replace(/\"/g,"")+'"',o.properties={endpoint:a},i}(0,0,t,p)),function(e,t){if(JSON){var n=T.fetch;if(n&&!y.useXhr)n(t,{method:N,body:JSON.stringify(e),mode:"cors"});else if(XMLHttpRequest){var a=new XMLHttpRequest;a.open(N,t),a.setRequestHeader("Content-type","application/json"),a.send(JSON.stringify(e))}}}(l,p))}function i(e,t){f||setTimeout(function(){!t&&m.core||a()},500)}var e=function(){var n=l.createElement(k);n.src=h;var e=y[w];return!e&&""!==e||"undefined"==n[w]||(n[w]=e),n.onload=i,n.onerror=a,n.onreadystatechange=function(e,t){"loaded"!==n.readyState&&"complete"!==n.readyState||i(0,t)},n}();y.ld<0?l.getElementsByTagName("head")[0].appendChild(e):setTimeout(function(){l.getElementsByTagName(k)[0].parentNode.appendChild(e)},y.ld||0)}try{m.cookie=l.cookie}catch(p){}function t(e){for(;e.length;)!function(t){m[t]=function(){var e=arguments;g||m.queue.push(function(){m[t].apply(m,e)})}}(e.pop())}var n="track",r="TrackPage",o="TrackEvent";t([n+"Event",n+"PageView",n+"Exception",n+"Trace",n+"DependencyData",n+"Metric",n+"PageViewPerformance","start"+r,"stop"+r,"start"+o,"stop"+o,"addTelemetryInitializer","setAuthenticatedUserContext","clearAuthenticatedUserContext","flush"]),m.SeverityLevel={Verbose:0,Information:1,Warning:2,Error:3,Critical:4};var s=(d.extensionConfig||{}).ApplicationInsightsAnalytics||{};if(!0!==d[I]&&!0!==s[I]){var c="onerror";t(["_"+c]);var u=T[c];T[c]=function(e,t,n,a,i){var r=u&&u(e,t,n,a,i);return!0!==r&&m["_"+c]({message:e,url:t,lineNumber:n,columnNumber:a,error:i}),r},d.autoExceptionInstrumented=!0}return m}(y.cfg);function a(){y.onInit&&y.onInit(n)}(T[t]=n).queue&&0===n.queue.length?(n.queue.push(a),n.trackPageView({})):a()}(window,document,{
src: "https://js.monitor.azure.com/scripts/b/ai.2.gbl.min.js", // The SDK URL Source
// name: "appInsights", // Global SDK Instance name defaults to "appInsights" when not supplied
// ld: 0, // Defines the load delay (in ms) before attempting to load the sdk. -1 = block page load and add to head. (default) = 0ms load after timeout,
// useXhr: 1, // Use XHR instead of fetch to report failures (if available),
crossOrigin: "anonymous", // When supplied this will add the provided value as the cross origin attribute on the script tag
// onInit: null, // Once the application insights instance has loaded and initialized this callback function will be called with 1 argument -- the sdk instance (DO NOT ADD anything to the sdk.queue -- As they won't get called)
cfg: { // Application Insights Configuration
  instrumentationKey:"INSTRUMENTATION_KEY"
}});
</script>
```

> [!NOTE]
> La dirección URL de SharePoint usa un formato de módulo diferente "...\ai.2.gbl.min.js" (observe el **.gbl** adicional). Este formato de módulo alternativo es necesario para evitar un problema causado por el orden en que se cargan los scripts, que provocará un error en la inicialización del SDK y dará lugar a la pérdida de eventos de telemetría.
>
> El problema lo provoca que requireJS se carga e inicializa antes que el SDK.

Inserte el script justo antes de la etiqueta &lt;/head&gt; de cada página que desea seguir. Si su sitio web tiene una página maestra, puede colocar el script allí. Por ejemplo, en un proyecto de ASP.NET MVC, lo colocaría en View\Shared\\_Layout.cshtml.

El script contiene la clave de instrumentación que dirige los datos de telemetría al recurso de Application Insights.

### <a name="add-the-code-to-your-site-pages"></a>Agregar el código a las páginas del sitio
#### <a name="on-the-master-page"></a>En la página maestra
Si se puede editar la página maestra del sitio, dispondrá de supervisión para cada página del sitio.

Desproteja y edite la página maestra mediante SharePoint Designer o cualquier otro editor.

![Captura de pantalla que muestra cómo editar la página maestra mediante SharePoint Designer u otro editor.](./media/sharepoint/03-master.png)

Agregue el código justo antes de la etiqueta </head> . 

![Captura de pantalla que muestra dónde agregar el código a la página del sitio.](./media/sharepoint/04-code.png)

#### <a name="or-on-individual-pages"></a>O, en páginas individuales
Para supervisar un conjunto limitado de páginas, agregue el script por separado a cada página. 

Inserte un elemento web e incruste el fragmento de código en él.

![Captura de pantalla que muestra cómo agregar el script para supervisar un conjunto limitado de páginas.](./media/sharepoint/05-page.png)

## <a name="view-data-about-your-app"></a>Visualización de los datos de la aplicación
Vuelva a implementar la aplicación.

Vuelva a la hoja de la aplicación en el [Portal de Azure](https://portal.azure.com).

Los primeros eventos aparecerán en la búsqueda. 

![Captura de pantalla que muestra los nuevos datos que se pueden ver en la aplicación.](./media/sharepoint/09-search.png)

Si espera más datos, haga clic en Actualizar después de unos segundos.

## <a name="capturing-user-id"></a>Captura del identificador de usuario
El fragmento de código de una página web estándar no captura el identificador de usuario de SharePoint, pero una pequeña modificación le permitirá hacerlo.

1. Copie la clave de instrumentación de la aplicación de la lista desplegable Essentials en Application Insights . 

    ![Captura de pantalla que muestra la copia de la instrumentación de la aplicación desde la lista desplegable de Essentials en Application Insights.](./media/sharepoint/02-props.png)

1. En el siguiente fragmento, sustituya la clave de instrumentación por "XXXX". 
2. Inserte el script en la aplicación de SharePoint en lugar del fragmento de código que obtenga desde el portal.

```


<SharePoint:ScriptLink ID="ScriptLink1" name="SP.js" runat="server" localizable="false" loadafterui="true" /> 
<SharePoint:ScriptLink ID="ScriptLink2" name="SP.UserProfiles.js" runat="server" localizable="false" loadafterui="true" /> 

<script type="text/javascript"> 
var personProperties; 

// Ensure that the SP.UserProfiles.js file is loaded before the custom code runs. 
SP.SOD.executeOrDelayUntilScriptLoaded(getUserProperties, 'SP.UserProfiles.js'); 

function getUserProperties() { 
    // Get the current client context and PeopleManager instance. 
    var clientContext = new SP.ClientContext.get_current(); 
    var peopleManager = new SP.UserProfiles.PeopleManager(clientContext); 

    // Get user properties for the target user. 
    // To get the PersonProperties object for the current user, use the 
    // getMyProperties method. 

    personProperties = peopleManager.getMyProperties(); 

    // Load the PersonProperties object and send the request. 
    clientContext.load(personProperties); 
    clientContext.executeQueryAsync(onRequestSuccess, onRequestFail); 
} 

// This function runs if the executeQueryAsync call succeeds. 
function onRequestSuccess() { 
var appInsights=window.appInsights||function(config){
function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
    }({
        instrumentationKey:"XXXX"
    });
    window.appInsights=appInsights;
    appInsights.trackPageView(document.title,window.location.href, {User: personProperties.get_displayName()});
} 

// This function runs if the executeQueryAsync call fails. 
function onRequestFail(sender, args) { 
} 
</script> 


```



## <a name="next-steps"></a>Pasos siguientes
* [Pruebas web](./monitor-web-app-availability.md) para supervisar la disponibilidad de su sitio.
* [Application Insights](./app-insights-overview.md) para otros tipos de aplicación.

<!--Link references-->
