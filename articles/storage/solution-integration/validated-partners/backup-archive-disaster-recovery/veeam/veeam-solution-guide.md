---
title: Copia de seguridad de los datos en Azure con Veeam
titleSuffix: Azure Blob Storage Docs
description: La página web proporciona información general de los factores a tener en cuenta y los pasos a seguir para aprovechar Azure como destino de almacenamiento y ubicación de recuperación para las copias de seguridad y la recuperación de Veeam.
keywords: Veeam, copia de seguridad en la nube, copia de seguridad, copia de seguridad en Azure, recuperación ante desastres, continuidad del negocio
author: karauten
ms.author: karauten
ms.date: 11/11/2020
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: 6d4b005a3f9c79ff14f5ba4053dc651c1ede56e0
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102124438"
---
# <a name="backup-to-azure-with-veeam"></a>Copia de seguridad en Azure con Veeam

En este artículo se proporciona una guía para la integración de una infraestructura de Veeam en Azure Blob Storage. Incluye los requisitos previos, los principios de Azure Storage e instrucciones de implementación y funcionamiento. En este artículo solo se aborda el uso de Azure como destino de copia de seguridad fuera del sitio y sitio de recuperación en caso de desastre, lo que impide el funcionamiento normal en el sitio principal. Veeam también ofrece una solución con un RTO menor, la replicación de Veeam, como medio para tener una VM en espera lista para arrancar y recuperarse más rápidamente en caso de que se produzca un desastre y para la protección de los recursos en un entorno de producción de Azure. Veeam también ofrece herramientas para realizar copias de seguridad de los recursos de Azure y Office 365. Estas funcionalidades quedan fuera del ámbito de este documento. 

## <a name="reference-architecture-for-on-premises-to-azure-and-in-azure-deployments"></a>Arquitectura de referencia para implementaciones del entorno local a Azure y dentro de Azure

![Arquitectura de referencia de Veeam a Azure](../media/veeam-architecture.png)

La implementación de Veeam existente se puede integrar fácilmente en Azure si se agregan una o varias cuentas de Azure Storage como repositorio de copia de seguridad en la nube. Veeam también le permite recuperar copias de seguridad desde el entorno local en Azure, lo que le ofrece un sitio de recuperación a petición en Azure.

## <a name="veeam-interoperability-matrix"></a>Matriz de interoperabilidad de Veeam
| Carga de trabajo | GPv2 y Blob Storage | Compatibilidad con el nivel de nivel esporádico | Compatibilidad con el nivel de archivo | Compatibilidad con la familia de Data Box |
|-----------------------|--------------------|--------------------|-------------------------|-------------------------|
| Datos y máquinas virtuales locales | v10a | v10a | N/D | 10a* |
| Máquinas virtuales de Azure | v10a | v10a | N/D | 10a* |
| Blob de Azure | v10a | v10a | N/D | 10a* |
| Azure Files | v10a | v10a | N/D | 10a* | 

> [!Note]
Veeam Backup and Replication solo admite la API REST para Azure Data Box, por lo tanto, Azure Data Box Disk no se admite. Se espera compatibilidad con el nivel de archivo de Azure Blob Storage en la versión 11 de Veeam.

## <a name="before-you-begin"></a>Antes de empezar

Con un poco de planificación previa se asegurará de que se une a los muchos clientes satisfechos que usan Azure como destino externo de copia de seguridad y sitio de recuperación.

### <a name="are-you-new-to-azure"></a>¿Es nuevo en Azure?

