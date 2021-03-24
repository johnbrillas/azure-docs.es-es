---
title: Acerca de Azure Cloud Services (soporte extendido)
description: Conozca los elementos secundarios del elemento Network Configuration del archivo de configuración del servicio, que especifica valores de Virtual Network y DNS.
ms.topic: article
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: ecf115b7ce902fcd8b50f0eca32ffda6ef47e068
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102618481"
---
# <a name="about-azure-cloud-services-extended-support"></a>Acerca de Azure Cloud Services (soporte extendido)

> [!IMPORTANT]
> Cloud Services (soporte extendido) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cloud Services (soporte extendido) es un nuevo modelo de implementación basado en  [Azure Resource Manager](../azure-resource-manager/management/overview.md) del producto  [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/), y se encuentra actualmente en versión preliminar pública. Cloud Services (soporte extendido) tiene la ventaja principal de proporcionar resistencia regional junto con la paridad de características con Azure Cloud Services implementado mediante Azure Service Manager. También ofrece algunas funcionalidades de ARM como el control de acceso basado en rol (RBAC), etiquetas y directivas, y admite plantillas de implementación.  

Con este cambio, el modelo de implementación basado en Azure Service Manager para Cloud Services pasará a llamarse [Cloud Services (clásico)](../cloud-services/cloud-services-choose-me.md). Conservará la capacidad de compilar e implementar rápidamente sus aplicaciones y servicios web y en la nube. Podrá escalar su infraestructura de servicios en la nube en función de la demanda actual y garantizar que se mantiene el rendimiento de sus aplicaciones al tiempo que se reducen los costes.  

## <a name="what-does-not-change"></a>Qué no cambia 
- El usuario crea el código, define las configuraciones y lo implementa en Azure. Azure configura el entorno de proceso, ejecuta su código y lo supervisa y mantiene automáticamente.
- Cloud Services (soporte extendido) también admite dos tipos de roles: [web y de trabajo](../cloud-services/cloud-services-choose-me.md). No hay ningún cambio en el diseño, la arquitectura o los componentes de los roles web y de trabajo. 
- Los tres componentes de un servicio en la nube —la definición del servicio (.csdef), la configuración del servicio (.cscfg) y el paquete del servicio (.cspkg)— se llevan a cabo y no hay ningún cambio en los [formatos](cloud-services-model-and-package.md). 
- No se requieren cambios en el código del entorno de ejecución, ya que el plano de datos es el mismo y el plano de control solo cambia. 
- Las versiones de Azure GuestOS y las actualizaciones asociadas están alineadas con Cloud Services (clásico).
- El proceso de actualización subyacente con respecto a los dominios de actualización, cómo se aplica la actualización, la reversión y los cambios de servicio permitidos durante una actualización no cambian.

## <a name="changes-in-deployment-model"></a>Cambios en el modelo de implementación

Se requieren cambios mínimos en los archivos de configuración de servicios (.cscfg) y de definición de servicios (.csdef) para implementar Cloud Services (soporte extendido). No se requieren cambios en el código del entorno de ejecución. Sin embargo, los scripts de implementación deberán actualizarse para llamar a las nuevas API basadas en Azure Resource Manager. 

:::image type="content" source="media/overview-image-1.png" alt-text="La imagen muestra la configuración del servicio Cloud Services clásico con la sección de adición de plantilla. ":::

Las principales diferencias entre Cloud Services (clásico) y Cloud Services (soporte extendido) con respecto a la implementación son las siguientes: 

- Las implementaciones de Azure Resource Manager usan [plantillas de ARM](../azure-resource-manager/templates/overview.md), que son un archivo JSON (notación de objetos JavaScript) que define tanto la infraestructura como la configuración de un proyecto. La plantilla usa sintaxis declarativa, lo que permite establecer lo que pretende implementar sin tener que escribir la secuencia de comandos de programación para crearla. El archivo de configuración del servicio y de definición del servicio deben ser coherentes con la [plantilla de ARM](../azure-resource-manager/templates/overview.md) al implementar Cloud Services (soporte extendido). Para ello, se puede [crear manualmente la plantilla de ARM](deploy-template.md) o utilizar [PowerShell](deploy-powershell.md), [Portal](deploy-portal.md) y [Visual Studio](deploy-visual-studio.md).  

