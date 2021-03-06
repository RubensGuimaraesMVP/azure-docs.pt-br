---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: bff2543ec48c66c10db697650def0077e3de28be
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133812"
---
Para habilitar a redefinição de senha refinada no aplicativo, é utilizada uma política de **redefinição de senha**. Observe que a opção de redefinição de senha para todo o locatário é especificada [aqui](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Esta política descreve as experiências pelas quais os clientes passarão durante a redefinição de senha e o conteúdo dos tokens que o aplicativo receberá após a conclusão com êxito.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Na seção de políticas das configurações, selecione **Políticas de redefinição de senha** e clique em **+ Adicionar**.

![Selecione as políticas de inscrição ou entrada e clique no botão Adicionar](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Insira um **Nome** de política para referência de seu aplicativo. Por exemplo, insira: `SSPR`.

Selecione **Provedores de identidade** e marque **Redefinir senha usando endereço de email**. Clique em **OK**.

![Selecione Redefinir senha usando endereço de email como provedor de identidade e clique no botão OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Selecione **Declarações do aplicativo**. Escolha as declarações que você quer retornar nos tokens de autorização enviados ao seu aplicativo após uma experiência de redefinição de senha bem-sucedida. Por exemplo, selecione **ID de Objeto do Usuário**.

![Selecione algumas declarações de aplicativo e clique no botão OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Clique em **Criar** para adicionar a política. A política está listada como **B2C_1_SSPR**. O prefixo **B2C_1_** está anexado ao nome.

Abra a política selecionando **B2C_1_SSPR**. Verifique as configurações especificadas na tabela e, depois, clique em **Executar agora**.

![Selecione a política e execute-a](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Configuração      | Valor  |
| ------------ | ------ |
| **Aplicativos** | Aplicativo B2C da Contoso |
| **Selecionar URL de resposta** | `https://localhost:44316/` |

Uma nova guia do navegador é aberta e você pode verificar a experiência de redefinição de senha do consumidor em seu aplicativo.

> [!NOTE]
> Leva até um minuto para a criação de políticas e as atualizações entrem em vigor.
>
