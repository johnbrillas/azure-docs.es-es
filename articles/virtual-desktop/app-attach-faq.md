---
title: 'Preguntas frecuentes sobre la asociación de aplicaciones en formato MSIX de Windows Virtual Desktop: Azure'
description: Preguntas más frecuentes sobre la asociación de aplicaciones en formato MSIX para Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 395c274630131c2ae5f451443913e1e69c7c422a
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101738708"
---
# <a name="msix-app-attach-faq"></a>Preguntas frecuentes sobre la asociación de aplicaciones en formato MSIX

En este artículo se responden las preguntas más frecuentes sobre la asociación de aplicaciones en formato MSIX para Windows Virtual Desktop.

## <a name="whats-the-difference-between-msix-and-msix-app-attach"></a>¿Cuál es la diferencia entre MSIX y la conexión de aplicaciones en formato MSIX ?

MSIX es un formato de empaquetado de aplicaciones, mientras que la conexión de aplicaciones en formato MSIX es la característica que proporciona paquetes MSIX a la implementación.

## <a name="does-msix-app-attach-use-fslogix"></a>¿La asociación de aplicaciones en formato MSIX usa FSLogix?

La asociación de aplicaciones en formato MSIX no utiliza FSLogix. Sin embargo, la asociación de aplicaciones en formato MSIX y FSLogix están diseñados para trabajar conjuntamente con el fin de proporcionar una experiencia de usuario sin problemas.

## <a name="can-i-use-the-msix-app-attach-outside-of-windows-virtual-desktop"></a>¿Puedo usar la asociación de aplicaciones con formato MSIX fuera de Windows Virtual Desktop?

Las API que permiten el funcionamiento de la conexión de aplicaciones en formato MSIX están disponibles para Windows 10 Enterprise. Estas se pueden usar fuera de Windows Virtual Desktop. Sin embargo, no hay ningún plano de administración para la asociación de aplicaciones en formato MSIX fuera de Windows Virtual Desktop.

## <a name="how-do-i-get-an-msix-package"></a>¿Cómo se obtiene un paquete MSIX?

Su proveedor de software le proporcionará un paquete MSIX. También puede convertir paquetes que no sean MSIX en MSIX. Obtenga más información en [Cómo trasladar los instaladores existentes a MSIX](/windows/msix/packaging-tool/create-an-msix-overview#how-to-move-your-existing-installers-to-msix).

## <a name="which-operating-systems-support-msix-app-attach"></a>¿Qué sistemas operativos admiten la asociación de aplicaciones en formato MSIX?

Windows 10 Enterprise y sesión múltiple de Windows 10 Enterprise, versión 2004 o posterior

## <a name="is-msix-app-attach-currently-generally-available"></a>¿La conexión de aplicaciones en formato MSIX está disponible con carácter general actualmente?

La conexión de aplicaciones en formato MSIX forma parte de Windows 10 Enterprise y la sesión múltiple de Windows 10 Enterprise, versión 2004 o una posterior. Ambos sistemas operativos están disponibles actualmente con carácter general. 

## <a name="can-i-use-msix-app-attach-outside-of-windows-virtual-desktop"></a>¿Puedo usar la asociación de aplicaciones con formato MSIX fuera de Windows Virtual Desktop?

Las API de la conexión de aplicaciones en formato MSIX y MSIX forman parte de Windows 10 Enterprise y la sesión múltiple de Windows 10 Enterprise, versión 2004 y las posteriores. Actualmente, no ofrecemos ningún software de administración para la conexión de aplicaciones en formato MSIX fuera de Windows Virtual Desktop.

## <a name="can-i-run-two-versions-of-the-same-application-at-the-same-time"></a>¿Puedo ejecutar dos versiones de la misma aplicación al mismo tiempo?

Para que dos versiones de las mismas aplicaciones MSIX se ejecuten simultáneamente, la familia de paquetes MSIX definida en el archivo appxmanifest.xml debe ser diferente para cada aplicación.

## <a name="should-i-disable-auto-update-when-using-msix-app-attach"></a>¿Debo deshabilitar la actualización automática cuando uso la conexión de aplicaciones en formato MSIX?

Sí. La conexión de aplicaciones en formato MSIX no admite la actualización automática para aplicaciones de MSIX.

## <a name="how-do-permissions-work-with-msix-app-attach"></a>¿Cómo funcionan los permisos con la conexión de aplicaciones en formato MSIX?

Todas las máquinas virtuales (VM) de un grupo de hosts que usa la conexión de aplicaciones en formato MSIX deben tener permisos de lectura en el recurso compartido de archivos en el que se almacenan las imágenes de MSIX. Si también usa Azure Files, deberán tener permisos de control de acceso basado en rol (RBAC) y de New Technology File System (NTFS).

## <a name="can-i-use-msix-app-attach-for-http-or-https"></a>¿Puedo usar la conexión de aplicaciones en formato MSIX para HTTP o HTTPS?

Actualmente, no se admite la conexión de aplicaciones en formato MSIX a través de HTTP o HTTPS.

## <a name="can-i-restage-the-same-msix-application"></a>¿Puedo realizar una copia intermedia de la misma aplicación MSIX?

Sí. Puede realizar una copia intermedia de las aplicaciones para las que ya la ha hecho y esto no debería producir ningún error.

## <a name="does-msix-app-attach-support-self-signed-certificates"></a>¿La conexión de aplicaciones en formato MSIX admite certificados autofirmados?

Sí. Debe instalar el certificado autofirmado en todas las máquinas virtuales de host de sesión donde se usa la asociación de aplicaciones en formato MSIX para hospedar la aplicación autofirmada.


## <a name="next-steps"></a>Pasos siguientes

Si quiere obtener más información sobre la conexión de aplicaciones en formato MSIX, consulte nuestra [introducción](what-is-app-attach.md) y [glosario](app-attach-glossary.md). De lo contrario, empiece a trabajar con [Configuración de la conexión de aplicaciones](app-attach.md).