- Los clientes deben usar [Azure Key Vault](../key-vault/general/overview.md) para [administrar certificados en Cloud Services (soporte extendido)](certificates-and-key-vault.md). Azure Key Vault le permite almacenar y administrar de forma segura las credenciales de la aplicación, como secretos, claves y certificados, en un repositorio central y seguro en la nube. Asimismo, las aplicaciones pueden autenticarse en Key Vault en tiempo de ejecución para recuperar las credenciales. 

- Todos los recursos implementados a través de [Azure Resource Manager](../azure-resource-manager/templates/overview.md) deben estar dentro de una red virtual. Las redes virtuales y las subredes se crean en Azure Resource Manager con las API de Azure Resource Manager existentes y se debe hacer referencia a ellas en la sección NetworkConfiguration de .cscfg al implementar Cloud Services (soporte extendido).   

- Cada servicio de Cloud Services (soporte extendido) es una única implementación independiente. Cloud Services (soporte extendido) no admite varias ranuras dentro de un único servicio en la nube.  
    - La capacidad de intercambio de VIP<sup>*</sup> se puede utilizar para intercambiar entre dos servicios de Cloud Services (soporte extendido). Para probar y ensayar una nueva versión de un servicio en la nube, implemente un servicio de Cloud Services (soporte extendido) y etiquételo como "con intercambio de VIP" con otro servicio de Cloud Services (soporte extendido)  

- La etiqueta del servicio de nombres de dominio (DNS) es opcional para un servicio de Cloud Services (soporte extendido). En Azure Resource Manager, la etiqueta DNS es una propiedad del recurso de dirección IP pública asociada al servicio en la nube. 


<sup>*</sup> El intercambio de VIP para Cloud Services (soporte extendido) no está disponible durante la versión preliminar pública.  

## <a name="migration-to-azure-resource-manager"></a>Migración a Azure Resource Manager

Cloud Services (soporte extendido) proporciona dos rutas de acceso para migrar desde [Azure Service Manager](/powershell/azure/servicemanagement/overview) a [Azure Resource Manager](../azure-resource-manager/management/overview.md). 
1) Los clientes implementan servicios en la nube directamente en Azure Resource Manager y, a continuación, eliminan el antiguo servicio en la nube en Azure Service Manager. 
2) La migración en contexto permite migrar Cloud Services (clásico) con un tiempo de inactividad mínimo o nulo a Cloud Services (soporte extendido). 

### <a name="additional-migration-options"></a>Opciones de migración adicionales

Al evaluar los planes de migración de Cloud Services (clásico) a Cloud Services (soporte extendido), es posible que desee investigar otros servicios de Azure, como: [Virtual Machine Scale Sets](../virtual-machine-scale-sets/overview.md), [App Service](../app-service/overview.md), [Azure Kubernetes Service](../aks/intro-kubernetes.md) y [Azure Service Fabric](../service-fabric/service-fabric-overview.md). Estos servicios seguirán incorporando funcionalidades adicionales, mientras que Cloud Services (soporte extendido) mantendrá principalmente la paridad de características con Cloud Services (clásico). 

En función de la aplicación, Cloud Services (soporte extendido) puede requerir menos esfuerzo para pasar a Azure Resource Manager en comparación con otras opciones. Si la aplicación no evoluciona, Cloud Services (soporte extendido) es una opción viable para tener en cuenta, ya que proporciona una ruta de migración rápida. Por el contrario, si la aplicación evoluciona continuamente y necesita un conjunto de características más moderno, explore otros servicios de Azure para abordar mejor sus requisitos actuales y futuros. 

## <a name="next-steps"></a>Pasos siguientes
- Revise los [requisitos previos de implementación](deploy-prerequisite.md) de Cloud Services (soporte extendido).
- Implemente un servicio de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).
- Vea las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).