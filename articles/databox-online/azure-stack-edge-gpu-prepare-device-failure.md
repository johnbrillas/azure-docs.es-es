---
title: Preparación para un error en un dispositivo de Azure Stack Edge Pro
description: Se describe cómo sustituir un dispositivo con error de Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 60469dc834c28bd8dbc73a1883ea01b06797c01f
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2021
ms.locfileid: "102442985"
---
# <a name="prepare-for-an-azure-stack-edge-pro-gpu-device-failure"></a>Preparación para un error en un dispositivo GPU de Azure Stack Edge Pro

[!INCLUDE [applies-to-GPU-and-pro-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-sku.md)]

Este artículo le ayuda a prepararse para un error de dispositivo mediante información detallada sobre cómo guardar y realizar una copia de seguridad de la configuración y los datos del dispositivo en el dispositivo GPU de Azure Stack Edge Pro. 

El artículo no incluye los pasos necesarios para realizar copias de seguridad de los contenedores de Kubernetes e IoT implementados en el dispositivo GPU de Azure Stack Edge Pro. 

## <a name="understand-device-failures"></a>Descripción de errores de dispositivo

El dispositivo GPU de Azure Stack Edge Pro puede experimentar dos tipos de errores de hardware.

- Errores tolerables que requieren que el usuario reemplace un componente de hardware. Estos errores le permitirán utilizar el dispositivo en un estado degradado. Entre los ejemplos de estos errores se incluyen una sola fuente de alimentación (PSU) con error o un único disco con error en el dispositivo. En cada uno de estos casos, el dispositivo puede continuar funcionando. Póngase en contacto con Soporte técnico de Microsoft lo antes posible para reemplazar los componentes con errores.

- Errores no tolerables que requieren que se reemplace todo el dispositivo, por ejemplo, cuando se ha producido un error en dos discos del dispositivo. En estos casos, póngase en contacto con Soporte técnico de Microsoft inmediatamente. Una vez que ellos determinen que es necesario el reemplazo de un dispositivo, el soporte técnico le ayudará a reemplazar el dispositivo de Azure Stack Edge.

Para estar preparado frente a errores no tolerables, debe realizar una copia de seguridad de lo siguiente en el dispositivo:

- Información sobre la configuración del dispositivo
- Datos de recursos compartidos locales y en la nube de Edge
- Archivos y carpetas asociados a las máquinas virtuales que se ejecutan en el dispositivo


## <a name="back-up-device-configuration"></a>Copia de seguridad de la configuración del dispositivo

Durante la configuración inicial del dispositivo, es importante que mantenga una copia de la información de configuración del mismo, tal como se indica en la [lista de comprobación de la implementación](azure-stack-edge-gpu-deploy-checklist.md). Durante la recuperación, esta información de configuración se usará para aplicarse al nuevo dispositivo de sustitución. 

## <a name="protect-device-data"></a>Protección de datos de dispositivos

Los datos del dispositivo puede ser de uno de los siguientes tipos:

- Datos de recursos compartidos en la nube de Edge
- Datos de recursos compartidos locales
- Archivos y copias de seguridad en máquinas virtuales

En las secciones siguientes se describen los pasos y las recomendaciones para proteger cada uno de estos tipos de datos en el dispositivo.

## <a name="protect-data-in-edge-cloud-shares"></a>Protección de datos de recursos compartidos en la nube de Edge

Puede crear recursos compartidos en la nube de Edge que organizan los datos en niveles desde el dispositivo a Azure. En función del ancho de banda de red disponible, configure plantillas de ancho de banda en el dispositivo para minimizar cualquier pérdida de datos en caso de que se produzca un error no tolerable.

> [!IMPORTANT]
> Si el dispositivo tiene un error no tolerable, es posible que se pierdan datos locales que no están organizados en niveles desde el dispositivo a Azure. 

## <a name="protect-data-in-edge-local-shares"></a>Protección de datos de recursos compartidos locales de Edge

Si va a implementar Kubernetes o IoT Edge, realice la configuración para guardar los datos de la aplicación en el dispositivo localmente y no sincronizar con Azure Storage. Los datos se almacenan en un recurso compartido en el dispositivo. Puede que sea importante realizar una copia de seguridad de los datos de estos recursos compartidos.

Las siguientes soluciones de protección de datos de terceros pueden proporcionar una solución de copia de seguridad para los datos de los recursos compartidos de SMB o NFS locales. 

| Software de terceros           | Referencia a la solución                               |
|--------------------------------|---------------------------------------------------------|
| Cohesity                       | [https://www.cohesity.com/solution/cloud/azure/](https://www.cohesity.com/solution/cloud/azure/) <br> Para obtener más información, póngase en contacto con Cohesity.          |
| Commvault                      | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br> Para obtener más información, póngase en contacto con Commvault.          |
| Veritas                        | [http://veritas.com/azure](http://veritas.com/azure) <br> Para obtener más información, póngase en contacto con Veritas.   |
| Veeam                          | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Para más información, póngase en contacto con Veeam. |


## <a name="protect-files-and-folders-on-vms"></a>Protección de archivos y carpetas en máquinas virtuales

Azure Stack Edge funciona con Azure Backup y otras soluciones de protección de datos de terceros para proporcionar una solución de copia de seguridad que protege los datos contenidos en las máquinas virtuales implementadas en el dispositivo. En la tabla siguiente se enumeran las referencias a las soluciones disponibles entre las que puede elegir.


| Soluciones de copia de seguridad        | SO admitido   | Referencia                                                                |
|-------------------------|----------------|--------------------------------------------------------------------------|
| Agente de Microsoft Azure Recovery Services (MARS) para Azure Backup | Windows        | [Acerca del agente de MARS](../backup/backup-azure-about-mars.md)    |
| Cohesity                | Windows, Linux | [Breve descripción de la solución de integración, copia de seguridad y recuperación Microsoft Azure](https://www.cohesity.com/solution/cloud/azure) <br>Para obtener más información, póngase en contacto con Cohesity.                          |
| Commvault               | Windows, Linux | [https://www.commvault.com/azure](https://www.commvault.com/azure) <br>Para obtener más información, póngase en contacto con Commvault.                          |
| Veritas                 | Windows, Linux | [https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370](https://vox.veritas.com/t5/Protection/Protecting-Azure-Stack-Edge-with-NetBackup/ba-p/883370) <br> Para obtener más información, póngase en contacto con Veritas.                    |
| Veeam                   | Windows, Linux | [https://www.veeam.com/kb4041](https://www.veeam.com/kb4041) <br> Para más información, póngase en contacto con Veeam. |


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre la [recuperación de un dispositivo GPU de Azure Stack Edge Pro con errores](azure-stack-edge-gpu-recover-device-failure.md).