---
title: Complemento Click Analytics Auto-collection para el SDK de JavaScript para Application Insights
description: Cómo instalar y usar el complemento Click Analytics Auto-collection para el SDK de JavaScript para Application Insights.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: lagayhar
ms.openlocfilehash: e69d5cc76f8f4b14ab87e13546c98859bb801418
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98234626"
---
# <a name="click-analytics-auto-collection-plugin-for-application-insights-javascript-sdk"></a>Complemento Click Analytics Auto-collection para el SDK de JavaScript para Application Insights

El complemento Click Analytics Auto-collection para el SDK de JavaScript para Application Insights permite realizar un seguimiento automático de los eventos de clic en páginas web con `data-*`etiquetas META. Este complemento usa los atributos globales de `data-*` para capturar los eventos de clic y rellenar los datos de telemetría.

## <a name="getting-started"></a>Introducción

Los usuarios pueden configurar el complemento Click Analytics Auto-collection a través de npm.

### <a name="npm-setup"></a>Configuración de npm

Instalación del paquete de npm:

```bash
npm install --save @microsoft/applicationinsights-clickanalytics-js @microsoft/applicationinsights-web
```

```js

import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { ClickAnalyticsPlugin } from '@microsoft/applicationinsights-clickanalytics-js';

const clickPluginInstance = new ClickAnalyticsPlugin();
// Click Analytics configuration
const clickPluginConfig = {
  autoCapture: true
};
// Application Insights Configuration
const configObj = {
  instrumentationKey: "YOUR INSTRUMENTATION KEY",
  extensions: [clickPluginInstance],
  extensionConfig: {
    [clickPluginInstance.identifier]: clickPluginConfig
  },
};

const appInsights = new ApplicationInsights({ config: configObj });
appInsights.loadAppInsights();
```

## <a name="how-to-effectively-use-the-plugin"></a>Procedimiento para usar el complemento de forma eficaz

1. Los datos de telemetría que se generan en los eventos de clic se almacenan como `customEvents` en la sección Application Insights de Azure Portal.
2. Para rellenarse el elemento `name` de customEvent se siguen estas reglas:
    1.  El elemento `id` que se proporciona en `data-*-id` se usará como nombre de customEvent. Por ejemplo, si el elemento HTML en el que se ha hecho clic tiene el atributo "data-sample-id"="button1", "button1" será el nombre de customEvent.
    2. Si no existe ese atributo y `useDefaultContentNameOrId` está establecido en `true` en la configuración, se utilizará el atributo HTML `id` del elemento en el que se ha hecho clic o el nombre de contenido del elemento como nombre de customEvent.
    3. Si el valor de `useDefaultContentNameOrId` es false, el nombre de customEvent será "not_specified".

    > [!TIP]
    > Se recomienda establecer `useDefaultContentNameOrId` en true para generar datos significativos.  
3. `parentDataTag` hace dos cosas:
    1. Si esta etiqueta está presente, el complemento capturará los atributos y valores de `data-*` de todos los elementos HTML primarios del elemento en el que se ha hecho clic.
    2. Para mejorar la eficacia, el complemento usa esta etiqueta como marca y cuando la encuentra, deja de procesar el DOM (Document Object Model).
    
    > [!CAUTION]
    > Una vez que se utiliza `parentDataTag`, tiene un efecto persistente en toda la aplicación, no solo en el elemento HTML en el que se usó.
