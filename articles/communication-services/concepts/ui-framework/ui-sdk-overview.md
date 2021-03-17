---
title: Introducción al marco de trabajo de la interfaz de usuario de Azure Communication Services
titleSuffix: An Azure Communication Services conceptual document
description: Más información sobre el marco de trabajo de la interfaz de usuario de Azure Communication Services
author: ddematheu2
ms.author: dademath
ms.date: 03/10/2021
ms.topic: quickstart
ms.service: azure-communication-services
ms.openlocfilehash: 47a32815ded5809edfde856a38c69ec7c6fd6fdf
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103493363"
---
# <a name="azure-communication-services-ui-framework"></a>Marco de trabajo de la interfaz de usuario de Azure Communication Services

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

:::image type="content" source="../media/ui-framework/pre-and-custom-composite.png" alt-text="Comparación entre compuestos pregenerados y personalizados":::

El marco de trabajo de la interfaz de usuario de Azure Communication Services facilita la creación de experiencias modernas de usuarios de comunicaciones. Proporciona una biblioteca de componentes de interfaz de usuario preparados para la producción que puede colocar en sus aplicaciones:

- **Componentes compuestos**: estos componentes son soluciones llave en mano que implementan escenarios de comunicación comunes. Puede agregar rápidamente experiencias de llamadas de vídeo o de chat a sus aplicaciones. Los compuestos son componentes de código abierto compilados mediante componentes base.
- **Componentes base**: estos componentes son bloques de creación de código abierto que permiten crear una experiencia de comunicaciones personalizada. Los componentes se ofrecen para las funcionalidades de llamadas y de chat que se pueden combinar para generar experiencias. 

Todas estas bibliotecas cliente de interfaz de usuario utilizan los recursos y el [lenguaje de diseño de Fluent de Microsoft](https://developer.microsoft.com/fluentui/). La interfaz de usuario de Fluent proporciona un nivel fundamental para el marco de trabajo de la interfaz de usuario que se ha probado exhaustivamente en los productos de Microsoft.

## <a name="differentiating-components-and-composites"></a>**Diferenciación entre componentes y compuestos**

Los **componentes base** se crean a partir de las bibliotecas cliente principales de Azure Communication Services e implementan acciones básicas, como la inicialización de las bibliotecas cliente principales, la representación de vídeo y la aportación de controles de usuario para silenciar, activar o desactivar el vídeo, etc. Puede usar estos **componentes base** para crear sus propias experiencias de diseño personalizadas mediante componentes de comunicación pregenerados y preparados para la producción.

:::image type="content" source="../media/ui-framework/component-overview.png" alt-text="Introducción al componente para el marco de trabajo de la interfaz de usuario":::

Los **componentes compuestos** combinan varios **componentes base** para crear unas experiencias de comunicación más completas. Estos componentes de nivel superior se pueden integrar fácilmente en una aplicación existente para colocar una experiencia de comunicación totalmente desarrollada sin tener que compilarla desde cero. Los desarrolladores pueden concentrarse en la creación de la experiencia circundante y el flujo que deseen en sus aplicaciones y dejar la complejidad de las comunicaciones para los componentes compuestos.

:::image type="content" source="../media/ui-framework/composite-overview.png" alt-text="Introducción a los compuestos para el marco de trabajo de la interfaz de usuario":::

## <a name="what-ui-framework-is-best-for-my-project"></a>¿Qué marco de trabajo de la interfaz de usuario es el mejor para mi proyecto?

Comprender estos requisitos le ayudará a elegir la biblioteca cliente adecuada:

- **¿Cuánta personalización desea?** Las bibliotecas cliente principales de Azure Communication no tienen experiencia de usuario y están diseñadas para que pueda compilar cualquier experiencia de usuario que quiera. Los componentes del marco de trabajo de la interfaz de usuario proporcionan recursos de interfaz de usuario a costa de una personalización reducida.
- **¿Necesita funciones para reuniones?** El sistema de reuniones tiene varias funcionalidades únicas que no están actualmente disponibles en las bibliotecas cliente principales de Azure Communication Services, como el fondo desenfocado y la mano levantada.
- **¿A qué plataformas quiere dirigirse?** Cada plataforma tiene diferentes funcionalidades.

Aquí encontrará información detallada sobre la disponibilidad de características en los distintos [SDK de la interfaz de usuario](ui-sdk-features.md), pero las ventajas principales se resumen a continuación.

|Biblioteca cliente / SDK|Complejidad de la implementación|    Capacidad de personalización|  Llamar| Chat| [Interoperabilidad de equipos](./../teams-interop.md)
|---|---|---|---|---|---|---|
|Componentes compuestos|Bajo|Bajo|✔|✔|✕
|Componentes básicos|Media|Media|✔|✔|✕
|Bibliotecas cliente principales|Alto|Alto|✔|✔ |✔

## <a name="cost"></a>Costos

El uso de marcos de trabajo de la interfaz de usuario de Azure no tiene ningún costo o medición de Azure adicional. Solo paga por el uso del servicio subyacente, con los mismos medidores de llamadas, chat y RTC.

## <a name="supported-use-cases"></a>Casos de uso admitidos

Llamadas:

- Unirse a la llamada de Azure Communication Services con el identificador de grupo

Chat:

- Unirse al chat de Azure Communication Services con el identificador de subproceso

## <a name="supported-identities"></a>Identidades admitidas:

Se necesita una identidad de Azure Communication Services para inicializar el marco de trabajo de la interfaz de usuario y autenticarse en el servicio. Para más información, consulte [Autenticación](../authentication.md) y [Tokens de acceso](../../quickstarts/access-tokens.md).


## <a name="recommended-architecture"></a>Arquitectura recomendada 

:::image type="content" source="../media/ui-framework/framework-architecture.png" alt-text="Arquitectura recomendada del marco de trabajo de la interfaz de usuario con la arquitectura cliente-servidor":::

Los componentes compuestos y base se inicializan con un token de acceso de Azure Communication Services. Los tokens de acceso se deben obtener de Azure Communication Services mediante un servicio de confianza que administre. Consulte [Quickstart: Creación y administración de tokens de acceso](../../quickstarts/access-tokens.md) y el artículo sobre el [tutorial de un servicio de confianza](../../tutorials/trusted-service-tutorial.md) para más información.

Estas bibliotecas cliente también requieren el contexto de la llamada o del chat al que se unirán. De forma similar a los tokens de acceso de usuario, este contexto se debe difundir a los clientes mediante su propio servicio de confianza. La siguiente lista resume las funciones de inicialización y administración de recursos que necesita para la puesta en práctica.

| Responsabilidades de Contoso                                 | Responsabilidades del marco de trabajo de la interfaz de usuario                         |
|----------------------------------------------------------|-----------------------------------------------------------------|
| Proporcionar un token de acceso de Azure                    | Pasar el token de acceso dado para inicializar los componentes        |
| Proporcionar la función de actualización                                 | Actualizar el token de acceso mediante la función proporcionada por el desarrollador          |
| Recuperar o pasar información de incorporación a llamada o chat          | Pasar la información de llamada y chat para inicializar los componentes |
| Recuperar o pasar información de usuario para cualquier modelo de datos personalizado | Pasar el modelo de datos personalizado a los componentes que se van a representar          |