Microsoft ofrece un marco para empezar a trabajar con Azure. [Cloud Adoption Framework](https://docs.microsoft.com/azure/architecture/cloud-adoption/) \(CAF\) es un enfoque detallado para la transformación digital empresarial y una guía completa para la planificación de una adopción de la nube de calidad de producción. CAF incluye una [guía de configuración de Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/ready/azure-setup-guide/) detallada para aquellos usuarios que no conocen Azure a fin de que puedan ponerse en marcha de forma rápida y segura. Puede encontrar una versión interactiva de esta en [Azure Portal](https://portal.azure.com/?feature.quickstart=true#blade/Microsoft_Azure_Resources/QuickstartCenterBlade). Encontrará arquitecturas de ejemplo y procedimientos recomendados específicos para la implementación de aplicaciones, así como recursos de aprendizaje gratuitos que le ayudarán a convertirse en un experto en Azure.

### <a name="consider-the-network-between-your-location-and-azure"></a>Consideraciones sobre la red entre su ubicación y Azure

Tanto si se aprovechan los recursos en la nube para ejecutar los procesos de producción, pruebas y desarrollo, como si se trata de un destino de copia de seguridad y un sitio de recuperación, es importante comprender las necesidades de ancho de banda para la propagación inicial de copias de seguridad y para las transferencias diarias en curso.

Azure Data Box proporciona un medio para transferir la base de referencia de copias de seguridad inicial a Azure sin necesidad de ancho de banda adicional si se calcula que la transferencia de la base de referencia tardará más tiempo del que puede tolerar. Puede aprovechar el estimador de transferencia de datos al crear una cuenta de almacenamiento para calcular el tiempo necesario para transferir la copia de seguridad inicial.

![Estimador de transferencia de datos de Azure Storage](../media/az-storage-transfer.png)

Recuerde que necesitará suficiente capacidad de red para admitir las transferencias de datos diarias en la ventana de transferencia necesaria (ventana de copia de seguridad) sin afectar a las aplicaciones de producción. En esta sección se describen las herramientas y técnicas disponibles para evaluar las necesidades de la red.

#### <a name="how-can-you-determine-how-much-bandwidth-you-will-need"></a>¿Cómo puede calcular la cantidad de ancho de banda que necesitará?

Hay disponibles varias opciones de evaluación para determinar la tasa de cambio y el tamaño total del conjunto de copia de seguridad para la transferencia de base de referencia inicial a Azure. A continuación, se muestran algunos ejemplos de herramientas de evaluación y creación de informes:
  - [MiTrend](https://mitrend.com/)
  - [Aptare](https://www.veritas.com/insights/aptare-it-analytics)
  - [Datavoss](https://www.datavoss.com/)

#### <a name="how-will-i-know-how-much-headroom-i-have-with-my-current-internet-connection"></a>¿Cómo sé cuánta capacidad de aumento tengo con mi conexión a Internet actual?

Es importante conocer la capacidad de aumento de ancho de banda (o que normalmente no utiliza) que tiene disponible a diario. De este modo, podrá evaluar correctamente si puede cumplir sus objetivos durante el tiempo inicial de carga, cuando no use Azure Data Box para la propagación sin conexión, y para completar las copias de seguridad diarias en función de la tasa de cambio identificada anteriormente y la ventana de copia de seguridad. A continuación, se muestran los métodos que puede usar para identificar la capacidad de aumento de ancho de banda que sus copias de seguridad de Azure pueden consumir de forma gratuita.

- ¿Es cliente actualmente de Azure ExpressRoute? Vea su [uso de circuitos](https://docs.microsoft.com/azure/expressroute/expressroute-monitoring-metrics-alerts#circuits-metrics) en Azure Portal.
- Puede ponerse en contacto con su ISP. Este debería poderle compartir informes que ilustren el uso diario y mensual actual.
- Hay varias herramientas que pueden medir el uso mediante la supervisión del tráfico de red en el nivel de enrutador o conmutador, entre otras:
  - [Bandwidth Analyzer Pack de Solarwinds](https://www.solarwinds.com/network-bandwidth-analyzer-pack?CMP=ORG-BLG-DNS)
  - [PRTG de Paessler](https://www.paessler.com/bandwidth_monitoring)
  - [Network Assistant de Cisco](https://www.cisco.com/c/en/us/products/cloud-systems-management/network-assistant/index.html)
  - [WhatsUp Gold](https://www.whatsupgold.com/network-traffic-monitoring)

### <a name="choosing-the-right-storage-options"></a>Elección de las opciones de almacenamiento adecuadas

Cuando se usa Azure como destino de copia de seguridad, los clientes utilizan [Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)\. Azure Blob Storage es la solución de almacenamiento de objetos de Microsoft. Blob Storage está optimizado para almacenar grandes cantidades de datos no estructurados, que no cumplen con ninguna definición o modelo de datos. Además, Azure Storage es duradero, seguro y escalable y ofrece una alta disponibilidad. La plataforma de Microsoft ofrece flexibilidad para seleccionar el almacenamiento adecuado para la carga de trabajo correcta con el fin de proporcionar el [nivel de resistencia](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=/azure/storage/blobs/toc.json) que cumple con los SLA internos. Blob Storage es un servicio de pago por uso. [Cada mes se le cobra](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers?tabs=azure-portal#pricing-and-billing) la cantidad de datos almacenados, el acceso a esos datos y, en el caso de los niveles de acceso esporádico y de archivo, un período de retención mínimo necesario. En las tablas siguientes se resumen las opciones de resistencia y de niveles aplicables a los datos de copia de seguridad.

**Opciones de resistencia de Azure Blob Storage:**

|  |Localmente redundante  |Redundancia de zona  |Redundancia geográfica  |Redundancia de zona geográfica  |
|---------|---------|---------|---------|---------|
|N.º efectivo de copias     | 3         | 3         | 6         | 6 |
|N.º de zonas de disponibilidad     | 1         | 3         | 2         | 4 |
|N.º de regiones     | 1         | 1         | 2         | 2 |
|Conmutación por error manual en la región secundaria     | N/D         | N/D         | Sí         | Sí |

**Niveles de Azure Blob Storage:**

|  | Nivel de acceso frecuente   |Nivel de acceso esporádico   | Nivel de archivo |
| ----------- | ----------- | -----------  | -----------  |
| Disponibilidad | 99,9 %         | 99%         | Sin conexión      |
| Cargos de uso | Mayores costos de almacenamiento, menores costos de acceso y transacciones | Menores costos de almacenamiento, mayores costos de acceso y transacciones | Menores costos de almacenamiento, mayores costos de acceso y transacciones |
| Retención de datos mínima requerida | N/D | 30 días | 180 días |
| Latencia (tiempo hasta el primer byte) | Milisegundos | Milisegundos | Horas |

#### <a name="sample-backup-to-azure-cost-model"></a>Ejemplo de modelo de costos de copia de seguridad en Azure

El concepto de pago por uso puede ser intimidatorio para los clientes que no están familiarizados con la nube pública. Aunque solo paga por la capacidad usada, el precio también incluye las transacciones (lectura o escritura) y la [salida de los datos](https://azure.microsoft.com/pricing/details/bandwidth/) que se vuelven a leer en el entorno local cuando se usa el [plan de datos ilimitados o local directo de Azure ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/), donde se incluye la salida de datos de Azure. Puede realizar análisis hipotéticos en función de los precios de las listas o con los [precios de capacidad reservada de Azure Storage](https://docs.microsoft.com/azure/cost-management-billing/reservations/save-compute-costs-reservations), que pueden ofrecer un ahorro de hasta el 38 %, en la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator/). Este es un ejercicio de precios de ejemplo para modelar el costo mensual de la copia de seguridad en Azure. Solo se trata de un ejemplo y ***los precios pueden variar debido a actividades que no se capturan aquí:***


|Factor de costo  |Costo mensual  |
|---------|---------|
|100 TB de datos de copia de seguridad en el almacenamiento esporádico     |1556,48 USD         |
|2 TB de datos nuevos escritos al día x 30 días     |72 USD en transacciones          |
|Total mensual estimado     |1628,48 USD         |
|---------|---------|
|Restauración única de 5 TB en un entorno local a través de la red pública de Internet   | 527.26 USD         |

> [!Note]
Esta estimación se generó en la Calculadora de precios de Azure con los precios de pago por uso del Este de EE. UU. y se basa en el valor predeterminado de tamaño de fragmento de Veeam de 256 KB para las transferencias WAN. Es posible que este ejemplo no sea aplicable a sus requisitos.

## <a name="implementation-and-operational-guidance"></a>Guía de implementación y funcionamiento

En esta sección se ofrece una breve guía para agregar Azure Storage a una implementación de Veeam local. Si le interesa obtener instrucciones detalladas y consideraciones de planeamiento, le recomendamos que revise la [guía de la copia de seguridad de conexión en la nube de Veeam](https://helpcenter.veeam.com/docs/backup/cloud/cloud_backup.html?ver=100).

1. Abra Azure Portal y busque "Cuentas de almacenamiento" o haga clic en el icono predeterminado de servicios.

      ![Azure Portal](../media/azure-portal.png)

      ![Cuentas de almacenamiento en Azure Portal](../media/locate-storage-account.png)

2. Elija Agregar una cuenta y seleccione o cree un grupo de recursos, proporcione un nombre único, elija la región, seleccione el rendimiento "Estándar", deje siempre el tipo de cuenta como "Almacenamiento v2" y elija el nivel de replicación que se ajuste a los SLA y el nivel predeterminado que el software de copia de seguridad va a usar. Una cuenta Azure Storage hace que los niveles de acceso frecuente, esporádico y de archivo estén disponibles en una sola cuenta. Así mismo, las directivas de Veeam le permiten aprovechar varios niveles para administrar de forma eficaz el ciclo de vida de los datos. Siga con el paso siguiente. 
    
      ![Creación de una cuenta de almacenamiento](../media/account-create-1.png)

3. Siga con las opciones de redes predeterminadas para ahora y pase a "Protección de datos". En este caso, puede habilitar la opción "Eliminación temporal", que le permite recuperar un archivo de copia de seguridad eliminado accidentalmente dentro del período de retención definido y, además, ofrece protección contra la eliminación accidental o malintencionada. 
    ![Creación de una cuenta de almacenamiento, parte 2](../media/account-create-2.png)

4. A continuación, le recomendamos usar la configuración predeterminada de la pantalla "Opciones avanzadas" para los casos de uso de copias de seguridad en Azure.

    ![Creación de una cuenta de almacenamiento, parte 3](../media/account-create-3.png) 

5. Agregue etiquetas para la organización si aprovecha el etiquetado y cree su cuenta. Ya dispone de petabytes de almacenamiento a petición.

6. Ahora solo debe completar dos pasos rápidos para poder agregar la cuenta al entorno de Veeam. En Azure Portal, vaya a la cuenta que creó y seleccione "Contenedores" en el menú "Blob service" de la hoja Portal. Agregue un nuevo contenedor y elija un nombre descriptivo. A continuación, navegue hasta el elemento "Claves de acceso" en "Configuración" y copie el "nombre de la cuenta de almacenamiento" y una de las dos claves de acceso. Necesitará el nombre del contenedor, el nombre de la cuenta y la clave de acceso en los pasos siguientes.

    ![Creación de un contenedor](../media/container-b.png)
    
    ![Obtención de la información de la cuenta](../media/access-key.png)
    
    > [!Note]
Veeam Backup and Replication ofrece opciones adicionales para conectarse a Azure. En el caso de uso de este artículo, el procedimiento recomendado es usar Microsoft Azure Blob Storage como destino de copia de seguridad con el método anterior.

7. ***(Opcional)*** Puede agregar capas de seguridad adicionales a la implementación.

     1. Configure el acceso basado en roles para limitar quién puede hacer cambios en la cuenta de almacenamiento. [Más información aquí](https://docs.microsoft.com/azure/storage/common/authorization-resource-provider?toc=/azure/storage/blobs/toc.json).

    1. Restrinja el acceso a la cuenta a segmentos de red específicos con el [firewall de Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal) para evitar intentos de acceso desde fuera de la red corporativa.

    ![Firewall de Storage](../media/storage-firewall.png) 

    1. Establezca un [bloqueo Delete](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) en la cuenta para evitar la eliminación accidental de la cuenta de almacenamiento.

    ![Bloqueo de recursos](../media/resource-lock.png) 1.) Configure los [procedimientos recomendados de seguridad](https://docs.microsoft.com/azure/storage/blobs/security-recommendations) adicionales. 
8. En la consola de administración de Veaam Backup and Replication, vaya a "Infraestructura de Backup" --> haga clic con el botón derecho en el panel de información general y seleccione "Add Backup Repository" ("Agregar repositorio de copia de seguridad") para abrir el Asistente para la configuración. En el cuadro de diálogo, seleccione almacenamiento de objetos --> Microsoft Azure Blob Storage --> Azure Blob Storage.
    
    ![Pantalla del Asistente para repositorios de Veeam a](../media/veeam-repo-a.png)

    ![Pantalla del Asistente para repositorios de Veeam b](../media/veeam-repo-b.png)

    ![Pantalla del Asistente para repositorios de Veeam c](../media/veeam-repo-c.png)

9. A continuación, especifique un nombre y una descripción del nuevo repositorio de Microsoft Azure Blob Storage.
    
    ![Pantalla del Asistente para repositorios de Veeam d](../media/veeam-repo-d.png)

10. En el paso siguiente, agregue las credenciales para acceder a su cuenta de Azure Storage. Seleccione "Cuenta de Microsoft Azure Storage" en el administrador de credenciales en la nube y escriba el nombre y la clave de acceso de la cuenta de almacenamiento. Seleccione Azure global en el selector de regiones y en cualquier servidor de puerta de enlace, si procede.
    
    ![Pantalla del Asistente para repositorios de Veeam e](../media/veeam-repo-e.png)

> [!Note]
Si opta por no usar un servidor de puerta de enlace de Veeam, asegúrese de que todas las extensiones del repositorio de escalabilidad horizontal tienen acceso directo a Internet.

11. En el registro de contenedor, seleccione el contenedor de Azure Storage y elija o cree una carpeta en la que almacenar las copias de seguridad. También puede definir un límite flexible de la capacidad de almacenamiento total que va a usar Veeam (opción recomendada). Revise la información mostrada en la sección Resumen y complete la herramienta de configuración. A continuación, se puede seleccionar el nuevo repositorio en la configuración del trabajo de copia de seguridad.

    ![Pantalla del Asistente para repositorios de Veeam f](../media/veeam-repo-f.png)
    
    ![Pantalla del Asistente para repositorios de Veeam g](../media/veeam-repo-g.png)

### <a name="azure-alerting-and-performance-monitoring"></a>Alertas y supervisión del rendimiento de Azure

Es aconsejable supervisar tanto los recursos de Azure como la capacidad de Veeam para aprovecharlos tal como lo haría con cualquier destino de almacenamiento en el que confíe para almacenar las copias de seguridad. Si combina las funcionalidades de supervisión de Azure Monitor y Veeam (es decir, la pestaña Estadísticas del nodo de trabajos de la consola de administración de Veeam o las opciones más avanzadas como Veeam One Reporter), le será más fácil mantener el entorno en buen estado.

#### <a name="microsoft-azure-portal"></a>Microsoft Azure Portal

Microsoft Azure proporciona una solución de supervisión sólida mediante [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/monitor-azure-resource). Puede [configurar Azure monitor](https://docs.microsoft.com/azure/storage/common/monitor-storage?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-powershell#configuration) para realizar un seguimiento de la capacidad, las transacciones, la disponibilidad, la autenticación, etc. de Azure Storage. [Aquí](https://docs.microsoft.com/azure/storage/common/monitor-storage-reference) puede encontrar la referencia completa de las métricas de las que se realiza un seguimiento. Algunas métricas útiles para hacer un seguimiento son BlobCapacity (para asegurarse de que permanece por debajo del [límite máximo de capacidad de la cuenta de almacenamiento](https://docs.microsoft.com/azure/storage/common/scalability-targets-standard-account)), Ingress y Egress (para hacer un seguimiento de la cantidad de datos que se escriben y se leen de la cuenta de Azure Storage) y SuccessE2ELatency (para hacer un seguimiento del tiempo de ida y vuelta de las solicitudes entre Azure Storage y MediaAgent). 

También puede [crear alertas de registro](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications) para hacer un seguimiento del estado del servicio Azure Storage y ver el [Panel de estado de Azure](https://status.azure.com/status) en cualquier momento.

#### <a name="veeam-reporting"></a>Informes de Veeam

[Configuración de Veeam One Reporting](https://helpcenter.veeam.com/docs/one/reporter/configure_reporter.html?ver=100)

[Alarmas de Veeam Backup and Replication](https://helpcenter.veeam.com/docs/one/monitor/backup_alarms.html?ver=100) 

### <a name="how-to-open-support-cases"></a>Procedimiento para abrir casos de soporte técnico

Si necesita ayuda con la solución de copia de seguridad en Azure, le recomendamos abrir un caso en Veeam y en Azure para que las organizaciones de soporte técnico puedan colaborar entre ellas, en caso de que sea necesario.

#### <a name="how-to-open-a-case-with-veeam"></a>Procedimiento para abrir un caso en Veeam

Vaya al [portal de atención al cliente de Veeam](https://www.veeam.com/support.html), inicie sesión y abra un caso.

Si necesita conocer las opciones de soporte técnico que Veeam pone a su disposición, consulte la [Directiva de asistencia al cliente de Veeam](https://www.veeam.com/veeam_software_support_policy_ds.pdf).

También puede llamar para abrir un caso:

[Números de soporte técnico internacionales](https://www.veeam.com/contacts.html?ad=in-text-link#support-numbers)

#### <a name="how-to-open-a-case-with-the-azure-support-team"></a>Procedimiento para abrir un caso para el equipo de soporte técnico de Azure

En [Azure Portal](https://portal.azure.com), busque "Soporte técnico" en la barra de búsqueda de la parte superior del portal y elija "+ Nueva solicitud de soporte técnico". 
> [!Note]
Cuando abra un caso, especifique que necesita ayuda con "Azure Storage" o "Redes de Azure" y **NO** con "Azure Backup". Azure Backup es un servicio nativo de Microsoft Azure, y el caso se enrutará incorrectamente.

### <a name="links-to-relevant-veeam-documentation"></a>Vínculos a la documentación pertinente de Veeam

Documentación de Veeam que proporciona más detalles:

[Guía de usuario de Veeam](https://helpcenter.veeam.com/docs/backup/hyperv/overview.html?ver=100)

[Guía de la arquitectura de Veeam](https://helpcenter.veeam.com/docs/backup/vsphere/backup_architecture.html?ver=100)

### <a name="link-to-marketplace-offering"></a>Vínculo a las ofertas de Marketplace

También puede seguir usando la solución de Veeam que conoce y en la que confía para proteger las cargas de trabajo que se ejecutan en Azure. Veeam ha facilitado la implementación de su solución en Azure y protege Azure Virtual Machines y muchos otros servicios de Azure.

[Implementación de Veeam B&R a través de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/veeam.veeam-backup-replication?tab=overview)

[Hoja de datos de Azure](https://www.veeam.com/backup-azure.html?ad=menu-products)


## <a name="next-steps"></a>Pasos siguientes

Explore los recursos adicionales en estos sitios web externos para obtener información sobre los escenarios de uso especializados:

[Vídeos de procedimientos de Veeam](https://www.veeam.com/how-to-videos.html?ad=menu-resources)

[Documentación técnica de Veeam](https://www.veeam.com/documentation-guides-datasheets.html?ad=menu-resources)

[Base de conocimiento de Veeam y preguntas más frecuentes](https://www.veeam.com/knowledge-base.html?ad=menu-resources)
