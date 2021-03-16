---
title: Inicio rápido de Content Moderator REST API
titleSuffix: Azure Cognitive Services
description: En este inicio rápido aprenderá a usar la biblioteca cliente de Azure Content Moderator REST API. Compile software de filtrado de contenido en la aplicación para cumplir con las regulaciones o para mantener el entorno previsto para los usuarios.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: include
ms.date: 12/08/2020
ms.author: pafarley
ms.openlocfilehash: 859c57fb6caeee730d5ba937bacf5d29a25ca173
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511022"
---
Introducción a Azure Content Moderator REST API. 

Content Moderator es un servicio de inteligencia artificial que le permite controlar el contenido que puede ser ofensivo, arriesgado o no deseable. Use el servicio de moderación de contenido basado en IA para analizar texto, imágenes y vídeos, así como para aplicar marcas de contenido automáticamente. Después, integre su aplicación con la herramienta de revisión, un entorno de moderador en línea para un equipo de revisores humanos. Compile software de filtrado de contenido en la aplicación para cumplir con las regulaciones o para mantener el entorno previsto para los usuarios.

Use Content Moderator REST API para:

* Moderar texto
* Moderar imágenes

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/)
* Una vez que tenga la suscripción de Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator"  title="cree un recurso de Content Moderator"  target="_blank">Creación de un recurso de Content Moderator</a> en Azure Portal para obtener la clave y el punto de conexión. Espere a que se implemente y haga clic en el botón **Ir al recurso**.
    * Necesitará la clave y el punto de conexión del recurso que cree para conectar la aplicación a Content Moderator. En una sección posterior de este mismo inicio rápido pegará la clave y el punto de conexión en el código siguiente.
    * Puede usar el plan de tarifa gratis (`F0`) para probar el servicio y actualizarlo más adelante a un plan de pago para producción.
* [PowerShell versión 6.0, o superior,](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows) o una aplicación de la línea de comandos similar.


## <a name="moderate-text"></a>Moderar texto

Usará un comando como el siguiente para llamar a Content Moderator API para que analice un cuerpo de texto e imprima los resultados en la consola.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/content-moderator/quickstart.sh" ID="textmod":::

Copie el comando en un editor de texto y realice los cambios siguientes:

1. Asigne `Ocp-Apim-Subscription-Key` a la clave de suscripción de Face válida.
1. Cambie la primera parte de la dirección URL de la consulta para que coincida con el punto de conexión que corresponde a la clave de suscripción.
   [!INCLUDE [subdomains-note](../../../../../includes/cognitive-services-custom-subdomains-note.md)]
1. También puede cambiar el cuerpo de la solicitud a cualquier cadena de texto que desee analizar.

Una vez realizados los cambios, abra un símbolo del sistema y escriba el nuevo comando. 

### <a name="examine-the-results"></a>Examen de los resultados

Debería ver los resultados de la moderación del texto mostrados como datos JSON en la ventana de la consola. Por ejemplo:

```json
{
  "OriginalText": "Is this a crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255,\n1 Microsoft Way, Redmond, WA 98052\n",
  "NormalizedText": "Is this a crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, \n1 Microsoft Way, Redmond, WA 98052",
  "AutoCorrectedText": "Is this a crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, \n1 Microsoft Way, Redmond, WA 98052",
  "Misrepresentation": null,
  "PII": {
    "Email": [
      {
        "Detected": "abcdef@abcd.com",
        "SubType": "Regular",
        "Text": "abcdef@abcd.com",
        "Index": 21
      }
    ],
    "IPA": [
      {
        "SubType": "IPV4",
        "Text": "255.255.255.255",
        "Index": 61
      }
    ],
    "Phone": [
      {
        "CountryCode": "US",
        "Text": "6657789887",
        "Index": 45
      }
    ],
    "Address": [
      {
        "Text": "1 Microsoft Way, Redmond, WA 98052",
        "Index": 78
      }
    ]
  },
 "Classification": {
    "Category1": 
    {
      "Score": 0.5
    },
    "Category2": 
    {
      "Score": 0.6
    },
    "Category3": 
    {
      "Score": 0.5
    },
    "ReviewRecommended": true
  },
  "Language": "eng",
  "Terms": [
    {
      "Index": 10,
      "OriginalIndex": 10,
      "ListId": 0,
      "Term": "crap"
    }
  ],
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "TrackingId": "1717c837-cfb5-4fc0-9adc-24859bfd7fac"
}
```

Para más información sobre los atributos de texto que Content Moderator analiza, consulte la guía [Conceptos de moderación de texto](../../text-moderation-api.md).

## <a name="moderate-images"></a>Moderar imágenes

Usará un comando como el siguiente para llamar a Content Moderator API para que modere una imagen remota e imprima los resultados en la consola.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/content-moderator/quickstart.sh" ID="imagemod":::

Copie el comando en un editor de texto y realice los cambios siguientes:

1. Asigne `Ocp-Apim-Subscription-Key` a la clave de suscripción de Face válida.
1. Cambie la primera parte de la dirección URL de la consulta para que coincida con el punto de conexión que corresponde a la clave de suscripción.
1. También puede cambiar la dirección URL de `"Value"` en el cuerpo de la solicitud a cualquier imagen remota que desee moderar.

> [!TIP]
> También puede moderar imágenes locales pasando sus datos de bytes en el cuerpo de la solicitud. Consulte la [documentación de referencia](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) para aprender a hacerlo.

Una vez realizados los cambios, abra un símbolo del sistema y escriba el nuevo comando. 

### <a name="examine-the-results"></a>Examen de los resultados

Debería ver los resultados de la moderación de la imagen mostrados como datos JSON en la ventana de la consola. 

```json
{
  "AdultClassificationScore": x.xxx,
  "IsImageAdultClassified": <Bool>,
  "RacyClassificationScore": x.xxx,
  "IsImageRacyClassified": <Bool>,
  "AdvancedInfo": [],
  "Result": false,
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "TrackingId": "<Request Tracking Id>"
}
```

Para más información sobre los atributos de imagen que Content Moderator analiza, consulte la guía [Conceptos de moderación de imágenes](../../image-moderation-api.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si quiere limpiar y eliminar una suscripción a Cognitive Services, puede eliminar el recurso o grupo de recursos. Al eliminar el grupo de recursos, también se elimina cualquier otro recurso que esté asociado a él.

* [Portal](../../../cognitive-services-apis-create-account.md#clean-up-resources)
* [CLI de Azure](../../../cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a usar Content Moderator REST API para realizar tareas de moderación. A continuación, lea una guía conceptual para aprender más sobre la moderación de imágenes u otro contenido multimedia.

* [Conceptos de moderación de imágenes](../../image-moderation-api.md)
* [Conceptos de moderación de texto](../../text-moderation-api.md)
* [¿Qué es Azure Content Moderator?](../../overview.md)