---
title: Rotación de certificados para servidor único de Azure Database for PostgreSQL
description: Obtenga información sobre los próximos cambios del certificado raíz que afectarán al servidor único de Azure Database for PostgreSQL.
author: mksuni
ms.author: sumuth
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/02/2020
ms.openlocfilehash: 6dc687879eb646b4abd081b40bce292d20ff3186
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123996"
---
# <a name="understanding-the-changes-in-the-root-ca-change-for-azure-database-for-postgresql-single-server"></a>Descripción de los cambios en la CA raíz para servidor único de Azure Database for PostgreSQL

Azure Database for MySQL: Servidor único ha completado correctamente el cambio de certificado raíz el **15 de febrero de 2021 (15/02/2021)** como parte de los procedimientos recomendados de seguridad y mantenimiento estándar. En este artículo se proporcionan más detalles sobre los cambios, los recursos afectados y los pasos necesarios para asegurarse de que la aplicación mantenga la conectividad con el servidor de bases de datos.

## <a name="why-root-certificate-update-is-required"></a>¿Por qué se requiere la actualización del certificado raíz?

Los usuarios de Azure Database for PostgreSQL solo pueden usar el certificado predefinido para conectarse a un servidor de PostgreSQL que se encuentra [aquí](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem). Sin embargo, [Certificate Authority (CA) Browser Forum](https://cabforum.org/)  recientemente publicó informes relativos a la incompatibilidad de varios certificados emitidos por proveedores de CA.

Según los requisitos de cumplimiento normativo del sector, los proveedores de CA comenzaron a revocar los certificados de CA de las CA no compatibles, lo que requiere que los servidores usen certificados emitidos por CA compatibles y firmados por certificados de CA de esas CA compatibles. Como Azure Database for MySQL ha utilizado uno de estos certificados no compatibles, necesitábamos rotar el certificado a la versión compatible a fin de minimizar la posible amenaza para los servidores MySQL.

El nuevo certificado se ha implementado y ha entrado en vigor el 15 de febrero de 2021 (15/02/2021). 

## <a name="what-change-was-performed-on-february-15-2021-02152021"></a>¿Qué cambio se ha realizado el 15 de febrero de 2021 (15/02/2021)?

El 15 de febrero de 2021, el [certificado raíz BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) se ha reemplazado por una **versión compatible** del mismo [certificado raíz BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) para garantizar que los clientes existentes no tengan que cambiar nada, y que sus conexiones con el servidor no se vean afectadas. Durante este cambio, el [certificado raíz BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) no se ha **reemplazado** por [DigiCertGlobalRootG2](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem), y se ha aplazado este cambio para que los clientes tengan más tiempo para realizarlo.

## <a name="do-i-need-to-make-any-changes-on-my-client-to-maintain-connectivity"></a>¿Tengo que hacer algún cambio en mi cliente para mantener la conectividad?

No se requiere ningún cambio por parte del cliente. Si ha seguido la recomendación anterior que se indica a continuación, todavía podrá seguir conectándose siempre y cuando **no se quite el certificado BaltimoreCyberTrustRoot** del certificado de CA combinado. **Se recomienda no quitar BaltimoreCyberTrustRoot del certificado de CA combinado hasta nuevo aviso para mantener la conectividad.**

### <a name="previous-recommendation"></a>Recomendación anterior

*   Descargue la CA raíz de BaltimoreCyberTrustRoot y DigiCertGlobalRootG2 de los vínculos siguientes:
    *   https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem
    *   https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem

*   Genere un almacén de certificados de CA combinado que incluya los certificados tanto de **BaltimoreCyberTrustRoot** como de **DigiCertGlobalRootG2**.
    *   Para los usuarios de Java (PostgreSQL JDBC) que usan DefaultJavaSSLFactory, se debe ejecute:

          ```console
          keytool -importcert -alias PostgreSQLServerCACert  -file D:\BaltimoreCyberTrustRoot.crt.pem  -keystore truststore -storepass password -noprompt
          ```

          ```console
          keytool -importcert -alias PostgreSQLServerCACert2  -file D:\DigiCertGlobalRootG2.crt.pem -keystore truststore -storepass password  -noprompt
          ```

          A continuación, reemplace el archivo de almacén de claves original por el nuevo generado:
        *   System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file"); 
        *   System.setProperty("javax.net.ssl.trustStorePassword","password");
        
    *   En el caso de un usuario de .NET (Npgsql) en Windows, debe asegurarse de que **Baltimore CyberTrustRoot** y **DigiCertGlobalRootG2** existan en el almacén de certificados de Windows, entidades de certificación raíz de confianza. Si no existe alguno de los certificados, importe el certificado que falta.

        ![Certificado .Net para Azure Database for PostgreSQL](media/overview/netconnecter-cert.png)

    *   En el caso de un usuario de .NET (Npgsql) en Linux con SSL_CERT_DIR, debe asegurarse de que **BaltimoreCyberTrustRoot** y **DigiCertGlobalRootG2** existan en el directorio indicado por SSL_CERT_DIR. Si no existe alguno de los certificados, cree el archivo de certificado que falta.

    *   Para otros usuarios de cliente de PostgreSQL, se pueden fusionar dos archivos de certificado de CA con el formato siguiente.

        </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA1: BaltimoreCyberTrustRoot.crt.pem)
 </br>-----END CERTIFICATE-----
 </br>-----BEGIN CERTIFICATE-----
 </br>(Root CA2: DigiCertGlobalRootG2.crt.pem)
 </br>-----END CERTIFICATE-----

