---
title: Atualizar serviço de aplicativo do Azure no Azure Stack | Microsoft Docs
description: Orientações detalhadas para a atualização do serviço de aplicativo do Azure no Azure Stack
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: anwestg
ms.openlocfilehash: e8a75afe2c7dbe91c7c98d0d35c319088f40748f
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612671"
---
# <a name="update-azure-app-service-on-azure-stack"></a>Atualizar serviço de aplicativo do Azure no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

> [!IMPORTANT]  
> Aplicar a atualização 1809 seu sistema integrado do Azure Stack ou implantar o kit de desenvolvimento mais recente do Azure Stack antes de implantar 1.4 de serviço de aplicativo do Azure.
>
>

Seguindo as instruções neste artigo, você pode atualizar o [provedor de recursos do serviço de aplicativo](azure-stack-app-service-overview.md) implantado em um ambiente do Azure Stack que está conectado à Internet.

> [!IMPORTANT]  
> Antes de executar a atualização, certifique-se de que você já tenha concluído a [implantação do serviço de aplicativo do Azure no provedor de recursos do Azure Stack](azure-stack-app-service-deploy.md)

## <a name="run-the-app-service-resource-provider-installer"></a>Execute o instalador de provedor de recursos de serviço de aplicativo

Durante esse processo, a atualização será:

* Detectar anterior em implantação do serviço de aplicativo
* Preparar todos os pacotes de atualização e novas versões das bibliotecas de todos os sistemas operacionais a serem implantados
* Carregar no armazenamento
* Atualizar todas as funções de serviço de aplicativo (controladores, gerenciamento, front-end, Publisher e Worker funções)
* Atualizar as definições do conjunto de dimensionamento do Serviço de Aplicativo
* Atualizar manifesto do provedor de recursos de serviço de aplicativo

> [!IMPORTANT]
> O instalador do serviço de aplicativo deve ser executado em um computador que pode alcançar o ponto de extremidade do Azure Stack administrador do Azure Resource Manager.
>
>

Para atualizar sua implantação do serviço de aplicativo no Azure Stack, siga estas etapas:

1. Baixe o [instalador do serviço de aplicativo](https://aka.ms/appsvcupdate4installer)

2. Execute appservice.exe como um administrador

    ![Instalador do serviço de aplicativo][1]

3. Clique em **implantar o serviço de aplicativo ou atualização para a versão mais recente.**

4. Examine e aceite os termos de licença de Software da Microsoft e, em seguida, clique em **próxima**.

5. Examine e aceite os termos de licença de terceiros e, em seguida, clique em **próxima**.

6. Certifique-se de que o ponto de extremidade de pilha do Azure Resource Manager e o locatário do Active Directory informações estão corretas. Se você usou as configurações padrão durante a implantação do Kit de desenvolvimento do Azure Stack, você pode aceitar os valores padrão aqui. No entanto, se você personalizou as opções quando você implantou o Azure Stack, você deve editar os valores nessa janela. Por exemplo, se você usar o sufixo do domínio *mycloud.com*, altere para o ponto de extremidade de pilha do Azure Resource Manager *management.region.mycloud.com*. Depois de confirmar suas informações, clique em **próxima**.

    ![Informações de nuvem do Azure Stack][2]

7. Na próxima página:

   1. Clique o **Connect** lado a **assinaturas do Azure Stack** caixa.
        * Se você estiver usando o Azure Active Directory (Azure AD), insira a conta de administrador do Azure AD e a senha que você forneceu quando você implantou o Azure Stack. Clique em **entrar**.
        * Se você estiver usando os serviços de Federação do Active Directory (AD FS), forneça sua conta de administrador. Por exemplo, *cloudadmin@azurestack.local*. Insira sua senha e clique em **Sign In**.
   2. No **assinaturas do Azure Stack** caixa, selecione a **assinatura do provedor padrão**.
   3. No **locais da pilha do Azure** , selecione o local que corresponde à região em que você está implantando. Por exemplo, selecione **local** se sua implantação para o Kit de desenvolvimento do Azure Stack.
   4. Se uma implantação de serviço de aplicativo existente for detectada, em seguida, a conta de armazenamento e grupo de recursos será preenchida e esmaecida.
   5. Clique em **próxima** para examinar o resumo da atualização.

    ![Instalação do serviço de aplicativo detectada][3]

8. Na página de resumo:
   1. Verifique se as seleções feitas por você. Para fazer alterações, use o **anterior** botões para visitar a páginas anteriores.
   2. Se as configurações estão corretas, selecione a caixa de seleção.
   3. Para iniciar a atualização, clique em **próxima**.

       ![Resumo de atualização do serviço de aplicativo][4]

9. Página progresso da atualização:
    1. Acompanhe o progresso da atualização. A duração da atualização do serviço de aplicativo no Azure Stack varia de acordo com o número de instâncias de função implantadas.
    2. Depois que a atualização for concluída com êxito, clique em **Exit**.

        ![Progresso da atualização do serviço de aplicativo][5]

<!--Image references-->
[1]: ./media/azure-stack-app-service-update/app-service-exe.png
[2]: ./media/azure-stack-app-service-update/app-service-azure-resource-manager-endpoints.png
[3]: ./media/azure-stack-app-service-update/app-service-installation-detected.png
[4]: ./media/azure-stack-app-service-update/app-service-upgrade-summary.png
[5]: ./media/azure-stack-app-service-update/app-service-upgrade-complete.png

## <a name="next-steps"></a>Próximas etapas

Você também pode experimentar outras [plataforma como um serviço (PaaS) serviços](azure-stack-tools-paas-services.md).

* [Provedor de recursos do SQL Server](azure-stack-sql-resource-provider-deploy.md)
* [Provedor de recursos MySQL](azure-stack-mysql-resource-provider-deploy.md)
