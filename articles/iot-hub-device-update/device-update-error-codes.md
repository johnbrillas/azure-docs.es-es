---
title: Seguridad de Device Update para Azure IoT Hub | Microsoft Docs
description: En este documento se proporciona una tabla de códigos de error de cliente para los distintos componentes de Device Update.
author: lichris
ms.author: lichris
ms.date: 2/18/2021
ms.topic: reference
ms.service: iot-hub-device-update
ms.openlocfilehash: 5251d0cb09e40305d1efd89c31d3af0fa36ad385
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678822"
---
# <a name="device-update-for-iot-hub-error-codes"></a>Códigos de error de Device Update para IoT Hub

En este documento se proporciona una tabla de códigos de error para los distintos componentes de Device Update. Está destinada a usarse como referencia para los usuarios que desean intentar analizar sus propios códigos de error para diagnosticar y solucionar problemas.

Hay dos componentes principales del lado cliente que pueden generar códigos de error: el agente de Device Update y el agente de Optimización de distribución.

## <a name="device-update-agent"></a>Agente de Device Update

### <a name="resultcode-and-extendedresultcode"></a>ResultCode y ExtendedResultCode

Device Update para la interfaz PnP de IoT Hub Core genera `ResultCode` y `ExtendedResultCode`, que se pueden usar para diagnosticar errores. [Obtenga más información](device-update-plug-and-play.md) sobre la interfaz PnP de Device Update Core.

#### <a name="resultcode"></a>ResultCode

`ResultCode` es un código de estado general que sigue la convención de códigos de estado HTTP.
[Obtenga más información](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) sobre los códigos de estado HTTP.

#### <a name="extendedresultcode"></a>ExtendedResultCode

`ExtendedResultCode` es un entero con información de error codificada.

Lo más probable es que vea `ExtendedResultCode` como un entero con signo en la interfaz PnP. Para descodificar `ExtendedResultCode`, convierta el entero con signo en un valor hexadecimal sin signo. Solo se usan los primeros cuatro bytes de `ExtendedResultCode` y tienen el formato `F` `FFFFFFF`, donde el primer recorte es el **código de la instalación** y el resto de los bits son el **código de error**.

**Códigos de instalación**

| Código de instalación     | Descripción  |
|-------------------|--------------|
| D                 | Error que genera el SDK de DO|
| E                 | El código de error es un valor errno. |


Por ejemplo:

`ExtendedResultCode` es `-536870781`

La representación hexadecimal sin signo de `-536870781` es `FFFFFFFF E0000083`.

| Ignore    | Código de instalación  | Código de error   |
|-----------|----------------|--------------|
| FFFFFFFF  | E              | 0000083      |

`0x83` en hexadecimal es `131` en decimal, que es el valor errno de `ENOLCK`.

## <a name="delivery-optimization-agent"></a>Agente de Optimización de distribución
En la tabla siguiente se enumeran los códigos de error relacionados con el componente de Optimización de distribución (DO) del cliente de Device Update. El componente DO es responsable de descargar el contenido de la actualización en el dispositivo IoT.

El código de error de DO se puede obtener mediante el examen de las excepciones que se producen en respuesta a una llamada a la API.

| Código de error  | Error de cadena                       | Tipo                 | Descripción |
|-------------|------------------------------------|----------------------|-------------|
| 0x80D01001L | OM_E_NO_SERVICE                    | N/D                  | El servicio Optimización de distribución no pudo proporcionar el servicio. |
| 0x80D02002L | DO_E_DOWNLOAD_NO_PROGRESS          | Trabajo de descarga         | La descarga de un archivo no ha observado ningún progreso dentro del período definido. |
| 0x80D02003L | DO_E_JOB_NOT_FOUND                 | Trabajo de descarga         | No se encontró el trabajo. |
| 0x80D02005L | DO_E_NO_DOWNLOADS                  | Trabajo de descarga         | Actualmente no existe ninguna descarga. |
| 0x80D0200CL | DO_E_JOB_TOO_OLD                   | Trabajo de descarga         | El trabajo no se completó o se canceló antes de alcanzar el umbral de antigüedad máximo. |
| 0x80D02011L | DO_E_UNKNOWN_PROPERTY_ID           | Trabajo de descarga         | Se llamó a SetProperty() o GetProperty() con un identificador de propiedad desconocido. |
| 0x80D02012L | DO_E_READ_ONLY_PROPERTY            | Trabajo de descarga         | No se puede llamar a SetProperty() en una propiedad de solo lectura. |
| 0x80D02013L | DO_E_INVALID_STATE                 | Trabajo de descarga         | La acción solicitada no se permite en el estado de trabajo actual. Es posible que el trabajo se haya cancelado o se haya completado la transferencia. Ahora se encuentra en estado de solo lectura. |
| 0x80D02018L | DO_E_FILE_DOWNLOADSINK_UNSPECIFIED | Trabajo de descarga         | No se puede iniciar una descarga porque no se especificó ningún receptor de descarga (archivo local o interfaz de flujo). |
| 0x80D02200L | DO_E_DOWNLOAD_NO_URI               | Interfaz IDODownload| La descarga se inició sin proporcionar ningún URI. |
| 0x80D03805L | DO_E_BLOCKED_BY_NO_NETWORK         | Condiciones transitorias | Descarga se puso en pausa debido a la pérdida de conectividad de red. |
| 0x80D05001L | DO_E_HTTP_BLOCKSIZE_MISMATCH       | HTTP                 | El servidor HTTP devolvió una respuesta con un tamaño de datos distinto al solicitado. |
| 0x80D05002L | DO_E_HTTP_CERT_VALIDATION          | HTTP                 | Se produjo un error al validar el certificado del servidor HTTP. |
| 0x80D05010L | DO_E_INVALID_RANGE                 | HTTP                 | El intervalo de bytes especificado no es válido. |
| 0x80D05011L | DO_E_INSUFFICIENT_RANGE_SUPPORT    | HTTP                 | El servidor no es compatible con el protocolo HTTP necesario. El servicio Optimización de distribución (DO) requiere que el servidor admita el encabezado de protocolo de intervalo. |
| 0x80D05012L | DO_E_OVERLAPPING_RANGES            | HTTP                 | La lista de intervalos de bytes contiene algunos intervalos superpuestos, lo que no se admite. |
## <a name="device-update-content-service"></a>Servicio de contenido de Device Update
En la tabla siguiente se enumeran los códigos de error relacionados con el componente de servicio de contenido del servicio Device Update. El componente de servicio de contenido es responsable de controlar la importación de contenido de actualización.