*   Reemplace el archivo PEM de la CA raíz original por el archivo de CA raíz combinado y reinicie la aplicación o el cliente.
*    En el futuro, después de que se implemente el nuevo certificado en el servidor, puede cambiar el archivo PEM de la CA a DigiCertGlobalRootG2.crt.pem.

> [!NOTE]
> No elimine ni modifique el **certificado de Baltimore** hasta que se realice el cambio de certificado. Una vez realizado el cambio se va a enviar una comunicación, después de lo cual es seguro eliminar el certificado de Baltimore. 

## <a name="why-was-baltimorecybertrustroot-certificate-not-replaced-to-digicertglobalrootg2-during-this-change-on-february-15-2021"></a>¿Por qué no se reemplazó el certificado BaltimoreCyberTrustRoot por DigiCertGlobalRootG2 durante este cambio el 15 de febrero de 2021?

Evaluamos la preparación de los clientes para aplicar este cambio y nos dimos cuenta de que muchos de ellos buscaban más tiempo para realizarlo. Con el fin de dar más tiempo a los clientes para que estuvieran preparados, decidimos aplazar el cambio de certificado a DigiCertGlobalRootG2 durante al menos un año, lo que proporciona un plazo de tiempo suficiente a los clientes y usuarios finales. 

Nuestra recomendación para los usuarios es que sigan los pasos mencionados anteriormente para crear un certificado combinado y se conecten al servidor, pero que no quiten el certificado BaltimoreCyberTrustRoot hasta que se envíe una comunicación para que lo hagan. 

## <a name="what-if-we-removed-the-baltimorecybertrustroot-certificate"></a>¿Qué ocurre si se quita el certificado BaltimoreCyberTrustRoot?

Comenzarán a producirse errores de conectividad al conectarse al servidor de Azure Database for PostgreSQL. Tendrá que configurar de nuevo SSL con el certificado [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) para mantener la conectividad.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

###    <a name="1-if-i-am-not-using-ssltls-do-i-still-need-to-update-the-root-ca"></a>1. Si no utilizo SSL/TLS, ¿aún tengo que actualizar la CA raíz?
No se requiere ninguna acción si no se usa SSL/TLS. 

