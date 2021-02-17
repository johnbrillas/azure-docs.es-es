---
title: Compatibilidad con el movimiento de recursos de Azure entre regiones
description: Enumera los tipos de recursos de Azure que se pueden transferir entre regiones de Azure.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/25/2020
ms.author: raynew
ms.openlocfilehash: 18d4d84462d528b718d784ff6a16ecf990ed0d20
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100094022"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Compatibilidad con el movimiento de recursos de Azure entre regiones

En este artículo se confirma si un tipo de recurso de Azure es compatible para el movimiento a otra región de Azure. 

Vaya a un espacio de nombres del proveedor de recursos:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.Purview](#microsoftpurview)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domainservices | No | 


## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | diagnosticsettings | No |
> | diagnosticsettingscategories | No |
> | privatelinkforazuread | No |
> | tenants |  No |

## <a name="microsoftaddons"></a>microsoft.Addons

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | supportproviders | No |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | aadsupportcases | No |
> | addsservices | No | 
> | agents | No | 
> | anonymousapiusers | No |
> | configuración | No | 
> | logs | No | 
> | reports | No | 
> | servicehealthmetrics | No | 
> | services | No | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | configuraciones | No | 
> | generaterecommendations | No |
> | metadata | No |
> | de películas | No |
> | suppressions | No | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | actionRules | No | 
> | alerts | No | 
> | alertslist | No | 
> | alertsmetadata | No | 
> | alertssummary | No | 
> | alertssummarylist | No | 
> | smartdetectoralertrules | No | 
> | smartgroups | No | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | No |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | reportfeedback | No |
> | service |  Sí (con una plantilla) <br/><br/> [Migración de API Management entre regiones](../../api-management/api-management-howto-migrate.md). | 

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | configurationstores | No | 
> | configurationstores/eventgridfilters | No |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | spring | No | 

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | apiapps | Sí (con una plantilla)<br/><br/> [Traslado de una aplicación de App Service a otra región](../../app-service/manage-move-across-regions.md) | 
> | appidentities | No | 
> | gateways | No | 

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | attestationproviders | No | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | classicadministrators | No | 
> | dataaliases | No | 
> | denyassignments | No | 
> | elevateaccess | No | 
> | findorphanroleassignments | No | 
> | locks | No | 
> | permisos | No | 
> | policyassignments | No | 
> | policydefinitions | No | 
> | policysetdefinitions | No | 
> | privatelinkassociations | No | 
> | resourcemanagementprivatelinks | No | 
> | roleassignments | No | 
> | roleassignmentsusagemetrics | No | 
> | roledefinitions | No | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | automationaccounts | Sí (con una plantilla) <br/><br/> [Uso de la replicación geográfica](../../automation/automation-managing-data.md#geo-replication-in-azure-automation) |  
> | automationaccounts/configurations | No | 
> | automationaccounts/runbooks | No | 

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | Suscripción |
> | ------------- | ----------- | 
> | privateclouds | No | 


## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | b2cdirectories | No | 
> | b2ctenants | No | 

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datacontrollers | No | 
> | hybriddatamanagers | No | 
> | postgresinstances | No | 
> | sqlinstances | No | 
> | sqlmanagedinstances | No |
> | sqlserverinstances | No | 
> | sqlserverregistrations | No |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | cloudmanifestfiles | No |
> | registrations | No | 

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | clusters | No | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | batchaccounts |  Las cuentas de Batch no se pueden migrar directamente de una región a otra, pero puede usar una plantilla para exportar una plantilla, modificarla e implementarla en la nueva región. <br/><br/> Más información sobre [cómo mover una cuenta de Batch entre regiones](../../batch/best-practices.md#moving-batch-accounts-across-regions). |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | billingaccounts | No | 
> | billingperiods | No | 
> | billingpermissions | No | 
> | billingproperty | No | 
> | billingroleassignments | No | 
> | billingroledefinitions | No | 
> | departments | No | 
> | enrollmentaccounts | No | 
> | invoices | No | 
> | transfers | No | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | mapapis | No | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | biztalk | No | 

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | blockchainmembers | No <br/><br/> La red de cadena de bloques no puede tener nodos en regiones diferentes. 
> | cordamembers | No |
> | watchers | No | 

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | tokenservices | No |


## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | blueprintassignments | No | 
> | blueprints | No |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | botservices | No | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | redis | No | 
> | redisenterprise | No | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | appliedreservations | No | 
> | calculateexchange | No | 
> | calculateprice | No | 
> | calculatepurchaseprice | No | 
> | catalogs | No | 
> | commercialreservationorders | No | 
> | cambio | No |
> | reservationorders | No | 
> | reservations | No | 
> | resources | No | 
> | validatereservationorder | No | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | No |
> | edgenodes | No
> | profiles | No | 
> | profiles/endpoints | No | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | certificateorders | No | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | capabilities | No | 
> | domainnames | Sí | No |
> | quotas | No | 
> | resourcetypes | No |
> | validatesubscriptionmoveavailability | No | 
> | virtualmachines | No 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | classicinfrastructureresources | No | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | capabilities | No | 
> | expressroutecrossconnections | No | 
> | expressroutecrossconnections/peerings | No | 
> | gatewaysupporteddevices | No | 
> | networksecuritygroups | No |
> | quotas | No |
> | reservedips | No | 
> | virtualnetworks | No | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | disks | No | 
> | images | No | 
> | osimages | No | 
> | osplatformimages | No | 
> | publicimages | No | 
> | quotas | No | 
> | storageaccounts | Sí |  
> | vmimages | No |

## <a name="microsoftclassicsubscription"></a>Microsoft.ClassicSubscription

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | operaciones | No | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 
> | Cognitive Search | Se admite con pasos manuales.<br/><br/> Más información sobre cómo [mover el servicio Azure Cognitive Search a otra región](../../search/search-howto-move-across-regions.md).

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | ratecard | No | 
> | usageaggregates | No | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | availabilitysets | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para mover conjuntos de disponibilidad. | 
> | diskaccesses | No |
> | diskencryptionsets | No | 
> | disks | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar máquinas virtuales de Azure y discos relacionados. | 
> | galleries | No | 
> | galleries/images | No | 
> | galleries/images/versions | No | 
> | hostgroups | No | 
> | hostgroups/hosts | No | 
> | images | No | 
> | proximityplacementgroups | No | 
> | restorepointcollections | No | 
> | sharedvmimages | No | 
> | sharedvmimages/versions | No | 
> | snapshots | No | 
> | sshpublickeys | No |
> | virtualmachines | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar máquinas virtuales de Azure. | 
> | virtualmachines/extensions | No | 
> | virtualmachinescalesets | No | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | aggregatedcost | No | 
> | balances | No | 
> | budgets | No | 
> | charges | No | 
> | costtags | No | 
> | credits | No | 
> | events | No | 
> | forecasts | No | 
> | lots | No | 
> | marketplaces | No | 
> | pricesheets | No | 
> | products | No | 
> | reservationdetails | No | 
> | reservationrecommendationdetails | No | 
> | reservationrecommendations | No | 
> | reservationsummaries | No | 
> | reservationtransactions | No | 
> | etiquetas | No | 
> | tenants | No | 
> | terms | No | 
> | usagedetails | No | 


## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | containergroups | No | 
> | serviceassociationlinks | No |


## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | registries | No |  
> | registries / agentpools | No | 
> | registries/buildtasks | No |  
> | registries/replications | No | 
> | registries/tasks | No |  
> | registries/webhooks | No | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | containerservices | No |
> | managedclusters | No | 
> | openshiftmanagedclusters | No | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications | No | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | alerts | No | 
> | billingaccounts | No | 
> | budgets | No | 
> | cloudconnectors | No | 
> | conectores | No | 
> | departments | No | 
> | dimensions | No | 
> | enrollmentaccounts | No | 
> | exports | No | 
> | externalbillingaccounts | No | 
> | forecast | No | 
> | Query | No | 
> | registro | No | 
> | reportconfigs | No | 
> | reports | No | 
> | configuración | No | 
> | showbackrules | No | 
> | views | No | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hubs | No |  

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | Solicitudes | No | 

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | associations | No |
> | resourceproviders | No | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | jobs | No | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | availableskus | No |
> | databoxedgedevices | No | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspaces | No | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | catalogs | No | 
> | datacatalogs | No | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | connectionmanagers | No | 

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | packages | No | 
> | plans | No | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datafactories | No | 
> | factories | No |  

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datalakeaccounts | No | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | No | 
> | services/projects | No | 
> | slots | No | 

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | ---------- |
> | backupvaults | No | 

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | Puede usar una réplica de lectura entre regiones para trasladar un servidor existente. [Más información](../../postgresql/howto-move-regions-portal.md).<br/><br/> Si el servicio se aprovisiona con almacenamiento de copia de seguridad con redundancia geográfica, puede usar la restauración geográfica para restaurar en otras regiones. [Más información](../../mariadb/concepts-business-continuity.md#recover-from-an-azure-regional-data-center-outage).

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | Puede usar una réplica de lectura entre regiones para trasladar un servidor existente. [Más información](../../mysql/howto-move-regions-portal.md).

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servergroups | No | 
> | servers | Puede usar una réplica de lectura entre regiones para trasladar un servidor existente. [Más información](../../postgresql/howto-move-regions-portal.md).
> | serversv2 | No | 

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | artifactsources | No | 
> | rollouts | No |  
> | servicetopologies | No | 
> | servicetopologies/services | No |  
> | servicetopologies/services/serviceunits | No | 
> | steps | No | 


## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | applicationgroups | No | 
> | workspaces | No | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | elasticpools | No. El recurso no está expuesto.
> | elasticpools/iothubtenants | No. El recurso no está expuesto.
> | iothubs | Sí. [Más información](../../iot-hub/iot-hub-how-to-clone.md)
> | provisioningservices | No | 

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | controllers | No | 


## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | controllers | No | 
> | Clúster de AKS | No<br/><br/> [Más información](../../dev-spaces/faq.md#can-i-migrate-my-aks-cluster-with-azure-dev-spaces-to-another-region) sobre el movimiento a otra región.

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | labcenters | No | 
> | labs | No | 
> | labs/environments | No |  
> | labs/servicerunners | No | 
> | labs/virtualmachines | No |  
> | schedules | No |  

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | digitaltwinsinstances | Sí, se vuelven a crear los recursos en una nueva región. [Más información](../../digital-twins/how-to-move-regions.md) |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | databaseaccounts | No | 
> | databaseaccounts | No | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domains | No | 
> | generatessorequest | No | 
> | topleveldomains | No | 
> | validatedomainregistrationinformation | No |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | No |  

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domains | No | 
> | eventsubscriptions | No |
> | extensiontopics | No | 
> | partnernamespaces | No | 
> | partnerregistrations | No | 
> | partnertopics | No | 
> | systemtopics | No | 
> | topics | No | 
> | topictypes | No | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | No |  
> | espacios de nombres | Sí (con una plantilla)<br/><br/> [Movimiento de un espacio de nombres de Event Hubs a otra región](../../event-hubs/move-across-regions.md) | 
> | sku | No |  

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | experimentworkspaces | No | 

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | espacios de nombres | No | 

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | featureproviders | No | 
> | features | No | 
> | providers | No | 
> | subscriptionfeatureregistrations | No | 

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | automanagedaccounts | No | 
> | automanagedvmconfigurationprofiles | No | 
> | guestconfigurationassignments | No | 
> | software | No | 
> | softwareupdateprofile | No | 
> | softwareupdates | No | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hanainstances | No | 
> | sapmonitors | No |  

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | dedicatedhsms | No | 


## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | No | 

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | No |  

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | machines | No | 
> | machines/extensions | No |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datamanagers |  No | 

## <a name="microsofthybridnetwork"></a>Microsoft.HybridNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | devices | No | 
> | vnfs | No | 

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | components | No | 
> | networkscopes | No | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | jobs |  No | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No. [Más información](../../azure-monitor/faq.md#how-do-i-move-an-application-insights-resource-to-a-new-region).
> | actiongroups |  No | 
> | activitylogalerts | No | 
> | alertrules |  No | 
> | autoscalesettings |  No | 
> | baseline | No |
> | components |  No |  
> | datacollectionrules | No | 
> | diagnosticsettings | No | 
> | diagnosticsettingscategories | No | 
> | eventcategories | No | 
> | eventtypes | No | 
> | extendeddiagnosticsettings | No | |
> | guestdiagnosticsettings | No | 
> | listmigrationdate | No | 
> | logdefinitions | No | 
> | logprofiles | No | 
> | logs | No | No |
> | metricalerts | No | 
> | metricbaselines | No | 
> | metricbatch | No | 
> | metricdefinitions | No | 
> | metricnamespaces | No | 
> | Métricas | No | 
> | migratealertrules | No |
> | migratetonewpricingmodel | No | 
> | myworkbooks | No |
> | notificationgroups | No | 
> | privatelinkscopes | No |
> | rollbacktolegacypricingmodel | No |
> | scheduledqueryrules |  No | 
> | Topología | No |
> | transacciones | No |
> | vminsightsonboardingstatuses | No |
> | webtests |  No | 
> | webtests/gettestresultfile | No |
> | workbooks |  No |  
> | workbooktemplates | No |


## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | apptemplates | No | 
> | iotapps | No | 



## <a name="microsoftiothub"></a>Microsoft.IoTHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> |  iothub |  Sí (clonación del centro) <br/><br/> [Clonación de una instancia de IoT Hub en otra región](../../iot-hub/iot-hub-how-to-clone.md)

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región |
> | ------------- | ----------- | 
> | graph | No | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | deletedvaults | No |
> | hsmpools | No | 
> | managedhsms | No |
> | vaults |  No | 

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | connectedclusters | No | 
> | registeredsubscriptions | No | 

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | sourcecontrolconfigurations | No | 

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters |  No |  

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | labaccounts | No | 
> | users | No | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No, es un servicio global.

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hostingenvironments | No | 
> | integrationaccounts |  No |  
> | integrationserviceenvironments | No | 
> | integrationserviceenvironments / managedapis | No |
> | isolatedenvironments | No | 
> | workflows |  No |  

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | commitmentplans |  No | 
> | webservices |  No | 
> | workspaces |  No | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | operationalizationclusters |  No | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 
> | teamaccounts | No | 


## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 


## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspaces | No | 

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | configurationassignments | Sí. [Más información](../../virtual-machines/move-region-maintenance-configuration.md) | 
> | maintenanceconfigurations | Sí. [Más información](../../virtual-machines/move-region-maintenance-configuration-resources.md) |
> | updates | No | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | Identidades | No | 
> | userassignedidentities | No | 

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | managednetworks | No | 
> | managednetworks / managednetworkgroups | No |
> | managednetworks / managednetworkpeeringpolicies | No | 
> | notificación | No | 

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | marketplaceregistrationdefinitions | No | 
> | registrationassignments | No |
> | registrationdefinitions | No | 

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | getentities | No | 
> | managementgroups | No | 
> | managementgroups/settings | No | 
> | resources | No | 
> | starttenantbackfill | No | 
> | tenantbackfillstatus | No | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts |  No, Azure Maps es un servicio geoespacial. 
> | accounts / privateatlases | No

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | offers | No | 
> | offertypes | No | 
> | privategalleryitems | No | 
> | privatestoreclient | No | 
> | privatestores | No | 
> | products | No | 
> | publishers | No | 
> | registro | No | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | classicdevservices | No | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | agreements | No | 
> | offertypes | No | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | mediaservices |  No | 
> | mediaservices/liveevents |  No | 
> | mediaservices/streamingendpoints |  No | 

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | appclusters | No | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | assessmentprojects | No | 
> | migrateprojects | No | 
> | movecollections | No
> | projects | No | 

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | No | 
> | objectunderstandingaccounts | No | 
> | remoterenderingaccounts | No | 
> | spatialanchorsaccounts | No | 

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | netappaccounts | No | 
> | netappaccounts/capacitypools | No | 
> | netappaccounts/capacitypools/volumes | No | 
> | netappaccounts/capacitypools/volumes/mounttargets | No | 
> | netappaccounts/capacitypools/volumes/snapshots | No | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applicationgateways | No |
> | applicationgatewaywebapplicationfirewallpolicies | No | 
> | applicationsecuritygroups |  No |  
> | azurefirewalls |  No |  
> | bastionhosts | No | 
> | bgpservicecommunities | No |
> | connections |  No | 
> | ddoscustompolicies |  No | 
> | ddosprotectionplans | No | 
> | dnszones |  No | 
> | expressroutecircuits | No | 
> | expressroutegateways | No | 
> | expressrouteserviceproviders | No | 
> | firewallpolicies | No |
> | frontdoors | No | 
> | ipallocations | No |
> | ipgroups | No |
> | loadbalancers | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar equilibradores de carga externos e internos. |
> | localnetworkgateways |  No | 
> | natgateways |  No | 
> | networkexperimentprofiles | No |
> | networkintentpolicies |  No | 
> | networkinterfaces | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar adaptadores de red. | 
> | networkprofiles | No | 
> | networksecuritygroups | Sí <br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar los grupos de seguridad de red (NGS). | 
> | networkwatchers |  No |  
> | networkwatchers/connectionmonitors |  No | 
> | networkwatchers / flowlogs |  No | 
> | networkwatchers/pingmeshes |  No | 
> | p2svpngateways | No | 
> | privatednszones |  No |  
> | privatednszones/virtualnetworklinks | No |> | privatednszonesinternal | No |
> | privateendpointredirectmaps | No |
> | privateendpoints | No | 
> | privatelinkservices | No | 
> | publicipaddresses | Sí<br/><br/> Use [Azure Resource Mover](../../resource-mover/tutorial-move-region-virtual-machines.md) para trasladar direcciones IP públicas. |
> | publicipprefixes | No | 
> | routefilters | No | 
> | routetables |  No | 
> | securitypartnerproviders | No |
> | serviceendpointpolicies |  No | 
> | trafficmanagergeographichierarchies | No | 
> | trafficmanagerprofiles |  No | 
> | trafficmanagerusermetricskeys | No |
> | virtualhubs | No | 
> | virtualnetworkgateways |  No |  
> | virtualnetworks |  No | 
> | virtualnetworktaps | No | 
> | virtualwans | No | 
> | vpngateways (Virtual WAN) | No | 
> | vpnsites (Virtual WAN) | No | 
> | vpnsites (Virtual WAN) | No |


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | espacios de nombres |  No | 
> | namespaces/notificationhubs |  No |  

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | osnamespaces | No | 

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | hypervsites | No | 
> | importsites | No | 
> | serversites | No | 
> | vmwaresites | No | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | No | 
> | deletedworkspaces | No | 
> | linktargets | No | 
> | storageinsightconfigs | No |
> | workspaces | No |



## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | managementassociations | No |
> | managementconfigurations |  No | 
> | solutions | No |
> | views |  No | 

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | legacypeerings | No | 
> | peerasns | No | 
> | peeringlocations | No | 
> | peerings | No | 
> | peeringservicecountries | No | 
> | peeringservicelocations | No | 
> | peeringserviceproviders | No | 
> | peeringservices | No | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | policyevents | No | 
> | policystates | No | 
> | policytrackedresources | No | 
> | remediations | No | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | consoles | No |
> | dashboards | No | 
> | usersettings | No | 


## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspacecollections |  No | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | capacities |  No | 

## <a name="microsoftpurview"></a>Microsoft.Purview

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | No | 

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | availableaccounts | No | 
> | providerregistrations | No | 
> | rollouts | No | 

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | workspaces | No | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | replicationeligibilityresults | No |
> | vaults | No.<br/><br/> No se admite la migración de almacenes de Recovery Services para Azure Backup entre regiones de Azure.<br/><br/> En almacenes de Recovery Services para Azure Site Recovery, puede [deshabilitar y volver a crear el almacén](../../site-recovery/move-vaults-across-regions.md) en la región de destino. | 

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | openshiftclusters | No | 

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | espacios de nombres |  No | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | Consultas |  No |  
> | resourcechangedetails | No | 
> | resourcechanges | No | 
> | resources | No | 
> | resourceshistory | No | 
> | subscriptionsstatus | No | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | childresources | No | 
> | emergingissues | No | 
> | events | No | 
> | metadata | No | 
> | Notificaciones | No | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región |
> | ------------- | ----------- |
> | deploymentScripts |  Sí<br/><br/>[Mover recursos de Microsoft.Resources a nueva región](microsoft-resources-move-regions.md) |
> | templateSpecs |  Sí<br/><br/>[Mover recursos de Microsoft.Resources a nueva región](microsoft-resources-move-regions.md) |  

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications |  No | 
> | saasresources | No | 


## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | resourcehealthmetadata | No |
> | searchservices |  No | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | adaptivenetworkhardenings | No | 
> | advancedthreatprotectionsettings | No | 
> | alerts | No | 
> | allowedconnections | No | 
> | applicationwhitelistings | No | 
> | assessmentmetadata | No | 
> | assessments | No | 
> | autodismissalertsrules | No | 
> | automations | No | 
> | autoprovisioningsettings | No |
> | complianceresults | No | 
> | compliances | No | 
> | datacollectionagents | No | 
> | devicesecuritygroups | No | 
> | discoveredsecuritysolutions | No | 
> | externalsecuritysolutions | No | 
> | informationprotectionpolicies | No | 
> | iotsecuritysolutions | No | 
> | iotsecuritysolutions/analyticsmodels | No | 
> | iotsecuritysolutions/analyticsmodels/aggregatedalerts | No | 
> | iotsecuritysolutions/analyticsmodels/aggregatedrecommendations | No | 
> | jitnetworkaccesspolicies | No | 
> | directivas | No | 
> | pricings | No | 
> | regulatorycompliancestandards | No | 
> | regulatorycompliancestandards/regulatorycompliancecontrols | No | 
> | regulatorycompliancestandards/regulatorycompliancecontrols/regulatorycomplianceassessments | No | 
> | securitycontacts | No | 
> | securitysolutions | No | 
> | securitysolutionsreferencedata | No | 
> | securitystatuses | No | 
> | securitystatusessummaries | No | 
> | servervulnerabilityassessments | No | 
> | configuración | No | 
> | subassessments | No |
> | tareas | No | 
> | topologies | No | 
> | workspacesettings | No | 

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | aggregations | No | 
> | alertrules | No | 
> | alertruletemplates | No | 
> | automationrules | No |
> | cases | No | 
> | dataconnectors | No | 
> | entities | No | 
> | entityqueries | No |
> | incidents | No | 
> | officeconsents | No | 
> | configuración | No | 
> | threatintelligence | No | 

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | consoleservices | No | 

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | gateways | No | 
> | nodes | No | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | espacios de nombres |  No | 
> | premiummessagingregions | No | 
> | sku | No | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications | No | 
> | clusters |  No |  
> | containergroups | No | 
> | containergroupsets | No | 
> | edgeclusters | No | 
> | managedclusters | No |
> | networks | No | 
> | secretstores | No | 
> | volumes | No | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications |  No | 
> | containergroups | No | 
> | gateways |  No | 
> | networks |  No | 
> | secrets |  No | 
> | volumes |  No |  

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | rollouts | No | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | signalr |  No |  

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | hybridusebenefits | No | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | appliancedefinitions | No | 
> | appliances | No | 
> | jitrequests | No | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | instancepools | No | 
> | locations | No |
> | managedinstances | Sí <br/><br/> [Más información](../../azure-sql/database/move-resources-across-regions.md) sobre cómo trasladar instancias administradas entre regiones. | 
> | managedinstances/databases | Sí | 
> | servers | Sí | 
> | servers/databases | Sí <br/><br/> [Más información](../../azure-sql/database/move-resources-across-regions.md) sobre cómo trasladar bases de datos entre regiones.<br/><br/> [Más información](../../resource-mover/tutorial-move-region-sql.md) sobre el uso de Azure Resource Mover para trasladar las bases de datos de Azure SQL.  | 
> | servers/elasticpools | Sí <br/><br/> [Más información](../../azure-sql/database/move-resources-across-regions.md) sobre cómo trasladar grupos elásticos entre regiones.<br/><br/> [Más información](../../resource-mover/tutorial-move-region-sql.md) sobre el uso de Azure Resource Mover para trasladar los grupos elásticos de Azure SQL.  | 
> | virtualclusters | Sí | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  No |  
> | sqlvirtualmachines |  No |  


## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storageaccounts | Sí<br/><br/> [Traslado de una cuenta de Azure Storage a otra región](../../storage/common/storage-account-move.md) | 

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | caches | No | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices |  No | 

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices | No | 

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices | No | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | managers | No | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | No |
> | streamingjobs |  No |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | environments | No | 
> | instances | No | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | subscriptions | No | 

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | services | No | 
> | supporttickets | No | 

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | workspaces | No | 
> | workspaces / bigdatapools | No | 
> | workspaces / sqlpools | No | 


## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | environments |  No | 
> | environments/eventsources |  No |  
> | environments/referencedatasets |  No | 

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | almacenes | No | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | imagetemplates | No | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | account |  No | 
> | account/extension |  No | 
> | account/project |  No | 

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | arczones | No | 
> | resourcepools | No | 
> | vcenters | No | 
> | virtualmachines | No | 
> | virtualmachinetemplates | No | 
> | virtualnetworks | No | 

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | dedicatedcloudnodes | No | 
> | dedicatedcloudservices | No | 
> | virtualmachines | No | 

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | devices | No | 
> | vnfs | No | 

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | accounts | No | 
> | plans | No | 
> | registeredsubscriptions | No |


## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | availablestacks | No | 
> | billingmeters | No | 
> | certificates | No | 
> | connectiongateways |  No |  
> | connections |  No |  
> | customapis |  No | 
> | deletedsites | No | 
> | deploymentlocations | No | 
> | georegions | No | 
> | hostingenvironments | No | 
> | kubeenvironments | No | 
> | publishingusers | No |
> | de películas | No | 
> | resourcehealthmetadata | No | 
> | runtimes | No | 
> | serverfarms | No |  
> | serverfarms/eventgridfilters | No
> | sites |  No | 
> | sites/premieraddons |  No |  
> | sites/slots |  No |  
> | sourcecontrols | No |
> | staticsites | No | 

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | multipleactivationkeys | No |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | deviceservices | No | 

## <a name="microsoftworkloadbuilder"></a>Microsoft.WorkloadBuilder

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | workloads | No | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | components | No |
> | componentssummary | No | 
> | monitorinstances | No | 
> | monitorinstancessummary | No | 
> | monitors | No | 
## <a name="third-party-services"></a>Servicios de terceros

Actualmente los servicios de terceros no son compatibles con la operación de traslado.

## <a name="next-steps"></a>Pasos siguientes

[Más información](../../resource-mover/overview.md) sobre el servicio Resource Mover.

