---
title: Requisitos previos para acceder mediante programación a los datos de análisis
description: Conozca los requisitos que debe cumplir para poder acceder mediante programación a los datos de análisis del marketplace comercial.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: sayantanroy83
ms.author: sroy
ms.date: 3/08/2021
ms.openlocfilehash: 3b109048be4a94990c26e31aa5bc2ad36fdd0211
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102583565"
---
# <a name="prerequisites-to-programmatically-access-analytics-data"></a>Requisitos previos para acceder mediante programación a los datos de análisis

Antes de poder acceder mediante programación a los datos de análisis del marketplace comercial debe cumplir los requisitos siguientes.

## <a name="commercial-marketplace-enrollment"></a>Inscripción en el marketplace comercial

Para acceder a los datos de análisis del marketplace comercial mediante programación, debe inscribirse en el programa del marketplace comercial y tener una cuenta del Centro de partners. Para información sobre cómo hacerlo, vea [Creación de una cuenta de Marketplace comercial en el Centro de partners](./partner-center-portal/create-account.md).

## <a name="create-azure-active-directory-application"></a>Creación de una aplicación de Azure Active Directory

Las credenciales de usuario normales no se pueden usar para el acceso mediante programación a datos del análisis del marketplace comercial. Se debe crear una aplicación de Azure Active Directory (Azure AD) junto con un secreto para acceder a las API de análisis. Para obtener información sobre cómo crear una aplicación de Azure AD y un secreto, consulte [Inicio rápido: Registro de una aplicación en la plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app).

## <a name="associate-the-azure-ad-application-to-the-partner-center-tenant"></a>Asociación de la aplicación de Azure AD al inquilino del Centro de partners

La aplicación de Azure AD que creó en Azure Portal debe estar vinculada a su cuenta del Centro de partners. Los pasos son los siguientes:

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
1. En la parte superior derecha, seleccione el icono de engranaje y luego **Configuración de cuenta**.
1. En el menú **Configuración de cuenta**, seleccione **Administración de usuarios**.
1. Seleccione **Aplicaciones de Azure AD** y, a continuación, **+ Crear una aplicación de Azure AD**.
1. Seleccione la aplicación de Azure AD que creó en Azure Portal y, después, **Siguiente**.
1. Seleccione la casilla **Manager (Windows)** [Administrador (Windows)] y, a continuación, **Agregar**.

    :::image type="content" source="./media/analytics-programmatic-access/azure-ad-roles.png" alt-text="Muestra la página Crear una aplicación de Azure AD con las casillas para seleccionar roles.":::

## <a name="generate-an-azure-ad-token"></a>Generación de un token de Azure AD

Debe generar un token de Azure AD con el id. de la aplicación (cliente). Este id. ayuda a identificar de forma única la aplicación cliente en la plataforma de identidad de Microsoft y el secreto de cliente del paso anterior. Para conocer los pasos para generar un token de Azure AD, consulte [Llamadas entre servicios mediante las credenciales del cliente (secreto compartido o certificado)](https://docs.microsoft.com/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow).

> [!NOTE]
> El token es válido durante una hora.

## <a name="next-steps"></a>Pasos siguientes

- [Paradigma del acceso mediante programación](analytics-programmatic-access.md)
