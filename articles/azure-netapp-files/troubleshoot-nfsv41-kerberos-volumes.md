---
title: Solución de problemas de volúmenes de NFSv4.1 de Kerberos para Azure NetApp Files | Microsoft Docs
description: Aquí se describen los mensajes de error y propuestas que pueden ayudar a solucionar los problemas de volúmenes de NFSv4.1 de Kerberos para Azure NetApp Files.
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
ms.topic: troubleshooting
ms.date: 01/12/2021
ms.author: b-juche
ms.openlocfilehash: 638607da02b1db4842cdc04f86a4fed1860c243f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98134320"
---
# <a name="troubleshoot-nfsv41-kerberos-volume-issues"></a>Solución de problemas de los volúmenes NFSv4.1 de Kerberos 

En este artículo se describen las soluciones a las condiciones de error que pueden darse al crear o administrar volúmenes de NFSv4.1 de Kerberos. 

## <a name="error-conditions-and-resolutions"></a>Condiciones de error y soluciones

|     Condiciones del error    |     Soluciones    |
|-|-|
|`Error allocating volume - Export policy rules does not match kerberosEnabled flag` | Azure NetApp Files no admite Kerberos para los volúmenes de NFSv3. Kerberos solo es compatible con el protocolo de NFSv4.1.  |
|`This NetApp account has no configured Active Directory   connections`  |  Configure Active Directory para la cuenta de NetApp con los campos **KDC IP** y **Nombre del servidor de AD**. Consulte [Configuración de Azure Portal](configure-kerberos-encryption.md#configure-the-azure-portal) para obtener instrucciones. |
|`Mismatch between KerberosEnabled flag value and ExportPolicyRule's access type parameter values.`  | Azure NetApp Files no admite la conversión de un volumen de NFSv4.1 sin formato en un volumen de NFSv4.1 de Kerberos y viceversa. |
|`mount.nfs: access denied by server when mounting volume <SMB_SERVER_NAME-XXX.DOMAIN_NAME>/<VOLUME_NAME>` <br>  Ejemplo: `smb-test-64d9.contoso.com:/nfs41-vol101` | <ol><li> Asegúrese de que los registros A/PTR están configurados correctamente y existen en la instancia de Active Directory correspondiente al nombre de servidor `smb-test-64d9.contoso.com`. <br> En el cliente NFS, si `nslookup` de `smb-test-64d9.contoso.com` se resuelve en la dirección IP IP1 (es decir, `10.1.1.68`), después `nslookup` de IP1 debe resolverse en un solo registro (es decir, `smb-test-64d9.contoso.com`). `nslookup` de IP1 *no debe* resolverse en varios nombres. </li>  <li>Establezca AES-256 para la cuenta de máquina NFS de tipo `NFS-<Smb NETBIOS NAME>-<few random characters>` en AD mediante PowerShell o la interfaz de usuario. <br> Comandos de ejemplo: <ul><li>`Set-ADComputer <NFS_MACHINE_ACCOUNT_NAME> -KerberosEncryptionType AES256` </li><li>`Set-ADComputer NFS-SMB-TEST-64 -KerberosEncryptionType AES256` </li></ul> </li> <li>Asegúrese de que la hora del cliente NFS, AD y el software de almacenamiento de Azure NetApp Files estén sincronizadas entre sí y se encuentren dentro de un rango de sesgo de cinco minutos. </li>  <li>Obtenga el vale de Kerberos correspondiente al cliente NFS mediante el comando `kinit <administrator>`.</li> <li>Reduzca el nombre de host del cliente NFS a menos de 15 caracteres y vuelva a realizar la operación realm join. </li><li>Reinicie el cliente NFS y el servicio `rpcgssd` como se indica a continuación. El comando puede variar en función del sistema operativo.<br> RHEL 7: <br> `service nfs restart` <br> `service rpcgssd restart` <br> CentOS 8: <br> `systemctl enable nfs-client.target && systemctl start nfs-client.target` <br> Ubuntu: <br> (Reinicie el servicio `rpc-gssd`). <br> `sudo systemctl start rpc-gssd.service` </ul>| 
|`mount.nfs: an incorrect mount option was specified`   | El problema puede estar relacionado con el problema del cliente NFS. Reinicie el cliente NFS.    |
|`Hostname lookup failed`   | Tiene que crear una zona de búsqueda inversa en el servidor DNS y luego agregar un registro de puntero (PTR) del equipo host de AD a esa zona de búsqueda inversa. <br> Por ejemplo, supongamos que la dirección IP de la máquina de AD es `10.1.1.4`, que el nombre de host de la máquina de AD (que se encuentra mediante el comando hostname) es `AD1` y el nombre de dominio es `contoso.com`. El registro PTR agregado a la zona de búsqueda inversa debe ser `10.1.1.4 -> AD1.contoso.com`. |
|`Volume creation fails due to unreachable DNS server`  | Existen dos posibles soluciones: <br> <ul><li> Este error indica que no se puede obtener acceso al servidor DNS. El motivo puede ser una dirección IP de DNS incorrecta o un problema de red. Compruebe la dirección IP de DNS especificada en la conexión de AD y asegúrese de que sea correcta. </li> <li> Asegúrese de que AD y el volumen estén en la misma región y en la misma red virtual. Si están en redes virtuales diferentes, asegúrese de establecer un emparejamiento entre las dos redes virtuales. </li></ul> |
|La creación del volumen de NFSv4.1 de Kerberos genera un error similar al ejemplo siguiente: <br> `Failed to enable NFS Kerberos on LIF "svm_e719cde8d6d0413fbd6adac0636cdecb_7ad0b82e_73349613". Failed to bind service principal name on LIF "svm_e719cde8d6d0413fbd6adac0636cdecb_7ad0b82e_73349613". SecD Error: server create fail join user auth.` |La dirección IP de KDC es incorrecta y se ha creado el volumen de Kerberos. Actualice la dirección IP de KDC con una dirección correcta. <br> Después de actualizar la dirección IP de KDC, el error no desaparecerá. Debe volver a crear el volumen. |

## <a name="next-steps"></a>Pasos siguientes  

* [Configuración del cifrado Kerberos de NFSv4.1 para Azure NetApp Files](configure-kerberos-encryption.md)
