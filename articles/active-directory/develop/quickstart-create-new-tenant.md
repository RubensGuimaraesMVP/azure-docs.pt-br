---
title: Criar um locatário do Azure Active Directory | Microsoft Docs
description: Saiba como criar um locatário do Azure AD a ser usado para registrar e criar aplicativos.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 731b68e3f7dbb46f2fa51a18cb5b3da6b4626fa6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963927"
---
# <a name="quickstart-set-up-a-dev-environment"></a>Início Rápido: Configurar um ambiente de desenvolvimento

A plataforma de identidade da Microsoft permite que os desenvolvedores criem aplicativos direcionados para uma ampla variedade de ambientes e identidades personalizados do Microsoft 365. Para começar a usar a plataforma de identidade da Microsoft, você acessará a um ambiente, também chamado de locatário do Azure AD, que pode registrar e gerenciar aplicativos, acessar os dados do Microsoft 365 e implantar restrições personalizadas de acesso condicional e de locatário. 

Um locatário é a representação de uma organização. É uma instância dedicada do Azure AD que uma organização ou um desenvolvedor de aplicativos recebe ao criar uma relação com Microsoft, como inscrever-se no Azure, no Microsoft Intune ou no Microsoft 365. 

Cada locatário do Azure AD é distinto, separado de outros locatários do Azure AD e tem sua própria representação de identidades corporativas e de estudante, de identidades de consumidor (caso seja um locatário do Azure AD B2C) e de registros de aplicativo. Um registro de aplicativo dentro de seu locatário pode permitir autenticações de contas somente dentro do seu locatário ou de todos os locatários. 

## <a name="determining-environment-type"></a>Determinando o tipo de ambiente

Há dois tipos de ambientes que você pode criar. A decisão de qual você precisa baseia-se exclusivamente nos tipos de usuários que seu aplicativo autenticará.

* Contas corporativas ou de estudante (contas do Azure AD) ou as contas Microsoft (como o outlook.com e o live.com)
* Contas locais e sociais (Azure AD B2C)

O guia de Início Rápido é dividido em dois cenários, dependendo do tipo de aplicativo que você deseja criar. Se você precisa de mais ajuda para direcionar um tipo de identidade, confira [Sobre a plataforma de identidade da Microsoft](about-microsoft-identity-platform.md)

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>Contas corporativas ou de estudante ou contas pessoais da Microsoft

### <a name="use-an-existing-tenant"></a>Usar um locatário existente

Muitos desenvolvedores já contam com locatários por meio de serviços ou assinaturas vinculadas a locatários do Azure AD como assinaturas do Microsoft 365 ou do Azure.

1. Para verificar o locatário, entre no [portal do Azure](https://portal.azure.com) com a conta que você deseja usar para gerenciar seu aplicativo.
1. Verifique o canto superior direito. Se houver um locatário, você será conectado a ele automaticamente e verá o nome do locatário diretamente abaixo do nome da conta.
   * Passe o mouse sobre o nome da conta no canto superior direito do portal do Azure para ver seu nome, email, diretório e ID do locatário (um GUID) e seu domínio.
   * Se sua conta estiver associada a vários locatários, você pode selecionar o nome da sua conta para abrir um menu no qual você pode alternar entre locatários. Cada locatário tem sua própria ID exclusiva.

> [!TIP]
> Se você precisar localizar a ID do locatário:
* Passe o mouse sobre o nome da sua conta para obter o diretório ou a ID do locatário ou
* Selecione **Azure Active Directory > Propriedades > ID do diretório** no portal do Azure

Se não houver um locatário existente associado à sua conta, você verá um GUID abaixo do nome da conta e só poderá executar ações, como registrar aplicativos, depois de seguir as etapas da próxima seção.

### <a name="create-a-new-azure-ad-tenant"></a>Criar um novo locatário do Azure Active Directory

Se você ainda não tem um locatário do Azure AD ou deseja criar um para desenvolvimento, siga a [experiência de criação de diretório](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory). Você precisará fornecer as informações a seguir para criar o locatário:

- **Nome da organização**
- **Domínio inicial** – fará parte do *.onmicrosoft.com. Você poderá personalizar o domínio mais tarde. 
- **País ou região**

## <a name="social-and-local-accounts"></a>Contas locais e sociais

Para começar a criar aplicativos que entram em contas sociais e locais, você precisará criar um locatário do Azure AD B2C. Para começar, siga [criando um locatário do Azure AD B2C](../../active-directory-b2c/tutorial-create-tenant.md). 

## <a name="next-steps"></a>Próximas etapas

* Experimente um início rápido de codificação e comece a autenticação de usuários. 
* Para obter exemplos de código mais detalhados, confira a seção **Tutoriais** da documentação.
* Deseja implantar seu aplicativo na nuvem? Confira [implantando contêineres no Azure](https://docs.microsoft.com/azure/index#pivot=products&panel=containers). 
