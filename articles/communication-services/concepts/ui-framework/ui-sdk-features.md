---
title: Funcionalidades del marco de trabajo de la interfaz de usuario de Azure Communication Services
titleSuffix: An Azure Communication Services conceptual document
description: Información sobre las funcionalidades del marco de trabajo de la interfaz de usuario
author: ddematheu2
ms.author: dademath
ms.date: 11/16/2020
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 51caf1e73cdeda5862e033a05878f35ddb6ff8dc
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99539752"
---
# <a name="ui-framework-capabilities"></a>Funcionalidades del marco de la interfaz de usuario

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

El marco de trabajo de la interfaz de usuario de Azure Communication Services permite crear experiencias de comunicación mediante un conjunto de componentes reutilizables. Estos componentes tienen dos versiones: Los **componentes base** son los bloques de creación más básicos de la experiencia de la interfaz de usuario, mientras que las combinaciones de estos componentes base se denominan componentes **compuestos**.

## <a name="ui-framework-composite-components"></a>Componentes compuestos del marco de trabajo de la interfaz de usuario

| Compuesto               | Descripción                                               | Web   | Android | iOS   |
|-------------------------|-----------------------------------------------------------|-------|---------|-------|
| Combinación de llamadas grupales | Experiencia de llamada saliente ligera de voz y vídeo ligera para Azure Communication Services mediante recursos de diseño de la interfaz de usuario Fluent. Admite la llamada grupal mediante el identificador de grupo de Azure Communication Services. Esta combinación permite las llamadas entre dos personas al hacer referencia a una identidad de Azure Communication Services o un número de teléfono para la RTC con un número de teléfono que se ha adquirido mediante Azure.                                    | React |  |  |
| Combinación de chat grupal    | Experiencia de chat ligera para Azure Communication Services mediante recursos de diseño de la interfaz de usuario Fluent. Esta experiencia se centra en la entrega de un cliente de chat sencillo que se puede conectar a los subprocesos Azure Communication Services. Permite a los usuarios enviar mensaje y ver los mensajes recibidos con indicadores de escritura y confirmaciones de lectura. Se permiten chats grupales entre dos personas o grupales. Admite un solo subproceso de chat.                         | React |  |  |

## <a name="ui-framework-base-components"></a>Componentes base del marco de trabajo de la interfaz de usuario

| Componente             | Descripción                                                                                                                                                                                                                                                                        | Web   | Android | iOS |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------|---------|-----|
| Proveedor de llamadas    | Componente de inicialización principal para llamar. Componente necesario para inicializar a continuación otros componentes basados en este. Controla la lógica principal para inicializar el cliente que realiza la llamada mediante los tokens de acceso de Azure Communication Services. Admite la combinación grupal. | React | N/D     | N/D |
| Controles multimedia   | Permite a los usuarios administrar la llamada actual: alternar el silenciador, activar/desactivar el vídeo y finalizar la llamada.                                                                                                                                                              | React | N/D     | N/D |
| Galería multimedia   | Muestra todos los participantes de la llamada en una sola galería. La galería admite participantes estáticos y habilitados para vídeo. Para los participantes habilitados para vídeo, el vídeo se representa.                                                                                                                | React | N/D     | N/D |
| Configuración del micrófono | Elija el micrófono que se usará para las llamadas. Este control se puede usar antes y durante una llamada para seleccionar el micrófono.                                                                                                                                               | React | N/D     | N/D |
| Configuración de la cámara     | Seleccione la cámara que se va a usar para las llamadas de vídeo. Este control se puede usar antes y durante una llamada a para seleccionar el dispositivo de vídeo.                                                                                                                                             | React | N/D     | N/D |
| Configuración del dispositivo     | Combina la configuración de micrófono y de cámara en un único componente.                                                                                                 | React | N/D     | N/D |
| Proveedor de chat       | Componente de inicialización principal para el chat. Componente necesario para inicializar a continuación otros componentes basados en este. Controla la lógica principal para inicializar el cliente de chat con un token de acceso de Azure Communication Services y el subproceso al que se unirá.                                     | React | N/D     | N/D |
| Cuadro de envío          | Componente de entrada que permite a los usuarios enviar mensajes al subproceso de chat. La entrada admite texto, hipervínculos, emojis y otros caracteres Unicode, incluidos otros alfabetos.                                                                                                                         | React | N/D     | N/D |
| Subproceso de chat           | Componente de subproceso que muestra al usuario los mensajes recibidos y enviados con la información del remitente. El subproceso admite indicadores de escritura y confirmaciones de lectura. Puede desplazarse por estos subprocesos para revisar el historial de chat.
| Lista de participantes      | Muestra todos los participantes del subproceso de llamada o chat en forma de lista.  | React | N/D     | N/D |

## <a name="ui-framework-capabilities"></a>Funcionalidades del marco de la interfaz de usuario

| Característica                                                             | Combinación de llamadas grupales | Combinación de chat grupal | Componentes base |
|---------------------------------------------------------------------|-------------------------|----------------------|-----------------|
| Unirse a una reunión de Teams                                                  |                         |                      |           
| Unirse a un evento en directo de Teams                                               |                         |                      | 
| Iniciar llamada de VoIP a un usuario de Teams                                       |                         |                      | 
| Unirse a un chat de reunión de Teams                                           |                         |                      |            
| Unirse a la llamada de Azure Communication Services con el identificador de grupo                | ✔                      |                      | ✔
| Iniciar una llamada de VoIP a uno o varios usuarios de Azure Communication Services |                         |                      |           
| Unirse a un subproceso de chat de Azure Communication Services                    |                         | ✔                   | ✔
| Desactivar/activar el audio de la llamada                                                    | ✔                       |                      | ✔
| Vídeo activado/desactivado en la llamada                                                | ✔                       |                      | ✔
| Uso compartido de la pantalla                                                      | ✔                       |                      | ✔
| Galería de participantes                                                 | ✔                       |                      | ✔
| Administración de micrófonos                                               | ✔                       |                      | ✔
| Administración de cámaras                                                   | ✔                       |                      | ✔
| Sala de llamadas                                                          |                         |                      | ✔
| Enviar mensaje de chat                                                   |                         | ✔                   |            
| Recibir mensaje de chat                                                |                         | ✔                   | ✔
| Indicadores de escritura                                                   |                         | ✔                   | ✔
| Confirmación de lectura                                                        |                         | ✔                   | ✔
| Lista de participantes                                                    |                         |                      | ✔


## <a name="customization-support"></a>Asistencia para la personalización

| Tipo de componente            | Temas     | Diseño                                                              | Modelos de datos |
|---------------------------|------------|---------------------------------------------------------------------|-------------|
| Componente compuesto       |     N/D    | N/D                                                                 |     N/D     |
| Componente base            |     N/D    | El diseño de los componentes puede modificarse con estilos externos         |     N/D     |


## <a name="platform-support"></a>Compatibilidad con plataformas

| SDK    | Windows            | macOS                | Ubuntu   | Linux    | Android  | iOS        |
|--------|--------------------|----------------------|----------|----------|----------|------------|
| SDK de IU | Chrome\*, nuevo Edge | Chrome\*, Safari\*\* | Chrome\* | Chrome\* | Chrome\* | Safari\*\* |

\*Tenga en cuenta que se admite la versión más reciente de Chrome además de las dos versiones anteriores.

\*\*Tenga en cuenta que se admiten las versiones de Safari 13.1 y posteriores. Todavía no se admite el vídeo de salida para Safari macOS, pero es compatible con iOS. El uso compartido de pantalla saliente solo se admite en iOS de escritorio.
