---
title: Erro Nenhuma assinatura encontrada ao tentar entrar no Portal do Azure ou no centro de contas do Azure | Microsoft Docs
description: Fornece a solução para um problema no qual o erro Nenhuma assinatura encontrada ocorre ao entrar no Portal do Azure ou no centro de contas do Azure.
services: ''
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: cwatson
ms.openlocfilehash: 584342b3dd223c45495db36ad49d83dece858137
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52581775"
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Erro Nenhuma assinatura encontrada no Portal do Azure ou no centro de contas do Azure

Você poderá receber uma mensagem de erro "Nenhuma assinatura encontrada" ao tentar entrar no [Portal do Azure](https://portal.azure.com/) ou no [Centro de Contas do Azure](https://account.windowsazure.com/Subscriptions). Este artigo fornece uma solução para esse problema.

## <a name="symptom"></a>Sintoma

Ao tentar entrar no [Portal do Azure](https://portal.azure.com/) ou no [Centro de contas do Azure](https://account.windowsazure.com/Subscriptions), você recebe a seguinte mensagem de erro: "Nenhuma assinatura encontrada".

## <a name="cause"></a>Causa

Esse problema ocorrerá se tiver selecionado no diretório errado ou se sua conta não tiver permissões suficientes. 

## <a name="solution"></a>Solução

### <a name="scenario-1-error-message-is-received-in-the-azure-portalhttpsportalazurecom"></a>Cenário 1: A mensagem de erro é recebida no [Portal do Azure](https://portal.azure.com)

Para corrigir esse problema:

* Verifique se o diretório correto do Azure está selecionado clicando em sua conta no canto superior direito.

  ![Selecione o diretório na parte superior direita do portal do Azure](./media/billing-no-subscriptions-found/directory-switch.png)
* Se o diretório correto do Azure estiver selecionado, mas você continuar recebendo a mensagem de erro, [atribua a função Proprietário à sua conta](../role-based-access-control/role-assignments-portal.md).

### <a name="scenario-2-error-message-is-received-in-the-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Cenário 2: a mensagem de erro é recebida no [Centro de Contas do Azure](https://account.windowsazure.com/Subscriptions)

Verifique se a conta usada é o Administrador da Conta. Para verificar quem é o Administrador da Conta, siga estas etapas:

1. Entre em [Modo de exibição Assinaturas no Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a assinatura que você deseja verificar e olhe as **Configurações**.
1. Selecione **Propriedades**. O administrador da conta da assinatura será exibido na caixa **Administrador da Conta** .  

## <a name="need-help-contact-us"></a>Precisa de ajuda? Fale conosco.

Se você tiver dúvidas ou precisar de ajuda, [crie uma solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
