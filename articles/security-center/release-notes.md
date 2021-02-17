---
title: Notas de la versión de Azure Security Center
description: Una descripción de las novedades y los cambios en Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/04/2021
ms.author: memildin
ms.openlocfilehash: fe031fa6de86b8059ba175fc4e1df6385ca7e796
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99551033"
---
# <a name="whats-new-in-azure-security-center"></a>Novedades de Azure Security Center

Defender está en desarrollo activo y recibe mejoras continuas. Para mantenerse al día con los avances más recientes, esta página proporciona información sobre las nuevas características, las correcciones de errores y las funcionalidades en desuso.

Esta página se actualiza con frecuencia, por lo que se recomienda visitarla a menudo. 

Para obtener información sobre los cambios *planeados* que están próximos a materializarse en Security Center, consulte el artículo [Próximos cambios importantes en Azure Security Center](upcoming-changes.md). 

> [!TIP]
> Si busca elementos de más de 6 meses, puede encontrarlos en las [Novedades de Azure Security Center](release-notes-archive.md).


## <a name="february-2021"></a>Febrero de 2021

Las actualizaciones de febrero incluyen:

- [Publicación de las recomendaciones de protección de cargas de trabajo de Kubernetes para disponibilidad general (GA)](#kubernetes-workload-protection-recommendations-released-for-general-availability-ga)
- [Vínculo directo a la directiva desde la página de detalles de la recomendación](#direct-link-to-policy-from-recommendation-details-page)
- [La recomendación de clasificación de datos SQL ya no afecta a la puntuación segura](#sql-data-classification-recommendation-no-longer-affects-your-secure-score)
- [Las automatizaciones de los flujos de trabajo se pueden desencadenar mediante los cambios en las evaluaciones de cumplimiento normativo (versión preliminar)](#workflow-automations-can-be-triggered-by-changes-to-regulatory-compliance-assessments-preview)

### <a name="kubernetes-workload-protection-recommendations-released-for-general-availability-ga"></a>Publicación de las recomendaciones de protección de la carga de trabajo de Kubernetes para disponibilidad general (GA)

Nos complace anunciar la disponibilidad general (GA) del conjunto de recomendaciones para las protecciones de la carga de trabajo de Kubernetes.

Para que las cargas de trabajo de Kubernetes sean seguras de forma predeterminada, Security Center ha agregado recomendaciones de protección de nivel de Kubernetes, incluidas opciones de cumplimiento con el control de admisión de Kubernetes.

Cuando el complemento de Azure Policy para Kubernetes está instalado en el clúster de Azure Kubernetes Service (AKS), todas las solicitudes al servidor de la API de Kubernetes se supervisarán con el conjunto predefinido de procedimientos recomendados (que aparecen en forma de 13 recomendaciones de seguridad) antes de que se guarden en el clúster. Después, puede realizar la configurar para aplicar los procedimientos recomendados y exigirlos para futuras cargas de trabajo.

Por ejemplo, puede exigir que no se creen los contenedores con privilegios y que se bloqueen las solicitudes futuras para este fin.

Más información en [Procedimientos recomendados de protección de cargas de trabajo con el control de admisión de Kubernetes](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)

> [!NOTE]
> Aunque las recomendaciones estaban en versión preliminar, no representaban los recursos de clúster de AKS en mal estado y no se incluían en los cálculos de la puntuación segura. Con este anuncio de disponibilidad general, se incluirán en el cálculo de puntuaciones. Si aún no ha hecho nada al respecto, esto podría afectar negativamente a su puntuación segura. Corríjalas siempre que sea posible como se describe en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).


### <a name="direct-link-to-policy-from-recommendation-details-page"></a>Vínculo directo a la directiva desde la página de detalles de la recomendación

Cuando se revisan los detalles de una recomendación, a menudo resulta útil poder ver la directiva subyacente. Para cada recomendación admitida en una directiva, hay un vínculo nuevo en la página de detalles de la recomendación:

:::image type="content" source="media/release-notes/view-policy-definition.png" alt-text="Vínculo a la página de Azure Policy de la directiva específica que admite una recomendación":::

Use este vínculo para ver la definición de directiva y revisar la lógica de evaluación. 

Si va a revisar la lista de recomendaciones de la [guía de referencia de recomendaciones de seguridad](recommendations-reference.md), también verá vínculos a las páginas de definición de directiva:

:::image type="content" source="media/release-notes/view-policy-definition-from-documentation.png" alt-text="Acceso a la página de Azure Policy de una directiva específica directamente desde la página de referencia de recomendaciones de Azure Security Center" lightbox="media/release-notes/view-policy-definition-from-documentation.png":::


### <a name="sql-data-classification-recommendation-no-longer-affects-your-secure-score"></a>La recomendación de clasificación de datos SQL ya no afecta a la puntuación segura

La recomendación **Los datos confidenciales de las bases de datos SQL deben clasificarse** ya no afecta a la puntuación segura. Esta es la única recomendación del control de seguridad **Apply data classification** (Aplicar clasificación de datos), de modo que el control ahora tiene un valor de puntuación segura de 0.


### <a name="workflow-automations-can-be-triggered-by-changes-to-regulatory-compliance-assessments-preview"></a>Las automatizaciones de los flujos de trabajo se pueden desencadenar mediante los cambios en las evaluaciones de cumplimiento normativo (versión preliminar)

Hemos agregado un tercer tipo de datos a las opciones del desencadenador para las automatizaciones del flujo de trabajo: cambios en las evaluaciones de cumplimiento normativo.

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="Uso de los cambios en las evaluaciones de cumplimiento normativo para desencadenar la automatización de un flujo de trabajo" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::


## <a name="january-2021"></a>Enero de 2021

Las actualizaciones de enero incluyen:

- [Azure Security Benchmark es ahora la iniciativa de directivas predeterminada para Azure Security Center](#azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center)
- [La valoración de vulnerabilidades tanto para máquinas locales como las que están en varias nubes se ha publicado en disponibilidad general (GA)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga)
- [Puntuación segura para grupos de administración ya está disponible en versión preliminar](#secure-score-for-management-groups-is-now-available-in-preview)
- [La API de Puntuación segura se ha publicado en disponibilidad general (GA)](#secure-score-api-is-released-for-general-availability-ga)
- [Las protecciones de DNS pendientes se han agregado a Azure Defender para App Service](#dangling-dns-protections-added-to-azure-defender-for-app-service)
- [Los conectores de nubes múltiples se publican en disponibilidad general (GA)](#multi-cloud-connectors-are-released-for-general-availability-ga)
- [Exclusión de recomendaciones completas de su puntuación segura para suscripciones y grupos de administración](#exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups)
- [Los usuarios ya pueden solicitar visibilidad en todo el inquilino de su administrador global](#users-can-now-request-tenant-wide-visibility-from-their-global-administrator)
- [Se han agregado 35 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).
- [Exportación de CSV de una lista filtrada de recomendaciones](#csv-export-of-filtered-list-of-recommendations)
- [Los recursos en estado "No aplicable" ahora se notifican como "Compatible" en las valoraciones de Azure Policy](#not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments)
- [Exportación de instantáneas semanales de datos de cumplimiento normativo y puntuación segura con exportación continua (versión preliminar)](#export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview)


### <a name="azure-security-benchmark-is-now-the-default-policy-initiative-for-azure-security-center"></a>Azure Security Benchmark es ahora la iniciativa de directivas predeterminada para Azure Security Center

Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. Este punto de referencia, que cuenta con un amplísimo respaldo, se basa en los controles del [Centro de seguridad de Internet (CIS)](https://www.cisecurity.org/benchmark/azure/) y del [Instituto Nacional de Normas y Tecnología (NIST)](https://www.nist.gov/), con un enfoque en seguridad centrada en la nube.

En los últimos meses, la lista de recomendaciones de seguridad integradas de Security Center ha crecido considerablemente para ampliar la cobertura de este punto de referencia.

A partir de esta versión, el punto de referencia es la base de las recomendaciones de Security Center y está totalmente integrado como iniciativa de directivas predeterminada. 

Todos los servicios de Azure tienen una página de línea base de seguridad en su documentación. Por ejemplo, [esta es la línea base de Security Center](security-baseline.md). Estas líneas base se basan en el Azure Security Benchmark.

Si usa el panel de cumplimiento normativo de Security Center, verá dos instancias del punto de referencia durante un período de transición:

:::image type="content" source="media/release-notes/regulatory-compliance-with-azure-security-benchmark.png" alt-text="Panel de cumplimiento normativo de Security Center que muestra Azure Security Benchmark":::

Las recomendaciones existentes no se ven afectadas y a medida que crezca el punto de referencia, los cambios se reflejarán automáticamente en Security Center. 

Para más información, consulte las páginas siguientes:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Personalización del conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-is-released-for-general-availability-ga"></a>La valoración de vulnerabilidades tanto para máquinas locales como las que están en varias nubes se ha publicado en disponibilidad general (GA)

En octubre, se anunció una versión preliminar para examinar servidores con Azure Arc habilitado con el analizador de evaluación de vulnerabilidades integrado de [Azure Defender para servidores](defender-for-servers-introduction.md) (con tecnología de Qualys).

Ahora se ha publicado en disponibilidad general (GA).

Al habilitar Azure Arc en máquinas que no son de Azure, Security Center le ofrecerá la opción de implementar en ellas el analizador de vulnerabilidades integrado, manualmente y a escala.

Con esta actualización, puede aprovechar toda la capacidad de **Azure Defender para servidores** para consolidar su programa de administración de vulnerabilidades en todos sus recursos, tanto si son de Azure como si no.

Principales funcionalidades:

- Supervisión del estado de aprovisionamiento del analizador de evaluación de vulnerabilidades en máquinas de Azure Arc.
- Aprovisionamiento del agente de evaluación de vulnerabilidades integrado en máquinas de Azure Arc Windows y Linux (manualmente y a escala).
- Recepción y análisis de vulnerabilidades detectadas por agentes implementados (manualmente y a escala).
- Experiencia unificada para máquinas virtuales de Azure y máquinas de Azure Arc.

[Obtenga más información sobre la implementación del analizador de vulnerabilidades integrado en las máquinas híbridas](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Obtenga más información sobre los servidores habilitados para Azure Arc](../azure-arc/servers/index.yml).


### <a name="secure-score-for-management-groups-is-now-available-in-preview"></a>Puntuación segura para grupos de administración ya está disponible en versión preliminar

La página de puntuación segura ahora muestra las puntuaciones seguras agregadas para los grupos de administración, además del nivel de suscripción. Por consiguiente, ahora puede ver la lista de grupos de administración de la organización y la puntuación de cada uno de ellos.

:::image type="content" source="media/secure-score-security-controls/secure-score-management-groups.png" alt-text="Visualización de las puntuaciones seguras de los grupos de administración.":::

Obtenga más información sobre la [puntuación segura y los controles de seguridad en Azure Security Center](secure-score-security-controls.md).

### <a name="secure-score-api-is-released-for-general-availability-ga"></a>La API de Puntuación segura se ha publicado en disponibilidad general (GA)

Ahora puede acceder a la puntuación a través de la [API de puntuación segura](/rest/api/securitycenter/securescores/). Los métodos de API proporcionan la flexibilidad necesaria para consultar los datos y crear su propio mecanismo de creación de informes de las puntuaciones seguras a lo largo del tiempo. Por ejemplo:

- use la API **Secure Scores** para obtener la puntuación de una suscripción específica.
- use la API **Secure Score Controls** para mostrar los controles de seguridad y la puntuación actual de las suscripciones.

Conozca las herramientas externas que son posibles gracias a la API de puntuación segura en el [área de puntuación segura de la comunidad de GitHub](https://github.com/Azure/Azure-Security-Center/tree/master/Secure%20Score).

Obtenga más información sobre la [puntuación segura y los controles de seguridad en Azure Security Center](secure-score-security-controls.md).


### <a name="dangling-dns-protections-added-to-azure-defender-for-app-service"></a>Las protecciones de DNS pendientes se han agregado a Azure Defender para App Service

Las adquisiciones de subdominios son una amenaza muy grave que se producen de forma común en las organizaciones. Una adquisición de subdominio puede producirse cuando se tiene un registro DNS que apunta a un sitio web desaprovisionado. Estos registros DNS también se conocen como entradas "DNS pendientes". Los registros CNAME son especialmente vulnerables a esta amenaza. 

Las adquisiciones de subdominios permiten a los actores de las amenazas redirigir el tráfico destinado al dominio de una organización a un sitio que realiza una actividad malintencionada.

Azure Defender para App Service ahora detecta las entradas DNS pendientes cuando se retira un sitio web de App Service. Este es el momento en el que la entrada DNS apunta a un recurso que no existe y su sitio web es vulnerable a la adquisición del subdominio. Estas protecciones están disponibles tanto si los dominios se administran con Azure DNS como con un registrador de dominios externo y se aplican a App Service en Windows y en Linux.

Más información:

- [Tabla de referencia de alertas de App Service](alerts-reference.md#alerts-azureappserv): incluye dos nuevas alertas de Azure Defender que se desencadenan cuando se detecta una entrada DNS pendiente
- [Evitar las entradas DNS pendientes y evitar la adquisición de subdominios](../security/fundamentals/subdomain-takeover.md): obtenga información sobre la amenaza de la adquisición de subdominios y el aspecto de los DNS pendientes
- [Introducción a Azure Defender para App Service](defender-for-app-service-introduction.md)


### <a name="multi-cloud-connectors-are-released-for-general-availability-ga"></a>Los conectores de nubes múltiples se publican en disponibilidad general (GA)

Las cargas de trabajo de nube abarcan normalmente varias plataformas de nube, por lo que los servicios de seguridad de la nube deben hacer lo mismo.

Azure Security Center protege las cargas de trabajo de Azure, Amazon Web Services (AWS) y Google Cloud Platform (GCP).

La conexión de las cuentas de AWS o GCP integra sus herramientas de seguridad nativas, como AWS Security Hub y GCP Security Center, en Azure Security Center.

Esta capacidad significa que Security Center proporciona visibilidad y protección en los principales entornos de nube. Algunas de las ventajas de esta integración son:

- Aprovisionamiento automático de agentes (Security Center usa Azure Arc para implementar el agente de Log Analytics en las instancias de AWS)
- Administración de directivas
- Administración de vulnerabilidades
- Detección y respuesta de puntos de conexión (EDR) integrados
- Detección de errores de configuración de seguridad
- Una vista única que muestra recomendaciones de seguridad de todos los proveedores de nube
- Incorporación de todos los recursos a los cálculos de puntuación segura de Security Center
- Evaluaciones de cumplimiento normativo de los recursos de AWS y GCP

En el menú de Security Center, seleccione **Multi cloud connectors** (Conectores en varias nubes) y verá las opciones para crear conectores:

:::image type="content" source="./media/quickstart-onboard-aws/add-aws-account.png" alt-text="Botón Add AWS account (Agregar cuenta de AWS) de la página Conectores multinube de Security Center":::

Puede encontrar más información en:
- [Conexión de las cuentas de AWS a Azure Security Center](quickstart-onboard-aws.md)
- [Conexión de las cuentas de GCP a Azure Security Center](quickstart-onboard-gcp.md)


### <a name="exempt-entire-recommendations-from-your-secure-score-for-subscriptions-and-management-groups"></a>Exclusión de recomendaciones completas de su puntuación segura para suscripciones y grupos de administración

Estamos ampliando la capacidad de exención para incluir recomendaciones completas. Proporcionar más opciones para ajustar las recomendaciones de seguridad que Security Center realiza para las suscripciones, el grupo de administración o los recursos.

En ocasiones, un recurso se mostrará como incorrecto cuando se sepa que el problema se ha resuelto mediante una herramienta de terceros que Security Center no ha detectado. O bien se mostrará una recomendación en un ámbito al que cree que no pertenece. La recomendación podría no ser adecuada para una suscripción concreta. O quizás la organización ha decidido aceptar los riesgos relacionados con la recomendación o el recurso específicos.

Con esta característica en vista previa (GB), ahora puede crear una exención para que una recomendación:

- **Excluya un recurso** para asegurarse de que no aparezca con los recursos incorrectos en el futuro y no afecte a la puntuación de seguridad. El recurso se mostrará como no aplicable y el motivo aparecerá como "exento" con la justificación específica que seleccione.

- **Excluir una suscripción o un grupo de administración** para asegurarse de que la recomendación no afecte a la puntuación de seguridad y no que no se muestre para la suscripción o el grupo de administración en el futuro. Esto se refiere a los recursos existentes y cualquiera que cree en el futuro. La recomendación se marcará con la justificación específica que seleccione para el ámbito que ha seleccionado.

Puede obtener más información en [Exención de recursos y recomendaciones de la puntuación de seguridad](exempt-resource.md).



### <a name="users-can-now-request-tenant-wide-visibility-from-their-global-administrator"></a>Los usuarios ya pueden solicitar visibilidad en todo el inquilino de su administrador global

Si algún usuario no tiene los permisos necesarios para ver los datos de Security Center, ahora verá un vínculo para solicitar permisos al administrador global de su organización. La solicitud incluye el rol que desean y la justificación de por qué es necesario.

:::image type="content" source="media/security-center-management-groups/request-tenant-permissions.png" alt-text="Banner que informa a los usuarios de que pueden solicitar permisos para todo el inquilino.":::

Para más información, consulte [Solicitud de permisos para todo el inquilino cuando el suyo sea insuficiente](security-center-management-groups.md#request-tenant-wide-permissions-when-yours-are-insufficient).


### <a name="35-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Se han agregado 35 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark.

Azure Security Benchmark es la iniciativa de directivas predeterminada de Azure Security Center. 

Para aumentar la cobertura del punto de referencia se han agregado a Security Center las siguientes 35 recomendaciones en versión preliminar.

> [!TIP]
> Las recomendaciones de la versión preliminar no representan un recurso incorrecto y no se incluyen en los cálculos de una puntuación segura. Corríjalas siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación. Puede encontrar más información sobre cómo responder a estas recomendaciones en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).

| Control de seguridad                     | Nuevas recomendaciones                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Habilitación del cifrado de datos en reposo            | - Las cuentas de Azure Cosmos DB deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Las áreas de trabajo de Azure Machine Learning deben cifrarse con una clave administrada por el cliente (CMK).<br>- La protección de datos de Bring Your Own Key debe estar habilitada para los servidores MySQL.<br>- La protección de datos de Bring Your Own Key debe estar habilitada para los servidores PostgreSQL.<br>- Las cuentas de Cognitive Services deben habilitar el cifrado de datos con una clave administrada por el cliente (CMK).<br>- Las instancias de Container Registry se deben cifrar con una clave administrada por el cliente (CMK).<br>- Las instancias administradas de SQL deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Los servidores SQL deben usar claves administradas por el cliente para cifrar los datos en reposo.<br>- Las cuentas de almacenamiento deben usar la clave administrada por el cliente (CMK) para el cifrado.                                                                                                                                                              |
| Implementación de procedimientos recomendados de seguridad    | - Las suscripciones deben tener una dirección de correo electrónico de contacto para los problemas de seguridad.<br> - El aprovisionamiento automático del agente de Log Analytics debe estar habilitado en su suscripción.<br> - La opción para enviar notificaciones por correo electrónico para alertas de gravedad alta debe estar habilitada.<br> - La opción para enviar notificaciones por correo electrónico al propietario de la suscripción en relación a alertas de gravedad alta debe estar habilitada.<br> - Los almacenes de claves deben tener habilitada la protección contra operaciones de purga.<br> - Los almacenes de claves deben tener habilitada la eliminación temporal. |
| Administración de acceso y permisos        | - Las aplicaciones de funciones deben tener la opción "Certificados de cliente (certificados de cliente entrantes)" habilitada. |
| Protección de aplicaciones contra ataques DDoS | - Web Application Firewall (WAF) debe estar habilitado para Application Gateway.<br> - Web Application Firewall (WAF) debe estar habilitado para Azure Front Door Service. |
| Restricción de los accesos de red no autorizados | - El firewall debe estar habilitado en Key Vault.<br> - Se debe configurar un punto de conexión privado para Key Vault.<br> - App Configuration debe usar un vínculo privado.<br> - Azure Cache for Redis debe residir en una red virtual.<br> - Los dominios de Azure Event Grid deben usar un vínculo privado.<br> - Los temas de Azure Event Grid deben usa un vínculo privado.<br> - Las áreas de trabajo de Azure Machine Learning deben usar un vínculo privado.<br> Azure SignalR Service debe usar un vínculo privado.<br> - Azure Spring Cloud debe usar la inserción de red.<br> -Las instancias de Container Registry no deben permitir el acceso de red sin restricciones.<br> - Las instancias de Container Registry deben usar vínculo privado.<br> -El acceso a redes públicas debe estar deshabilitado para los servidores de MariaDB.<br> El acceso a las redes públicas debe estar deshabilitado para los servidores de MySQL<br> - El acceso a redes públicas debe estar deshabilitado para los servidores de PostgreSQL.<br> - La cuenta de almacenamiento debería utilizar una conexión de vínculo privado<br> - Las cuentas de almacenamiento deben restringir el acceso a la red mediante el uso de reglas de red virtual.<br> - Las plantillas de VM Image Builder deben usar un vínculo privado.|
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Vínculos relacionados:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Más información sobre Azure Database for MariaDB](../mariadb/overview.md)
- [Más información sobre Azure Database for MySQL](../mysql/overview.md)
- [Más información sobre Azure Database for PostgreSQL](../postgresql/overview.md)




### <a name="csv-export-of-filtered-list-of-recommendations"></a>Exportación de CSV de una lista filtrada de recomendaciones 

En noviembre de 2020, hemos agregado filtros a la página de recomendaciones (consulte [La lista de recomendaciones ahora incluye filtros](#recommendations-list-now-includes-filters)). En diciembre, hemos expandido esos filtros ([La página Recomendaciones tiene nuevos filtros de entorno, gravedad y respuestas disponibles](#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)). 

Con este anuncio, vamos a cambiar el comportamiento del botón **Descargar como CSV** para que la exportación de CSV solo incluya las recomendaciones que se muestran actualmente en la lista filtrada. 

Por ejemplo, en la imagen siguiente puede ver que la lista se ha filtrado a dos recomendaciones. El archivo CSV que se genera incluye los detalles de estado de cada recurso afectado por esas dos recomendaciones.   

:::image type="content" source="media/security-center-managing-and-responding-alerts/export-to-csv-with-filters.png" alt-text="Exportación de recomendaciones filtradas a un archivo CSV":::

Más información en [Recomendaciones de seguridad en el Centro de seguridad de Azure](security-center-recommendations.md)


### <a name="not-applicable-resources-now-reported-as-compliant-in-azure-policy-assessments"></a>Los recursos en estado "No aplicable" ahora se notifican como "Compatible" en las valoraciones de Azure Policy

Anteriormente, los recursos que se evaluaban para una recomendación y se detectaba que eran **no aplicables** aparecían en in Azure Policy como "No compatible". Ninguna acción del usuario podría cambiar su estado a "Compatible". Con este cambio, se notificarán como "Compatible" para aumentar la claridad.

El único impacto se verá en Azure Policy, donde el número de recursos compatibles aumentará. Esto no afectará a la puntuación de seguridad de Azure Security Center.


### <a name="export-weekly-snapshots-of-secure-score-and-regulatory-compliance-data-with-continuous-export-preview"></a>Exportación de instantáneas semanales de datos de cumplimiento normativo y puntuación segura con exportación continua (versión preliminar)

Hemos agregado una nueva característica en versión preliminar a las herramientas de [exportación continua](continuous-export.md) para exportar instantáneas semanales de datos de cumplimiento normativo y puntuación segura.

Al definir una exportación continua, establezca la frecuencia de exportación:

:::image type="content" source="media/release-notes/export-frequency.png" alt-text="Elección de la frecuencia de la exportación continua":::

- **Streaming**: las evaluaciones se enviarán en tiempo real cuando se actualice el estado de mantenimiento de un recurso (si no se produce ninguna actualización, no se enviarán datos).
- **Instantáneas**: se enviará una instantánea del estado actual de todas las evaluaciones de cumplimiento normativo cada semana (se trata de una característica en versión preliminar para las instantáneas semanales de datos de cumplimiento normativo y puntuaciones seguras).

Puede encontrar más información sobre las funcionalidades completas de esta característica en [Exportación continua de datos de Security Center](continuous-export.md).

## <a name="december-2020"></a>Diciembre de 2020

Las actualizaciones de diciembre incluyen:

- [Disponibilidad general de Azure Defender para servidores SQL Server en máquinas](#azure-defender-for-sql-servers-on-machines-is-generally-available)
- [Compatibilidad de Azure Defender para SQL para el grupo de SQL dedicado de Azure Synapse Analytics disponible con carácter general](#azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available)
- [Ahora, los administradores globales se pueden conceder a sí mismos permisos de nivel de inquilino](#global-administrators-can-now-grant-themselves-tenant-level-permissions)
- [Dos nuevos planes de Azure Defender: Azure Defender para DNS y Azure Defender para Resource Manager (en versión preliminar)](#two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview)
- [Nueva página de alertas de seguridad en Azure Portal (versión preliminar)](#new-security-alerts-page-in-the-azure-portal-preview)
- [Experiencia de Security Center revitalizada en Azure SQL Database y SQL Managed Instance](#revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance)
- [Se han actualizado las herramientas y filtros para el inventario de recursos](#asset-inventory-tools-and-filters-updated)
- [La recomendación acerca de las aplicaciones web que solicitan certificados SSL ya no forma parte de la puntuación segura](#recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score)
- [La página Recomendaciones tiene nuevos filtros de entorno, gravedad y respuestas disponibles](#recommendations-page-has-new-filters-for-environment-severity-and-available-responses)
- [La exportación continua obtiene nuevos tipos de datos y directivas deployifnotexist mejoradas](#continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies)


### <a name="azure-defender-for-sql-servers-on-machines-is-generally-available"></a>Disponibilidad general de Azure Defender para servidores SQL Server en máquinas

Azure Security Center ofrece dos planes de Azure Defender para servidores SQL Server:

- **Azure Defender para servidores de bases de datos Azure SQL**: defiende los servidores SQL Server nativos de Azure. 
- **Azure Defender para servidores SQL Server en máquinas**: amplía las mismas protecciones a los servidores SQL Server en entornos híbridos, multinube y locales.

Con este anuncio, **Azure Defender para SQL** ahora protege las bases de datos y sus datos dondequiera que se encuentren.

Azure Defender para SQL incluye funcionalidades de evaluación de vulnerabilidades. La herramienta de evaluación de vulnerabilidades incluye las siguientes características avanzadas:

- La **configuración de línea de base** (nuevo) para limitar de forma inteligente los resultados de los análisis de vulnerabilidades a aquellas que podrían representar problemas de seguridad reales. Una vez establecido el estado de seguridad de la línea de base, la herramienta de evaluación de vulnerabilidades solo informa de las desviaciones con respecto a esa línea de base. Los resultados que coinciden con la línea de base se consideran como correctos en análisis posteriores. Esto le permite a usted y a sus analistas centrar la atención en lo que importa.
- **Información comparativa detallada** para ayudarle a *comprender* los hechos detectados y por qué se relacionan con los recursos.
- **Scripts de corrección** para ayudarle a mitigar los riesgos identificados.

Más información sobre [Azure Defender for SQL](defender-for-sql-introduction.md).


### <a name="azure-defender-for-sql-support-for-azure-synapse-analytics-dedicated-sql-pool-is-generally-available"></a>Compatibilidad de Azure Defender para SQL para Azure Synapse Analytics disponible con carácter general

Azure Synapse Analytics (anteriormente SQL Data Warehouse) es un servicio de análisis que combina almacenamiento de datos empresariales con análisis de macrodatos. Los grupos de SQL dedicados son las características de almacenamiento de datos empresariales de Azure Synapse. Más información en [¿Qué es Azure Synapse Analytics (anteriormente SQL Data Warehouse)?](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Azure Defender para SQL protege los grupos de SQL dedicados con:

- **Protección contra amenazas avanzada** para detectar amenazas y ataques. 
- **Funcionalidades de evaluación de vulnerabilidades** para identificar y corregir configuraciones erróneas de seguridad.

La compatibilidad de Azure Defender para SQL con los grupos de SQL de Azure Synapse Analytics se agrega automáticamente al paquete de bases de datos SQL de Azure en Azure Security Center. Encontrará una nueva pestaña "Azure Defender para SQL" en la página del área de trabajo de Synapse en Azure Portal.

Más información sobre [Azure Defender for SQL](defender-for-sql-introduction.md).


### <a name="global-administrators-can-now-grant-themselves-tenant-level-permissions"></a>Ahora, los administradores globales se pueden conceder a sí mismos permisos de nivel de inquilino.

Un usuario con el rol de **administrador global** de Azure Active Directory puede tener responsabilidades en todo el inquilino, pero carecer de los permisos de Azure para ver la información de toda la organización en Azure Security Center. 

Para asignarse a sí mismo permisos de nivel de inquilino, siga las instrucciones que se indican en [Concederse a uno mismo permisos para todo el inquilino](security-center-management-groups.md#grant-tenant-wide-permissions-to-yourself).


### <a name="two-new-azure-defender-plans-azure-defender-for-dns-and-azure-defender-for-resource-manager-in-preview"></a>Dos nuevos planes de Azure Defender: Azure Defender para DNS y Azure Defender para Resource Manager (en versión preliminar)

Se han agregado dos nuevas funcionalidades de protección contra amenazas con amplitud nativa de nube para el entorno de Azure.

Estas nuevas protecciones mejoran enormemente la resistencia frente a los ataques de actores de amenazas y aumentan considerablemente el número de recursos de Azure protegidos por Azure Defender.

- **Azure Defender para Resource Manager**: supervisa automáticamente todas las operaciones de administración de recursos realizadas en la organización. Para más información, consulte:
    - [Introducción a Azure Defender para Resource Manager](defender-for-resource-manager-introduction.md)
    - [Respuesta a las alertas de Azure Defender para Resource Manager](defender-for-resource-manager-usage.md)
    - [Lista de alertas proporcionadas por Azure Defender para Resource Manager](alerts-reference.md#alerts-resourcemanager)

- **Azure Defender para DNS**: supervisa continuamente todas las consultas de DNS de los recursos de Azure. Para más información, consulte:
    - [Introducción a Azure Defender para DNS](defender-for-dns-introduction.md)
    - [Respuesta a las alertas de Azure Defender para DNS](defender-for-dns-usage.md)
    - [Lista de alertas proporcionadas por Azure Defender para DNS](alerts-reference.md#alerts-dns)


### <a name="new-security-alerts-page-in-the-azure-portal-preview"></a>Nueva página de alertas de seguridad en Azure Portal (versión preliminar)

La página de alertas de seguridad de Azure Security Center se ha rediseñado para proporcionar:

- **Mejora en la evaluación de prioridades en las alertas**: ahora es más fácil ayudar a reducir la fatiga de las alertas y centrarse en las amenazas más importantes, la lista incluye filtros personalizables y opciones de agrupación.
- **Más información en la lista de alertas**: por ejemplo las tácticas ATT y ACK de MITRE
- **Botón para crear alertas de ejemplo**: para evaluar las funcionalidades de Azure Defender y probar la configuración de las alertas (para la integración de SIEM, las notificaciones por correo electrónico y las automatizaciones de los flujos de trabajo), puede crear alertas de ejemplo desde todos los planes de Azure Defender.
- **Alineación con la experiencia de incidentes de Azure Sentinel**: para los clientes que usan ambos productos, cambiar entre ellos ahora es más sencillo y es fácil que uno aprenda del otro.
- **Mejor rendimiento** de listas de alertas grandes.
- **Desplazamiento mediante el teclado** a través de la lista de alertas.
- **Alertas de Azure Resource Graph**: puede consultar las alertas en Azure Resource Graph, la API similar a Kusto para todos los recursos. Esto también resulta útil si va a crear sus propios paneles de alertas. [Más información sobre Azure Resource Graph](../governance/resource-graph/index.yml).

Para acceder a la nueva experiencia, use el vínculo "Intentarlo ahora" del banner de la parte superior de la página de alertas de seguridad.

:::image type="content" source="media/security-center-managing-and-responding-alerts/preview-alerts-experience-banner.png" alt-text="Banner con un vínculo a la versión preliminar de la nueva experiencia de alertas":::

Para crear alertas de ejemplo a partir de la nueva experiencia de alertas, consulte el apartado [Generación de alertas de ejemplo de Azure Defender](security-center-alert-validation.md#generate-sample-azure-defender-alerts).


### <a name="revitalized-security-center-experience-in-azure-sql-database--sql-managed-instance"></a>Experiencia de Security Center revitalizada en Azure SQL Database y SQL Managed Instance 

La experiencia de Security Center en SQL proporciona acceso a las siguientes características de Security Center y Azure Defender para SQL:

- **Recomendaciones de seguridad**: Azure Security Center analiza periódicamente el estado de seguridad de todos los recursos de Azure conectados para identificar posibles configuraciones erróneas de seguridad. A continuación, proporciona recomendaciones sobre cómo corregir esas vulnerabilidades y mejorar la postura de seguridad de las organizaciones.
- **Alertas de seguridad**: un servicio de detección que supervisa continuamente las actividades de Azure SQL para detectar amenazas tales como la inyección de código SQL, ataques por fuerza bruta y abuso de privilegios. Este servicio desencadena alertas de seguridad detalladas y orientadas a acciones en Security Center, y proporciona opciones para continuar las investigaciones con Azure Sentinel, la solución SIEM nativa de Microsoft Azure.
- **Conclusiones**: un servicio de evaluación de vulnerabilidades que supervisa continuamente las configuraciones de Azure SQL y ayuda a corregir las vulnerabilidades. Los análisis de evaluación proporcionan una visión general de los estados de seguridad de Azure SQL junto con conclusiones detalladas de seguridad.     

:::image type="content" source="media/release-notes/azure-security-center-experience-in-sql.png" alt-text="Las características de seguridad de Azure Security Center para SQL están disponibles en Azure SQL":::


### <a name="asset-inventory-tools-and-filters-updated"></a>Se han actualizado las herramientas y filtros para el inventario de recursos

La página Inventario de Azure Security Center se ha actualizado con los cambios siguientes:

- Se han agregado **guías y comentarios** a la barra de herramientas. Esto permite abrir un panel con vínculos a información y herramientas relacionados. 
- Se ha agregado un **filtro de suscripciones** a los filtros predeterminados disponibles para los recursos.
- Se ha agregado el enlace **Abrir consulta** para abrir las opciones de filtro actuales como una consulta de Azure Resource Graph (anteriormente se denominaba "Ver en Resource Graph Explorer").
- **Opciones de operador** para cada filtro. Ahora puede elegir entre más operadores lógicos adicionales distintos de "=". Por ejemplo, puede que desee buscar todos los recursos con recomendaciones activas cuyos títulos incluyan la cadena "Encrypt". 

    :::image type="content" source="media/release-notes/inventory-filter-operators.png" alt-text="Controles para la opción de operador en los filtros de Asset Inventory":::

Más información acerca del inventario en [Exploración y administración de los recursos con Asset Inventory](asset-inventory.md).


### <a name="recommendation-about-web-apps-requesting-ssl-certificates-no-longer-part-of-secure-score"></a>La recomendación acerca de las aplicaciones web que solicitan certificados SSL ya no forma parte de la puntuación segura

La recomendación "Las aplicaciones web deben solicitar un certificado SSL a todas las solicitudes entrantes" se ha trasladado desde el control de seguridad **Administración de acceso y permisos** (con una puntuación máxima de 4 puntos) a **Implementación de procedimientos recomendados de seguridad** (con ningún punto). 

Asegurarse de que una aplicación web solicita un certificado ciertamente hace que sea más segura. Sin embargo, para las aplicaciones web de acceso público, esto es irrelevante. Si accede a su sitio a través de HTTP y no de HTTPS, no recibirá ningún certificado de cliente. Por lo tanto, si la aplicación requiere certificados de cliente, no debe permitir solicitudes a la aplicación mediante HTTP. Puede encontrar más información en [Configuración de la autenticación mutua de TLS en Azure App Service](../app-service/app-service-web-configure-tls-mutual-auth.md).

Con este cambio, la recomendación es ahora un procedimiento recomendado que no influye en la puntuación. 

Obtenga información sobre qué recomendaciones se encuentran en cada control de seguridad en [Controles de seguridad y sus recomendaciones](secure-score-security-controls.md#security-controls-and-their-recommendations).


### <a name="recommendations-page-has-new-filters-for-environment-severity-and-available-responses"></a>La página Recomendaciones tiene nuevos filtros de entorno, gravedad y respuestas disponibles

Azure Security Center supervisa todos los recursos conectados y genera recomendaciones de seguridad. Use estas recomendaciones para fortalecer su postura en la nube híbrida y realizar un seguimiento del cumplimiento de las directivas y los estándares importantes para su organización, sector y país.

A medida que Security Center amplía su cobertura y características, la lista de recomendaciones de seguridad crece cada mes. Por ejemplo, consulte [Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).

Con una lista en continuo crecimiento, existe la necesidad de filtrar las recomendaciones para encontrar las de mayor interés. En noviembre, hemos agregado filtros a la página Recomendaciones (consulte [La lista de recomendaciones ahora incluye filtros](#recommendations-list-now-includes-filters)).

Los filtros que se han agregado este mes proporcionan opciones para refinar la lista de recomendaciones según:

- El **entorno**: vea las recomendaciones para los recursos de AWS, GCP o Azure (o cualquier combinación).
- La **gravedad**: vea las recomendaciones según la clasificación de gravedad establecida por Security Center.
- Las **acciones de respuesta**: vea las recomendaciones según la disponibilidad de las opciones de respuesta de Security Center: Corrección rápida, denegación y aplicación

    > [!TIP]
    > El filtro de acciones de respuesta reemplaza al filtro **Solución rápida disponible (Sí/No)** . 
    > 
    > Puede aprender más información acerca de cada una de estas opciones de respuesta:
    > - [Solución por corrección rápida](security-center-remediate-recommendations.md#quick-fix-remediation)
    > - [Evitar errores de configuración con las recomendaciones Aplicar o Denegar](prevent-misconfigurations.md).

:::image type="content" source="./media/release-notes/added-recommendations-filters.png" alt-text="Recomendaciones agrupadas por control de seguridad" lightbox="./media/release-notes/added-recommendations-filters.png":::

### <a name="continuous-export-gets-new-data-types-and-improved-deployifnotexist-policies"></a>La exportación continua obtiene nuevos tipos de datos y directivas deployifnotexist mejoradas

Las herramientas de exportación continua de Azure Security Center permiten exportar las recomendaciones y alertas de Security Center para su uso con otras herramientas de supervisión de su entorno.

La exportación continua le permite personalizar completamente qué se exportará y a dónde irá. Para más información, consulte [Exportación continua de datos de Security Center](continuous-export.md).

Estas herramientas se han mejorado y ampliado de las siguientes maneras:

- **Se han mejorado las directivas deployifnotexist de la exportación continua**. Las directivas ahora:

    - **Comprueban si la configuración está habilitada.** Si no es así, la directiva aparecerá como no compatible y creará un recurso compatible. Obtenga más información acerca de las plantillas de Azure Policy proporcionadas en la pestaña "Implementación a gran escala con Azure Policy" de[Configuración de una exportación continua](continuous-export.md#set-up-a-continuous-export).

    - **Compatibilidad con la exportación de conclusiones de seguridad.** Cuando usa las plantillas de Azure Policy, puede configurar la exportación continua para que incluya conclusiones. Esto es importante cuando se exportan recomendaciones que incluyen "subrecomendaciones", como en el caso de los resultados de los escáneres de evaluación de vulnerabilidades o las actualizaciones específicas del sistema para la recomendación principal "Se deben instalar actualizaciones del sistema en las máquinas".
    
    - **Compatibilidad con la exportación de datos de puntuación de seguridad.**

- **Se han agregado datos de evaluación de cumplimiento normativo (en versión preliminar).** Ahora puede exportar continuamente las actualizaciones de las evaluaciones de cumplimiento normativo, incluidas las iniciativas personalizadas, a un área de trabajo Log Analytics o a un centro de eventos. Esta característica no está disponible en las nubes nacionales o soberanas.

    :::image type="content" source="media/release-notes/continuous-export-regulatory-compliance-option.png" alt-text="Opciones para incluir información de evaluación conforme a la normativa con los datos de exportación continua.":::


## <a name="november-2020"></a>Noviembre de 2020

Las actualizaciones de noviembre incluyen:

- [Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark](#29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark).
- [Se ha agregado NIST SP 800 171 R2 al panel de cumplimiento normativo de Security Center](#nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard).
- [La lista de recomendaciones ahora incluye filtros](#recommendations-list-now-includes-filters)
- [Mejora y ampliación de la experiencia de aprovisionamiento automático](#auto-provisioning-experience-improved-and-expanded)
- [La puntuación de seguridad ahora está disponible en la exportación continua (versión preliminar)](#secure-score-is-now-available-in-continuous-export-preview)
- [La recomendación "Las actualizaciones del sistema deben estar instaladas en las máquinas" ahora incluye recomendaciones secundarias](#system-updates-should-be-installed-on-your-machines-recommendation-now-includes-subrecommendations)
- [En la página de administración de directivas de Azure Portal ahora se muestra el estado de las asignaciones de directivas predeterminadas](#policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments)

### <a name="29-preview-recommendations-added-to-increase-coverage-of-azure-security-benchmark"></a>Se han agregado 29 recomendaciones en versión preliminar para aumentar la cobertura de Azure Security Benchmark.

Azure Security Benchmark es el conjunto de directrices específico de Azure creado por Microsoft para ofrecer los procedimientos recomendados de seguridad y cumplimiento basados en marcos de cumplimiento comunes. [Mas información sobre Azure Security Benchmark](../security/benchmarks/introduction.md).

Se han agregado a Security Center las siguientes 29 recomendaciones en versión preliminar para aumentar la cobertura de la prueba comparativa.

Las recomendaciones de la versión preliminar no representan un recurso incorrecto y no se incluyen en los cálculos de una puntuación segura. Corríjalas siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación. Puede encontrar más información sobre cómo responder a estas recomendaciones en [Recomendaciones de corrección en Azure Security Center](security-center-remediate-recommendations.md).

| Control de seguridad                     | Nuevas recomendaciones                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|--------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cifrado de los datos en tránsito              | - La aplicación de la conexión SSL debe estar habilitada para los servidores de base de datos PostgreSQL.<br>- La aplicación de la conexión SSL debe estar habilitada para los servidores de bases de datos MySQL.<br>- TLS debe actualizarse a la versión más reciente en la aplicación de API.<br>- TLS debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- TLS debe actualizarse a la versión más reciente en la aplicación web.<br>- Es necesario exigir FTPS en la aplicación de API.<br>- Es necesario exigir FTPS en la aplicación de funciones.<br>- Es necesario exigir FTPS en la aplicación web.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Administración de acceso y permisos        | - Las aplicaciones web deben solicitar un certificado SSL a todas las solicitudes entrantes.<br>- Se debe usar una identidad administrada en la aplicación de API.<br>- Se debe usar una identidad administrada en la aplicación de funciones.<br>- Se debe usar una identidad administrada en la aplicación web.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Restricción de los accesos de red no autorizados | - El punto de conexión privado debe estar habilitado para servidores PostgreSQL.<br>- El punto de conexión privado debe estar habilitado para servidores MariaDB.<br>- El punto de conexión privado debe estar habilitado para servidores MySQL.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Habilitar auditoría y registro          | - Los registros de diagnóstico de App Services deben estar habilitados                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Implementación de procedimientos recomendados de seguridad    | - Azure Backup debería habilitarse en las máquinas virtuales.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MariaDB.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for MySQL.<br>- La copia de seguridad con redundancia geográfica debe estar habilitada para Azure Database for PostgreSQL.<br>- PHP debe actualizarse a la versión más reciente en la aplicación de API.<br>- PHP debe actualizarse a la versión más reciente en la aplicación web.<br>- Java debe actualizarse a la versión más reciente en la aplicación de API.<br>- Java debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- Java debe actualizarse a la versión más reciente en la aplicación web.<br>- Python debe actualizarse a la versión más reciente en la aplicación de API.<br>- Python debe actualizarse a la versión más reciente en la aplicación de funciones.<br>- Python debe actualizarse a la versión más reciente en la aplicación web.<br>- La retención de la auditoría en los servidores SQL Server debe establecerse en 90 días, como mínimo. |
|                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Vínculos relacionados:

- [Más información sobre Azure Security Benchmark](../security/benchmarks/introduction.md)
- [Más información sobre las aplicaciones de API de Azure](../app-service/app-service-web-tutorial-rest-api.md)
- [Más información sobre las aplicaciones de funciones de Azure](../azure-functions/functions-overview.md)
- [Más información sobre las aplicaciones web de Azure](../app-service/overview.md)
- [Más información sobre Azure Database for MariaDB](../mariadb/overview.md)
- [Más información sobre Azure Database for MySQL](../mysql/overview.md)
- [Más información sobre Azure Database for PostgreSQL](../postgresql/overview.md)


### <a name="nist-sp-800-171-r2-added-to-security-centers-regulatory-compliance-dashboard"></a>Se ha agregado NIST SP 800 171 R2 al panel de cumplimiento normativo de Security Center.

La norma NIST SP 800-171 R2 ahora está disponible como una iniciativa integrada para su uso con el panel de cumplimiento normativo de Azure Security Center. Las asignaciones de los controles se describen en [Detalles de la iniciativa integrada de cumplimiento normativo NIST SP 800-171 R2](../governance/policy/samples/nist-sp-800-171-r2.md). 

Para aplicar la norma a sus suscripciones y supervisar continuamente el estado de cumplimiento, siga las instrucciones que se describen en [Personalización del conjunto de normas del panel de cumplimiento normativo](update-regulatory-compliance-packages.md).

:::image type="content" source="media/release-notes/nist-sp-800-171-r2-standard.png" alt-text="Norma NIST SP 800 171 R2 en el panel de cumplimiento normativo de Security Center":::

Para más información acerca de este estándar normativo, consulte [NIST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).


### <a name="recommendations-list-now-includes-filters"></a>La lista de recomendaciones ahora incluye filtros

Ahora puede filtrar la lista de recomendaciones de seguridad de acuerdo con una gama de criterios. En el ejemplo siguiente, la lista de recomendaciones se ha filtrado para mostrar recomendaciones que:

- estén **disponibles con carácter general** (es decir, no en versión preliminar).
- sean para **cuentas de almacenamiento**.
- admitan las correcciones de **Corrección rápida**.

:::image type="content" source="media/release-notes/recommendations-filters.png" alt-text="Filtros de la lista de recomendaciones":::


### <a name="auto-provisioning-experience-improved-and-expanded"></a>Mejora y ampliación de la experiencia de aprovisionamiento automático

La característica de aprovisionamiento automático ayuda a reducir la sobrecarga de administración mediante la instalación de las extensiones necesarias en máquinas virtuales de Azure nuevas y existentes para que puedan beneficiarse de la protección de Security Center. 

A medida que crece Azure Security Center, se han desarrollado más extensiones y Security Center puede supervisar una lista más amplia de tipos de recursos. Las herramientas de aprovisionamiento automático ahora se han ampliado para admitir otras extensiones y tipos de recursos que aprovechan las funcionalidades de Azure Policy.

Ahora puede configurar el aprovisionamiento automático de:

- Agente de Log Analytics
- (Nuevo) Complemento de Azure Policy para Kubernetes
- (Nuevo) Microsoft Dependency Agent

Obtenga más información en [Aprovisionamiento automático de agentes y extensiones de Azure Security Center](security-center-enable-data-collection.md).


### <a name="secure-score-is-now-available-in-continuous-export-preview"></a>La puntuación de seguridad ahora está disponible en la exportación continua (versión preliminar)

Con la exportación continua de la puntuación de seguridad, puede transmitir los cambios de la puntuación en tiempo real a Azure Event Hubs o a un área de trabajo de Log Analytics. Use esta funcionalidad para:

- Realizar un seguimiento de la puntuación de seguridad en el tiempo con informes dinámicos
- Exportar datos de puntuación de seguridad a Azure Sentinel (o a cualquier otro SIEM)
- Integrar estos datos con cualquier proceso que ya esté usando para supervisar la puntuación de seguridad en la organización

Obtenga más información sobre la [Exportación continua de datos de Security Center](continuous-export.md).


### <a name="system-updates-should-be-installed-on-your-machines-recommendation-now-includes-subrecommendations"></a>La recomendación "Las actualizaciones del sistema deben estar instaladas en las máquinas" ahora incluye recomendaciones secundarias

La recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas** se ha mejorado. La nueva versión incluye recomendaciones secundarias para cada actualización que falta y ofrece las siguientes mejoras:

- Una nueva experiencia en las páginas de Azure Security Center de Azure Portal. La página de detalles de la recomendación **Las actualizaciones del sistema deben estar instaladas en las máquinas** incluye la lista de conclusiones que se muestran a continuación. Al seleccionar una única búsqueda, se abre el panel de detalles con un vínculo a la información de corrección y una lista de los recursos afectados.

    :::image type="content" source="./media/upcoming-changes/system-updates-should-be-installed-subassessment.png" alt-text="Apertura de una de las recomendaciones secundarias en la experiencia del portal de la recomendación actualizada":::

- Datos enriquecidos para la recomendación de Azure Resource Graph (ARG). ARG es un servicio de Azure diseñado para proporcionar una exploración de recursos eficaz. Puede usar ARG para realizar consultas a escala para un conjunto determinado de suscripciones a fin de controlar eficazmente su entorno. 

    Para Azure Security Center, puede usar ARG y el [lenguaje de consulta Kusto (KQL)](/azure/data-explorer/kusto/query/) para consultar una amplia variedad de datos de posición de seguridad.

    Anteriormente, si consultaba esta recomendación en ARG, la única información disponible era que la recomendación debía corregirse en una máquina. La siguiente consulta de la versión mejorada devolverá las actualizaciones del sistema que faltan agrupadas por máquina.

    ```kusto
    securityresources
    | where type =~ "microsoft.security/assessments/subassessments"
    | where extract(@"(?i)providers/Microsoft.Security/assessments/([^/]*)", 1, id) == "4ab6e3c5-74dd-8b35-9ab9-f61b30875b27"
    | where properties.status.code == "Unhealthy"
    ```

### <a name="policy-management-page-in-the-azure-portal-now-shows-status-of-default-policy-assignments"></a>En la página de administración de directivas de Azure Portal ahora se muestra el estado de las asignaciones de directivas predeterminadas

Ahora puede ver si las suscripciones tienen la directiva de Security Center predeterminada asignada, en la página **Directiva de seguridad** de Security Center en Azure Portal.

:::image type="content" source="media/release-notes/policy-assignment-info-per-subscription.png" alt-text="En la página de administración de directivas de Azure Security Center se muestran las asignaciones de directivas predeterminadas":::

## <a name="october-2020"></a>Octubre de 2020

Las actualizaciones de octubre incluyen:
- [Evaluación de vulnerabilidades para máquinas locales y en varias nubes (versión preliminar)](#vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview)
- [Recomendación sobre Azure Firewall agregada (versión preliminar)](#azure-firewall-recommendation-added-preview)
- [La recomendación de que los intervalos IP autorizados deben definirse en los servicios de Kubernetes se ha actualizado con una corrección rápida](#authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix)
- [El panel de cumplimiento normativo ahora incluye la opción para eliminar estándares](#regulatory-compliance-dashboard-now-includes-option-to-remove-standards)
- [Tabla Microsoft.Security/securityStatuses quitada de Azure Resource Graph (ARG)](#microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg)

### <a name="vulnerability-assessment-for-on-premise-and-multi-cloud-machines-preview"></a>Evaluación de vulnerabilidades para máquinas locales y en varias nubes (versión preliminar)

El analizador de evaluación de vulnerabilidades integrado de [Azure Defender para servidores](defender-for-servers-introduction.md) (con tecnología de Qualys) ahora analiza los servidores habilitados para Azure Arc.

Al habilitar Azure Arc en máquinas que no son de Azure, Security Center le ofrecerá la opción de implementar en ellas el analizador de vulnerabilidades integrado, manualmente y a escala.

Con esta actualización, puede aprovechar toda la capacidad de **Azure Defender para servidores** para consolidar su programa de administración de vulnerabilidades en todos sus recursos, tanto si son de Azure como si no.

Principales funcionalidades:

- Supervisión del estado de aprovisionamiento del analizador de evaluación de vulnerabilidades en máquinas de Azure Arc.
- Aprovisionamiento del agente de evaluación de vulnerabilidades integrado en máquinas de Azure Arc Windows y Linux (manualmente y a escala).
- Recepción y análisis de vulnerabilidades detectadas por agentes implementados (manualmente y a escala).
- Experiencia unificada para máquinas virtuales de Azure y máquinas de Azure Arc.

[Obtenga más información sobre la implementación del analizador de vulnerabilidades integrado en las máquinas híbridas](deploy-vulnerability-assessment-vm.md#deploy-the-integrated-scanner-to-your-azure-and-hybrid-machines).

[Obtenga más información sobre los servidores habilitados para Azure Arc](../azure-arc/servers/index.yml).


### <a name="azure-firewall-recommendation-added-preview"></a>Recomendación sobre Azure Firewall agregada (versión preliminar)

Se ha agregado una nueva recomendación para proteger todas las redes virtuales con Azure Firewall.

La recomendación **Azure Firewall debe proteger las redes virtuales** le aconseja restringir el acceso a las redes virtuales y evitar posibles amenazas mediante el uso de Azure Firewall.

Más información acerca de [Azure Firewall](https://azure.microsoft.com/services/azure-firewall/).


### <a name="authorized-ip-ranges-should-be-defined-on-kubernetes-services-recommendation-updated-with-quick-fix"></a>La recomendación de que los intervalos IP autorizados deben definirse en los servicios de Kubernetes se ha actualizado con una corrección rápida

La recomendación **Authorized IP ranges should be defined on Kubernetes Services** (Los intervalos IP autorizados deben definirse en los servicios de Kubernetes) ahora tiene una opción de corrección rápida.

Para más información sobre esta y todas las demás recomendaciones de Security Center, consulte [Guía de referencia sobre las recomendaciones de seguridad](recommendations-reference.md).

:::image type="content" source="./media/release-notes/authorized-ip-ranges-recommendation.png" alt-text="La recomendación Authorized IP ranges should be defined on Kubernetes Services (Los intervalos IP autorizados deben definirse en los servicios de Kubernetes) con la opción de corrección rápida":::


### <a name="regulatory-compliance-dashboard-now-includes-option-to-remove-standards"></a>El panel de cumplimiento normativo ahora incluye la opción para eliminar estándares

En el panel de cumplimiento normativo de Security Center se proporciona información sobre su postura de cumplimiento en función de cómo cumple los requisitos y controles de cumplimiento específicos.

El panel incluye un conjunto predeterminado de estándares normativos. Si alguno de ellos no es pertinente para su organización, quitarlo de la interfaz de usuario ahora es un proceso sencillo en una suscripción. Los estándares se pueden quitar solo en el nivel de *suscripción*, no en el ámbito del grupo de administración.

Encontrará más información en [Eliminación de un estándar del panel](update-regulatory-compliance-packages.md#removing-a-standard-from-your-dashboard).


### <a name="microsoftsecuritysecuritystatuses-table-removed-from-azure-resource-graph-arg"></a>Tabla Microsoft.Security/securityStatuses quitada de Azure Resource Graph (ARG)

Azure Resource Graph es un servicio de Azure diseñado para proporcionar una exploración de recursos eficaz con la capacidad de consultar a escala a través de un conjunto especificado de suscripciones para que pueda controlar eficazmente el entorno. 

Para Azure Security Center, puede usar ARG y el [lenguaje de consulta Kusto (KQL)](/azure/data-explorer/kusto/query/) para consultar una amplia variedad de datos de posición de seguridad. Por ejemplo:

- El inventario de recursos utiliza ARG.
- Hemos documentado una consulta de ARG de ejemplo sobre la [Identificación de cuentas sin Multi-Factor Authentication (MFA) habilitado](security-center-identity-access.md#identify-accounts-without-multi-factor-authentication-mfa-enabled).

En ARG hay tablas de datos que se pueden usar en las consultas.

:::image type="content" source="./media/release-notes/azure-resource-graph-tables.png" alt-text="Explorador de Azure Resource Graph y tablas disponibles":::

> [!TIP]
> La documentación de ARG muestra todas las tablas disponibles en la [tabla de Azure Resource Graph y la referencia de tipo de recurso](../governance/resource-graph/reference/supported-tables-resources.md).

A partir de esta actualización, se ha quitado la tabla **Microsoft.Security/securityStatuses**. La API securityStatuses sigue estando disponible.

La tabla Microsoft.Security/Assessments puede usar sustitución de datos.

La diferencia principal entre Microsoft.Security/securityStatuses y Microsoft.Security/Assessments es que, mientras que la primera muestra la agregación de las evaluaciones, la segunda contienen un único registro para cada una.

Por ejemplo, Microsoft.Security/securityStatuses devolvería un resultado con una matriz de dos policyAssessments:

```
{
id: "/subscriptions/449bcidd-3470-4804-ab56-2752595 felab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/securityStatuses/mico-rg-vnet",
name: "mico-rg-vnet",
type: "Microsoft.Security/securityStatuses",
properties:  {
    policyAssessments: [
        {assessmentKey: "e3deicce-f4dd-3b34-e496-8b5381bazd7e", category: "Networking", policyName: "Azure DDOS Protection Standard should be enabled",...},
        {assessmentKey: "sefac66a-1ec5-b063-a824-eb28671dc527", category: "Compute", policyName: "",...}
    ],
    securitystateByCategory: [{category: "Networking", securityState: "None" }, {category: "Compute",...],
    name: "GenericResourceHealthProperties",
    type: "VirtualNetwork",
    securitystate: "High"
}
```
Por su parte, Microsoft.Security/Assessments contendrá un registro para cada una de estas evaluaciones de directivas como se indica a continuación:

```
{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft. Network/virtualNetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/e3delcce-f4dd-3b34-e496-8b5381ba2d70",
name: "e3deicce-f4dd-3b34-e496-8b5381ba2d70",
properties:  {
    resourceDetails: {Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourceGroups/mico-rg/providers/Microsoft.Network/virtualNetworks/mico-rg-vnet"...},
    displayName: "Azure DDOS Protection Standard should be enabled",
    status: (code: "NotApplicable", cause: "VnetHasNOAppGateways", description: "There are no Application Gateway resources attached to this Virtual Network"...}
}

{
type: "Microsoft.Security/assessments",
id:  "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet/providers/Microsoft.Security/assessments/80fac66a-1ec5-be63-a824-eb28671dc527",
name: "8efac66a-1ec5-be63-a824-eb28671dc527",
properties: {
    resourceDetails: (Source: "Azure", Id: "/subscriptions/449bc1dd-3470-4804-ab56-2752595f01ab/resourcegroups/mico-rg/providers/microsoft.network/virtualnetworks/mico-rg-vnet"...),
    displayName: "Audit diagnostic setting",
    status:  {code: "Unhealthy"}
}
```

**Ejemplo de conversión de una consulta de ARG existente usando securityStatuses para usar ahora la tabla Assessments:**

Consulta que hace referencia a SecurityStatuses:

```kusto
SecurityResources 
| where type == 'microsoft.security/securitystatuses' and properties.type == 'virtualMachine'
| where name in ({vmnames}) 
| project name, resourceGroup, policyAssesments = properties.policyAssessments, resourceRegion = location, id, resourceDetails = properties.resourceDetails
```

Consulta de sustitución para la tabla Assessments:

```kusto
securityresources
| where type == "microsoft.security/assessments" and id contains "virtualMachine"
| extend resourceName = extract(@"(?i)/([^/]*)/providers/Microsoft.Security/assessments", 1, id)
| extend source = tostring(properties.resourceDetails.Source)
| extend resourceId = trim(" ", tolower(tostring(case(source =~ "azure", properties.resourceDetails.Id,
source =~ "aws", properties.additionalData.AzureResourceId,
source =~ "gcp", properties.additionalData.AzureResourceId,
extract("^(.+)/providers/Microsoft.Security/assessments/.+$",1,id)))))
| extend resourceGroup = tolower(tostring(split(resourceId, "/")[4]))
| where resourceName in ({vmnames}) 
| project resourceName, resourceGroup, resourceRegion = location, id, resourceDetails = properties.additionalData
```

Para más información, consulte los siguientes vínculos:
- [Creación de consultas con Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md)
- [Lenguaje de consulta de Kusto (KQL)](/azure/data-explorer/kusto/query/)


## <a name="september-2020"></a>Septiembre de 2020

Las actualizaciones de septiembre incluyen:
- [Security Center tiene un nuevo aspecto](#security-center-gets-a-new-look)
- [Lanzamiento de Azure Defender](#azure-defender-released)
- [Azure Defender para Key Vault está disponible con carácter general](#azure-defender-for-key-vault-is-generally-available)
- [La protección de Azure Defender para Storage en archivos y ADLS Gen2 está disponible con carácter general](#azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available)
- [Las herramientas para el inventario de recursos ya están disponibles con carácter general](#asset-inventory-tools-are-now-generally-available)
- [Deshabilitación de la detección de una vulnerabilidad específica para análisis de registros de contenedores y máquinas virtuales](#disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines)
- [Exclusión de un recurso de una recomendación](#exempt-a-resource-from-a-recommendation)
- [Los conectores de AWS y GCP de Security Center aportan una experiencia multinube](#aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience)
- [Conjunto de recomendaciones de protección de cargas de trabajo de Kubernetes](#kubernetes-workload-protection-recommendation-bundle)
- [Los resultados de la evaluación de vulnerabilidades ahora están disponibles en la exportación continua](#vulnerability-assessment-findings-are-now-available-in-continuous-export)
- [Imponer recomendaciones al crear recursos para prevenir errores de configuración de seguridad](#prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources)
- [Mejoras de las recomendaciones del grupo de seguridad de red](#network-security-group-recommendations-improved)
- [La recomendación de AKS en versión preliminar "Se deben definir las directivas de seguridad de pod en los servicios de Kubernetes" está en desuso](#deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services)
- [Notificaciones por correo electrónico mejoradas en Azure Security Center](#email-notifications-from-azure-security-center-improved)
- [La puntuación de seguridad no incluye recomendaciones en versión preliminar](#secure-score-doesnt-include-preview-recommendations)
- [Las recomendaciones incluyen ahora un indicador de gravedad y el intervalo de actualización](#recommendations-now-include-a-severity-indicator-and-the-freshness-interval)


### <a name="security-center-gets-a-new-look"></a>Security Center tiene un nuevo aspecto

Hemos lanzado una interfaz de usuario actualizada para las páginas del portal de Security Center. Las nuevas páginas incluyen una nueva página de información general, así como paneles para puntuación de seguridad, inventario de recursos y Azure Defender.

La página de información general renovada ahora incorpora un icono para acceder a los paneles de puntuación de seguridad, inventario de recursos y Azure Defender. También tiene un icono vinculado al panel de cumplimiento normativo.

Más información sobre la [página de información general](overview-page.md)


### <a name="azure-defender-released"></a>Lanzamiento de Azure Defender

**Azure Defender** es la plataforma de protección de cargas de trabajo en la nube (CWPP) integrada en Security Center para la protección avanzada e inteligente de las cargas de trabajo híbridas y de Azure. Reemplaza a la opción del plan de tarifa estándar de Security Center. 

Al habilitar Azure Defender en el área **Precios y configuración** de Azure Security Center, se habilitan a la vez los siguientes planes de Defender, que proporcionan defensas completas para las capas de proceso, datos y servicio de su entorno:

- [Azure Defender para servidores](defender-for-servers-introduction.md)
- [Azure Defender para App Service](defender-for-app-service-introduction.md)
- [Azure Defender para Storage](defender-for-storage-introduction.md)
- [Azure Defender para SQL](defender-for-sql-introduction.md)
- [Azure Defender para Key Vault](defender-for-key-vault-introduction.md)
- [Azure Defender para Kubernetes](defender-for-kubernetes-introduction.md)
- [Azure Defender para registros de contenedor](defender-for-container-registries-introduction.md)

Cada uno de estos planes se explica por separado en la documentación de Security Center.

Con su panel dedicado, Azure Defender proporciona alertas de seguridad y protección contra amenazas avanzada para máquinas virtuales, bases de datos SQL, contenedores, aplicaciones web, la red y mucho más.

[Más información sobre Azure Defender](azure-defender.md)

### <a name="azure-defender-for-key-vault-is-generally-available"></a>Azure Defender para Key Vault está disponible con carácter general

Azure Key Vault es un servicio en la nube que protege las claves de cifrado y los secretos, como certificados, cadenas de conexión y contraseñas. 

**Azure Defender para Key Vault** ofrece protección frente a amenazas avanzada, nativa de Azure, para Azure Key Vault, que proporciona una capa adicional de inteligencia de seguridad. Por extensión, Azure Defender para Key Vault protege por consiguiente muchos de los recursos que dependen de las cuentas de Key Vault.

El plan opcional ahora está en disponibilidad general. Esta característica se encontraba en versión preliminar como "protección contra amenazas avanzada para Azure Key Vault".

Además, las páginas de Key Vault de Azure Portal ahora incluyen una página de **Seguridad** dedicada para las recomendaciones y las alertas de **Security Center**.

Más información en [Azure Defender para Key Vault](defender-for-key-vault-introduction.md)


### <a name="azure-defender-for-storage-protection-for-files-and-adls-gen2-is-generally-available"></a>La protección de Azure Defender para Storage en archivos y ADLS Gen2 está disponible con carácter general 

**Azure Defender para Storage** detecta actividades potencialmente dañinas en las cuentas de Azure Storage. Los datos se pueden proteger tanto si se almacenan como contenedores de blobs, recursos compartidos de archivos o lagos de datos.

La compatibilidad con [Azure Files](../storage/files/storage-files-introduction.md) y [Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md) ahora está disponible con carácter general.

A partir del 1 de octubre de 2020, empezaremos a cobrar por la protección de los recursos de estos servicios.

Más información en [Azure Defender para Storage](defender-for-storage-introduction.md)


### <a name="asset-inventory-tools-are-now-generally-available"></a>Las herramientas para el inventario de recursos ya están disponibles con carácter general

La página de inventario de recursos de Azure Security Center proporciona una sola página para ver la posición de seguridad de los recursos que se han conectado a Security Center.

Security Center analiza periódicamente el estado de seguridad de los recursos de Azure para identificar posibles puntos vulnerables de la seguridad. A continuación, se proporcionan recomendaciones sobre cómo corregir dichos puntos vulnerables.

Cuando algún recurso tenga recomendaciones pendientes, aparecerán en el inventario.

Más información en [Exploración y administración de los recursos con Asset Inventory](asset-inventory.md).



### <a name="disable-a-specific-vulnerability-finding-for-scans-of-container-registries-and-virtual-machines"></a>Deshabilitación de la detección de una vulnerabilidad específica para análisis de registros de contenedores y máquinas virtuales

Azure Defender incluye detectores de vulnerabilidades para examinar imágenes en Azure Container Registry y en las máquinas virtuales.

Si tiene una necesidad organizativa de omitir un resultado, en lugar de corregirlo, tiene la opción de deshabilitarlo. Los resultados deshabilitados no afectan a la puntuación de seguridad ni generan ruido no deseado.

Cuando un resultado coincide con los criterios que ha definido en las reglas de deshabilitación, no aparecerá en la lista de resultados.

Esta opción está disponible en las páginas de detalles de recomendaciones para:

- **Se deben corregir las vulnerabilidades de las imágenes de Azure Container Registry**
- **Es necesario corregir las vulnerabilidades de las máquinas virtuales**

Obtenga más información en [Deshabilitación de resultados específicos para las imágenes de contenedor](defender-for-container-registries-usage.md#disable-specific-findings-preview) y [Deshabilitación de resultados específicos para las máquinas virtuales](remediate-vulnerability-findings-vm.md#disable-specific-findings-preview).


### <a name="exempt-a-resource-from-a-recommendation"></a>Exclusión de un recurso de una recomendación

En ocasiones, un recurso se mostrará como incorrecto en relación con una recomendación específica y, por lo tanto, se reduce la puntuación de seguridad, aunque crea que no debería ser así. Es posible que lo haya corregido un proceso del que Security Center no ha realizado un seguimiento. O, quizás, la organización ha decidido aceptar el riesgo de ese recurso específico. 

En tales casos, puede crear una regla de exención para asegurarse de que el recurso no aparezca entre los recursos incorrectos en el futuro. Estas reglas pueden incluir justificaciones documentadas, tal y como se describe a continuación.

Más información en [Exención de un recurso de las recomendaciones y la puntuación de seguridad](exempt-resource.md)


### <a name="aws-and-gcp-connectors-in-security-center-bring-a-multi-cloud-experience"></a>Los conectores de AWS y GCP de Security Center aportan una experiencia multinube

Las cargas de trabajo de nube abarcan normalmente varias plataformas de nube, por lo que los servicios de seguridad de la nube deben hacer lo mismo.

Azure Security Center ahora protege las cargas de trabajo de Azure, Amazon Web Services (AWS) y Google Cloud Platform (GCP).

Al incorporar las cuentas de AWS y GCP a Security Center, se integran AWS Security Hub, el centro de comandos de seguridad GCP y Azure Security Center. 

Obtenga más información en [Conexión de las cuentas de AWS a Azure Security Center](quickstart-onboard-aws.md) y [Conexión de las cuentas de GCP a Azure Security Center](quickstart-onboard-gcp.md).


### <a name="kubernetes-workload-protection-recommendation-bundle"></a>Conjunto de recomendaciones de protección de cargas de trabajo de Kubernetes

Para asegurarse de que las cargas de trabajo de Kubernetes son seguras de forma predeterminada, Security Center está agregando recomendaciones de protección en el nivel de Kubernetes, incluidas las opciones de cumplimiento con el control de admisión de Kubernetes.

Una vez instalado el complemento de Azure Policy para Kubernetes en el clúster de AKS, todas las solicitudes al servidor de la API de Kubernetes se supervisarán según el conjunto predefinido de procedimientos recomendados antes de que se guarden en el clúster. Después, puede realizar la configurar para aplicar los procedimientos recomendados y exigirlos para futuras cargas de trabajo.

Por ejemplo, puede exigir que no se creen los contenedores con privilegios y que se bloqueen las solicitudes futuras para este fin.

Más información en [Procedimientos recomendados de protección de cargas de trabajo con el control de admisión de Kubernetes](container-security.md#workload-protection-best-practices-using-kubernetes-admission-control)


### <a name="vulnerability-assessment-findings-are-now-available-in-continuous-export"></a>Los resultados de la evaluación de vulnerabilidades ahora están disponibles en la exportación continua

Use la exportación continua para transmitir sus alertas y recomendaciones en tiempo real a Azure Event Hubs, áreas de trabajo de Log Analytics o Azure Monitor. A partir de ahí, puede integrar estos datos con SIEM (como Azure Sentinel, Power BI, Azure Data Explorer y mucho más).

Las herramientas de evaluación de vulnerabilidades integradas en Security Center devuelven resultados sobre los recursos, como recomendaciones útiles dentro de una recomendación "principal", tipo "Es necesario corregir las vulnerabilidades de las máquinas virtuales.". 

Los resultados de seguridad ahora están disponibles para la exportación mediante la exportación continua al seleccionar recomendaciones y habilitar la opción **include security findings** (Incluir resultados de seguridad).

:::image type="content" source="./media/continuous-export/include-security-findings-toggle.png" alt-text="Incluir la alternancia de los resultados de seguridad en la configuración de la exportación continua" :::

Páginas relacionadas:

- [Solución de evaluación de vulnerabilidades integrada en Security Center para Azure Virtual Machines](deploy-vulnerability-assessment-vm.md)
- [Solución de evaluación de vulnerabilidades integrada en Security Center para las imágenes de Azure Container Registry](defender-for-container-registries-usage.md)
- [Exportación continua](continuous-export.md)

### <a name="prevent-security-misconfigurations-by-enforcing-recommendations-when-creating-new-resources"></a>Imponer recomendaciones al crear recursos para prevenir errores de configuración de seguridad

Los errores de configuración de seguridad son una causa principal de los incidentes de seguridad. Security Center ofrece ahora la posibilidad de ayudar a *evitar* errores de configuración de los nuevos recursos con respecto a recomendaciones específicas. 

Esta característica puede ayudar a mantener las cargas de trabajo seguras y a estabilizar la puntuación de seguridad.

La aplicación de una configuración segura, basada en una recomendación específica, se ofrece mediante dos opciones:

- Con el efecto **Denegar** de Azure Policy, puede impedir que se creen recursos incorrectos.

- Con la opción **Exigir**, puede sacar partido del efecto de **DeployIfNotExist** de Azure Policy y corregir automáticamente los recursos no compatibles tras la creación.
 
Está disponible para las recomendaciones de seguridad seleccionadas y puede encontrarse en la parte superior de la página de detalles del recurso.

Más información en [Impedir errores de configuración con las recomendaciones Exigir/Denegar](prevent-misconfigurations.md).

###  <a name="network-security-group-recommendations-improved"></a>Mejoras de las recomendaciones del grupo de seguridad de red

Se han mejorado las siguientes recomendaciones de seguridad relacionadas con los grupos de seguridad de red para reducir algunas instancias de falsos positivos.

- Todos los puertos de red deben estar restringidos en el NSG asociado a la máquina virtual
- Se deben cerrar los puertos de administración en las máquinas virtuales
- Las máquinas virtuales accesibles desde Internet deben estar protegidas con grupos de seguridad de red
- Las subredes deben estar asociadas con un grupo de seguridad de red.


### <a name="deprecated-preview-aks-recommendation-pod-security-policies-should-be-defined-on-kubernetes-services"></a>La recomendación de AKS en versión preliminar "Se deben definir las directivas de seguridad de pod en los servicios de Kubernetes" está en desuso

La recomendación en versión preliminar "Las directivas de seguridad de pod deben definirse en los servicios de Kubernetes" está en desuso, tal y como se describe en la documentación de [Azure Kubernetes Service](../aks/use-pod-security-policies.md).

La característica de la directiva de seguridad de pod (versión preliminar), está programada para quedar en desuso y dejará de estar disponible a partir del 15 de octubre de 2020 en favor de Azure Policy para AKS.

Una vez que la directiva de seguridad de pod (versión preliminar) haya quedado en desuso, deberá deshabilitar la característica en todos los clústeres existentes que la incluyan para realizar futuras actualizaciones de clústeres y seguir recibiendo el soporte técnico de Azure.


### <a name="email-notifications-from-azure-security-center-improved"></a>Notificaciones por correo electrónico mejoradas en Azure Security Center

Se han mejorado las siguientes áreas de los correos electrónicos con respecto a las alertas de seguridad: 

- Se ha agregado la capacidad de enviar notificaciones por correo electrónico sobre las alertas de todos los niveles de gravedad.
- Se ha agregado la capacidad de notificar a los usuarios con diferentes roles de Azure en la suscripción.
- Se notifica de forma proactiva y predeterminada a los propietarios de la suscripción sobre las alertas de gravedad alta (con una probabilidad elevada de que se produzcan infracciones genuinas).
- Se ha eliminado el campo de número de teléfono de la página de configuración de notificaciones por correo electrónico.

Más información en [Configuración de notificaciones de alertas de seguridad por correo electrónico](security-center-provide-security-contact-details.md)


### <a name="secure-score-doesnt-include-preview-recommendations"></a>La puntuación de seguridad no incluye recomendaciones en versión preliminar 

Security Center evalúa continuamente los recursos, suscripciones y la organización en busca de problemas de seguridad. A continuación, agrega todos los resultados a una sola puntuación para que pueda conocer de un vistazo la situación de la seguridad actual: cuanto mayor sea la puntuación, menor será el nivel de riesgo identificado.

A medida que se detectan nuevas amenazas, se habilitan nuevos consejos de seguridad en Security Center mediante recomendaciones nuevas. Para evitar cambios inesperados en la puntuación de seguridad y ofrecer un período de gracia en el que pueda explorar nuevas recomendaciones antes de que afecten a las puntuaciones, las recomendaciones marcadas como **Versión preliminar** ya no se incluyen en los cálculos de la puntuación de seguridad. Aun así, deben corregirse siempre que sea posible, de tal forma que, cuando finalice el período de versión preliminar, contribuyan a la puntuación.

Además, las recomendaciones en **Versión preliminar** no representan un recurso "Incorrecto".

Un ejemplo de una recomendación en versión preliminar:

:::image type="content" source="./media/secure-score-security-controls/example-of-preview-recommendation.png" alt-text="Recomendación con la marca de versión preliminar":::

[Más información sobre la puntuación de seguridad](secure-score-security-controls.md)


### <a name="recommendations-now-include-a-severity-indicator-and-the-freshness-interval"></a>Las recomendaciones incluyen ahora un indicador de gravedad y el intervalo de actualización

La página de detalles de las recomendaciones incluye ahora un indicador del intervalo de actualización (siempre que sea pertinente) y una visualización clara de la gravedad de la recomendación.

:::image type="content" source="./media/release-notes/recommendations-severity-freshness-indicators.png" alt-text="Página de recomendaciones que muestra la actualización y la gravedad":::