---
title: Información general del modelo de programación de Service Fabric
description: 'Service Fabric ofrece dos marcos para la creación de servicios: el marco de actores y el marco de servicios. Ofrecen distintas ventajas e inconvenientes en simplicidad y control.'
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: 523c9e0064d8b78698f1a0dd3544bda58436800c
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96575727"
---
# <a name="service-fabric-programming-model-overview"></a>Información general del modelo de programación de Service Fabric

Service Fabric ofrece varias maneras de escribir y administrar los servicios. Los servicios pueden optar por usar las API de Service Fabric para sacar el máximo provecho de los marcos de aplicaciones y las características de la plataforma. Los servicios también pueden ser cualquier programa ejecutable compilado escrito en cualquier lenguaje o código que se ejecute en un contenedor hospedado en un clúster de Service Fabric.

## <a name="guest-executables"></a>Ejecutables de invitado

Un [ejecutable de invitado](service-fabric-guest-executables-introduction.md) es un ejecutable arbitrario y existente (escrito en cualquier lenguaje) que se pueda ejecutar como un servicio en la aplicación. Los ejecutables de invitado no llaman directamente a las API del SDK de Service Fabric. Sin embargo, todavía se beneficiarán de las características que ofrece la plataforma, como la detección de servicio y la creación de informes de carga y de estado personalizada mediante la llamada a API de REST expuestas por Service Fabric. También tienen soporte técnico completo de ciclo de vida de aplicación.

Introducción a los archivos ejecutables de invitado mediante la implementación de la primera [aplicación ejecutable de invitado](service-fabric-deploy-existing-app.md).

## <a name="containers"></a>Contenedores

De forma predeterminada, Service Fabric implementa y activa estos servicios como procesos. Service Fabric puede implementar también servicios en [contenedores](service-fabric-containers-overview.md). Service Fabric admite la implementación de contenedores de Linux y contenedores de Windows en Windows Server 2016 o posterior. Las imágenes de contenedor se pueden extraer de cualquier repositorio de contenedor e implementarse en la máquina. Puede implementar las aplicaciones existentes como ejecutables de invitado, servicios de confianza con estado o sin estado de Service Fabric o Reliable Actors en contenedores, y puede mezclar los servicios en procesos y servicios en contenedores en la misma aplicación.

[más información sobre la inclusión en contenedores de los servicios de Windows o Linux](./service-fabric-get-started-containers.md)

## <a name="reliable-services"></a>Reliable Services

Reliable Services es un marco de trabajo ligero para escribir servicios que se integran con la plataforma Service Fabric y que se benefician de todo el conjunto de características de la plataforma. Reliable Services proporciona un conjunto mínimo de API que permiten que el tiempo de ejecución de Service Fabric administre el ciclo de vida de los servicios y que permiten a los servicios interactuar con el tiempo de ejecución. El marco de trabajo de la aplicación es mínimo, lo que proporciona un control total sobre las opciones de diseño e implementación, y puede usarse para hospedar cualquier otro marco de trabajo de la aplicación, como ASP.NET Core.

Reliable Services puede no tener estado, de forma similar a la mayoría de las plataformas de servicios, como servidores web, en que cada instancia del servicio se crea de la misma y se conserva el estado en una solución externa, como Azure Table Storage y Azure DB.

Reliable Services también pueden tener estado, exclusivo de Service Fabric, donde se conserva el estado directamente en el propio servicio mediante Reliable Collections. El estado cuenta con una alta disponibilidad mediante la replicación y se distribuye a través de particiones, todo administrado automáticamente por Service Fabric.

[Aprenda más sobre Reliable Services](service-fabric-reliable-services-introduction.md) o comience por [escribir su primer Reliable Service](service-fabric-reliable-services-quick-start.md).

## <a name="aspnet-core"></a>ASP.NET Core

ASP.NET Core es un marco de código abierto multiplataforma para crear aplicaciones conectadas a Internet modernas y basadas en la nube, como aplicaciones web, aplicaciones de IoT y back-ends móviles. Service Fabric se integra con ASP.NET para que pueda escribir aplicaciones ASP.NET Core sin estado y con estado que aproveche las funcionalidades de orquestación avanzada de Reliable Collections y Service Fabric.

[Más información sobre ASP.NET Core en Service Fabric](service-fabric-reliable-services-communication-aspnetcore.md) o comience a [escribir su primera aplicación Service Fabric de ASP.NET Core](service-fabric-tutorial-create-dotnet-app.md).

## <a name="reliable-actors"></a>Reliable Actors

Basado en Reliable Services, el marco de Reliable Actor es un marco de aplicación que implementa el diseño de [Virtual Actor](https://research.microsoft.com/en-us/projects/orleans/), basado en el [modelo de actor](https://en.wikipedia.org/wiki/Actor_model) computacional. El marco de Reliable Actor usa unidades independientes del proceso y el estado con la ejecución de subproceso único denominadas *actores*. El marco de Reliable Actor proporciona comunicaciones integradas para actores y las configuraciones de escalado horizontal y persistencia de estado establecidas previamente.

Dado que Reliable Actors es un marco de aplicación basado en Reliable Services, está totalmente integrado con la plataforma Service Fabric y se beneficia del conjunto completo de características que ofrece la plataforma.

[Aprenda más sobre Reliable Actors](service-fabric-reliable-actors-introduction.md) o comience por [escribir el primer servicio de Reliable Actor](service-fabric-reliable-actors-get-started.md).

[Crear un servicio de front-end mediante ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)

## <a name="next-steps"></a>Pasos siguientes

[Información general sobre Service Fabric y contenedores](service-fabric-containers-overview.md)

[Información general sobre Reliable Services](service-fabric-reliable-services-introduction.md)

[Introducción a Reliable Actors](service-fabric-reliable-actors-introduction.md)

[Service Fabric y ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md)
