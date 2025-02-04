---
title: Preguntas frecuentes sobre Azure DNS privado
description: En este artículo, consulte las preguntas frecuentes sobre Azure DNS privado.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 01/15/2021
ms.author: rohink
ms.openlocfilehash: 3272f4fea4169fd5e91b1ba2b97ad72dfbb49080
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98247393"
---
# <a name="azure-private-dns-faq"></a>Preguntas frecuentes sobre Azure DNS privado

Las siguientes son preguntas frecuentes sobre Azure DNS privado.

## <a name="does-azure-dns-support-private-domains"></a>¿Azure DNS admite dominios privados?

Los dominios privados se admiten mediante la característica de zonas DNS privadas de Azure. Las zonas DNS privadas solo se pueden resolver desde dentro de las redes virtuales especificadas. Para más información, consulte la [introducción](private-dns-overview.md).

Para sobre otras opciones de DNS internas de Azure, consulte [Resolución de nombres para las máquinas virtuales e instancias de rol](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

## <a name="will-azure-private-dns-zones-work-across-azure-regions"></a>¿Funcionarán las zonas DNS privadas de Azure entre regiones de Azure?

Sí. Las zonas privadas se admiten para la resolución DNS entre redes virtuales de distintas regiones de Azure. Las zonas privadas funcionan incluso sin emparejamiento explícito de las redes virtuales. Todas las redes virtuales están vinculadas a la zona DNS privada.

## <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>¿Requieren las zonas privadas que las redes virtuales tengan conectividad con Internet?

No. Las zonas privadas funcionan junto con las redes virtuales. Úselas para administrar los dominios de las máquinas virtuales u otros recursos dentro y entre redes virtuales. No se requiere conectividad con Internet para la resolución de nombres.

## <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>¿Se puede usar la misma zona privada para varias redes virtuales para la resolución?

Sí. Puede vincular una zona DNS privada con miles de redes virtuales. Para más información, consulte [Límites de Azure DNS](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits).

## <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-linked-to-a-private-zone"></a>¿Se puede vincular a una zona privada una red virtual que pertenezca a otra suscripción?

Sí. Debe tener permiso para la operación de escritura en las redes virtuales y en la zona DNS privada. El permiso de escritura se puede conceder a varios roles de Azure. Por ejemplo, el rol de Azure de Colaborador de la red clásica tiene permisos de escritura en las redes virtuales y el rol de Colaborador de zonas DNS privadas tiene permisos de escritura en las zonas DNS privadas. Para obtener más información sobre los roles de Azure, consulte [¿Qué es el control de acceso basado en roles de Azure (RBAC)?](../role-based-access-control/overview.md).

## <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-you-delete-the-virtual-machine"></a>¿Se eliminarán automáticamente los registros de DNS de una máquina virtual registrada en una zona privada cuando se elimine la máquina virtual?

Sí. Si elimina una máquina virtual que se encuentra dentro de una red virtual vinculada que tiene habilitado el registro automático, se eliminan automáticamente los registros anotados.

## <a name="ive-reconfigured-the-os-in-my-virtual-machine-to-have-a-new-host-name-or-static-ip-address-why-dont-i-see-that-change-reflected-in-the-private-zone"></a>He reconfigurado el sistema operativo en mi máquina virtual para tener un nombre de host o una dirección IP estática nuevos. ¿Por qué no veo ese cambio reflejado en la zona privada?

El servicio DHPC de rellena los registros de la zona privada; se omiten los mensajes de registro del cliente. Si ha deshabilitado la compatibilidad con el cliente DHCP en la máquina virtual mediante la configuración de una dirección IP estática, los cambios en el nombre de host o en la dirección IP estática de la máquina virtual no se reflejan en la zona.

## <a name="i-have-configured-a-preferred-dns-suffix-in-my-windows-virtual-machine-why-are-my-records-still-registered-in-the-zone-linked-to-the-virtual-network"></a>He configurado un sufijo DNS preferido en mi máquina virtual Windows. ¿Por qué mis registros todavía aparecen anotados en la zona vinculada a la red virtual?

El servicio DHCP de Azure omite todos los sufijos DNS cuando registra la zona DNS privada. Por ejemplo, si la máquina virtual está configurada con el sufijo DNS principal de `contoso.com`, pero la red virtual está vinculada a la zona DNS privada de `fabrikam.com`, el registro de la máquina virtual aparece en la zona DNS privada de `fabrikam.com`.

## <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-linked-virtual-network-be-deleted-manually"></a>¿Se puede eliminar manualmente un registro de máquina virtual registrado automáticamente en una zona privada de una red virtual vinculada?

Sí. Puede sobrescribir los registros de DNS que se registraron automáticamente con un registro de DNS que se creó de manera manual en la zona. La siguiente pregunta y su respuesta abordan este tema.

## <a name="what-happens-when-i-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-linked-virtual-network"></a>¿Qué ocurre cuando se intenta crear manualmente un registro de DNS en una zona privada que tiene el mismo nombre de host que una máquina virtual existente registrada de manera automática en una red virtual vinculada?

Intenta crear manualmente un registro de DNS en una zona privada con el mismo nombre de host que una máquina virtual existente registrada de manera automática en una red virtual vinculada. Al hacerlo, el nuevo registro de DNS sobrescribe el registro de máquina virtual que se registró automáticamente. Si intenta eliminar nuevamente este registro de DNS que se creó de manera manual de la zona, la eliminación se realizará correctamente. El registro automático vuelve a ocurrir siempre que la máquina virtual siga existiendo y tenga adjunta una dirección IP privada. El registro de DNS se vuelve a crear de manera automática en la zona.

## <a name="what-happens-when-we-unlink-a-linked-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>¿Qué ocurre si desvinculamos una red virtual vinculada de una zona privada? ¿Los registros de máquina virtual registrados automáticamente de la red virtual también se quitarán de la zona?

Sí. Para desvincular una red virtual vinculada de una zona privada, debe actualizar la zona DNS para quitar el vínculo de la red virtual asociada. En este proceso, los registros de máquina virtual que se registraron automáticamente se quitan de la zona.

## <a name="what-happens-when-we-delete-a-linked-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-linked-virtual-network-from-the-zone"></a>¿Qué ocurre cuando se elimina una red virtual vinculada a una zona privada? ¿Es necesario actualizar manualmente la zona privada para desvincular la red virtual vinculada de la zona?

No. Cuando se elimina una red virtual vinculada sin desvincularla primero de una zona privada, la operación de eliminación se realiza correctamente y los vínculos a la zona DNS se eliminan de forma automática.

## <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>¿Funcionará la resolución de DNS con el valor de FQDN predeterminado (internal.cloudapp.net) aunque una zona privada (por ejemplo, private.contoso.com) esté vinculada a una red virtual?

Sí. Las zonas privadas no reemplazan la zona internal.cloudapp.net proporcionada por Azure. Ya sea si confía en la zona internal.cloudapp.net proporcionada por Azure o en su propia zona privada, use el nombre de dominio completo de la zona en la que quiere la resolución.

## <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>¿Se cambiará el sufijo DNS en las máquinas virtuales de una red virtual vinculada por el de la zona privada?

No. El sufijo DNS de las máquinas virtuales en la red virtual vinculada permanecerá como el sufijo predeterminado proporcionado por Azure ("*.internal.cloudapp.net"). En las máquinas virtuales, dicho sufijo DNS se puede cambiar manualmente por el de la zona privada.
Para instrucciones sobre cómo cambiar este sufijo, consulte [Uso de DNS dinámico para registrar nombres de host en su propio servidor DNS](../virtual-network/virtual-networks-name-resolution-ddns.md#windows-clients).

## <a name="what-are-the-usage-limits-for-azure-dns-private-zones"></a>¿Qué son los límites de uso de zonas privadas de Azure DNS?

Consulte [Límites de Azure DNS](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits) para más información sobre los límites de uso de zonas privadas de Azure DNS.

## <a name="why-dont-my-existing-private-dns-zones-show-up-in-new-portal-experience"></a>¿Por qué no aparecen las zonas DNS privadas existentes en la nueva experiencia del portal?

Si la zona DNS privada existente se creó con la API de la versión preliminar, debe migrar estas zonas al nuevo modelo de recurso. Las zonas DNS privadas creadas con la API de la versión preliminar no se mostrarán en la nueva experiencia del portal. Vea a continuación cómo migrar al nuevo modelo de recursos.

## <a name="how-do-i-migrate-my-existing-private-dns-zones-to-the-new-model"></a>¿Cómo se migran las zonas DNS privadas existentes al nuevo modelo?

Se recomienda realizar la migración al nuevo modelo tan pronto como sea posible. Aunque se admitirá el modelo de recursos heredado, no se desarrollarán más características basadas en él. En el futuro, está previsto dejar de usarlo en favor del nuevo modelo de recursos. Para instrucciones sobre cómo migrar las zonas DNS privadas al nuevo modelo de recursos, consulte la [guía de migración de las zonas privadas de Azure DNS](private-dns-migration-guide.md).

### <a name="does-azure-dns-private-zones-store-any-customer-content"></a>¿Las zonas privadas de Azure DNS almacenan el contenido de los clientes?

No, las zonas privadas de Azure DNS no almacenan ningún contenido de los clientes.

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre Azure DNS privado](private-dns-overview.md)
