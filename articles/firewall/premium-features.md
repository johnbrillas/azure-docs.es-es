---
title: Características de la versión preliminar de Azure Firewall Prémium
description: Azure Firewall Prémium es un servicio administrado de seguridad de red basado en la nube que protege los recursos de Microsoft Azure Virtual Network.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/16/2021
ms.author: victorh
ms.openlocfilehash: e823e1efc66592e9f48b7ff5e53a176a4e8cb514
ms.sourcegitcommit: 5a999764e98bd71653ad12918c09def7ecd92cf6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549498"
---
# <a name="azure-firewall-premium-preview-features"></a>Características de la versión preliminar de Azure Firewall Prémium

:::image type="content" source="media/premium-features/icsa-cert-firewall-small.png" alt-text="Logotipo de certificación ICSA" border="false"::: :::image type="content" source="media/premium-features/pci-logo.png" alt-text="Logotipo de certificación PCI" border="false":::


> [!IMPORTANT]
> Azure Firewall Prémium se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

 La versión preliminar de Azure Firewall Prémium es un firewall de próxima generación con funcionalidades que son necesarias en entornos de alta confidencialidad y regulados.

:::image type="content" source="media/premium-features/premium-overview.png" alt-text="Diagrama de información general de Azure Firewall Prémium":::

La versión preliminar de Azure Firewall Prémium usa la directiva de firewall, un recurso global que se puede usar para administrar de forma centralizada los firewalls mediante Azure Firewall Manager. A partir de esta versión, todas las características nuevas se podrán configurar solo mediante la directiva de firewall. Las reglas de firewall (clásicas) se siguen admitiendo y se pueden usar para configurar las características de Firewall Estándar existentes.  La directiva de firewall se puede administrar de forma independiente o con Azure Firewall Manager. Las directivas de firewall asociadas a un solo firewall no tienen ningún cargo.

> [!IMPORTANT]
> Actualmente, la SKU de Firewall Prémium no se admite en las implementaciones de Secure Hub y en las configuraciones de túnel forzado. 

La versión preliminar de Azure Firewall Prémium incluye las siguientes características:

- **Inspección de TLS**: descifra el tráfico saliente, procesa los datos y, luego, los cifra y los envía al destino.
- **IDPS**: sistema de detección y prevención de intrusiones en la red (IDPS) que permite supervisar la actividad malintencionada de la red, registrar información sobre esta actividad, notificarla y, opcionalmente, intentar bloquearla.
- **Filtrado de direcciones URL**: extiende la funcionalidad de filtrado de nombres de dominio completos de Azure Firewall para tener en cuenta una dirección URL completa. Por ejemplo, `www.contoso.com/a/c` en lugar de `www.contoso.com`.
- **Categorías web**: los administradores pueden permitir o denegar el acceso de los usuarios a categorías de sitios web como, por ejemplo, sitios web de apuestas, de redes sociales u otros.

## <a name="features"></a>Características

### <a name="tls-inspection"></a>La inspección de TLS

Azure Firewall Prémium termina las conexiones TLS de salida y horizontales de derecha a izquierda. La inspección de TLS entrante es compatible con [Azure Application Gateway](../web-application-firewall/ag/ag-overview.md) lo cual permite el cifrado completo. Azure Firewall realiza las funciones de seguridad de valor agregado necesarias y vuelve a cifrar el tráfico que se envía al destino original.

> [!TIP]
> TLS 1.0 y 1.1 están en desuso y no tendrán soporte técnico. Se ha detectado que las versiones TLS 1.0 y 1.1 de TLS/Capa de sockets seguros (SSL) son vulnerables y, aunque todavía funcionan para permitir la compatibilidad con versiones anteriores, no se recomiendan. Migre a la versión TLS 1.2 lo antes posible.

Para más información sobre los requisitos de los certificados de la entidad de certificación intermedia de la versión preliminar de Azure Firewall Prémium, consulte [Certificados de la versión preliminar de Azure Firewall Prémium](premium-certificates.md).

### <a name="idps"></a>IDPS

Sistema de detección y prevención de intrusiones en la red (IDPS) que permite supervisar la actividad malintencionada de la red, registrar información sobre esta actividad, notificarla y, opcionalmente, intentar bloquearla. 

La versión preliminar de Azure Firewall Premium proporciona un IDPS basado en firma que permite detectar ataques rápidamente mediante la búsqueda de patrones específicos, como secuencias de bytes en el tráfico de red o las secuencias de instrucciones malintencionadas conocidas que utiliza el malware. Las firmas de IDP son totalmente administradas y se actualizan de forma continua.

IDPS permite detectar ataques en todos los puertos y protocolos para el tráfico no cifrado. Sin embargo, cuando hay que inspeccionar el tráfico HTTPS, Azure Firewall puede usar su funcionalidad de inspección de TLS para descifrar el tráfico y detectar mejor las actividades malintencionadas.  

La lista de omisión de IDPS le permite no filtrar el tráfico a ninguna de las direcciones IP, los intervalos y las subredes especificados en la lista de omisión.  

### <a name="url-filtering"></a>Filtrado para direcciones URL

El filtrado de direcciones URL extiende la funcionalidad de filtrado de nombres de dominio completos de Azure Firewall para que tenga en cuenta direcciones URL completas. Por ejemplo, `www.contoso.com/a/c` en lugar de `www.contoso.com`.  

