---
title: 'Lista de direcciones URL requeridas de Windows Virtual Desktop: Azure'
description: Una lista de direcciones URL que debe desbloquear para garantizar que su implementación de Windows Virtual Desktop funcione según lo previsto.
author: Heidilohr
ms.topic: conceptual
ms.date: 12/04/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 872a67d8d9f41c0c809df54304352b2a5f58e011
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98251767"
---
# <a name="required-url-list"></a>Lista de direcciones URL requeridas

Para implementar y usar Windows Virtual Desktop, debe desbloquear determinadas direcciones URL para que las máquinas virtuales (VM) puedan acceder a ellas en cualquier momento. En este artículo se enumeran las direcciones URL requeridas que se deben desbloquear para que Windows Virtual Desktop funcione correctamente. 

>[!IMPORTANT]
>Windows Virtual Desktop no admite implementaciones que bloqueen las direcciones URL que se muestran en este artículo.

## <a name="virtual-machines"></a>Máquinas virtuales

Las máquinas virtuales de Azure que cree para Windows Virtual Desktop deben tener acceso a las siguientes direcciones URL en la nube comercial de Azure:

|Dirección|Puerto TCP de salida|Propósito|Etiqueta de servicio|
|---|---|---|---|
|*.wvd.microsoft.com|443|Tráfico de servicio|WindowsVirtualDesktop|
|gcs.prod.monitoring.core.windows.net|443|Tráfico de agente|AzureCloud|
|production.diagnostics.monitoring.core.windows.net|443|Tráfico de agente|AzureCloud|
|*xt.blob.core.windows.net|443|Tráfico de agente|AzureCloud|
|*eh.servicebus.windows.net|443|Tráfico de agente|AzureCloud|
|*xt.table.core.windows.net|443|Tráfico de agente|AzureCloud|
|*xt.queue.core.windows.net|443|Tráfico de agente|AzureCloud|
|catalogartifact.azureedge.net|443|Azure Marketplace|AzureCloud|
|kms.core.windows.net|1688|Activación de Windows|Internet|
|mrsglobalsteus2prod.blob.core.windows.net|443|Actualizaciones de la pila de agentes y SXS|AzureCloud|
|wvdportalstorageblob.blob.core.windows.net|443|Soporte técnico de Azure Portal|AzureCloud|
| 169.254.169.254 | 80 | [Punto de conexión de servicio de metadatos de instancias de Azure](../virtual-machines/windows/instance-metadata-service.md) | N/D |
| 168.63.129.16 | 80 | [Seguimiento de estado del host de sesión](../virtual-network/network-security-groups-overview.md#azure-platform-considerations) | N/D |

>[!IMPORTANT]
>Windows Virtual Desktop ahora admite la etiqueta FQDN. Para más información, consulte [Uso de Azure Firewall para proteger las implementaciones de Windows Virtual Desktop](../firewall/protect-windows-virtual-desktop.md).
>
>Se recomienda usar etiquetas FQDN o etiquetas de servicio, en lugar de las direcciones URL, para evitar problemas de servicio. Las direcciones URL y etiquetas de la lista solo corresponden a sitios y recursos de Windows Virtual Desktop. No incluyen direcciones URL para otros servicios como Azure Active Directory.

Las máquinas virtuales de Azure que cree para Windows Virtual Desktop deben tener acceso a las siguientes direcciones URL en la nube de Azure Government:

|Dirección|Puerto TCP de salida|Propósito|Etiqueta de servicio|
|---|---|---|---|
|*.wvd.microsoft.us|443|Tráfico de servicio|WindowsVirtualDesktop|
|gcs.monitoring.core.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|monitoring.core.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|fairfax.warmpath.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|*xt.blob.core.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|*.servicebus.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|*xt.table.core.usgovcloudapi.net|443|Tráfico de agente|AzureCloud|
|Kms.core.usgovcloudapi.net|1688|Activación de Windows|Internet|
|mrsglobalstugviffx.blob.core.usgovcloudapi.net|443|Actualizaciones de la pila de agentes y SXS|AzureCloud|
|wvdportalstorageblob.blob.core.usgovcloudapi.net|443|Soporte técnico de Azure Portal|AzureCloud|
| 169.254.169.254 | 80 | [Punto de conexión de servicio de metadatos de instancias de Azure](../virtual-machines/windows/instance-metadata-service.md) | N/D |
| 168.63.129.16 | 80 | [Seguimiento de estado del host de sesión](../virtual-network/network-security-groups-overview.md#azure-platform-considerations) | N/D |

En la tabla siguiente se enumeran las direcciones URL opcionales a las que pueden tener acceso las máquinas virtuales de Azure:

|Dirección|Puerto TCP de salida|Propósito|Azure Gov|
|---|---|---|---|
|*.microsoftonline.com|443|Autenticación en Microsoft Online Services|login.microsoftonline.us|
|*.events.data.microsoft.com|443|Servicio de telemetría|None|
|www.msftconnecttest.com|443|Detecta si el sistema operativo está conectado a Internet.|None|
|*.prod.do.dsp.mp.microsoft.com|443|Windows Update|None|
|login.windows.net|443|Inicio de sesión en Microsoft Online Services, Microsoft 365|login.microsoftonline.us|
|*.sfx.ms|443|Actualizaciones del software cliente de OneDrive|oneclient.sfx.ms|
|*.digicert.com|443|Comprobación de revocación de certificados|None|
|*.azure-dns.com|443|Resolución de Azure DNS|None|
|*.azure-dns.net|443|Resolución de Azure DNS|None|

>[!NOTE]
>Windows Virtual Desktop actualmente no tiene una lista de intervalos de direcciones IP que puede desbloquear para permitir el tráfico de red. En este momento, solo se pueden desbloquear direcciones URL específicas.
>
>Si usa un firewall de última generación (NGFW), tendrá que usar una lista dinámica específica creada específicamente para las direcciones IP, a fin de asegurarse de que puede conectarse.
>
>Para obtener una lista de direcciones URL seguras relacionadas con Office, incluidas las direcciones URL requeridas relacionadas con Azure Active Directory, consulte [Direcciones URL e intervalos de direcciones IP de Office 365](/office365/enterprise/urls-and-ip-address-ranges).
>
>Debe usar el carácter comodín (*) para las direcciones URL que impliquen tráfico de servicio. Si prefiere no usar el carácter comodín (*) para el tráfico relacionado con el agente, aquí se muestra cómo buscar las direcciones URL sin estos caracteres:
>
>1. Registre las máquinas virtuales en el grupo de hosts de Windows Virtual Desktop.
>2. Abra el **Visor de eventos** y, después, diríjase a **Registros de Windows** > **Aplicación** > **WVD-Agent** y busque el evento con el identificador 3701.
>3. Desbloquee las direcciones URL que se encuentran en el identificador de evento 3701. Las direcciones URL del identificador de evento 3701 son específicas de una región. Deberá repetir el proceso de desbloqueo con las direcciones URL pertinentes de cada región en la que quiera implementar las máquinas virtuales.

## <a name="remote-desktop-clients"></a>Clientes de Escritorio remoto

Todos los clientes de Escritorio remoto que use deben tener acceso a las siguientes direcciones URL:

|Dirección|Puerto TCP de salida|Propósito|Cliente(s)|Azure Gov|
|---|---|---|---|---|
|*.wvd.microsoft.com|443|Tráfico de servicio|All|*.wvd.microsoft.us|
|*.servicebus.windows.net|443|Solución de problemas de los datos|All|*.servicebus.usgovcloudapi.net|
|go.microsoft.com|443|Microsoft FWLinks|All|None|
|aka.ms|443|Acortador de direcciones URL de Microsoft|All|None|
|docs.microsoft.com|443|Documentación|All|None|
|privacy.microsoft.com|443|Declaración de privacidad|All|None|
|query.prod.cms.rt.microsoft.com|443|Actualizaciones de clientes|Escritorio de Windows|Ninguno|

>[!IMPORTANT]
>Para conseguir una experiencia de cliente confiable, es esencial abrir estas direcciones URL. No se admite el bloqueo del acceso a estas direcciones URL y afectará a la funcionalidad del servicio.
>
>Estas direcciones URL solo se corresponden con los sitios de cliente y los recursos. No incluyen direcciones URL para otros servicios como Azure Active Directory. Las direcciones URL de Azure Active Directory se pueden encontrar en el id. 56 en las [direcciones URL e intervalos de direcciones IP de Office 365](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online).