4. Si `customDataPrefix` lo proporciona el usuario, debe comenzar por `data-`, por ejemplo `data-sample-`. En HTML, los atributos globales de `data-*` forman una clase de atributos denominados atributos de datos personalizados, que permiten intercambiar información propietaria entre el código HTML y su representación DOM mediante scripts. Los exploradores más antiguos (Internet Explorer, Safari) eliminarán los atributos que no comprenda, salvo que empiecen por `data-`.

    El signo `*` de `data-*`  se pueden reemplazar por cualquier nombre que siga la [regla de producción de nombres XML](https://www.w3.org/TR/REC-xml/#NT-Name), pero existen las siguientes restricciones:
    - El nombre no debe empezar por "xml", y da igual si se usan mayúsculas o minúsculas.
    - El nombre no debe contener ningún signo de punto y coma (U + 003A).
    - El nombre no debe contener mayúsculas.

## <a name="configuration"></a>Configuración

| Nombre                  | Tipo                               | Valor predeterminado | Descripción                                                                                                                              |
| --------------------- | -----------------------------------| --------| ---------------------------------------------------------------------------------------------------------------------------------------- |
| autoCapture           | boolean                            | true    | Configuración de captura automática.                                                                                                         |
| devolución de llamada              | [IValueCallback](#ivaluecallback)  | null    | Configuración de las devoluciones de llamada.                                                                                                                 |
| pageTags              | string                             | null    | Etiquetas de página.                                                                                                                               |
| dataTags              | [ICustomDataTags](#icustomdatatags)| null    | Etiquetas de datos personalizadas que se proporcionan para invalidar las etiquetas predeterminadas que se usan para capturar datos de clic.                                                           |
| urlCollectHash        | boolean                            | false   | Habilita el registro de todos los valores que haya después de un carácter "#" de la dirección URL.                                                                          |
| urlCollectQuery       | boolean                            | false   | Habilita el registro de la cadena de consulta de la dirección URL.                                                                                      |
| behaviorValidator     | Función                           | null  | Función de devolución de llamada que se va a usar para la validación del valor `data-*-bhvr`. Para más información, vaya a la [sección behaviorValidator](#behaviorvalidator).|
| defaultRightClickBhvr | cadena (o) número                 | ''      | El valor predeterminado de Behavior cuando se ha producido un evento de clic con el botón derecho. Este valor se reemplazará si el elemento tiene el atributo `data-*-bhvr`. |
| dropInvalidEvents     | boolean                            | false   | Marca para eliminar todos aquellos eventos que no tengan datos de clic útiles.                                                                                   |

### <a name="ivaluecallback"></a>IValueCallback

| Nombre               | Tipo     | Valor predeterminado | Descripción                                                                             |
| ------------------ | -------- | ------- | --------------------------------------------------------------------------------------- |
| pageName           | Función | null    | Función para reemplazar el comportamiento predeterminado de captura de pageName.                           |
| pageActionPageTags | Función | null    | Función de devolución de llamada para aumentar el valor predeterminado de pageTags que se recopila en el evento pageAction.  |
| contentName        | Función | null    | Función de devolución de llamada para rellenar el valor de contentName personalizado.                                 |

### <a name="icustomdatatags"></a>ICustomDataTags

| Nombre                      | Tipo    | Valor predeterminado   | Descripción                                                                                       |
|---------------------------|---------|-----------|---------------------------------------------------------------------------------------------------|
| useDefaultContentNameOrId | boolean | false     | Cuando un elemento determinado no está etiquetado con el valor de customDataPrefix predeterminado o el usuario no proporciona el valor de customDataPrefix, esta marca se usa para recopilar el atributo HTML estándar de contentName. |
| customDataPrefix          | string  | `data-`   | El nombre de contenido de captura automática y el valor de los elementos que se etiquetan con el prefijo proporcionado.       |
| aiBlobAttributeTag        | string  | `ai-blob` | El complemento admite el etiquetado de metadatos del contenido de los blobs JSON, en lugar de los atributos individuales de `data-*`. |
| metaDataPrefix            | string  | null      | Nombre y contenido del elemento meta del encabezado HTML de captura automática con prefijo proporcionado. |
| captureAllMetaDataContent | string  | null      | Captura automática de todos los nombres y el contenido de los elementos meta del encabezado HTML. El valor predeterminado es False. Si está habilitado, invalidará el valor de metaDataPrefix proporcionado. |
| parentDataTag             | string  | null      | Detiene el recorrido hasta llegar al DOM para capturar el nombre de contenido y el valor de los elementos cuando encuentran esta etiqueta.|
| dntDataTag                | string  | `ai-dnt`  | El complemento omitirá los elementos HTML con este atributo al capturar datos de telemetría.|

### <a name="behaviorvalidator"></a>behaviorValidator

La función behaviorValidator se puede usar cuando se desee garantizar la coherencia de los datos, aunque las comprobaciones automáticas que han etiquetado los comportamientos del código se ajusten a una lista predefinida de taxonomía conocida y aceptada dentro de la empresa. No es necesario, ni se espera, que la mayoría de los clientes de Azure Monitor lo usen, pero está disponible para escenarios avanzados. Hay tres funciones de devolución de llamada de behaviorValidator que se exponen como parte de esta extensión. Sin embargo, si ninguna de ellas cubre sus necesidades, los usuarios pueden usar las suyas propias. La intención es usar su propia estructura de datos de comportamientos, el complemento usa esta función del validador al extraer los comportamientos de las etiquetas de datos.

| Nombre                   | Descripción                                                                        |
| ---------------------- | -----------------------------------------------------------------------------------|
| BehaviorValueValidator | Esta función de devolución de llamada se usa si la estructura de datos de comportamientos es una matriz de cadenas.|
| BehaviorMapValidator   | Esta función de devolución de llamada se usa si la estructura de datos de comportamientos es un diccionario.       |
| BehaviorEnumValidator  | Esta función de devolución de llamada se usa si la estructura de datos de comportamientos es una enumeración.            |

#### <a name="sample-usage-with-behaviorvalidator"></a>Ejemplo de uso con behaviorValidator

```js
var clickPlugin = Microsoft.ApplicationInsights.ClickAnalyticsPlugin;
var clickPluginInstance = new clickPlugin();

// Behavior enum values
var behaviorMap = {
  UNDEFINED: 0, // default, Undefined

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Page Experience [1-19]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  NAVIGATIONBACK: 1, // Advancing to the previous index position within a webpage
  NAVIGATION: 2, // Advancing to a specific index position within a webpage
  NAVIGATIONFORWARD: 3, // Advancing to the next index position within a webpage
  APPLY: 4, // Applying filter(s) or making selections
  REMOVE: 5, // Applying filter(s) or removing selections
  SORT: 6, // Sorting content
  EXPAND: 7, // Expanding content or content container
  REDUCE: 8, // Sorting content
  CONTEXTMENU: 9, // Context Menu
  TAB: 10, // Tab control
  COPY: 11, // Copy the contents of a page
  EXPERIMENTATION: 12, // Used to identify a third party experimentation event
  PRINT: 13, // User printed page
  SHOW: 14, //  Displaying an overlay
  HIDE: 15, //  Hiding an overlay
  MAXIMIZE: 16, //  Maximizing an overlay
  MINIMIZE: 17, // Minimizing an overlay
  BACKBUTTON: 18, //  Clicking the back button

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Scenario Process [20-39]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  STARTPROCESS: 20, // Initiate a web process unique to adopter
  PROCESSCHECKPOINT: 21, // Represents a checkpoint in a web process unique to adopter
  COMPLETEPROCESS: 22, // Page Actions that complete a web process unique to adopter
  SCENARIOCANCEL: 23, // Actions resulting from cancelling a process/scenario

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Download [40-59]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  DOWNLOADCOMMIT: 40, // Initiating an unmeasurable off-network download
  DOWNLOAD: 41, // Initiating a download

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Search [60-79]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SEARCHAUTOCOMPLETE: 60, // Auto-completing a search query during user input
  SEARCH: 61, // Submitting a search query
  SEARCHINITIATE: 62, // Initiating a search query
  TEXTBOXINPUT: 63, // Typing or entering text in the text box

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Commerce [80-99]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VIEWCART: 82, // Viewing the cart
  ADDWISHLIST: 83, // Adding a physical or digital good or services to a wishlist
  FINDSTORE: 84, // Finding a physical store
  CHECKOUT: 85, // Before you fill in credit card info
  REMOVEFROMCART: 86, // Remove an item from the cart
  PURCHASECOMPLETE: 87, // Used to track the pageView event that happens when the CongratsPage or Thank You page loads after a successful purchase
  VIEWCHECKOUTPAGE: 88, // View the checkout page
  VIEWCARTPAGE: 89, // View the cart page
  VIEWPDP: 90, // View a PDP
  UPDATEITEMQUANTITY: 91, // Update an item's quantity
  INTENTTOBUY: 92, // User has the intent to buy an item
  PUSHTOINSTALL: 93, // User has selected the push to install option

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Authentication [100-119]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SIGNIN: 100, // User sign-in
  SIGNOUT: 101, // User sign-out

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Social [120-139]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SOCIALSHARE: 120, // "Sharing" content for a specific social channel
  SOCIALLIKE: 121, // "Liking" content for a specific social channel
  SOCIALREPLY: 122, // "Replying" content for a specific social channel
  CALL: 123, // Click on a "call" link
  EMAIL: 124, // Click on an "email" link
  COMMUNITY: 125, // Click on a "community" link

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Feedback [140-159]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VOTE: 140, // Rating content or voting for content
  SURVEYCHECKPOINT: 145, // reaching the survey page/form

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Registration, Contact [160-179]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  REGISTRATIONINITIATE: 161, // Initiating a registration process
  REGISTRATIONCOMPLETE: 162, // Completing a registration process
  CANCELSUBSCRIPTION: 163, // Canceling a subscription
  RENEWSUBSCRIPTION: 164, // Renewing a subscription
  CHANGESUBSCRIPTION: 165, // Changing a subscription
  REGISTRATIONCHECKPOINT: 166, // Reaching the registration page/form

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Chat [180-199]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  CHATINITIATE: 180, // Initiating a chat experience
  CHATEND: 181, // Ending a chat experience

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Trial [200-209]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  TRIALSIGNUP: 200, // Signing-up for a trial
  TRIALINITIATE: 201, // Initiating a trial

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Signup [210-219]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  SIGNUP: 210, // Signing-up for a notification or service
  FREESIGNUP: 211, // Signing-up for a free service

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Referals [220-229]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  PARTNERREFERRAL: 220, // Navigating to a partner's web property

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Intents [230-239]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  LEARNLOWFUNNEL: 230, // Engaging in learning behavior on a commerce page (ex. "Learn more click")
  LEARNHIGHFUNNEL: 231, // Engaging in learning behavior on a non-commerce page (ex. "Learn more click")
  SHOPPINGINTENT: 232, // Shopping behavior prior to landing on a commerce page

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  // Video [240-259]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  VIDEOSTART: 240, // Initiating a video
  VIDEOPAUSE: 241, // Pausing a video
  VIDEOCONTINUE: 242, // Pausing or resuming a video.
  VIDEOCHECKPOINT: 243, // Capturing predetermined video percentage complete.
  VIDEOJUMP: 244, // Jumping to a new video location.
  VIDEOCOMPLETE: 245, // Completing a video (or % proxy)
  VIDEOBUFFERING: 246, // Capturing a video buffer event
  VIDEOERROR: 247, // Capturing a video error
  VIDEOMUTE: 248, // Muting a video
  VIDEOUNMUTE: 249, // Unmuting a video
  VIDEOFULLSCREEN: 250, // Making a video full screen
  VIDEOUNFULLSCREEN: 251, // Making a video return from full screen to original size
  VIDEOREPLAY: 252, // Making a video replay
  VIDEOPLAYERLOAD: 253, // Loading the video player
  VIDEOPLAYERCLICK: 254, //  Click on a button within the interactive player
  VIDEOVOLUMECONTROL: 255, //  Click on video volume control
  VIDEOAUDIOTRACKCONTROL: 256, // Click on audio control within a video
  VIDEOCLOSEDCAPTIONCONTROL: 257, //  Click on the closed caption control
  VIDEOCLOSEDCAPTIONSTYLE: 258, //  Click to change closed caption style
  VIDEORESOLUTIONCONTROL: 259, //  Click to change resolution

  ///////////////////////////////////////////////////////////////////////////////////////////////////
  //    Advertisement Engagement [280-299]
  ///////////////////////////////////////////////////////////////////////////////////////////////////
  ADBUFFERING: 283, // Ad is buffering
  ADERROR: 284, // Ad error
  ADSTART: 285, // Ad start
  ADCOMPLETE: 286, // Ad complete
  ADSKIP: 287, // Ad skipped
  ADTIMEOUT: 288, // Ad timed-out
  OTHER: 300 // Other
};

// Application Insights Configuration
var configObj = {
  instrumentationKey: "YOUR INSTRUMENTATION KEY",
  extensions: [clickPluginInstance],
  extensionConfig: {
    [clickPluginInstance.identifier]: {
      behaviorValidator: Microsoft.ApplicationInsights.BehaviorMapValidator(behaviorMap),
      defaultRightClickBhvr: 9
    },
  },
};
var appInsights = new Microsoft.ApplicationInsights.ApplicationInsights({
  config: configObj
});
appInsights.loadAppInsights();
```

## <a name="sample-app"></a>Aplicación de ejemplo

[Sencilla aplicación web con el complemento Click Analytics Auto-collection habilitado](https://go.microsoft.com/fwlink/?linkid=2152871).

## <a name="next-steps"></a>Pasos siguientes

- Use el [análisis de eventos en la experiencia de uso](usage-segmentation.md) para analizar los principales clics y segmentarlos por dimensiones disponibles.
- Busque los datos de clic en el campo de contenido del atributo customDimensions de la tabla CustomEvents de [Log Analytics](../log-query/log-analytics-tutorial.md#write-a-query).
- Compile un [libro](../platform/workbooks-overview.md) para crear visualizaciones personalizadas de los datos de clic.
