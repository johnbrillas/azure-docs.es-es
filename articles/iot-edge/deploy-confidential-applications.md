---
title: Aplicaciones confidenciales como módulos de Azure IoT Edge
description: Uso de la API y el SDK de Open Enclave para escribir aplicaciones confidenciales e implementarlas como módulos de IoT Edge para la computación confidencial
author: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/27/2021
ms.author: kgremban
ms.openlocfilehash: f9dff1b4c6b2489edd3cd685e3546618961d9757
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "103487724"
---
# <a name="confidential-computing-at-the-edge"></a>Computación confidencial en el perímetro

[!INCLUDE [iot-edge-version-all-supported](../../includes/iot-edge-version-all-supported.md)]

Azure IoT Edge admite aplicaciones confidenciales que se ejecutan dentro de enclaves seguros en el dispositivo. El cifrado proporciona seguridad para los datos mientras están en tránsito o en reposo, pero los enclaves proporcionan seguridad para los datos y las cargas de trabajo mientras están en uso. IoT Edge admite Open Enclave como estándar para el desarrollo de aplicaciones confidenciales.

La seguridad siempre ha sido un aspecto importante del Internet de las cosas (IoT) porque los dispositivos IoT rara vez se encuentran protegidos dentro de una instalación privada. Esta exposición de los dispositivos los pone en riesgo de sufrir alteraciones y falsificaciones, porque son físicamente accesibles para actores malintencionados. Los dispositivos IoT Edge necesitan incluso más confianza e integridad, porque permiten que las cargas de trabajo confidenciales se ejecuten en el perímetro. A diferencia de lo que ocurre con sensores y accionadores comunes, estos dispositivos de inteligencia perimetral exponen potencialmente cargas de trabajo confidenciales que anteriormente se ejecutaban solo dentro de entornos protegidos locales o de nube.

El [administrador de seguridad de IoT Edge](iot-edge-security-manager.md) aborda una parte del desafío de la computación confidencial. El administrador de seguridad usa un módulo de seguridad de hardware (HSM) para proteger las cargas de trabajo de identidad y los procesos en curso de un dispositivo IoT Edge.

Otro aspecto de la computación confidencial es proteger los datos que están en uso en el perímetro. Un *entorno de ejecución de confianza* (TEE) es un entorno aislado seguro en un procesador y a veces se le llama *enclave*. Una *aplicación confidencial* es una aplicación que se ejecuta en un enclave. Debido a la naturaleza de los enclaves, las aplicaciones confidenciales están protegidas de otras aplicaciones que se ejecutan en el procesador principal o en el entorno de ejecución de confianza.

## <a name="confidential-applications-on-iot-edge"></a>Aplicaciones confidenciales en IoT Edge

Las aplicaciones confidenciales están cifradas en tránsito y en reposo y solo se descifran para su ejecución dentro de un entorno de ejecución de confianza. Este estándar es válido para las aplicaciones confidenciales implementadas como módulos de IoT Edge.

El desarrollador crea la aplicación confidencial y la empaqueta como un módulo de IoT Edge. La aplicación se cifra antes de insertarse en el registro de contenedor. La aplicación permanece cifrada durante todo el proceso de implementación de IoT Edge hasta que el módulo se inicie en el dispositivo IoT Edge. Una vez que la aplicación confidencial se encuentra dentro del entorno de ejecución de confianza, se descifra y puede empezar a ejecutarse.

![Diagrama: las aplicaciones confidenciales se cifran dentro de los módulos de IoT Edge hasta que se implementan en el enclave seguro.](./media/deploy-confidential-applications/confidential-applications-encrypted.png)

Las aplicaciones confidenciales en IoT Edge son una extensión lógica de la [computación confidencial de Azure](../confidential-computing/overview.md). Las cargas de trabajo que se ejecutan dentro de enclaves seguros en la nube también se pueden implementar para ejecutarse dentro de enclaves seguros en el perímetro.

## <a name="open-enclave"></a>Open Enclave

El [SDK de Open Enclave](https://openenclave.io/sdk/) es un proyecto de código abierto que ayuda a los desarrolladores a crear aplicaciones confidenciales para varias plataformas y entornos. El SDK de Open Enclave funciona dentro del entorno de ejecución de confianza de un dispositivo, mientras que la API de Open Enclave sirve como interfaz entre el entorno TEE y el entorno de procesamiento no TEE.

Open Enclave admite varias plataformas de hardware. Actualmente, la compatibilidad de IoT Edge con los enclaves requiere el sistema operativo Open Portable TEE (SO OP-TEE). Para más información, consulte el artículo sobre el [SDK de Open Enclave para OP-TEE OS](https://github.com/openenclave/openenclave/blob/master/docs/GettingStartedDocs/OP-TEE/Introduction.md).

El repositorio de Open Enclave también incluye ejemplos que ayudan a los desarrolladores a empezar a trabajar. Para más información, elija uno de los artículos introductorios:

* [Compilación de ejemplos del SDK de Open Enclave en Linux](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesLinux.md)
* [Compilación de ejemplos del SDK de Open Enclave en Windows](https://github.com/openenclave/openenclave/blob/master/samples/BuildSamplesWindows.md)

## <a name="hardware"></a>Hardware

Actualmente, [TrustBox de Scalys](https://scalys.com/trustbox-industrial/) es el único dispositivo compatible con acuerdos de servicio del fabricante para implementar aplicaciones confidenciales como módulos de IoT Edge. TrustBox se basa en los dispositivos TrustBox Edge y TrustBox EdgeXL en los que están cargados previamente el SDK de Open Enclave y Azure IoT Edge.

Para más información, consulte el artículo de [introducción a Open Enclave para TrustBox de Scalys](https://aka.ms/scalys-trustbox-edge-get-started).

## <a name="develop-and-deploy"></a>Desarrollo e implementación

Cuando esté listo para desarrollar e implementar una aplicación confidencial, la extensión [Microsoft Open Enclave](https://marketplace.visualstudio.com/items?itemName=ms-iot.msiot-vscode-openenclave) para Visual Studio Code puede ser útil. Puede usar Linux o Windows como máquina de desarrollo para desarrollar módulos para TrustBox.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo empezar a desarrollar aplicaciones confidenciales como módulos de IoT Edge con la [extensión de Open Enclave para Visual Studio Code](https://github.com/openenclave/openenclave/tree/master/devex/vscode-extension).
