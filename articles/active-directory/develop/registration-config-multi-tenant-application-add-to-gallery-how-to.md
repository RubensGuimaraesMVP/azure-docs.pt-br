---
title: Adicionar um aplicativo multilocatário à galeria de aplicativos do Azure Active Directory | Microsoft Docs
description: Explica como é possível listar o aplicativo multilocatário personalizado na galeria de aplicativos do Azure Active Directory.
services: active-directory
documentationCenter: na
author: CelesteDG
manager: mtillman
ms.assetid: 92c1651a-675d-42c8-b337-f78e7dbcc40d
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: jeedes
ms.openlocfilehash: b89be37d9c77fb83e607a1af0bc2f6ba716ab2c1
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2018
ms.locfileid: "44723597"
---
# <a name="add-a-multitenant-application-to-the-azure-ad-application-gallery"></a>Adicionar um aplicativo multilocatário à galeria de aplicativos do Azure Active Directory

## <a name="what-is-the-azure-ad-application-gallery"></a>O que é a galeria de aplicativos do Azure Active Directory?

O Azure Active Directory é um serviço de identidade baseado em nuvem. A [galeria de aplicativos do Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/) está na loja de aplicativos do Azure Marketplace, onde todos os conectores de aplicativos são publicados para logon único e provisionamento de usuário. Os clientes que usam o Azure Active Directory como provedor de identidade localizam diferentes conectores de aplicativos SaaS publicados aqui. Os administradores de TI adicionam conectores da galeria de aplicativos e, em seguida, configuram e usam os conectores para logon único e provisionamento. O Azure Active Directory fornece suporte a todos os principais protocolos de federação, incluindo SAML 2.0, OpenID Connect, OAuth e WS-Fed para logon único. 

## <a name="if-your-application-supports-saml-or-openidconnect"></a>Se seu aplicativo der suporte a SAML ou OpenIDConnect
Se você tiver um aplicativo multilocatário que deseja listar na galeria de aplicativos do Azure Active Directory, será necessário primeiro certificar-se de que o aplicativo fornece suporte a uma das tecnologias de logon único seguir:

- **OpenID Connect**: para que o aplicativo seja listado, crie o aplicativo multilocatário no Azure Active Directory e implemente a [estrutura de consentimento do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#overview-of-the-consent-framework) para o aplicativo. Envie a solicitação de logon para um ponto de extremidade comum para que qualquer cliente possa dar consentimento ao aplicativo. É possível pode controlar o acesso de um usuário com base na ID do locatário e o UPN do usuário recebido no token. Envie o aplicativo usando o processo descrito em [Listando seu aplicativo na galeria de aplicativos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

- **SAML**: Se o aplicativo tiver suporte para SAML 2.0, o aplicativo poderá ser listado na galeria. Siga as instruções em [Listando seu aplicativo na galeria de aplicativos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="if-your-application-does-not-support-saml-or-openidconnect"></a>Se seu aplicativo não der suporte a SAML ou OpenIDConnect
Aplicativos que não têm suporte para SAML ou OpenIDConnect poderão ainda ser integrados na galeria de aplicativos por meio de tecnologia de logon único de senha.

O logon único de senha, também chamado de cofre de senha, permite que você gerencie o acesso e senhas de usuários a aplicativos Web que não têm suporte para federação de identidade. Também é útil para cenários, nos quais vários usuários precisam compartilhar uma única conta, como contas de aplicativo de mídia social da sua organização. 

Se você quiser listar o aplicativo com essa tecnologia:
1. Crie um aplicativo Web que tenha uma página de logon HTML para configurar o [logon único de senha](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis). 
2. Envie a solicitação, conforme descrito em [Listando seu aplicativo na galeria de aplicativos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

## <a name="escalations"></a>Escalonamentos

Para qualquer escalonamentos, envie um email para a [Equipe de Integração de SSO do Azure Active Directory](<mailto:SaaSApplicationIntegrations@service.microsoft.com>) e retornaremos o mais rápido possível.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre como [listar seu aplicativo na galeria de aplicativos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).
