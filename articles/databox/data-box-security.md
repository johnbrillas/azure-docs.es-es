---
title: Introducción a la seguridad de Microsoft Azure Data Box | Microsoft Docs en datos
description: Describe las características de seguridad de Azure Data Box en el dispositivo, el servicio y los datos que residen en Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: conceptual
ms.date: 12/16/2020
ms.author: alkohli
ms.openlocfilehash: 4d6c77b3e8920cabc397cdcbc235baefa031e5ab
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97655499"
---
# <a name="azure-data-box-security-and-data-protection"></a>Protección de datos y seguridad de Azure Data Box

Data Box proporciona una solución segura para proteger los datos al garantizar que solo las entidades autorizadas puedan ver, modificar o eliminar los datos. En este artículo se describen las características de seguridad de Azure Data Box que ayudan a proteger cada uno de los componentes de la solución Data Box y los datos almacenados en ellos.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="data-flow-through-components"></a>Flujo de datos a través de componentes

La solución Microsoft Azure Data Box consta de cuatro componentes principales que interactúan entre sí:

- **Servicio Azure Data Box hospedado en Azure**: el servicio de administración que se usa para crear el pedido del dispositivo, configurar el dispositivo y, a continuación, supervisar el pedido hasta que se realiza.
- **Dispositivo de Data Box**: el dispositivo de transferencia que se envía para importar los datos del entorno local a Azure.
- **Clientes/hosts conectados al dispositivo**: los clientes en la infraestructura que se conectan al dispositivo de Data Box y contienen los datos que se deben proteger.
- **Almacenamiento en la nube** : la ubicación en la nube de Azure donde se almacenan los datos. Suele ser la cuenta de almacenamiento vinculada al recurso de Azure Data Box que ha creado.

El diagrama siguiente indica el flujo de datos a través de la solución de Azure Data Box desde el entorno local a Azure y las diversas características de seguridad utilizadas a medida que los datos fluyen a través de la solución. Este flujo es para un pedido de importación de Data Box.

![Seguridad de importación en Data Box](media/data-box-security/data-box-security-import.png)

El siguiente diagrama muestra el orden de exportación de Data Box.

![Seguridad de exportación en Data Box](media/data-box-security/data-box-security-export.png)

Mientras los datos fluyen a través de esta solución, se registran los eventos y se generan registros. Para obtener más información, consulte:

- [Seguimiento y registro de eventos para los pedidos de importación de Azure Data Box](data-box-logs.md).
- [Seguimiento y registro de eventos para los pedidos de exportación de Azure Data Box](data-box-export-logs.md).

## <a name="security-features"></a>Características de seguridad

Data Box proporciona una solución segura para proteger los datos al garantizar que solo las entidades autorizadas puedan ver, modificar o eliminar los datos. Las características de seguridad de esta solución están destinadas al disco y al servicio asociado, lo que garantiza la seguridad de los datos almacenados.

### <a name="data-box-device-protection"></a>Protección del dispositivo Data Box

El dispositivo Data Box se protege mediante las siguientes características:

- Una carcasa resistente para el dispositivo, capaz de resistir impactos, transporte adverso y condiciones ambientales. 
- Detección de manipulaciones en hardware y software que impide más operaciones en el dispositivo.
- Ejecuta solo el software específico para Data Box.
- Arranca en un estado bloqueado.
- Controla el dispositivo a través de una clave de paso de desbloqueo de dispositivo. Esta clave de paso está protegida por una clave de cifrado. Puede usar su propia clave administrada por el cliente para proteger la clave de paso. Para más información, consulte [Uso de claves administradas por el cliente en Azure Key Vault para Azure Data Box](data-box-customer-managed-encryption-key-portal.md).
- Credenciales de acceso para copiar datos hacia y desde el dispositivo. Todos los accesos a la página **Credenciales del dispositivo** en Azure Portal quedan registrados en los [registros de actividad](data-box-logs.md#query-activity-logs-during-setup).
- Puede usar sus propias contraseñas para el acceso de dispositivos y recursos compartidos. Para más información, consulte el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md).

### <a name="establish-trust-with-the-device-via-certificates"></a>Establecer la confianza con el dispositivo mediante certificados

Un dispositivo Data Box le permite traer sus propios certificados e instalar los que se usarán para conectarse a la interfaz de usuario web local y a Blob Storage. Para obtener más información, consulte [Uso de sus propios certificados con los dispositivos Data Box y Data Box Heavy](data-box-bring-your-own-certificates.md).

