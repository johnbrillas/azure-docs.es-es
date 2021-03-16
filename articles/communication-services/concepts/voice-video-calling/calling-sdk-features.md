---
title: Información general de la biblioteca cliente de llamadas de Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Proporciona información general sobre la biblioteca cliente de llamadas.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/04/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: fe1e08434a9137fced3dc8aa061affc2d6580b67
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102489283"
---
# <a name="calling-client-library-overview"></a>Información general de la biblioteca cliente de llamadas

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]


Hay dos familias independientes de bibliotecas cliente de llamadas, la de *clientes* y la de *servicios*. Las bibliotecas cliente disponibles actualmente están pensadas para experiencias de usuario final: sitios web y aplicaciones nativas.

Las bibliotecas cliente de servicio aún no están disponibles. Estas proporcionan acceso a los planos de datos de vídeo y de voz sin procesar, que son adecuados para la integración con bots y otros servicios.

## <a name="calling-client-library-capabilities"></a>Funcionalidades de la biblioteca cliente de llamadas

En la lista siguiente se presenta el conjunto de características que están disponibles actualmente en las bibliotecas cliente de llamadas de Azure Communication Services.

| Grupo de características | Capacidad                                                                                                          | JS  | Java (Android) | Objective-C (iOS)
| ----------------- | ------------------------------------------------------------------------------------------------------------------- | ---  | -------------- | -------------
| Funcionalidades principales | Realizar una llamada uno a uno entre dos usuarios                                                                           | ✔️   | ✔️            | ✔️
|                   | Realizar una llamada de grupo con más de dos usuarios (hasta 350)                                                       | ✔️   | ✔️            | ✔️
|                   | Promocionar una llamada uno a uno con dos usuarios a una llamada de grupo con más de dos usuarios                                 | ✔️   | ✔️            | ✔️
|                   | Unirse a una llamada de grupo después de que se haya iniciado                                                                              | ✔️   | ✔️            | ✔️
|                   | Invitar a otro participante de VoIP a unirse a una llamada de grupo en curso                                                       | ✔️   | ✔️            | ✔️
|  Control durante la llamada | Activar o desactivar el vídeo                                                                                              | ✔️   | ✔️            | ✔️ 
|                   | Desactivar/activar audio del micrófono                                                                                                     | ✔️   | ✔️            | ✔️         
|                   | Cambiar entre las cámaras                                                                                              | ✔️   | ✔️            | ✔️           
|                   | Retención/reanudación local                                                                                                  | ✔️   | ✔️            | ✔️           
|                   | Altavoz activo                                                                                                      | ✔️   | ✔️            | ✔️           
|                   | Elegir el altavoz para llamadas                                                                                            | ✔️   | ✔️            | ✔️           
|                   | Elegir el micrófono para llamadas                                                                                         | ✔️   | ✔️            | ✔️           
|                   | Mostrar el estado de un participante<br/>*Inactivo, elementos multimedia iniciales, conectando, conectado, en espera, en la sala de espera, desconectado*         | ✔️   | ✔️            | ✔️           
|                   | Mostrar el estado de una llamada<br/>*Elementos multimedia iniciales, entrante, conectando, llamando, conectada, en espera, desconectando, desconectada* | ✔️   | ✔️            | ✔️           
|                   | Mostrar si un participante está silenciado                                                                                      | ✔️   | ✔️            | ✔️           
|                   | Mostrar el motivo por el que un participante abandonó una llamada                                                                       | ✔️   | ✔️            | ✔️     
| Uso compartido de la pantalla    | Compartir la pantalla completa desde la aplicación                                                                 | ✔️   | ❌            | ❌           
|                   | Compartir una aplicación específica (desde la lista de aplicaciones en ejecución)                                                | ✔️   | ❌            | ❌           
|                   | Compartir una pestaña del explorador web desde la lista de pestañas abiertas                                                                  | ✔️   | ❌            | ❌           
|                   | El participante puede ver el uso compartido de pantalla remota                                                                            | ✔️   | ✔️            | ✔️         
| Lista            | Enumerar participantes                                                                                                   | ✔️   | ✔️            | ✔️           
|                   | Quitar un participante                                                                                                | ✔️   | ✔️            | ✔️         
| RTC              | Realizar una llamada uno a uno con un participante de RTC                                                                     | ✔️   | ✔️            | ✔️   
|                   | Realizar una llamada de grupo con participantes de RTC                                                                           | ✔️   | ✔️            | ✔️
|                   | Promocionar una llamada uno a uno con un participante de RTC a una llamada de grupo                                                 | ✔️   | ✔️            | ✔️
|                   | Llamada saliente de una llamada de grupo como participante de RTC                                                                    | ✔️   | ✔️            | ✔️   
| General           | Probar el micrófono, el altavoz y la cámara con un servicio de prueba de audio (disponible llamando a 8:echo123).                   | ✔️   | ✔️            | ✔️ 
| Administración del dispositivo | Solicitar permiso para usar audio y/o vídeo                                                                       | ✔️   | ✔️            | ✔️
|                   | Obtener la lista de cámaras                                                                                                     | ✔️   | ✔️            | ✔️ 
|                   | Establecer cámara                                                                                                          | ✔️   | ✔️            | ✔️
|                   | Obtener la cámara seleccionada                                                                                                 | ✔️   | ✔️            | ✔️
|                   | Obtener la lista de micrófonos                                                                                                 | ✔️   | ✔️            | ✔️
|                   | Establecer micrófono                                                                                                      | ✔️   | ✔️            | ✔️
|                   | Obtener el micrófono seleccionado                                                                                             | ✔️   | ✔️            | ✔️
|                   | Obtener la lista de altavoces                                                                                                   | ✔️   | ✔️            | ✔️
|                   | Establecer el altavoz                                                                                                         | ✔️   | ✔️            | ✔️
|                   | Obtener el altavoz seleccionado                                                                                                | ✔️   | ✔️            | ✔️
| Representación de vídeo   | Representar un único vídeo en muchos lugares (cámara local o flujo remoto)                                                  | ✔️   | ✔️            | ✔️
|                   | Establecer y actualizar el modo de escalado                                                                                           | ✔️   | ✔️            | ✔️ 
|                   | Representar secuencias de vídeo remoto                                                                                          | ✔️   | ✔️            | ✔️



