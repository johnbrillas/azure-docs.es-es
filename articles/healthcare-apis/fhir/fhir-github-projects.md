---
title: Proyectos de GitHub relacionados para la API de Azure para FHIR
description: Lista de todos los repositorios de código abierto (GitHub) de la API de Azure para FHIR.
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/01/2021
ms.author: ginle
ms.openlocfilehash: 9977765183bd567377592631d65f3e548bef4b34
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2021
ms.locfileid: "105097645"
---
# <a name="related-github-projects"></a>Proyectos de GitHub relacionados

Tenemos muchos proyectos de código abierto en GitHub que proporcionan el código fuente y las instrucciones para implementar servicios para varios usos. Siempre le agradecemos que visite nuestros repositorios de GitHub para aprender y experimentar con nuestras características y productos. 

## <a name="fhir-server"></a>Servidor de FHIR
* [Microsoft/fhir-Server](https://github.com/microsoft/fhir-server/): fhir Server de código abierto, que es la base de la API de Azure para fhir
* Para ver las versiones más recientes, consulte las [notas](https://github.com/microsoft/fhir-server/releases) de la versión.
* [Microsoft/fhir-Server-samples](https://github.com/microsoft/fhir-server-samples): un entorno de ejemplo

## <a name="data-conversion--anonymization"></a>& de conversión de datos anonimización

#### <a name="fhir-converter"></a>Convertidor de FHIR
* [Microsoft/FHIR-Converter](https://github.com/microsoft/FHIR-Converter): una utilidad de conversión para traducir formatos de datos heredados en FHIR
* Integración con la API de Azure para FHIR, así como con el servidor FHIR para Azure en forma de $convert: operación de datos
* Mejoras continuas en OSS y la integración continua en los servidores de FHIR
 
#### <a name="fhir-converter---vs-code-extension"></a>FHIR Converter: extensión de VS Code
* [Microsoft/FHIR-Tools-for-anonimización](https://github.com/microsoft/FHIR-Tools-for-Anonymization): un conjunto de herramientas para ayudar con los datos (en formato FHIR) anonimización
* Integración con la API de Azure para FHIR, así como con el servidor de FHIR para Azure en forma de "exportación identificada".

#### <a name="fhir-tools-for-anonymization"></a>Herramientas de FHIR para anonimización
* [Microsoft/vscode-azurehealthcareapis-Tools](https://github.com/microsoft/vscode-azurehealthcareapis-tools): una extensión de vs Code que contiene una colección de herramientas para trabajar con las API de Azure Healthcare
* Liberado a Visual Studio Marketplace
* Se usa para crear plantillas líquidas que se usarán en el convertidor FHIR

## <a name="iot-connector"></a>Conector de IoT

#### <a name="integration-with-iot-hub-and-iot-central"></a>Integración con IoT Hub y IoT Central
* [Microsoft/iomt-fhir](https://github.com/microsoft/iomt-fhir): integración con IoT Hub o IOT central en fhir con la normalización de datos y la conversión fhir de los datos normalizados
* Normalización: la información de los datos del dispositivo se extrae en un formato común para su posterior procesamiento
* Conversión FHIR: los datos normalizados y agrupados se asignan a FHIR. Las observaciones se crean o actualizan según las plantillas configuradas y se vinculan al dispositivo y al paciente.
* [Herramientas para ayudar a crear el mapa de conversación](https://github.com/microsoft/iomt-fhir/tree/master/tools/data-mapper): visualice la configuración de asignación para normalizar los datos de entrada del dispositivo y transformarlos en los recursos de FHIR. Los desarrolladores pueden usar esta herramienta para editar y probar las asignaciones, la asignación de dispositivos y la asignación de FHIR y exportarlas para cargarlas en el conector de IoT en el Azure Portal.

#### <a name="healthkit-and-fhir-integration"></a>Integración de HealthKit y FHIR
* [Microsoft/healthkit-on-fhir](https://github.com/microsoft/healthkit-on-fhir): una biblioteca de SWIFT que automatiza la exportación de datos de Healthkit de Apple a un servidor de fhir

 