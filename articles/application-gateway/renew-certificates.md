---
title: Renovar un certificado de Azure Application Gateway
description: Obtenga información sobre cómo renovar un certificado asociado con un agente de escucha de Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: how-to
ms.date: 01/20/2021
ms.author: victorh
ms.openlocfilehash: f0c06a94498f4d2481a6e953b959d766c60415fb
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98622187"
---
# <a name="renew-application-gateway-certificates"></a>Renovar certificados de Application Gateway

En algún momento, tendrá que renovar los certificados si ha configurado Application Gateway para el cifrado TLS/SSL.

Puede renovar un certificado asociado con un agente de escucha a través de Azure Portal, Azure PowerShell o la CLI de Azure:

## <a name="azure-portal"></a>Portal de Azure

Para renovar un certificado de agente de escucha desde el portal, vaya a los agentes de escucha de Application Gateway. Seleccione el cliente de escucha que tiene un certificado que debe renovarse y, a continuación, seleccione **Renovar o editar el certificado seleccionado**.

:::image type="content" source="media/renew-certificate/ssl-cert.png" alt-text="Renovar un certificado":::

Cargue el nuevo certificado PFX, asígnele un nombre, escriba la contraseña y, a continuación, seleccione **Guardar**.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para renovar el certificado con Azure PowerShell, use el siguiente script:

```azurepowershell-interactive
$appgw = Get-AzApplicationGateway `
  -ResourceGroupName <ResourceGroup> `
  -Name <AppGatewayName>

$password = ConvertTo-SecureString `
  -String "<password>" `
  -Force `
  -AsPlainText

set-AzApplicationGatewaySSLCertificate -Name <oldcertname> `
-ApplicationGateway $appgw -CertificateFile <newcertPath> -Password $password

Set-AzApplicationGateway -ApplicationGateway $appgw
```
## <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
az network application-gateway ssl-cert update \
  -n "<CertName>" \
  --gateway-name "<AppGatewayName>" \
  -g "ResourceGroupName>" \
  --cert-file <PathToCerFile> \
  --cert-password "<password>"
```

## <a name="next-steps"></a>Pasos siguientes

Para aprender a configurar la descarga de TLS con Azure Application Gateway, consulte [Configuración de la descarga TLS](./create-ssl-portal.md).