## <a name="javascript-calling-client-library-support-by-os-and-browser"></a>Compatibilidad de la biblioteca cliente de llamadas de JavaScript por sistema operativo y explorador

En la tabla siguiente se representa el conjunto de exploradores compatibles que están disponibles actualmente. Se admiten las tres versiones más recientes del explorador, a menos que se indique lo contrario.

|                                  | Chrome | Safari*  | Edge (Chromium) | 
| -------------------------------- | -------| ------  | --------------  |
| Android                          |  ✔️    | ❌     | ❌             |
| iOS                              |  ❌    | ✔️**** | ❌             |
| macOS***                         |  ✔️    | ✔️**   | ❌             |
| Windows***                       |  ✔️    | ❌     | ✔️             |
| Ubuntu/Linux                     |  ✔️    | ❌     | ❌             |

*Se admiten las versiones de Safari 13.1 y posteriores. 

**Son necesarios Safari 14+ y macOS 11+ para la compatibilidad con vídeo saliente. 

***El uso compartido de la pantalla de salida solo se admite en las plataformas de escritorio (Windows, macOS y Linux), independientemente de la versión del explorador, y no se admite en ninguna plataforma móvil (Android, iOS, iPad y tabletas).

****Una aplicación iOS en Safari no puede enumerar ni seleccionar dispositivos de micrófono y altavoz (por ejemplo, Bluetooth); se trata de una limitación del sistema operativo y siempre hay un único dispositivo.


## <a name="calling-client---browser-security-model"></a>Cliente que llama: modelo de seguridad del explorador

### <a name="user-webrtc-over-https"></a>WebRTC de usuario sobre HTTPS

Las API de WebRTC, como `getUserMedia`, requieren que la aplicación que llama a estas API se atienda a través de HTTPS.

Para el desarrollo local, puede usar `http://localhost`.

### <a name="embed-the-communication-services-calling-sdk-in-an-iframe"></a>Inserción de Calling SDK de Communication Services en un iframe

Una nueva [directiva de permisos (también denominado directiva de características)](https://www.w3.org/TR/permissions-policy-1/#iframe-allow-attribute) se está adoptando en diversos exploradores. Esta directiva afecta a los escenarios de llamada al controlar cómo las aplicaciones pueden acceder a la cámara y el micrófono de un dispositivo mediante un elemento iframe entre orígenes.

Si quiere usar un iframe para hospedar parte de la aplicación desde un dominio diferente, debe agregar el atributo `allow` con el valor correcto a su iframe.

Por ejemplo, este iframe permite el acceso a la cámara y el micrófono:

```html
<iframe allow="camera *; microphone *">
```

## <a name="calling-client-library-streaming-support"></a>Compatibilidad con el streaming de la biblioteca cliente de llamada
La biblioteca cliente de llamada de Communication Services admite las siguientes configuraciones de streaming:

|           |Web | Android/iOS|
|-----------|----|------------|
|**Número de secuencias salientes que se pueden enviar simultáneamente** |1 vídeo + 1 uso compartido de pantalla | 1 vídeo + 1 uso compartido de pantalla|
|**número de secuencias entrantes que se pueden representar simultáneamente** |1 vídeo + 1 uso compartido de pantalla| 6 vídeo + 1 uso compartido de pantalla |


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción a las llamadas](../../quickstarts/voice-video-calling/getting-started-with-calling.md)

Para más información, consulte los siguientes artículos.
- Familiarización con los [flujos de llamada](../call-flows.md) generales
- Más información sobre los [tipos de llamada](../voice-video-calling/about-call-types.md)
- [Planeación de una solución RTC](../telephony-sms/plan-solution.md)
