---
title: Quando e como usar um Provedor de Autenticação Multifator do Microsoft Azure?
description: Quando você deve usar um Provedor de Autenticação com o Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 41edbc621baede92c548f43ad2730b73f59756c0
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496096"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Quando usar um Provedor de Autenticação Multifator do Microsoft Azure

A autenticação em duas etapas está disponível por padrão para os administradores globais que têm o Azure Active Directory e os usuários do Office 365. No entanto, se você quiser aproveitar os [recursos avançados](howto-mfa-mfasettings.md), então, deverá adquirir a versão completa da Autenticação Multifator do Azure (MFA).

Um Provedor de Autenticação Multifator do Azure é usado para aproveitar os recursos fornecidos pela Autenticação Multifator do Microsoft Azure para usuários que **não têm licenças**.

Se você tiver licenças que abrangem todos os usuários em sua organização, em seguida, não será necessário um Provedor de Autenticação Multifator do Microsoft Azure. Crie um Provedor de Autenticação Multifator do Microsoft Azure apenas se também precisar fornecer verificação em duas etapas para alguns usuários que não tenham licenças.

> [!NOTE]
> A partir de 1º de setembro de 2018, novos provedores de autenticação não poderão mais ser criados. Os provedores de autenticação existentes podem continuar sendo usados e atualizados. A autenticação multifator continuará disponível como um recurso nas licenças do Microsoft Azure Active Directory Premium.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Limitações relacionadas ao SDK do Azure MFA

Observe que o SDK foi descontinuado e continuará funcionando até 14 de novembro de 2018. Após essa data, as chamadas para o SDK falharão.

## <a name="what-is-an-mfa-provider"></a>O que é um Provedor MFA?

Há dois tipos de provedores de autenticação e a diferença está em como a assinatura do Azure é cobrada. A opção por autenticação calcula o número de autenticações executadas em seu locatário em um mês. Essa será a melhor opção se você tiver um número de usuários que se autenticam apenas ocasionalmente. A opção por usuário calcula o número de pessoas no seu locatário que executam a verificação em duas etapas em um mês. Essa é a melhor opção se você tiver alguns usuários com licenças, mas precisar estender MFA para mais usuários, além de seus limites de licença.

## <a name="manage-your-mfa-provider"></a>Gerenciar seu `Provedor MFA

Não é possível alterar o modelo de uso (por usuário habilitado ou por autenticação) após a criação de um provedor de MFA.

Se você comprou licenças suficientes para cobrir todos os usuários que estão habilitados para MFA, é possível excluir o provedor MFA completamente.

Se o seu provedor de MFA não estiver vinculado a um locatário do Azure AD, ou você vincula o novo provedor de MFA a um locatário diferente do Azure AD, as opções de definições de usuário e de configuração não serão transferidas. Além disso, os servidores MFA existentes do Azure precisarão ser reativados usando as credenciais de ativação geradas por meio do provedor de MFA. Reativar os servidores MFA para vinculá-los ao provedor de MFA não afeta a autenticação de chamada telefônica e de mensagens de texto, mas as notificações de aplicativo móvel deixarão de funcionar para todos os usuários até que eles reativem o aplicativo móvel.

## <a name="next-steps"></a>Próximas etapas

[Definir as configurações da Autenticação Multifator](howto-mfa-mfasettings.md)
