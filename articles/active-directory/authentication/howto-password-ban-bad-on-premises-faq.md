---
title: P+F de la protección con contraseña de Azure AD local
description: Revise las preguntas más frecuentes para la protección con contraseña de Azure AD en un entorno de Active Directory Domain Services local.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f80990854fd0c584d8e6582fdf35108e67d9202b
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99625135"
---
# <a name="azure-ad-password-protection-on-premises-frequently-asked-questions"></a>Preguntas más frecuentes sobre protección con contraseña de Azure AD local

En esta sección se dan respuestas a muchas de las preguntas más frecuentes sobre la protección con contraseña de Azure AD.

## <a name="general-questions"></a>Preguntas generales

**P: ¿Qué guía debe proporcionarse a los usuarios sobre cómo seleccionar una contraseña segura?**

La guía actual de Microsoft sobre este tema puede encontrarse en el siguiente vínculo:

[Guía de contraseñas de Microsoft](https://www.microsoft.com/research/publication/password-guidance)

**P: ¿La protección con contraseña de Azure AD local se admite en las nubes que no son públicas?**

La protección de contraseñas de Azure AD local se admite en la nube pública y en la nube de Arlington. No se ha anunciado ninguna fecha para la disponibilidad en otras nubes.

El portal de Azure AD permite la modificación de la configuración de "protección mediante contraseña para Windows Server Active Directory" en el entorno local, incluso en nubes no compatibles. Estos cambios se conservarán; si no fuera así, nunca surtirán efecto. No se admite el registro de agentes o bosques de proxy local en nubes no compatibles, y cualquier otro intento de registro siempre producirá un error.

**P: ¿Cómo puedo aplicar las ventajas de la protección con contraseña de Azure AD a un subconjunto de mis usuarios locales?**

No compatible. Una vez implementada y habilitada, la característica Protección con contraseña de Azure AD no es discriminatoria: todos los usuarios reciben ventajas de seguridad por igual.

**P: ¿Cuál es la diferencia entre un cambio de contraseña y el establecimiento (o restablecimiento) de una contraseña?**

Un cambio de contraseña es cuando un usuario elige una nueva contraseña después de demostrar que conoce la contraseña antigua. Por ejemplo, un cambio de contraseña es lo que sucede cuando un usuario inicia sesión en Windows y, a continuación, debe elegir una contraseña nueva.

Un establecimiento de contraseña (denominado a veces restablecimiento de contraseña) es cuando un administrador reemplaza la contraseña de una cuenta por una contraseña nueva; por ejemplo mediante la herramienta de administración de equipos y usuarios de Active Directory. Esta operación requiere un alto nivel de privilegios (normalmente, administrador del dominio) y la persona que realiza la operación normalmente no conoce la contraseña anterior. Los departamentos de soporte técnico suelen establecer contraseñas, por ejemplo cuando ayudan a un usuario que ha olvidado su contraseña. También verá eventos de establecimiento de contraseña cuando se crea por primera vez una nueva cuenta de usuario con contraseña.

La directiva de validación de contraseña se comporta de la misma forma, independientemente de si se realiza un cambio o un establecimiento de contraseña. El servicio del agente de control de dominio para protección con contraseña de Azure AD registra eventos diferentes para informarle si se realizó una operación de cambio o establecimiento de contraseña.  Consulte [Supervisión y registro de la protección con contraseña de Azure AD](./howto-password-ban-bad-on-premises-monitor.md).

**P: ¿La protección con contraseña de Azure AD valida las contraseñas existentes después de su instalación?**

No. La protección con contraseña de Azure AD solo puede exigir la directiva de contraseñas en las contraseñas de texto no cifrado durante un cambio de contraseña o una operación de establecimiento. Una vez que Active Directory acepta una contraseña, solo se conservan los hashes específicos del protocolo de autenticación de dicha contraseña. La contraseña de texto no cifrado nunca se conserva, por lo que la protección con contraseña de Azure AD no puede validar las contraseñas existentes.

Después de la implementación inicial de la protección con contraseña de Azure AD, todos los usuarios y las cuentas comenzarán a usar una contraseña validada por la protección con contraseña de Azure AD, ya que las contraseñas existentes suelen expirar a lo largo del tiempo. Si lo desea, este proceso se puede acelerar mediante una expiración manual única de contraseñas de cuentas de usuario.

Las cuentas configuradas con "La contraseña no expira nunca" nunca se verán obligadas a cambiar su contraseña a menos que se realice una expiración manual.

**P: ¿Por qué se registran eventos de rechazo de contraseña duplicados al intentar establecer una contraseña no segura mediante el complemento de administración Usuarios y equipos de Active Directory?**

El complemento de administración Usuarios y equipos de Active Directory intentará establecer primero la nueva contraseña mediante el protocolo Kerberos. En caso de error, el complemento intentará una segunda vez establecer la contraseña mediante un protocolo heredado (RPC de SAM) (los protocolos concretos que se usan no son importantes). Si Protección con contraseña de Azure AD considera que la nueva contraseña no es segura, este comportamiento de complemento generará el registro de dos conjuntos de eventos de rechazo de restablecimiento de contraseña.

**P: ¿Por qué los eventos de validación de contraseña de la protección con contraseña de Azure AD se registran con un nombre de usuario vacío?**

Active Directory admite la capacidad de probar una contraseña para ver si supera los requisitos de complejidad de contraseña actuales del dominio, por ejemplo, con la API [NetValidatePasswordPolicy](/windows/win32/api/lmaccess/nf-lmaccess-netvalidatepasswordpolicy). Cuando una contraseña se valida de esta manera, las pruebas también incluyen la validación por parte de los productos basados en el archivo dll de filtro de contraseñas, como la protección con contraseña de Azure AD, pero los nombres de usuario que se pasan a un archivo dll de filtro de contraseñas dado estarán vacíos. En este escenario, la protección con contraseña de Azure AD aún validará la contraseña con la directiva de contraseñas actualmente en vigor y emitirá un mensaje de registro de eventos para capturar el resultado; sin embargo, el mensaje del registro de eventos tendrá campos de nombre de usuario vacíos.

**P: ¿Es posible instalar la protección con contraseña de Azure AD junto con otros productos basados en filtros de contraseña?**

Sí. La compatibilidad con varios archivos DLL de filtro de contraseña registrados es una característica principal de Windows y no es específica de la protección con contraseña de Azure AD. Todos los archivos DLL de filtro de contraseña registrados deben coincidir para que se acepte una contraseña.

**P: ¿Cómo puedo implementar y configurar Protección con contraseña de Azure AD en mi entorno de Active Directory sin usar Azure?**

No compatible. Protección con contraseña de AD Azure es una característica de Azure que puede extenderse a un entorno de Active Directory local.

**P: ¿Cómo puedo modificar el contenido de la directiva en el nivel de Active Directory?**

No compatible. La directiva solo se puede administrar mediante el portal de Azure AD. Consulte también la pregunta anterior.

**P: ¿Por qué se requiere DFSR para la replicación SYSVOL?**

FRS (la tecnología predecesora a DFSR) tiene muchos problemas conocidos y es totalmente incompatible en las versiones más recientes de Windows Server Active Directory. No se hará ninguna prueba de la protección con contraseña de Azure AD en los dominios configurados para FRS.

Para obtener más información, consulte los artículos siguientes:

[The Case for Migrating sysvol replication to DFSR](/archive/blogs/askds/the-case-for-migrating-sysvol-to-dfsr) (El caso de migrar la replicación de SYSVOL a DFSR)

[The End is Nigh for FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs) (El final de FRS está cerca)

Si el dominio no usa aún DFSR, DEBE migrarlo para que lo utilice antes de instalar la protección con contraseña de Azure AD. Para más información, consulte el vínculo siguiente:

[Guía de migración de replicación de SYSVOL: Replicación de FRS a DFS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019(v=ws.10))

> [!WARNING]
> El software del agente de DC de Protección con contraseña de Azure AD se instala actualmente en los controladores de dominio de los dominios que usan aún FRS para la replicación de SYSVOL, pero NO funciona correctamente en este entorno. Otros efectos secundarios negativos incluyen archivos que no se pueden replicar y procedimientos de restauración de SYSVOL que aparentemente funcionan pero que en realidad no pueden replicar todos los archivos. Migre el dominio para usar DFSR lo antes posible, tanto por las ventajas inherentes de DFSR como también para desbloquear la implementación de Protección con contraseña de Azure AD. Las versiones futuras del software se deshabilitarán automáticamente cuando se ejecuten en un dominio que aún use FRS.

**P: ¿Cuánto espacio en disco requiere la característica en el recurso compartido sysvol del dominio?**

El uso exacto del espacio varía puesto que depende de factores como el número y la longitud de los tokens no permitidos en la lista global de Microsoft de no permitidos y la lista personalizada por inquilino, además de la sobrecarga de cifrado. El contenido de estas listas es probable que aumente en el futuro. Teniendo esto en cuenta, la característica debería necesitar al menos cinco (5) megabytes de espacio en el recurso compartido sysvol del dominio.

**P: ¿Por qué se requiere un reinicio para instalar o actualizar el software del agente del controlador de dominio?**

Este requisito lo causa un comportamiento principal de Windows.

**P: ¿Existe alguna forma de configurar un agente de controlador de dominio para que use un servidor proxy específico?**

No. Puesto que el servidor proxy es sin estado, no es importante qué servidor proxy específico se utilice.

**P: ¿Se puede implementar el servicio de proxy de Protección con contraseña de Azure AD en paralelo a otros servicios como Azure AD Connect?**

Sí. El servicio de proxy de Protección con contraseña de Azure AD y Azure AD Connect no deben nunca entrar en conflicto directamente entre sí.

Desafortunadamente, se ha encontrado una incompatibilidad entre la versión del servicio de actualización del agente de Microsoft Azure AD Connect que se instala mediante el software del proxy de protección con contraseña de Azure AD y la versión del servicio que se instala mediante el software [Azure Active Directory Application Proxy](../manage-apps/application-proxy.md). Esta incompatibilidad puede dar lugar a que el servicio de actualización del agente no pueda ponerse en contacto con Azure para realizar las actualizaciones de software. No se recomienda instalar el proxy de protección con contraseña de Azure AD y Azure Active Directory Application Proxy en el mismo equipo.

**P: ¿En qué orden se deben instalar y registrar los agentes del controlador de dominio y los servidores proxy?**

Se admite cualquier orden de instalación del agente de proxy, instalación del agente de controlador de domino, registro del bosque y registro de proxy.

**P: ¿Debe preocuparme el rendimiento de mis controladores de dominio por implementar esta característica?**

El servicio de agente de controlador de dominio de Protección con contraseña de Azure AD no debería afectar significativamente al rendimiento del controlador de dominio en una implementación existente de Active Directory en buen estado.

En la mayoría de las implementaciones de Active Directory, las operaciones de cambio de contraseña representan una pequeña proporción de la carga de trabajo global de un controlador de dominio determinado. Por ejemplo, imagínese un dominio de Active Directory con 10 000 cuentas de usuario y una directiva MaxPasswordAge establecida en 30 días. De promedio, este dominio sufrirá 10 000/30 = ~333 operaciones de cambio de contraseña cada día; se trata de una cifra baja de operaciones para incluso un solo controlador de dominio. Un escenario con el peor caso posible: imagínese que esos ~333 cambios de contraseña en un único controlador de dominio se realizan en una sola hora. Por ejemplo, este escenario puede ocurrir cuando muchos empleados vienen a trabajar el lunes por la mañana. Incluso en ese caso, se siguen observando ~333/60 minutos = 6 cambios de contraseña por minuto, que tampoco es una carga significativa.

Sin embargo, si los controladores de dominio actuales ya se están ejecutando en los niveles de rendimiento limitado (por ejemplo, si se sobrepasa con respecto a CPU, espacio en disco, E/S de disco, etc.), es conveniente agregar más controladores de dominio o expandir el espacio disponible en disco antes de implementar esta característica. Consulte también la pregunta anterior sobre el uso de espacio en disco de sysvol.

**P: Quiero probar Protección con contraseña de Azure AD en solo unos pocos controladores de dominio de mi dominio. ¿Se pueden forzar los cambios de contraseña de usuario para usar esos controladores de dominio específicos?**

No. El sistema operativo de cliente Windows controla qué controlador de dominio se utiliza cuando un usuario cambia su contraseña. El controlador de dominio se selecciona en función de diversos factores, como las asignaciones de sitio y subred de Active Directory, la configuración de red específica de entorno, etcétera. Protección con contraseña de Azure AD no controla estos factores y no puede influir en qué controlador de dominio se selecciona para cambiar la contraseña de un usuario.

Una manera de lograr este objetivo sería implementar parcialmente Protección con contraseña de Azure AD en todos los controladores de dominio de un determinado sitio de Active Directory. Este enfoque proporcionará una cobertura razonable a los clientes de Windows que están asignados a ese sitio, y por lo tanto, también a los usuarios que están iniciando sesión en los clientes y cambiando sus contraseñas.

**P: Si instalo el servicio de agente de controlador de dominio de Protección con contraseña de Azure AD solamente en el controlador de dominio principal (PDC), ¿también se protegerán el resto de los controladores de dominio del dominio?**

No. Cuando se cambia la contraseña de un usuario en un controlador de dominio determinado que no es PDC, nunca se envía la contraseña no cifrada al PDC (esta idea es una percepción errónea común). Una vez que se acepta una contraseña nueva en un controlador de dominio determinado, ese controlador de dominio usa esa contraseña para crear los hashes específicos del protocolo de autenticación de esa contraseña y, a continuación, conserva los hashes en el directorio. La contraseña no cifrada no se mantiene. Los hashes actualizados se replican luego en el PDC. En algunos casos, las contraseñas de usuario se pueden volver a cambiar directamente en el PDC según varios factores, como la topología de red y el diseño del sitio de Active Directory. Consulte la pregunta anterior.

En resumen, se requiere implementar el servicio de agente de controlador de dominio de Protección con contraseña de Azure AD en el PDC para llegar al 100 % de cobertura de seguridad de la característica en todo el dominio. Si se implementa solamente la característica en el PDC, no se proporcionan las ventajas de seguridad de Protección con contraseña de Azure AD para los otros controladores de dominio del dominio.

**P: ¿Por qué el bloqueo inteligente personalizado no funciona incluso después de que los agentes se instalan en el entorno de Active Directory local?**

El bloqueo inteligente personalizado solo se admite en Azure AD. Los cambios en la configuración del bloqueo inteligente personalizado en el portal de Azure AD no tiene ningún efecto en el entorno de Active Directory local, incluso con los agentes instalados.

**P: ¿Hay algún módulo de administración de Operations Manager disponible para Protección con contraseña de Azure AD?**

No.

**P: ¿Por qué Azure AD sigue rechazando las contraseñas no seguras, aunque haya configurado la directiva para que esté en modo de auditoría?**

Solo se admite el modo de auditoría en el entorno de Active Directory local. Azure AD siempre está implícitamente en modo "aplicar" cuando evalúa las contraseñas.

**P: Mis usuarios ven el mensaje de error tradicional de Windows cuando Protección con contraseña de Azure AD rechaza una contraseña. ¿Es posible personalizar este mensaje de error para que los usuarios sepan lo que ha sucedido realmente?**

No. El mensaje de error que ven los usuarios cuando un controlador de dominio rechaza una contraseña está controlado por la máquina cliente, no por el controlador de dominio. Este comportamiento se produce si las directivas predeterminadas de contraseñas de Active Directory o una solución basada en filtro de contraseña, como Protección con contraseña de Azure AD, rechazan una contraseña.

## <a name="password-testing-procedures"></a>Procedimientos de prueba de contraseñas

Puede que quiera realizar algunas pruebas básicas de varias contraseñas con el fin de validar el funcionamiento adecuado del software y comprender mejor el [algoritmo de evaluación de contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated). En esta sección se describe un método para tales pruebas que está diseñado para generar resultados repetibles.

¿Por qué es necesario seguir estos pasos? Hay varios factores que dificultan la realización de pruebas controlables y repetibles de contraseñas en el entorno de Active Directory local:

* La directiva de contraseñas se configura y se conserva en Azure y los agentes de controlador de dominio locales sincronizan periódicamente las copias de la directiva mediante un mecanismo de sondeo. La latencia inherente a este ciclo de sondeo puede provocar confusión. Por ejemplo, si configura la directiva en Azure, pero olvida sincronizarla con el agente de controlador de dominio, es posible que las pruebas no produzcan los resultados esperados. El intervalo de sondeo está actualmente codificado de forma rígida para que sea una vez por hora, pero esperar una hora entre cambios de directiva no es una buena idea en un escenario de pruebas interactivo.
* Una vez que una nueva directiva de contraseñas se sincroniza con un controlador de dominio, se producirá más latencia mientras se replica en otros controladores de dominio. Estos retrasos pueden producir resultados inesperados cuando se prueba un cambio de contraseña en un controlador de dominio que todavía no ha recibido la versión más reciente de la directiva.
* La prueba de los cambios de contraseña a través de una interfaz de usuario dificulta la confianza en los resultados. Por ejemplo, es fácil escribir incorrectamente una contraseña no válida en una interfaz de usuario, dado que principalmente la mayoría de las interfaces de usuario de contraseña ocultan la entrada del usuario (por ejemplo, la interfaz de usuario de Windows de Ctrl-Alt-Supr-> Cambiar contraseña).
* No es posible controlar estrictamente qué controlador de dominio se usa al probar los cambios de contraseña de los clientes unidos a un dominio. El sistema operativo cliente de Windows selecciona un controlador de dominio en función de diversos factores, como las asignaciones de sitio y subred de Active Directory, la configuración de red específica del entorno, etc.

Para evitar estos problemas, los siguientes pasos se basan en la prueba mediante la línea de comandos del restablecimiento de contraseña mientras se inicia sesión en un controlador de dominio.

> [!WARNING]
> Estos procedimientos deben usarse solo en entornos de prueba, ya que todos los cambios y restablecimientos de contraseña entrantes se aceptarán sin validarse mientras el servicio del agente de controlador de dominio esté detenido, y también para evitar el aumento de los riesgos inherentes al inicio de sesión en un controlador de dominio.

En los pasos siguientes se da por supuesto que ha instalado el agente de controlador de dominio en al menos un controlador de dominio, que ha instalado al menos un proxy y que ha registrado tanto el proxy como el bosque.

1. Inicie sesión en un controlador de dominio con credenciales de administrador de dominio (u otras credenciales que tengan privilegios suficientes para crear cuentas de usuario de prueba y restablecer contraseñas) que tenga instalado el software de agente de controlador de dominio y que se haya reiniciado.
1. Abra el Visor de eventos y vaya al [registro de eventos de administración del agente de controlador de dominio](howto-password-ban-bad-on-premises-monitor.md#dc-agent-admin-event-log).
1. Abra una ventana del símbolo del sistema con permisos elevados.
1. Creación de una cuenta de prueba para realizar pruebas de contraseñas

   Hay muchas maneras de crear una cuenta de usuario, pero aquí se ofrece una opción mediante la línea de comandos para facilitar esta operación durante ciclos de prueba repetitivos:

   ```text
   net.exe user <testuseraccountname> /add <password>
   ```

   A efectos del presente análisis, suponga que hemos creado una cuenta de prueba llamada "ContosoUser", por ejemplo:

   ```text
   net.exe user ContosoUser /add <password>
   ```

1. Abra un explorador web (puede que tenga que usar otro dispositivo en lugar del controlador de dominio), inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a Azure Active Directory > Seguridad > Métodos de autenticación > Protección con contraseña.
1. Modifique la directiva de protección con contraseña de Azure AD según sea necesario para las pruebas que quiere realizar.  Por ejemplo, puede decidir configurar el Modo aplicado o el Modo de auditoría, o modificar la lista de términos prohibidos en la lista personalizada de contraseñas prohibidas.
1. Para sincronizar la nueva directiva, detenga y reinicie el servicio del agente de controlador de dominio.

   Este paso se puede realizar de varias maneras. Una manera sería usar la consola administrativa de administración de servicios; para ello, haga clic con el botón derecho en el servicio del agente de controlador de dominio de protección con contraseña de Azure AD y elija "Reiniciar". Otra manera es usar la ventana del símbolo del sistema, como, por ejemplo:

   ```text
   net stop AzureADPasswordProtectionDCAgent && net start AzureADPasswordProtectionDCAgent
   ```
    
1. Compruebe el Visor de eventos para confirmar que se ha descargado una nueva directiva.

   Cada vez que el servicio del agente de controlador de dominio se detiene y se inicia, verá que se emiten dos eventos 30006 en estrecha sucesión. El primer evento 30006 reflejará la directiva que se almacenó en caché en el disco del recurso compartido sysvol. El segundo evento 30006 (si existe) debe tener una fecha de directiva de inquilino actualizada y, si es así, reflejará la directiva que se descargó de Azure. El valor de fecha de la directiva de inquilino está actualmente codificado para mostrar la marca de tiempo aproximada en que la directiva se descargó de Azure.
   
   Si el segundo evento 30006 no aparece, debe solucionar el problema antes de continuar.
   
   Los eventos 30006 tendrán un aspecto similar al del ejemplo siguiente:
 
   ```text
   The service is now enforcing the following Azure password policy.

   Enabled: 1
   AuditOnly: 0
   Global policy date: ‎2018‎-‎05‎-‎15T00:00:00.000000000Z
   Tenant policy date: ‎2018‎-‎06‎-‎10T20:15:24.432457600Z
   Enforce tenant policy: 1
   ```

   Por ejemplo, si se cambia entre el Modo aplicado y el Modo de auditoría, se modificará la marca AuditOnly (la directiva anterior con AuditOnly = 0 está en Modo aplicado); los cambios en la lista personalizada de contraseñas prohibidas no se reflejan directamente en el evento 30006 anterior (y no se registran en ninguna parte por motivos de seguridad). La descarga correcta de la directiva de Azure después de este cambio también incluirá la lista modificada de contraseñas prohibidas.

1. Ejecute una prueba intentando restablecer una nueva contraseña en la cuenta de usuario de prueba.

   Este paso se puede realizar desde la ventana del símbolo del sistema, como se indica a continuación:

   ```text
   net.exe user ContosoUser <password>
   ```

   Después de ejecutar el comando, consulte el Visor de eventos para obtener más información sobre el resultado del comando. Los eventos de resultados de validación de contraseñas se documentan en el tema [Registro de eventos de administración del agente de controlador de dominio](howto-password-ban-bad-on-premises-monitor.md#dc-agent-admin-event-log); tales eventos se usarán para validar el resultado de la prueba, además de la salida interactiva de los comandos net.exe.

   Vamos a probar un ejemplo: intentaremos establecer una contraseña prohibida en la lista global de Microsoft (tenga en cuenta que la lista [no está documentada](concept-password-ban-bad.md#global-banned-password-list), pero podemos probarla aquí con un término prohibido conocido). En este ejemplo se da por supuesto que ha configurado la directiva en el Modo aplicado y que ha agregado cero términos a la lista personalizada de contraseñas prohibidas.

   ```text
   net.exe user ContosoUser PassWord
   The password does not meet the password policy requirements. Check the minimum password length, password complexity and password history requirements.

   More help is available by typing NET HELPMSG 2245.
   ```

   Según la documentación, dado que nuestra prueba era una operación de restablecimiento de contraseña, debería ver un evento 10017 y 30005 para el usuario ContosoUser.

   El evento 10017 se parecerá al de este ejemplo:

   ```text
   The reset password for the specified user was rejected because it did not comply with the current Azure password policy. Please see the correlated event log message for more details.
 
   UserName: ContosoUser
   FullName: 
   ```

   El evento 30005 se parecerá al de este ejemplo:

   ```text
   The reset password for the specified user was rejected because it matched at least one of the tokens present in the Microsoft global banned password list of the current Azure password policy.
 
   UserName: ContosoUser
   FullName: 
   ```

   Ha estado bien. Vamos a probar otro ejemplo. Esta vez intentaremos establecer una contraseña prohibida de la lista personalizada de contraseñas prohibidas mientras la directiva está en Modo de auditoría. En este ejemplo se da por supuesto que ha realizado los pasos siguientes: ha configurado la directiva en Modo de auditoría, ha agregado el término "lachrymose" a la lista personalizada de contraseñas prohibidas y ha sincronizado la nueva directiva resultante con el controlador de dominio mediante el proceso del servicio del agente de controlador de dominio, como se ha descrito anteriormente.

   Bien, establezca una variación de la contraseña prohibida:

   ```text
   net.exe user ContosoUser LaChRymoSE!1
   The command completed successfully.
   ```

   Recuerde que esta vez ha funcionado porque la directiva está en Modo de auditoría. Debería ver los eventos 10025 y 30007 del usuario ContosoUser.

   El evento 10025 se parecerá al de este ejemplo:
   
   ```text
   The reset password for the specified user would normally have been rejected because it did not comply with the current Azure password policy. The current Azure password policy is configured for audit-only mode so the password was accepted. Please see the correlated event log message for more details.
 
   UserName: ContosoUser
   FullName: 
   ```

   El evento 30007 se parecerá al de este ejemplo:

   ```text
   The reset password for the specified user would normally have been rejected because it matches at least one of the tokens present in the per-tenant banned password list of the current Azure password policy. The current Azure password policy is configured for audit-only mode so the password was accepted.
 
   UserName: ContosoUser
   FullName: 
   ```

1. Siga probando las distintas contraseñas que elija y compruebe los resultados en el Visor de eventos mediante los procedimientos descritos en los pasos anteriores. Si necesita cambiar la directiva en Azure Portal, no olvide sincronizar la nueva directiva con el agente de controlador de dominio como se ha descrito anteriormente.

Se han descrito procedimientos que le permiten realizar pruebas controladas del comportamiento de la validación de contraseñas de la protección mediante contraseña de Azure AD. El restablecimiento de contraseñas de usuario desde la línea de comandos directamente en un controlador de dominio puede parecer un medio extraño de realizar estas pruebas, pero, como se ha descrito anteriormente, está diseñado para generar resultados repetibles. A medida que se prueban varias contraseñas, tenga en cuenta el [algoritmo de evaluación de contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated), ya que puede ayudar a explicar los resultados que no esperaba.

> [!WARNING]
> Cuando finalicen todas las pruebas, no olvide eliminar las cuentas de usuario creadas con fines de prueba.

## <a name="additional-content"></a>Contenido adicional

Los vínculos siguientes no forman parte de la documentación principal de Protección con contraseña de Azure AD, pero pueden ser una fuente de información adicional útil sobre la característica.

[La protección con contraseña de Azure AD ya está en modo de disponibilidad general](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-is-now-generally-available/ba-p/377487)

[Email Phishing Protection Guide – Part 15: Implement the Microsoft Azure AD Password Protection Service (for On-Premises too!)](http://kmartins.com/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/) (Guía de protección contra la suplantación de identidad en correos electrónicos: Implementación del servicio Protección con contraseña de Microsoft Azure AD)

[Azure AD Password Protection and Smart Lockout are now in Public Preview!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529) (Protección con contraseña de Azure AD y Bloqueo inteligente ya están en versión preliminar pública)

## <a name="microsoft-premierunified-support-training-available"></a>Aprendizaje de soporte técnico de Microsoft Premier\Unified disponible

Si le interesa obtener más detalles sobre Protección con contraseña de Azure AD e implementarla en su entorno, puede usar un servicio automático de Microsoft disponible para aquellos clientes que tengan un contrato de soporte técnico Premier o Unified. El servicio se denomina Azure Active Directory: Protección con contraseña. Póngase en contacto con su administrador de cuentas técnica para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Si tiene alguna pregunta sobre la protección con contraseña de Azure AD local que no aparece respondida aquí, envíe un elemento de comentarios a continuación. ¡Gracias!

[Implementación de la protección de contraseñas de Azure AD](howto-password-ban-bad-on-premises-deploy.md)