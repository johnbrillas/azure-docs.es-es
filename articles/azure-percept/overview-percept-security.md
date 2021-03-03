---
title: Introducción sobre la seguridad de Azure Percept
description: Más información acerca de la seguridad de Azure Percept.
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/18/2021
ms.custom: template-concept
ms.openlocfilehash: b5a345139114842c83cb1f11792076efb1461870
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678833"
---
# <a name="azure-percept-security-overview"></a>Introducción sobre la seguridad de Azure Percept

Los dispositivos de Azure Percept DK están diseñados con una seguridad integrada de raíz de confianza del hardware en cada dispositivo. Ayuda a proteger los sensores con acceso a información confidencial de la privacidad, como cámaras y micrófonos, a inferir datos, y permite la autenticación y autorización del dispositivo para los servicios de Azure Percept Studio.

> [!NOTE]
> Azure Percept DK está diseñado para su uso en entornos de desarrollo y pruebas, y para escenarios de prueba de concepto.

## <a name="devices"></a>Dispositivos

### <a name="azure-percept-dk"></a>Azure Percept DK

Azure Percept DK incluye la versión 2.0 de un módulo de plataforma segura (TPM) que se puede usar para conectar el dispositivo de forma segura a los servicios de aprovisionamiento de dispositivos de Azure. TPM es un estándar ISO de Trusted Computing Group para todo el sector; puede obtener más información sobre TPM en [la especificación TPM 2.0 completa](https://trustedcomputinggroup.org/resource/tpm-library-specification/) o la especificación ISO/IEC 11889. Para más información sobre cómo DPS puede aprovisionar dispositivos de forma segura, consulte [Servicio Azure IoT Hub Device Provisioning: Atestación de TPM](https://docs.microsoft.com/azure/iot-dps/concepts-tpm-attestation).

### <a name="azure-percept-system-on-module-som"></a>Módulo de sistema Azure Percept

El módulo de sistema habilitado para la visión de Azure Percept DK y el módulo de sistema Azure Percept Audio incluyen una unidad con microcontrolador (MCU) para proteger el acceso a los sensores integrados de inteligencia artificial. En cada arranque, el firmware de la MCU autentica y autoriza el acelerador de inteligencia artificial en los servicios de Azure Percept Studio mediante la arquitectura del motor Device Identifier Composition Engine (DICE). DICE funciona dividiendo el arranque en capas y creando secretos únicos para cada capa y configuración que se basan en un secreto de dispositivo único (UDS). Si se arranca con otro código o configuración, en cualquier punto de la cadena, los secretos serán diferentes. Para más información sobre DICE, consulte las [especificaciones del grupo de trabajo de DICE](https://trustedcomputinggroup.org/work-groups/dice-architectures/). Para configurar el acceso a Azure Percept Studio y a los servicios necesarios, consulte **Configuración de firewalls para Azure Percept DK** a continuación.

Los dispositivos de Azure Percept usan la raíz de confianza del hardware para proteger el firmware. La ROM de arranque garantiza la integridad del firmware entre la ROM y el cargador del sistema operativo que, a su vez, garantiza la integridad de los demás componentes de software mediante la creación de una cadena de confianza.

## <a name="services"></a>Servicios

### <a name="iot-edge"></a>IoT Edge

Azure Percept DK se conecta de forma segura a Azure Percept Studio y a otros servicios de Azure mediante el protocolo de Seguridad de la capa de transporte (TLS). Azure Percept DK es un dispositivo habilitado para Azure IoT Edge. El entorno de ejecución de IoT Edge es una colección de programas que convierten un dispositivo en un dispositivo IoT Edge. Colectivamente, los componentes del entorno de ejecución de Azure IoT Edge permiten que los dispositivos IoT Edge reciban el código para ejecutarse en el perímetro y comunicar los resultados. Azure Percept DK emplea contenedores de Docker para aislar cargas de trabajo de IoT Edge del sistema operativo host y de las aplicaciones habilitadas para Edge. Para más información acerca del marco de seguridad de Azure IoT Edge, consulte [Administrador de seguridad de IoT Edge](https://docs.microsoft.com/azure/iot-edge/iot-edge-security-manager?view=iotedge-2018-06).

### <a name="device-update-for-iot-hub"></a>Device Update para IoT Hub

Device Update para IoT Hub habilita la actualización segura, escalable y confiable que proporciona seguridad renovable a los dispositivos de Azure Percept. Proporciona controles de administración enriquecidos y comprobación de actualizaciones a través de información. Azure Percept DK incluye una solución de actualización de dispositivos preintegrada que proporciona una actualización resistente (A/B) desde el firmware a las capas del sistema operativo.

<!---I think the below topics need to be somewhere else, (i.e. not on the main page)
--->

## <a name="configuring-firewalls-for-azure-percept-dk"></a>Configuración de firewalls para Azure Percept DK

Si la configuración de redes requiere agregar en la lista de permitidos las conexiones hechas desde los dispositivos de Azure Percept DK, revise la siguiente lista de componentes:

Esta lista de comprobación es un punto de partida para las reglas de firewall:

|URL (* = comodín) |Puertos TCP salientes|    Uso|
|-------------------|------------------|---------|
|*.auth.azureperceptdk.azure.net|   443|    Autenticación y autorización del módulo de sistema de Azure Percept DK|
|*.auth.projectsantacruz.azure.net| 443|    Autenticación y autorización del módulo de sistema de Azure Percept DK|

Además, revise la lista de [conexiones que usa Azure IOT Edge](https://docs.microsoft.com/azure/iot-edge/production-checklist?view=iotedge-2018-06#allow-connections-from-iot-edge-devices).

## <a name="additional-recommendations-for-deployment-to-production"></a>Recomendaciones adicionales para la implementación en producción

Azure Percept DK ofrece una gran variedad de funcionalidades de seguridad listas para usarse. Además de esas eficaces características de seguridad incluidas en la versión actual, Microsoft también sugiere las siguientes directrices al tener en cuenta las implementaciones de producción:

- Protección física sólida del propio dispositivo
- Asegurarse de que el cifrado de datos en reposo esté habilitado
- Supervisión continua de la postura del dispositivo y respuesta rápida a las alertas
- Limitación del número de administradores que tienen acceso al dispositivo

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre los [modelos de inteligencia artificial de Azure Percept](./overview-ai-models.md)que están disponibles.