---
title: Impacto en el rendimiento de Kerberos en los volúmenes Azure NetApp Files NFS v4.1 | Microsoft Docs
description: Describe las opciones de seguridad disponibles, los vectores de rendimiento probados y el impacto de rendimiento esperado de Kerberos en los volúmenes NFS v4.1 de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: b-juche
ms.openlocfilehash: e9054f11c8f55e9fe00266840a9f6e257f14e659
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101744061"
---
# <a name="performance-impact-of-kerberos-on-azure-netapp-files-nfsv41-volumes"></a>Impacto en el rendimiento de Kerberos en los volúmenes NFS v4.1 de Azure NetApp Files

Azure NetApp Files admite el [cifrado de cliente NFS en los modos de Kerberos](configure-kerberos-encryption.md) (krb5, krb5i y krb5p) con el cifrado AES-256. En este artículo se describe el impacto en el rendimiento de Kerberos en los volúmenes de NFS v4.1. 

## <a name="available-security-options"></a>Opciones de seguridad disponibles 

Las opciones de seguridad disponibles actualmente para los volúmenes de NFSv4.1 son las siguientes: 

* **sec=sys** usa UID y GID de UNIX locales mediante AUTH_SYS para autenticar las operaciones de NFS.
* **sec=krb5** usa la versión 5 de Kerberos en lugar de los UID y GID de UNIX locales para autenticar a los usuarios.
* **sec=krb5i** usa la versión 5 de Kerberos para la autenticación de usuarios y realiza la comprobación de la integridad de las operaciones de NFS con sumas de comprobación seguras para evitar la manipulación de datos.
* **sec=krb5p** usa la versión 5 de Kerberos para la autenticación de usuarios y la comprobación de la integridad. Cifra el tráfico de NFS para evitar el examen del tráfico. Esta opción es la más segura, pero también implica la mayor sobrecarga en el rendimiento.

## <a name="performance-vectors-tested"></a>Vectores de rendimiento probados

En esta sección se describe el impacto en el rendimiento del cliente único de las diversas opciones de `sec=*`.

* El impacto en el rendimiento se probó en dos niveles: baja simultaneidad (baja carga) y alta simultaneidad (límites superiores de E/S y rendimiento).  
* Se probaron tres tipos de cargas de trabajo:  
    * Lectura/escritura aleatoria de operación pequeña (mediante FIO)
    * Lectura/escritura secuencial de operación grande (mediante FIO)
    * Carga de trabajo pesada de metadatos generada por aplicaciones como Git

## <a name="expected-performance-impact"></a>Impacto en el rendimiento esperado 

Hay dos áreas de enfoque: carga ligera y límite superior. En las listas siguientes se describe la configuración de seguridad del impacto de rendimiento por configuración de seguridad y escenario por escenario. Todas las comparaciones se realizan en relación con el parámetro de seguridad `sec=sys`. La prueba se realizó en un único volumen con un solo cliente. 

Impacto sobre el rendimiento de krb5:

* Simultaneidad baja (l/e):
    * Aumento de latencia secuencial de 0,3 ms.
    * Aumento de latencia de E/S aleatoria de 0,2 ms.
    * Aumento de la latencia de E/S de metadatos de 0,2 ms.
* Simultaneidad alta (l/e): 
    * El rendimiento secuencial máximo no se ve afectado por krb5.
    * La E/S aleatoria máxima se redujo un 30 % en las cargas de trabajo de lectura puras con una reducción a cero del impacto total, a medida que la carga de trabajo cambia a escritura pura. 
    * La carga de trabajo máxima de metadatos se redujo un 30 %.

Impacto sobre el rendimiento de krb5i: 

* Simultaneidad baja (l/e):
    * Aumento de latencia secuencial de 0,5 ms.
    * Aumento de latencia de E/S aleatoria de 0,2 ms.
    * Aumento de la latencia de E/S de metadatos de 0,2 ms.
* Simultaneidad alta (l/e): 
    * El rendimiento secuencial máximo disminuyó un 70 % en general, independientemente de la combinación de la carga de trabajo.
    * La E/S aleatoria máxima se redujo un 50 % en las cargas de trabajo de lectura puras con una reducción al 25 % del impacto total, a medida que la carga de trabajo cambia a escritura pura. 
    * La carga de trabajo máxima de metadatos se redujo un 30 %.

Impacto sobre el rendimiento de krb5p:

* Simultaneidad baja (l/e):
    * Aumento de latencia secuencial de 0,8 ms.
    * Aumento de latencia de E/S aleatoria de 0,2 ms.
    * Aumento de la latencia de E/S de metadatos de 0,2 ms.
* Simultaneidad alta (l/e): 
    * El rendimiento secuencial máximo disminuyó un 85 % en general, independientemente de la combinación de la carga de trabajo. 
    * La E/S aleatoria máxima se redujo un 65 % en las cargas de trabajo de lectura puras con una reducción al 43 % del impacto total, a medida que la carga de trabajo cambia a escritura pura. 
    * La carga de trabajo máxima de metadatos se redujo un 30 %.

## <a name="next-steps"></a>Pasos siguientes  

* [Configuración del cifrado Kerberos de NFSv4.1 para Azure NetApp Files](configure-kerberos-encryption.md) 
