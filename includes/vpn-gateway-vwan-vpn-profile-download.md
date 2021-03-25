---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/08/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f3eb2d9469ab3a3d2c1d09e4adc3ee2cb1f86e6e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99979080"
---
## <a name="extract-the-zip-file"></a>Extraiga el archivo ZIP

Extraiga el archivo ZIP. El archivo contiene las siguientes carpetas:

* AzureVPN
* Genérico
* OpenVPN (si ha habilitado la **configuración de autenticación** de OpenVPN y **certificado de Azure** en la puerta de enlace). Seleccione el artículo adecuado correspondiente a su configuración para crear un inquilino.

  * [VPN Gateway: creación de un inquilino](../articles/vpn-gateway/openvpn-azure-ad-tenant.md).
  * [Virtual WAN: creación de un inquilino](../articles/virtual-wan/openvpn-azure-ad-tenant.md).

## <a name="retrieve-information"></a>Recuperar información

En la carpeta **AzureVPN**, desplácese hasta el archivo **_azurevpnconfig.xml_** y ábralo con el Bloc de notas. Tome nota del texto entre las etiquetas siguientes.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Detalles de perfil

Cuando agregue una conexión, use la información que recopiló en el paso anterior para la página de detalles del perfil. Los campos corresponden con la siguiente información:

* **Audiencia:** Identifica el recurso de destinatario al que está destinado el token.
* **Emisor:** identifica el servicio de token de seguridad (STS) que emitió el token, así como el inquilino de Azure AD.
* **Inquilino:** Contiene un identificador único e inmutable del inquilino de directorio que emitió el token.
* **FQDN:** nombre de dominio completo (FQDN) en Azure VPN Gateway.
* **ServerSecret:** la clave previamente compartida de VPN Gateway.

## <a name="folder-contents"></a>Contenido de la carpeta

* La **carpeta genérica** contiene el certificado de servidor público y el archivo VpnSettings.xml. El archivo VpnSettings.xml contiene la información necesaria para configurar un cliente genérico.

* El archivo zip descargado también puede contener carpetas **WindowsAmd64** y **WindowsX86**. Estas carpetas contienen el instalador de SSTP y IKEv2 para clientes de Windows. Necesita derechos de administrador en el cliente para instalarlos.
