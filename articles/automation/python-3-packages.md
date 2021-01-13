---
title: Administración de paquetes de Python 3 en Azure Automation
description: En este artículo se describe cómo administrar paquetes de Python 3 (versión preliminar) en Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 12/22/2020
ms.topic: conceptual
ms.openlocfilehash: 3f39f49ff47b935da7ffc777ee45bd219f5740b5
ms.sourcegitcommit: f7084d3d80c4bc8e69b9eb05dfd30e8e195994d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2020
ms.locfileid: "97734308"
---
# <a name="manage-python-3-packages-preview-in-azure-automation"></a>Administración de paquetes de Python 3 (versión preliminar) en Azure Automation

Azure Automation permite ejecutar runbooks de Python 3 (versión preliminar) en Azure y en Hybrid Runbook Worker de Linux. Para simplificar los runbooks, puede usar los paquetes de Python para importar los módulos que necesite. En este artículo se describe cómo administrar y usar paquetes de Python 3 (versión preliminar) en Azure Automation.

## <a name="import-packages"></a>Importación de paquetes

Seleccione **Paquetes de Python** en **Recursos compartidos**, en la cuenta de Automation. Seleccione **+ Agregar un paquete de Python**.

:::image type="content" source="media/python-3-packages/add-python-3-package.png" alt-text="Captura de pantalla de la página Paquetes de Python 3 que muestra los paquetes de Python 3 en el menú izquierdo y la opción para agregar un paquete de Python 2 resaltada.":::

En la página **Agregar un paquete de Python**, seleccione Python 3 para la **versión** y elija un paquete local para cargar. El paquete puede ser un archivo **.whl** o **.tar.gz**. Con el paquete seleccionado, haga clic en **Aceptar** para cargarlo.

:::image type="content" source="media/python-3-packages/upload-package.png" alt-text="Captura de pantalla que muestra la página Agregar un paquete de Python 3, con un archivo tar.gz cargado y seleccionado.":::

Una vez que se ha importado un paquete, se muestra en la página paquetes de Python de la cuenta de Automation, en la pestaña **Paquetes de Python 3 (versión preliminar)** . Si necesita quitar un paquete, selecciónelo y haga clic en **Eliminar**.

:::image type="content" source="media/python-3-packages/python-3-packages-list.png" alt-text="Captura de pantalla que muestra la página Paquetes de Python 3 después de importar un paquete.":::

## <a name="import-packages-with-dependencies"></a>Importación de paquetes con dependencias

Azure Automation no resuelve las dependencias de los paquetes de Python durante el proceso de importación. Sin embargo, hay una manera de importar un paquete con todas sus dependencias.

### <a name="manually-download"></a>Descarga manual

En una máquina Windows de 64 bits con [Python 3.8](https://www.python.org/downloads/release/python-380/) y [pip](https://pip.pypa.io/en/stable/) instalados, ejecute el siguiente comando para descargar un paquete y todas sus dependencias:

```cmd
C:\Python38\Scripts\pip3.8.exe download -d <output dir> <package name>
```

Una vez que se han descargado los paquetes, puede importarlos en la cuenta de Automation.

## <a name="use-a-package-in-a-runbook"></a>Usar un paquete en un runbook

Puede usar el paquete importado en un runbook. Agregue el código siguiente para mostrar todos los grupos de recursos de una suscripción de Azure.

```python
import os  
import azure.mgmt.resource  
import automationassets  

def get_automation_runas_credential(runas_connection):  
    from OpenSSL import crypto  
    import binascii  
    from msrestazure import azure_active_directory  
    import adal 

    # Get the Azure Automation RunAs service principal certificate  
    cert = automationassets.get_automation_certificate("AzureRunAsCertificate")  
    pks12_cert = crypto.load_pkcs12(cert)  
    pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())  

    # Get run as connection information for the Azure Automation service principal 
    application_id = runas_connection["ApplicationId"]  
    thumbprint = runas_connection["CertificateThumbprint"]  
    tenant_id = runas_connection["TenantId"]  

    # Authenticate with service principal certificate  
    resource ="https://management.core.windows.net/"  
    authority_url = ("https://login.microsoftonline.com/"+tenant_id)  
    context = adal.AuthenticationContext(authority_url)  
    return azure_active_directory.AdalAuthentication(  
    lambda: context.acquire_token_with_client_certificate(  
            resource,  
            application_id,  
            pem_pkey,  
            thumbprint) 
    ) 

# Authenticate to Azure using the Azure Automation RunAs service principal  
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")  
azure_credential = get_automation_runas_credential(runas_connection)  

# Intialize the resource management client with the RunAs credential and subscription  
resource_client = azure.mgmt.resource.ResourceManagementClient(  
    azure_credential,  
    str(runas_connection["SubscriptionId"]))  

# Get list of resource groups and print them out  
groups = resource_client.resource_groups.list()  
for group in groups:  
    print(group.name) 
```

## <a name="next-steps"></a>Pasos siguientes

Para preparar un runbook de Python, consulte [Creación de un runbook de Python](learn/automation-tutorial-runbook-textual-python-3.md).
