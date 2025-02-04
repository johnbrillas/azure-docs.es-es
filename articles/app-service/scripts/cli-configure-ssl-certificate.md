---
title: 'CLI: Carga y enlace de un certificado TLS/SSL a una aplicación'
description: Aprenda a usar la CLI de Azure para automatizar la implementación y administración de la aplicación App Service. En este ejemplo se indica cómo enlazar un certificado TLS/SSL personalizado a una aplicación.
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 1d03018cca9757c8a6ddddbff96738a407bba7d4
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97006402"
---
# <a name="bind-a-custom-tlsssl-certificate-to-an-app-service-app-using-cli"></a>Enlace de un certificado TLS/SSL personalizado a una aplicación de App Service mediante la CLI

Este script de ejemplo crea una aplicación en App Service con sus recursos relacionados y, luego, enlaza a ella el certificado TLS/SSL de un nombre de dominio personalizado. Para este ejemplo, se necesita:

* Acceso a la página de configuración DNS de su registrador de dominios.
* Un archivo PFX válido y su contraseña para el certificado TLS/SSL que desea cargar y enlazar.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, necesitará la CLI de Azure versión 2.0 o posterior. Para encontrar la versión, ejecute `az --version`. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to an app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [`az group create`](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create) | Crea un plan de App Service, |
| [`az webapp create`](/cli/azure/webapp#az-webapp-create) | Crea una aplicación de App Service. |
| [`az webapp config hostname add`](/cli/azure/webapp/config/hostname#az-webapp-config-hostname-add) | Asigna un dominio personalizado a una aplicación de App Service. |
| [`az webapp config ssl upload`](/cli/azure/webapp/config/ssl#az-webapp-config-ssl-upload) | Carga un certificado TLS/SSL en una aplicación de App Service. |
| [`az webapp config ssl bind`](/cli/azure/webapp/config/ssl#az-webapp-config-ssl-bind) | Enlaza un certificado TLS/SSL cargado a una aplicación de App Service. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Puede encontrar ejemplos de script adicionales de la CLI de App Service en la [documentación de Azure App Service](../samples-cli.md).
