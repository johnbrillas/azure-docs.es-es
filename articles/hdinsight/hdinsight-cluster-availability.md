---
title: Supervisión de la disponibilidad del clúster con Apache Ambari en Azure HDInsight
description: Aprenda a usar Apache Ambari para supervisar la disponibilidad y el estado del clúster.
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/01/2020
ms.openlocfilehash: dcbe44defeb89ad2f67833b263e5f4983070a46c
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863537"
---
# <a name="how-to-monitor-cluster-availability-with-apache-ambari-in-azure-hdinsight"></a>Supervisión de la disponibilidad del clúster con Apache Ambari en Azure HDInsight

Los clústeres de HDInsight incluyen Apache Ambari, que proporciona información de estado de un vistazo y alertas predefinidas.

En este artículo se muestra cómo usar Ambari para supervisar el clúster y le guía a través de algunos ejemplos para configurar una alerta de Ambari, supervisar un índice de disponibilidad de nodo y crear una alerta de Azure Monitor que se desencadena cuando no se recibe ningún latido de uno o varios nodos durante cinco horas.

## <a name="dashboard"></a>Panel

Para acceder al panel de Ambari, seleccione el vínculo **Inicio de Ambari** de la sección **Paneles de clúster** de la hoja de información general de HDInsight en Azure Portal, tal como se muestra a continuación. También puede acceder a él yendo a `https://CLUSTERNAME.azurehdinsight.net` en un explorador en el que CLUSTERNAME es el nombre del clúster.

:::image type="content" source="media/hdinsight-cluster-availability/azure-portal-dashboard-ambari.png" alt-text="Vista de portal de recursos de HDInsight":::

Luego, se le pedirá un nombre de usuario y contraseña de inicio de sesión del clúster. Escriba las credenciales que eligió al crear el clúster.

Se le redirigirá al panel de Ambari, que contiene widgets que muestran una serie de métricas para ofrecerle una vista rápida del estado del clúster de HDInsight. Estos widgets muestran métricas como, por ejemplo, el número de elementos DataNodes (nodos de trabajo) en directo y elementos JournalNodes (nodo de zookeeper), el tiempo de actividad del elemento NameNodes (nodos principales), así como también métricas específicas para determinados tipos de clúster, como el tiempo de actividad de YARN ResourceManager para los clústeres de Spark y Hadoop.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-dashboard.png" alt-text="Pantalla del panel de uso de Apache Ambari":::

## <a name="hosts--view-individual-node-status"></a>Hosts: ver el estado de nodo individual.

También puede ver información de estado de los nodos individuales. Seleccione la pestaña **Hosts** para ver una lista de todos los nodos del clúster y ver información básica sobre cada nodo. La marca de verificación verde a la izquierda de cada nombre de nodo indica que todos los componentes están activos en el nodo. Si un componente está inactivo en un nodo, verá un triángulo de alerta rojo en lugar de la marca de verificación verde.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-hosts1.png" alt-text="Vista de hosts de Apache Ambari para HDInsight":::

Luego, puede seleccionar el **nombre** de un nodo para ver métricas más detalladas del host para ese nodo concreto. En esta vista se muestra la disponibilidad y el estado de cada componente individual.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-hosts-node.png" alt-text="Vista de nodo único de hosts de Apache Ambari":::

## <a name="ambari-alerts"></a>Alertas de Ambari

Ambari también ofrece varias alertas configurables que pueden proporcionar una notificación de determinados eventos. Cuando se desencadenan alertas, se muestran en la esquina superior izquierda de Ambari en una notificación de color rojo que contiene el número de alertas. Al seleccionar esta notificación aparece una lista de las alertas actuales.

:::image type="content" source="media/hdinsight-cluster-availability/apache-ambari-alerts.png" alt-text="Recuento de alertas actuales de Apache Ambari":::

Para ver una lista de definiciones de alertas y sus estados, seleccione la pestaña **Alertas**, tal como se muestra a continuación.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alerts-definitions.png" alt-text="Ver las definiciones de alertas de Ambari":::

Ambari ofrece muchas alertas predefinidas relacionadas con la disponibilidad, entre las que se incluyen las siguientes:

| Nombre de la alerta                        | Descripción   |
|---|---|
| Resumen de estado de DataNode           | Esta alerta de nivel de servicio se desencadena si hay elementos DataNodes en mal estado.|
| Estado de disponibilidad alta de NameNode | Esta alerta de nivel de servicio se desencadena si el elemento NameNode activo o NameNode en espera no está en ejecución.|
| Porcentaje de JournalNodes disponible    | Esta alerta se desencadena si el número de elementos JournalNodes inactivos en el clúster es mayor que el umbral crítico configurado. Agrega los resultados de las comprobaciones del proceso JournalNode. |
| Porcentaje de DataNodes disponible       | Esta alerta se desencadena si el número de elementos DataNodes inactivos en el clúster es mayor que el umbral crítico configurado. Agrega los resultados de las comprobaciones del proceso DataNode.|


Para ver los detalles de una alerta o modificar los criterios, seleccione el **nombre** de la alerta. Tome **Resumen de estado de DataNode** como ejemplo. Puede ver una descripción de la alerta, así como los criterios específicos que desencadenarán una alerta de "advertencia" o "crítica", así como el intervalo de comprobación de los criterios. Para editar la configuración, seleccione el botón **Editar** de la esquina superior derecha del cuadro de configuración.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alert-configuration.png" alt-text="Configuración de alertas de Apache Ambari":::

Aquí, puede editar la descripción y, lo que es más importante, el intervalo de comprobación y los umbrales para las alertas de advertencia o críticas.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png" alt-text="Vista de edición de las configuraciones de alertas de Ambari":::

En este ejemplo, puede hacer que 2 elementos DataNodes en mal estado desencadenen una alerta crítica y 1 elemento DataNode en mal estado desencadenen solo una advertencia. Cuando termine la edición, seleccione **Guardar**.

## <a name="email-notifications"></a>Notificaciones por correo electrónico

Opcionalmente, también puede configurar notificaciones por correo electrónico para las alertas de Ambari. Para ello, en la pestaña **Alertas**, haga clic en el botón **Acciones** de la esquina superior izquierda y seleccione **Administrar notificaciones**.

:::image type="content" source="media/hdinsight-cluster-availability/ambari-manage-notifications.png" alt-text="Acción de administración de las notificaciones de Ambari":::

Se abrirá un cuadro de diálogo para administrar las notificaciones de alerta. Seleccione **+** en la parte inferior del cuadro de diálogo y rellene los campos obligatorios para proporcionar a Ambari los detalles del servidor de correo electrónico desde el que se van a enviar correos electrónicos.

> [!TIP]
> La configuración de notificaciones por correo electrónico de Ambari puede ser una buena manera de recibir alertas en un solo lugar a la hora de administrar muchos clústeres de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

- [Disponibilidad y confiabilidad de clústeres de Apache Hadoop en HDInsight](./hdinsight-business-continuity.md)
- [Disponibilidad del clúster: registros de Azure Monitor](./cluster-availability-monitor-logs.md)
- [Uso de registros de Azure Monitor](hdinsight-hadoop-oms-log-analytics-tutorial.md)
- [Notificaciones por correo electrónico de Apache Ambari](apache-ambari-email.md)
