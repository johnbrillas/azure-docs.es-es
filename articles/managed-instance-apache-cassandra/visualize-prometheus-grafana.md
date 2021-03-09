---
title: Configuración de Grafana para visualizar las métricas emitidas desde Azure Managed Instance for Apache Cassandra
description: Aprenda a instalar y configurar Grafana en una máquina virtual para visualizar las métricas emitidas desde un clúster de Azure Managed Instance for Apache Cassandra.
author: TheovanKraay
ms.service: managed-instance-apache-cassandra
ms.topic: how-to
ms.date: 03/02/2021
ms.author: thvankra
ms.openlocfilehash: ed0ff343595429a4cb81fef280203f1180eeb098
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101743798"
---
# <a name="configure-grafana-to-visualize-metrics-emitted-from-the-managed-instance-cluster"></a>Configuración de Grafana para visualizar las métricas emitidas desde un clúster de instancia administrada

> [!IMPORTANT]
> Azure Managed Instance for Apache Cassandra se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cuando se implementa un clúster de Azure Managed Instance for Apache Cassandra, el servicio aprovisiona un servidor que hospeda [Prometheus](https://prometheus.io/), que se puede usar desde varias herramientas de cliente. Prometheus es una solución de supervisión de código abierto. La instancia administrada emitirá métricas y conservará 10 minutos o 10 GB de datos (el umbral que se alcance primero). En este artículo se describe cómo configurar Grafana para visualizar las métricas emitidas desde el clúster de Managed Instance. Son necesarias las siguientes tareas para visualizar las métricas:

* Implementar una máquina virtual con Ubuntu dentro de la red virtual de Azure donde esté presente la instancia administrada.
* Instalar la [herramienta Grafana](https://grafana.com/grafana/) de código abierto para crear paneles y visualizar las métricas emitidas desde Prometheus.

## <a name="deploy-a-ubuntu-server"></a>Implementación de un servidor Ubuntu

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. Vaya al grupo de recursos donde se encuentra el clúster de Managed Instance. Seleccione **Agregar** y busque la imagen **Ubuntu Server 18.04 LTS**:

   :::image type="content" source="./media/visualize-prometheus-grafana/select-ubuntu-image.png" alt-text="Busque la imagen de Ubuntu Server en Azure Portal." border="true":::

1. Elija la imagen y seleccione **Crear**.

1. En la hoja **Crear una máquina virtual**, escriba los valores de los campos siguientes; puede dejar los valores predeterminados para los demás campos:

   * **Nombre de la máquina virtual**: escriba un nombre para la máquina virtual.
   * **Región**: seleccione la misma región en la que se ha implementado la red virtual.

   :::image type="content" source="./media/visualize-prometheus-grafana/create-vm-ubuntu.png" alt-text="Rellene el formulario para crear una máquina virtual con la imagen de Ubuntu Server." border="true":::

1. En la pestaña **Redes**, seleccione la red virtual en la que se ha implementado la instancia administrada:

   :::image type="content" source="./media/visualize-prometheus-grafana/configure-networking-details.png" alt-text="Configure las opciones de red del servidor Ubuntu." border="true":::

1. Por último, seleccione **Revisar y crear** para crear el servidor de Grafana.

## <a name="install-grafana"></a>Instalación de Grafana

1. En Azure Portal, abra la red virtual en la que ha implementado la instancia administrada y el servidor Grafana. Debería ver una instancia de conjunto de escalado de máquinas virtuales llamado **cassandra-jump (instance 0)** . Estas métricas de Prometheus se hospedan en este conjunto de escalado de máquinas virtuales. Anote la dirección IP de esta instancia:

   :::image type="content" source="./media/visualize-prometheus-grafana/prometheus-instance-address.png" alt-text="Obtención de la dirección IP de la instancia de Prometheus." border="true":::

1. Conéctese al servidor Ubuntu recién creado con la [CLI de Azure](../virtual-machines/linux/ssh-from-windows.md#ssh-clients) o la herramienta cliente preferida para conectarse mediante SSH.

1. Después de conectarse a la máquina virtual, debe instalar y configurar Grafana para conectarse al conjunto de escalado de máquinas virtuales donde se hospedan las métricas. Abra un símbolo del sistema y escriba el comando `nano` para abrir el editor de texto Nano. Pegue el siguiente script en el editor de texto y asegúrese de reemplazar `<prometheus IP address>` por la dirección IP que anotó en el paso anterior:

   ```bash
   #!/bin/bash
   
   echo "Installing Grafana..."
   
   if ! $SSH dpkg -s grafana prometheus > /dev/null; then
       echo "Installing packages."
       echo 'deb https://packages.grafana.com/oss/deb stable main' | $SSH sudo tee /etc/apt/sources.list.d/grafana.list > /dev/null
       curl https://packages.grafana.com/gpg.key | $SSH sudo apt-key add -
       $SSH sudo apt-get update
       $SSH sudo apt-get install -y grafana prometheus
   else
       echo "Skipping package installation"
   fi
   
   echo "Configuring grafana"
   cat <<EOF | $SSH sudo tee /etc/grafana/provisioning/datasources/prometheus.yml
   apiVersion: 1
   datasources:
     - name: Prometheus
       type: prometheus
       url: https://<prometheus IP address>:9443
       jsonData:
         tlsSkipVerify: true
   EOF
   
   echo "Restarting Grafana"
   $SSH sudo systemctl enable grafana-server
   $SSH sudo systemctl restart grafana-server
   
   echo "Installing Grafana plugins"
   $SSH sudo grafana-cli plugins install natel-discrete-panel
   $SSH sudo grafana-cli plugins install grafana-polystat-panel
   $SSH sudo systemctl restart grafana-server
   ```

1. Escriba `ctrl + X` para guardar el archivo. Puede asignar al archivo el nombre `grafana.sh`.

1. Escriba el comando `./grafana.sh` en el símbolo del sistema para instalar Grafana.

1. Una vez completada la instalación, Grafana estará disponible en el **puerto 3000** en la dirección IP del servidor, tal y como se muestra en la siguiente captura de pantalla:

   :::image type="content" source="./media/visualize-prometheus-grafana/open-grafana-port.png" alt-text="Ejecución de Grafana en el puerto 3000." border="true":::

1. Puede elegir entre los paneles de código abierto creados para Apache Cassandra en Grafana, como el archivo JSON [cluster-overview](https://github.com/TheovanKraay/cassandra-exporter/blob/master/grafana/instaclustr/cluster-overview.json). Descargue e importe la definición JSON del panel en Grafana:

   :::image type="content" source="./media/visualize-prometheus-grafana/grafana-import.png" alt-text="Importación de la definición JSON de Grafana." border="true":::

   :::image type="content" source="./media/visualize-prometheus-grafana/grafana-upload-json.png" alt-text="Carga de la definición JSON de Grafana." border="true":::

1. Después, puede supervisar el clúster de la instancia administrada de Cassandra con el panel elegido:

   :::image type="content" source="./media/visualize-prometheus-grafana/monitor-cassandra-metrics.gif" alt-text="Visualización de las métricas de la instancia administrada de Cassandra en el panel." border="true":::

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a configurar los paneles para visualizar las métricas de Prometheus con Grafana. Más información sobre Azure Managed Instance for Apache Cassandra en los siguientes artículos:

* [¿Qué es Azure Managed Instance for Apache Cassandra? (versión preliminar)](introduction.md)
* [Implementación de un clúster administrado de Apache Spark (versión preliminar) con Azure Databricks](deploy-cluster-databricks.md)
