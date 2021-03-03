---
title: Manifiesto de actualización de Device Update para IoT Hub | Microsoft Docs
description: Aprenda cómo enviar las propiedades desde el servicio Device Update al dispositivo durante una actualización.
author: andbrown
ms.author: andbrown
ms.date: 2/17/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ea438a4a8db8dce9cf2acb0b5b3ff4e250c3cf39
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660740"
---
# <a name="device-update-for-iot-hub-update-manifest"></a>Manifiesto de actualización de Device Update para IoT Hub

## <a name="overview"></a>Información general

Device Update para IoT Hub usa un _manifiesto de actualización_ para comunicar acciones y también metadatos que dan soporte a esas acciones mediante las propiedades de la interfaz [AzureDeviceUpdateCore.OrchestratorMetadata:4](./device-update-plug-and-play.md).
En este documento se describen los aspectos básicos de la forma en que Device Update Agent utiliza la propiedad `updateManifest` de la interfaz de `AzureDeviceUpdateCore.OrchestratorMetadata:4`. Las propiedades de la interfaz `AzureDeviceUpdateCore.OrchestratorMetadata:4` se envían desde el servicio Device Update para IoT Hub a Device Update Agent. `updateManifest` es un objeto JSON serializado que Device Update Agent analiza.

### <a name="an-example-update-manifest"></a>Un manifiesto de actualización de ejemplo

```JSON
{
    "manifestVersion": "1",
    "updateId": {
        "provider": "DuTest",
        "name": "DuTestUser",
        "version": "2020.611.534.16"
    },
    "updateType": "microsoft/swupdate:1",
    "installedCriteria": "1.0",
    "files": {
        "00000": {
            "fileName": "image.swu",
            "sizeInBytes": 256000,
            "hashes": {
                "sha256": "IhIIxBJpLfazQOk/PVi6SzR7BM0jf4HDqw+6gdZ3vp8="
            }
        }
    },
    "createdDateTime": "2020-06-12T00:38:13.9350278"
}
```

El objetivo del manifiesto de actualización es describir el contenido de una actualización, es decir, su identidad, tipo, los criterios instalados y los metadatos del archivo de actualización. Además, el manifiesto de actualización se firma criptográficamente para que Device Update Agent pueda comprobar su autenticidad. Consulte la documentación relativa a la [seguridad de Device Update](./device-update-security.md) para más información sobre cómo usar el manifiesto de actualización para importar contenido de forma segura.

## <a name="import-manifest-vs-update-manifest"></a>Diferencias entre los manifiestos de importación y los de actualización

Es importante conocer las diferencias entre los conceptos de manifiesto de importación y manifiesto actualización en Device Update para IoT Hub. 
* El [manifiesto de importación](./import-concepts.md) lo crea la persona que crea la actualización correspondiente. Describe el contenido de la actualización que se va a importar en Device Update para IoT Hub. 
* El manifiesto de actualización lo genera automáticamente Device Update para IoT Hub, para lo que utiliza algunas de las propiedades que se definieron en el manifiesto de importación. Se usa para comunicar información relevante a Device Update Agent durante el proceso de actualización. 

Cada tipo de manifiesto tiene su propio esquema y versión de esquema.

## <a name="update-manifest-properties"></a>Propiedades del manifiesto de actualización

Las definiciones de alto nivel de las propiedades del manifiesto de actualización se pueden encontrar en las definiciones de interfaz que se incluyen [aquí](./device-update-plug-and-play.md). Para proporcionar un mayor contexto, examine detenidamente las propiedades y cómo se usan en el sistema.

### <a name="updateid"></a>updateId

Contiene `provider`, `name` y `version`, que representa la identidad de actualización exacta de Device Update para IoT Hub que se usa para determinar los dispositivos compatibles con la actualización.

### <a name="updatetype"></a>updateType

Representa el tipo de actualización que se controla mediante un tipo específico de controlador de actualizaciones. Sigue la forma de `microsoft/swupdate:1` para una actualización basada en imágenes y de `microsoft/apt:1` para una actualización basada en paquetes (consulte la sección `Update Handler Types`, que encontrará a continuación).

### <a name="installedcriteria"></a>installedCriteria

Cadena que contiene la información que necesita el controlador de actualizaciones de Device Update Agent para determinar si la actualización está instalada en el dispositivo. En la `Update Handler Types` sección se documenta el formato de `installedCriteria`, para cada tipo de actualización compatible con Device Update para IoT Hub.

### <a name="files"></a>archivos

Indica a Device Update Agent qué archivos se van a descargar y el hash que se va a usar para comprobar que los archivos se descargaron correctamente.
Esta es una mirada más detallada sobre el contenido de la propiedad `files`:

```json
"files":{
        <FILE_ID_STRING>:{
            "fileName":<STRING>,
            "sizeInBytes":<INTEGER>,
            "hashes":{
                <HASH-TYPE>:<HASH-STRING>
            }
        }
    }
```

Fuera de `updateManifest` se encuentra la matriz `fileUrls` de objetos JSON.

```json
"fileUrls":{
      <FILE_ID_STRING>: <URL-in-String-Format>
 }
```

`FILE_ID_STRING`, en `fileUrls` y `files` son iguales (por ejemplo, "0000" en `files` tiene la dirección URL en "0000" dentro de `fileUrls`).

### <a name="manifestversion"></a>manifestVersion

Cadena que representa la versión del esquema.

## <a name="update-handler-types"></a>Tipos de controlador de actualización

|Método de actualización|Tipo de controlador de actualización|Tipo de actualización|Criterios instalados|Archivos que se esperan publicar|
|-------------|-------------------|----------|-----------------|--------------|
|Basada en imágenes|SWUpdate|"microsoft/swupdate:version"|La imagen de referencia guarda la sugerencia de su versión en el archivo /etc/adu-version.  |Archivo .swu que contiene una imagen de SWUpdate|
|Basado en paquetes|APT|"microsoft/apt:version"|`<name>` + "-" + `<version>` (propiedades definidas en el archivo de manifiesto APT)|`<APT Update Manifest>`.json que contiene la lista de paquetes y la configuración de APT|