El filtrado de direcciones URL se puede aplicar al tráfico HTTP y HTTPS. Cuando se inspecciona el tráfico HTTPS, la versión preliminar de Azure Firewall Prémium puede usar su funcionalidad de inspección de TLS para descifrar el tráfico y extraer la dirección URL de destino para validar si se permite el acceso. La inspección de TLS requiere participación en el nivel de reglas de la aplicación. Una vez habilitada, puede usar las direcciones URL para filtrar con HTTPS. 

### <a name="web-categories"></a>Categorías web

Las categorías web hacen que los administradores puedan permitir o denegar el acceso de los usuarios a categorías de sitios web como, por ejemplo, sitios web de apuestas, de redes sociales u otros. Las categorías web también se incluirán en Azure Firewall Estándar, pero estarán más perfeccionadas en la versión preliminar de Azure Firewall Prémium. Frente a la funcionalidad de categorías web de la SKU Estándar que coincide con la categoría basada en un nombre de dominio completo, la SKU Prémium coincide con la categoría según la dirección URL completa tanto para el tráfico HTTP como para el HTTPS. 

Por ejemplo, si Azure Firewall intercepta una solicitud HTTPS para `www.google.com/news`, se espera la siguiente categorización: 

- Firewall Estándar: solo se examinará la parte del nombre de dominio completo, por lo que `www.google.com` se clasificará como *Motor de búsqueda*. 

- Firewall Prémium: se examinará la dirección URL completa, por lo que se `www.google.com/news` clasificará como *Noticias*.

Las categorías se organizan en función de la gravedad en **Responsabilidad**, **Ancho de banda alto**, **Uso empresarial**, **Pérdida de productividad**, **Navegación general** y **Sin categoría**.

#### <a name="category-exceptions"></a>Excepciones de las categorías

Puede crear excepciones a las reglas de la categoría web. Cree una colección de reglas de permiso o denegación independiente con una prioridad más alta dentro del grupo de recopilación de reglas. Por ejemplo, puede configurar una colección de reglas que permita `www.linkedin.com` con prioridad 100, con una colección de reglas que deniegue las **redes sociales** con prioridad 200. Esto crea la excepción para la categoría web **Redes sociales** predefinida. 

## <a name="known-issues"></a>Problemas conocidos

La versión preliminar de Azure Firewall Prémium presenta los siguientes problemas conocidos:

|Problema  |Descripción  |Mitigación  |
|---------|---------|---------|
|Inspección de TLS solo se admite en el puerto estándar HTTPS|Inspección de TLS solo admite HTTPS/443. |Ninguno. Los restantes puertos se admitirán durante la fase de disponibilidad general.|
|Compatibilidad de ESNI con la resolución de nombres de dominio completo en HTTPS|Indicación de nombre de servidor cifrada no se admite en el protocolo de enlace HTTPS.|Actualmente, solo Firefox admite ESNI a través de la configuración personalizada. La solución que se recomienda es deshabilitar esta característica.|
|Certificados de cliente (TLS)|Los certificados de cliente se usan para crear una confianza de identidad mutua entre el cliente y el servidor. Los certificados de cliente se usan durante una negociación de TLS. Azure Firewall vuelve a negociar una conexión con el servidor y no tiene acceso a la clave privada de los certificados de cliente.|None|
|QUIC/HTTP3|QUIC es la nueva versión principal de HTTP. Es un protocolo basado en UDP a través de los puertos 80 (PLAN) y 443 (SSL). No se admitirá la inspección de FQDN/URL/TLS.|Configure el uso de UDP 80/443 como reglas de red.|
|Secure Hub y la tunelización forzada no se admiten en la versión Prémium|Actualmente, la SKU de Firewall Prémium no se admite en las implementaciones de Secure Hub ni en las configuraciones de túnel forzado.|Corrección programada para la fase de disponibilidad general.|
Certificados firmados por clientes que no son de confianza|El firewall no confía en los certificados firmados por el cliente una vez que se han recibido de un servidor web basado en intranet.|Corrección programada para la fase de disponibilidad general.
|Direcciones IP de origen y destino erróneas en alertas de IDP con inspección de TLS.|Cuando se habilita la inspección de TLS e IDPS emite una alerta nueva, la dirección IP de origen o destino que se muestra es incorrecta (se muestra la dirección IP interna, en lugar de la dirección IP original).|Corrección programada para la fase de disponibilidad general.|
|Dirección IP de origen incorrecta en las alertas con IDPS para HTTP (sin inspección de TLS).|Cuando el tráfico HTTP de texto sin formato está en uso e IDPS emite una nueva alerta y el destino es una dirección IP pública, la dirección IP de origen que se muestra es incorrecta (se muestra la dirección IP interna, en lugar de la dirección IP original).|Corrección programada para la fase de disponibilidad general.|
|Propagación de certificados|Cuando se aplica un certificado de entidad de certificación en el firewall, puede tardar entre 5 y 10 minutos en surtir efecto.|Corrección programada para la fase de disponibilidad general.|
|Omisión de IDPS|La omisión de IDPS no funciona para el tráfico terminado por TLS y no se admiten la dirección IP de origen ni los grupos de IP de origen.|Corrección programada para la fase de disponibilidad general.|




## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre los certificados de Azure Firewall Prémium](premium-certificates.md)
- [Implementación y configuración de la versión preliminar de Azure Firewall Prémium](premium-deploy.md)
- [Migración a la versión preliminar de Azure Firewall Prémium](premium-migrate.md)