### <a name="2-if-i-am-using-ssltls-do-i-need-to-restart-my-database-server-to-update-the-root-ca"></a>2. Si uso SSL/TLS, ¿es necesario reiniciar el servidor de base de datos para actualizar la CA raíz?
No, no es necesario reiniciar el servidor de base de datos para empezar a usar el nuevo certificado. Se trata de un cambio en el lado cliente y las conexiones de cliente entrantes deben usar el nuevo certificado para asegurarse de que puedan conectarse al servidor de bases de datos.

### <a name="3-how-do-i-know-if-im-using-ssltls-with-root-certificate-verification"></a>3. ¿Cómo saber si utilizo SSL/TLS con la comprobación del certificado raíz?

Para saber si las conexiones comprueban el certificado raíz, puede revisar la cadena de conexión.
-    Si la cadena de conexión incluye `sslmode=verify-ca` o `sslmode=verify-full`, tiene que actualizar el certificado.
-    Si la cadena de conexión incluye `sslmode=disable`, `sslmode=allow`, `sslmode=prefer` o `sslmode=require`, no tiene que actualizar los certificados. 
-    Si la cadena de conexión no especifica sslmode, no tiene que actualizar los certificados.

Si usa un cliente que abstrae la cadena de conexión, revise la documentación del cliente para saber si comprueba los certificados. Para entender sslmode de PostgreSQL, consulte las [descripciones del modo SSL](https://www.postgresql.org/docs/11/libpq-ssl.html#ssl-mode-descriptions) en la documentación de PostgreSQL.

### <a name="4-what-is-the-impact-if-using-app-service-with-azure-database-for-postgresql"></a>4. ¿Qué consecuencia tendrá si se usa App Service con Azure Database for PostgreSQL?
En el caso de los servicios de aplicaciones de Azure que se conectan a Azure Database for PostgreSQL, podemos tener dos escenarios posibles y depende de cómo se use SSL con la aplicación.
*   Este nuevo certificado se ha agregado a App Service en el nivel de la plataforma. Si usa en la aplicación los certificados SSL incluidos en la plataforma de App Service, no es necesario realizar ninguna acción.
*   Si va a incluir explícitamente la ruta de acceso al archivo de certificado SSL en el código, debería descargar el nuevo certificado y actualizar el código para usar el nuevo certificado. Un buen ejemplo de este escenario es cuando se usan contenedores personalizados de App Service como compartidos en la [documentación de App Service](../app-service/tutorial-multi-container-app.md#configure-database-variables-in-wordpress).

### <a name="5-what-is-the-impact-if-using-azure-kubernetes-services-aks-with-azure-database-for-postgresql"></a>5. ¿Qué consecuencia tendrá si se usa Azure Kubernetes Service (AKS) con Azure Database for PostgreSQL?
Si está intentando conectarse a Azure Database for PostgreSQL con Azure Kubernetes Services (AKS), es similar a acceder desde un entorno de host de clientes dedicados. Consulte los pasos [aquí](../aks/ingress-own-tls.md).

### <a name="6-what-is-the-impact-if-using-azure-data-factory-to-connect-to-azure-database-for-postgresql"></a>6. ¿Qué consecuencia tendrá si se usa Azure Data Factory para conectarse con Azure Database for PostgreSQL?
Para el conector que usa Azure Integration Runtime, el conector aprovecha los certificados del almacén de certificados de Windows en el entorno hospedado en Azure. Estos certificados ya son compatibles con los certificados recién aplicados y, por lo tanto, no es necesaria ninguna acción.

Para el conector que usa el entorno de ejecución de integración autohospedado donde se incluye explícitamente la ruta de acceso al archivo de certificado SSL en la cadena de conexión, deberá descargar el [nuevo certificado](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) y actualizar la cadena de conexión para utilizarlo.

### <a name="7-do-i-need-to-plan-a-database-server-maintenance-downtime-for-this-change"></a>7. ¿Es necesario planear un tiempo de inactividad de mantenimiento del servidor de base de datos para este cambio?
No. Dado que el cambio aquí solo está en el lado del cliente para conectarse al servidor de base de datos, este cambio no requiere ningún tiempo de inactividad de mantenimiento para el servidor de base de datos.

### <a name="8-if-i-create-a-new-server-after-february-15-2021-02152021-will-i-be-impacted"></a>8. Si creo un nuevo servidor después del 15 de febrero de 2021 (15/02/2021), ¿me afectará este cambio?
En los servidores creados después del 15 de febrero de 2021 (15/02/2021), seguirá utilizando [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) para que las aplicaciones se conecten mediante SSL.

### <a name="9-how-often-does-microsoft-update-their-certificates-or-what-is-the-expiry-policy"></a>9. ¿Con qué frecuencia actualiza Microsoft sus certificados o cuál es la directiva de expiración?
Los certificados utilizados por Azure Database for PostgreSQL provienen de entidades de certificación (CA) de confianza. Por lo tanto, la compatibilidad de estos certificados está ligada a la compatibilidad de estos certificados por parte de la entidad de certificación. El certificado [BaltimoreCyberTrustRoot](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) está programado para que expire en 2025, por lo que Microsoft deberá realizar un cambio de certificado antes de la expiración. Además, en caso de que se produzcan errores imprevistos en estos certificados predefinidos, Microsoft tendrá que realizar la rotación de certificados lo más pronto posible, de forma parecida al cambio realizado el 15 de febrero de 2021, para asegurarse de que el servicio sea seguro y compatible en todo momento.

### <a name="10-if-i-am-using-read-replicas-do-i-need-to-perform-this-update-only-on-the-primary-server-or-the-read-replicas"></a>10. Si utilizo réplicas de lectura, ¿tengo que realizar esta actualización solo en el servidor principal o en las réplicas de lectura?
Puesto que esta actualización es un cambio en el lado cliente, si el cliente leía los datos del servidor réplica, deberá aplicar también los cambios para esos clientes. 

### <a name="11-do-we-have-server-side-query-to-verify-if-ssl-is-being-used"></a>11. ¿Tenemos que consultar en el lado servidor para comprobar si se está utilizando SSL?
Para comprobar si está usando la conexión SSL para conectarse al servidor, consulte [Comprobación de la conexión SSL](concepts-ssl-connection-security.md#applications-that-require-certificate-verification-for-tls-connectivity).

### <a name="12-is-there-an-action-needed-if-i-already-have-the-digicertglobalrootg2-in-my-certificate-file"></a>12. ¿Hay alguna acción necesaria si ya tengo DigiCertGlobalRootG2 en el archivo de certificado?
No. No es necesario realizar ninguna acción si el archivo de certificado ya contiene **DigiCertGlobalRootG2**.

### <a name="13-what-if-you-are-using-docker-image-of-pgbouncer-sidecar-provided-by-microsoft"></a>13. ¿Qué ocurre si se está usando la imagen de Docker de sidecar PgBouncer proporcionada por Microsoft?
Se ha publicado una nueva imagen de Docker compatible con [**Baltimore**](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) y [**DigiCert**](https://cacerts.digicert.com/DigiCertGlobalRootG2.crt.pem) [aquí](https://hub.docker.com/_/microsoft-azure-oss-db-tools-pgbouncer-sidecar) (etiqueta más reciente). Puede extraer esta nueva imagen para evitar cualquier interrupción de conectividad a partir del 15 de febrero de 2021. 

### <a name="14-what-if-i-have-further-questions"></a>14. ¿Qué hago si tengo más preguntas?
Si tiene alguna pregunta, obtenga respuestas de expertos de la comunidad en [Microsoft Q&A](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com). Si tiene un plan de soporte técnico y necesita ayuda técnica, [póngase en contacto con nosotros](mailto:AzureDatabaseforPostgreSQL@service.microsoft.com)
