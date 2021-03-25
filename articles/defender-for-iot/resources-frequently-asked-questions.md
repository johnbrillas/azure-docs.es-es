---
title: Preguntas más frecuentes sobre Defender para IoT
description: Encuentre respuestas a las preguntas más frecuentes sobre las características y servicios de Azure Defender para IoT.
ms.topic: conceptual
ms.date: 03/02/2021
ms.openlocfilehash: 0ce8ded3eea344d72679e0f8b805f45b00279b58
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104778600"
---
# <a name="azure-defender-for-iot-frequently-asked-questions"></a>Preguntas más frecuentes sobre Azure Defender para IoT

En este artículo se proporciona una lista de las preguntas más frecuentes y las respuestas sobre Defender para IoT.

## <a name="what-is-azures-unique-value-proposition-for-iot-security"></a>¿Cuál es la propuesta de valor propia de Azure para la seguridad de IoT?

Defender para IoT permite a las empresas ampliar su vista existente de la seguridad cibernética a toda la solución de IoT. Azure proporciona una vista integral de su solución empresarial, lo que le permite realizar acciones relacionadas con la empresa y tomar decisiones según su posición de seguridad empresarial y los datos recopilados. La seguridad combinada mediante Azure IoT, Azure IoT Edge y Azure Security Center le permite crear la solución que quiere con la seguridad que necesita.

## <a name="our-organization-uses-proprietary-non-standard-industrial-protocols-are-they-supported"></a>Nuestra organización usa protocolos industriales patentados que no son estándar. ¿Se admiten? 

Azure Defender para IoT ofrece compatibilidad completa con protocolos. Además de la compatibilidad con el protocolo incrustado, puede proteger los dispositivos IoT y OT que ejecutan protocolos patentados y personalizados, o protocolos que se desvían de cualquier estándar. Con el SDK de Horizon Open Development Environment (ODE), los desarrolladores pueden crear complementos de disección que descodifican el tráfico basado en protocolos definidos. Los servicios analizan el tráfico para ofrecer supervisión, alertas e informes completos. Use Horizon para:
- Ampliar la visibilidad y control sin necesidad de actualizar a nuevas versiones.
- Proteger la información de su propiedad al desarrollar el protocolo del sitio como complemento externo. 
- Localizar el texto de las alertas, eventos y parámetros del protocolo.

Esta solución única para desarrollar protocolos como complementos no requiere de equipos de desarrolladores dedicados ni lanzamientos de versión para admitir un nuevo protocolo. Los desarrolladores, los socios y los clientes pueden desarrollar protocolos y compartir información y conclusiones con Horizon de forma segura. 

## <a name="do-i-have-to-purchase-hardware-appliances-from-microsoft-partners"></a>¿Tengo que comprar dispositivos de hardware de los socios de Microsoft?
El sensor de Azure Defender para IoT se ejecuta con especificaciones de hardware específicas, como se describe en la [guía de especificaciones de hardware](./how-to-identify-required-appliances.md). Los clientes pueden adquirir hardware certificado de los socios de Microsoft o usar la lista de materiales (BOM) suministrada y adquirirlos por su cuenta. 

El hardware certificado se ha probado en nuestros laboratorios para garantizar la estabilidad del controlador, la colocación de paquetes y el tamaño de la red.


## <a name="regulation-does-not-allow-us-to-connect-our-system-to-the-internet-can-we-still-utilize-defender-for-iot"></a>La normativa no nos permite conectar nuestro sistema a Internet. ¿De todos modos podemos usar Defender para IoT?

Sí, puede hacerlo. La solución en el entorno local de la plataforma de Azure Defender para IoT se implementa como un dispositivo de sensor físico o virtual que ingiere de manera pasiva el tráfico (a través de SPAN, RSPAN o TAP) para analizar, detectar y supervisar continuamente redes de TI, OT y IoT. En el caso de las empresas de mayor tamaño, varios sensores pueden agregar sus datos a una consola de administración local.

## <a name="where-in-the-network-should-i-connect-monitoring-ports"></a>¿En qué parte de la red deberían conectarse los puertos de supervisión?

El sensor de Azure Defender para IoT se conecta a un puerto SPAN o a un punto de acceso de terminal de red e inmediatamente comienza a recopilar el tráfico de ICS mediante supervisión pasiva (sin agente). No tiene ningún impacto en las redes OT, porque no se coloca en la ruta de acceso de datos y no examina de manera activa los dispositivos OT.

Por ejemplo:
- Un único dispositivo (virtual o físico) puede estar en el nivel de la red perimetral de la planta, de modo que todo el tráfico de las celdas de la planta se enrutan a este nivel.
- También puede colocar sensores pequeños en cada celda de la planta mediante la administración local o en la nube que residirá en el nivel de la red perimetral de la planta. Otro dispositivo (virtual o físico) puede supervisar el tráfico en el nivel de la red perimetral de la planta (para SCADA, Historian o MES).

## <a name="how-does-defender-for-iot-compare-to-the-competition"></a>¿Cómo es Defender para IoT en relación con la competencia?

Azure Defender para IoT ofrece seguridad completa en todos los dispositivos IoT/OT. Para las **organizaciones de usuarios finales**, Azure Defender para IoT ofrece una seguridad de capa de red sin agentes que se implementa rápidamente, funciona con diversos equipos de OT propietarios y sistemas Windows heredados, e interopera con Azure Sentinel y otras herramientas de SOC. Se puede implementar de forma local o en entornos conectados de Azure. En el caso de los **compiladores de dispositivos IoT**, Azure Defender para IoT ofrece agentes ligeros para insertar seguridad de nivel de dispositivo en nuevas iniciativas IoT/OT.

## <a name="do-i-have-to-be-an-azure-customer"></a>¿Tengo que ser cliente de Azure?

No, para la versión sin agente de Azure Defender para IoT no es necesario ser cliente de Azure. Sin embargo, si desea enviar alertas a Azure Sentinel, aprovisionar sensores de red y supervisar su estado desde la nube y beneficiarse de las actualizaciones automáticas de software e inteligencia sobre amenazas, tendrá que conectar el sensor a Azure a través de Azure IoT Hub.

Para la versión basada en agente de Azure Defender para IoT, debe ser cliente de Azure.

## <a name="can-i-create-my-own-alerts"></a>¿Puedo crear mis propias alertas?

Sí, puede crear alertas personalizadas basadas en varios parámetros, como la dirección IP/MAC, el tipo de protocolo, la clase, el servicio, la función, el comando, etc., así como los valores de las etiquetas personalizadas contenidas en las cargas.  Consulte [Creación de alertas personalizadas](quickstart-create-custom-alerts.md) para más información sobre las alertas personalizadas y cómo crearlas.

## <a name="what-happens-when-the-internet-connection-stops-working"></a>¿Qué ocurre cuando la conexión a Internet deja de funcionar?

Los sensores y agentes siguen ejecutándose y almacenan los datos, siempre y cuando el dispositivo esté en ejecución. Los datos se almacenan en la caché de mensajes de seguridad según la configuración de tamaño. Cuando el dispositivo vuelve a tener conectividad, se reanuda el envío de los mensajes de seguridad.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los primeros pasos con Defender para IoT, consulte los siguientes artículos:

- Lea la [información general](overview.md) de Defender para IoT.
- Compruebe los [requisitos previos del sistema](quickstart-system-prerequisites.md).
- Obtenga más información sobre cómo [comenzar a usar Defender para IoT](getting-started.md).
- Información sobre las [alertas de seguridad de Defender para IoT](concept-security-alerts.md)