### <a name="data-box-data-protection"></a>Protección de datos de Data Box

Los datos que fluyen en Data Box se protegen mediante las siguientes características:

- Cifrado AES de 256 bits para datos en reposo. En un entorno de alta seguridad, puede usar el cifrado doble basado en software. Para más información, consulte el [Tutorial: Realización de pedidos de Azure Data Box](data-box-deploy-ordered.md).
- Se pueden usar protocolos cifrados para los datos en movimiento. Se recomienda utilizar SMB 3.0 con cifrado para proteger los datos cuando se copien de los servidores de datos.
- Eliminación segura de los datos desde el dispositivo una vez completada la carga de datos en Azure. La eliminación de datos se ha establecido de acuerdo con las instrucciones del [Apéndice A para las unidades de disco duro ATA en el estándar NIST 800-88r1](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf). El evento de eliminación de datos se registra en el [historial de pedidos](data-box-logs.md#download-order-history).

### <a name="data-box-service-protection"></a>Protección del servicio Data Box

El servicio Data Box se protege mediante las siguientes características:

- El acceso al servicio Data Box requiere que la organización tenga una suscripción de Azure que incluya dicho servicio. Su suscripción regula las características a las que puede tener acceso en Azure Portal.
- Dado que el servicio Data Box está hospedado en Azure, se encuentra protegido por las características de seguridad de Azure. Para obtener más información acerca de las características de seguridad que proporciona Microsoft Azure, visite el [Centro de confianza de Microsoft Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx).
- El acceso al pedido de Data Box se puede controlar a través del uso de roles de Azure. Para obtener más información, consulte [Set up access control for Data Box order](data-box-logs.md#set-up-access-control-on-the-order) (Configuración del control de accesos para el pedido de Data Box).
- El servicio Data Box almacena la contraseña de desbloqueo que se usa para desbloquear el dispositivo en el servicio.
- El servicio Data Box almacena los detalles del pedido y del estado en el servicio. Esta información se elimina cuando se elimina el pedido.

## <a name="managing-personal-data"></a>Administración de datos personales

Azure Data Box recopila y muestra la información personal en las siguientes instancias claves en el servicio:

- **Configuración de notificaciones**: cuando se crea un pedido, se configura la dirección de correo electrónico de los usuarios en la configuración de notificación. El administrador puede ver esta información. El servicio elimina esta información cuando el trabajo alcanza el estado terminal o al eliminar el pedido.

- **Detalles de pedidos**: una vez creado el pedido, la dirección de envío, el correo electrónico y la información de contacto de los usuarios se almacenan en Azure Portal. Entre la información guardada se incluyen los siguientes datos:

  - Nombre de contacto
  - Número de teléfono
  - Email
  - Dirección
  - City
  - Código postal
  - State
  - País/región/provincia
  - Número de cuenta del transportista
  - Número de seguimiento del envío

    El servicio Data Box elimina los detalles del pedido cuando el trabajo se completa o al eliminar el pedido.

- **Dirección de envío**: después de realizar el pedido, el servicio Data Box proporciona la dirección de envío a compañías de transporte como UPS o DHL. 

Para obtener más información, revise la directiva de privacidad de Microsoft en el [Centro de confianza](https://www.microsoft.com/trustcenter).


## <a name="security-guidelines-reference"></a>Referencia de directrices de seguridad

Las siguientes directrices de seguridad están implementadas en Data Box:

|Directrices   |Descripción   |
|---------|---------|
|[IEC 60529 IP52](https://www.iec.ch/)    | Para la protección contra agua y polvo         |
|[ISTA 2A](https://ista.org/docs/2Aoverview.pdf)     | Para la resistencia ante las condiciones adversas de transporte          |
|[NIST SP 800-147](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-147.pdf)      | Para la actualización segura del firmware         |
|[FIPS 140-2 nivel 2](https://csrc.nist.gov/csrc/media/publications/fips/140/2/final/documents/fips1402.pdf)      | Para la protección de datos         |
|Apéndice A, para unidades de disco duro ATA en [NIST SP 800-88r1](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-88r1.pdf)      | Para el saneamiento de los datos         |

## <a name="next-steps"></a>Pasos siguientes

- Revise los [requisitos de Data Box](data-box-system-requirements.md).
- Información acerca de los [límites de Data Box](data-box-limits.md).
- Implemente rápidamente [Azure Data Box](data-box-quickstart-portal.md) en Azure Portal.
