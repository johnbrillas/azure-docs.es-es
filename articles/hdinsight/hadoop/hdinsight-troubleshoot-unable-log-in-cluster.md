---
title: No se puede registrar en el clúster de Azure HDInsight
description: Solución de los problemas por los que no se puede iniciar sesión en el clúster de Apache Hadoop en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 07/31/2019
ms.openlocfilehash: 03657606f7e24436ff5a851fe7eef1e0c4d5dc24
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "98944317"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Escenario: No se puede registrar en el clúster de Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede registrar en el clúster de Azure HDInsight.

## <a name="cause"></a>Causa

Los motivos pueden variar. Al iniciar sesión en los paneles de clúster o la aplicación, recuerde usar el "inicio de sesión de clúster" o las credenciales de HTTP. Cuando se conecte remotamente, use sus credenciales de Shell seguro (SSH) o del Escritorio remoto.

## <a name="resolution"></a>Solución

Para resolver problemas habituales, pruebe uno o varios de los pasos siguientes.

* Intente abrir el panel del clúster en una nueva pestaña del explorador en modo de privacidad.

* Para los clústeres Linux, si no puede recuperar las credenciales de SSH, puede [restablecer las credenciales en la interfaz de usuario de Ambari](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]