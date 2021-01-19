---
title: Redes de App Service Environment
description: Detalles de las redes de App Service Environment
author: ccompy
ms.assetid: 6f262f63-aef5-4598-88d2-2f2c2f2bfc24
ms.topic: article
ms.date: 11/16/2020
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 680b1f3b6af186eba27a4dd926016a04cd863760
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98013499"
---
# <a name="app-service-environment-networking"></a>Redes de App Service Environment

> [!NOTE]
> Este artículo trata sobre App Service Environment v3 (versión preliminar).
> 

App Service Environment (ASE) es una implementación de inquilino único de Azure App Service que hospeda aplicaciones web, aplicaciones de API y aplicaciones de funciones. Cuando se instala ASE, se elige la instancia de Azure Virtual Network (VNet) en la que desea que se implemente. Toda la aplicación de tráfico de entrada y salida estará dentro de la VNet que especifique.  

ASEv3 usa dos subredes.  Una subred se usa para el punto de conexión privado que controla el tráfico de entrada. Puede ser una subred ya existente o una nueva.  La subred de entrada se puede usar para otros fines diferentes del punto de conexión privado. La subred de salida solo se puede usar para el tráfico de salida desde ASE. No puede haber nada más en la subred de salida de ASE.

## <a name="addresses"></a>Direcciones 
ASE tiene las siguientes direcciones en la creación:

| Tipo de dirección | description |
|--------------|-------------|
| Dirección de entrada | La dirección de entrada es la dirección del punto de conexión privado que ASE usa. |
| Subred de salida | La subred de salida también es la subred de ASE. Durante la versión preliminar, esta subred solo se usa para el tráfico de salida. |
| Dirección de salida de Windows | De forma predeterminada, las aplicaciones de Windows de ASE usarán esta dirección al realizar llamadas de salida a Internet. |
| Dirección de salida de Linux | De forma predeterminada, las aplicaciones de Linux de ASE usarán esta dirección al realizar llamadas de salida a Internet. |

ASEv3 tiene detalles sobre las direcciones que usa el ASE en la parte de **direcciones IP** del portal de ASE.

![Interfaz de usuario de direcciones de ASE](./media/networking/networking-ip-addresses.png)

Si elimina el punto de conexión privado que ASE usa, no podrá acceder a las aplicaciones de ASE.  

ASE usa las direcciones de la subred de salida para admitir la infraestructura usada por ASE. A medida que escale los planes de App Service en su ASE, usará más direcciones. Las aplicaciones de ASE no tienen direcciones dedicadas en la subred de salida. Las direcciones que usa una aplicación de la subred de salida cambiarán con el tiempo.

## <a name="ports"></a>Puertos

ASE recibe el tráfico de la aplicación en los puertos 80 y 443.  No hay requisitos de puerto de entrada o de salida para ASE. 

## <a name="extra-configurations"></a>Configuraciones adicionales

A diferencia de ASEv2, con ASEv3 puede establecer grupos de seguridad de red (NSG) y tablas de enrutamientos (UDR) como considere oportuno y sin restricción. Si desea forzar el túnel para todo el tráfico de salida desde ASE a un dispositivo NVA. Puede colocar dispositivos WAF delante de todo el tráfico de entrada a ASE. 

## <a name="dns"></a>DNS

Las aplicaciones de ASE usarán el DNS con el que está configurada la VNet. Siga las instrucciones que se indican en [Uso de App Service Environment](https://docs.microsoft.com/azure/app-service/environment/using#dns-configuration) para configurar el servidor DNS de modo que apunte a su ASE. Si desea que algunas aplicaciones usen un servidor DNS distinto al que está configurado en la VNet, puede establecerlo manualmente en cada aplicación con las configuraciones de aplicación WEBSITE_DNS_SERVER y WEBSITE_DNS_ALT_SERVER. La configuración de aplicación WEBSITE_DNS_ALT_SERVER configura el servidor DNS secundario. El servidor DNS secundario solo se usa cuando no hay ninguna respuesta del servidor DNS principal. 

## <a name="preview-limitation"></a>Limitación de la versión preliminar

Hay algunas características de red que no están disponibles con ASEv3.  Los elementos que no están disponibles en ASEv3 son:

• FTP • Depuración remota • Implementación del equilibrador de carga externo • Capacidad de acceder a un registro de contenedor privado para implementaciones de contenedores • Capacidad para realizar llamadas a redes virtuales emparejadas globalmente • Capacidad para realizar copias de seguridad o restaurar con una cuenta de almacenamiento protegida de punto de conexión de servicio o punto de conexión privado • Capacidad de hacer referencia a keyvault de la configuración de aplicación en cuentas de keyvault protegidas de punto de conexión de servicio o punto de conexión privado • Capacidad de usar BYOS para una cuenta de almacenamiento protegida de punto de conexión de servicio o punto de conexión privado • Uso de Network Watcher o NSG Flow en el tráfico de salida
    
    