| Código de error                    | Error de cadena                                                               | Pasos siguientes                         |
|-------------------------------|----------------------------------------------------------------------------|------------------------------------|
| "UpdateAlreadyExists"         | Ya existe una actualización con la misma identidad.                              | Asegúrese de que está importando una actualización que todavía no se ha importado en esta instancia de Device Update para IoT Hub. |
| "DuplicateContentImport"      | Se importó contenido idéntico simultáneamente varias veces.                  | Igual que para UpdateAlreadyExists. |
| "CannotProcessImportManifest" | Error al procesar el manifiesto de importación.                                          | Consulte la documentación de [conceptos de importación](./import-concepts.md) y [actualización de la importación](./import-update.md) para obtener un formato de manifiesto de importación adecuado. |
| "CannotDownload"              | No se puede descargar el manifiesto de importación.                                           | Asegúrese de que la dirección URL del archivo de manifiesto de importación sigue siendo válida. |
| "CannotParse"                 | No se puede analizar el manifiesto de importación.                                              | Compruebe la precisión del manifiesto de importación con el esquema definido en la documentación de [actualización de la importación](./import-update.md). |
| "UnsupportedVersion"          | No se admite la versión de esquema del manifiesto de importación.                           | Asegúrese de que el manifiesto de importación usa el esquema más reciente definido en la documentación de [actualización de la importación](./import-update.md). |
| "UpdateLimitExceeded"         | Error al importar la actualización debido a que se superó el límite.                              | Alcanzó un límite en el número de proveedores, nombres o versiones diferentes permitido en la instancia de Device Update para IoT Hub. Elimine algunas actualizaciones de la instancia y vuelva a intentarlo. |
| "UpdateProvider"              | No se puede importar un nuevo proveedor de actualizaciones.                                       | Alcanzó un límite en el número de __proveedores__ diferentes permitidos en la instancia de Device Update para IoT Hub. Elimine algunas actualizaciones de la instancia y vuelva a intentarlo. |
| "UpdateName"                  | No se puede importar un nuevo nombre de actualización para el proveedor especificado.                | Alcanzó un límite en el número de __nombres__ diferentes permitidos en un proveedor en la instancia de Device Update para IoT Hub. Elimine algunas actualizaciones de la instancia y vuelva a intentarlo. |
| "UpdateVersion"               | No se puede importar una nueva versión de actualización para el proveedor y el nombre especificados.    | Alcanzó un límite en el número de __versiones__ diferentes permitidas en un proveedor y un nombre en la instancia de Device Update para IoT Hub. Elimine algunas actualizaciones con ese nombre de la instancia y vuelva a intentarlo. |
| "UpdateProviderCompatibility" | No se puede importar un proveedor de actualizaciones adicional con la compatibilidad especificada. | Al definir las propiedades de compatibilidad del modelo de dispositivo y del fabricante del dispositivo en un manifiesto de importación, tenga en cuenta que Device Update para IoT Hub admite una combinación de nombre y proveedor única para un determinado fabricante o modelo. Esto significa que, si intenta utilizar las mismas propiedades de compatibilidad de modelo o fabricante con más de una combinación de proveedor o nombre, verá estos errores. Para resolverlo, asegúrese de que todas las actualizaciones de un dispositivo determinado (según la definición del fabricante o el modelo) usen el mismo proveedor y el mismo nombre. Aunque no es necesario, puede considerar la posibilidad de que el proveedor coincida con el fabricante y el nombre coincida con el modelo, para simplificar el proceso. |
| "UpdateNameCompatibility"     | No se puede importar un nombre de actualización adicional con la compatibilidad especificada.     | Igual que para UpdateProviderCompatibility.ContentLimitNamespaceCompatibility. |
| "UpdateVersionCompatibility"  | No se puede importar una versión de actualización adicional con la compatibilidad especificada.  | Igual que para UpdateProviderCompatibility.ContentLimitNamespaceCompatibility. |
| "CannotProcessUpdateFile"     | Error al procesar el archivo de código fuente.                                              |                                    |
| "ContentFileCannotDownload"   | No se puede descargar el archivo de origen.                                               | Asegúrese de que la dirección URL de los archivos de actualización sigue siendo válida. |

**[Siguiente paso: solucionar problemas con Device Update](.\troubleshoot-device-update.md)**