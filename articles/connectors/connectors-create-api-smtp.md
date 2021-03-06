---
title: Conectar-se ao SMTP por meio do Aplicativo Lógico do Azure | Microsoft Docs
description: Automatizar tarefas e fluxos de trabalho que enviam email por meio de sua conta de protocolo SMTP (SMTP) usando o Aplicativo Lógico do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.topic: article
tags: connectors
ms.date: 08/25/2018
ms.openlocfilehash: 78b1eb6272fa97ef392e97723454d29cf56bb4bf
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50230498"
---
# <a name="send-email-from-your-smtp-account-with-azure-logic-apps"></a>Enviar um email de sua conta SMTP com Aplicativo Lógico do Azure

Com o Aplicativo Lógico do Azure e o conector de protocolo SMTP (SMTP), você pode criar tarefas automatizadas e fluxos de trabalho que enviam email usando sua conta SMTP. Você também pode fazer com que outras ações usem a saída das ações de SMTP. Por exemplo, depois que o SMTP envia um email, você pode notificar sua equipe no Slack com o conector do Slack. Se ainda não estiver familiarizado com os aplicativos lógicos, veja [O que é o Aplicativo Lógico do Azure?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se em uma conta gratuita do Azure</a>. 

* Suas credenciais de usuário e conta SMTP

  Suas credenciais autorizam o aplicativo lógico a criar uma conexão e acessar sua conta SMTP.

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* O aplicativo lógico no qual você deseja acessar a conta SMTP. Para usar uma ação de SMTP, inicie o aplicativo lógico com um gatilho, como um gatilho do Salesforce se você tiver uma conta do Salesforce.

  Por exemplo, você pode iniciar o aplicativo lógico com o gatilho **Quando um registro é criado** do Salesforce. 
  Esse gatilho é acionado sempre que um novo registro, como um lead, for criado no Salesforce. 
  Então, você pode dar sequência neste gatilho com a ação de SMTP **Enviar Email**. Dessa forma, quando o novo registro for criado, seu aplicativo lógico enviará um email a respeito do novo registro usando sua conta SMTP.

## <a name="connect-to-smtp"></a>Conectar-se ao SMTP

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Entre no [portal do Azure](https://portal.azure.com) e abra seu aplicativo lógico no Designer de Aplicativo Lógico, se ele ainda não estiver aberto.

1. Na última etapa em que você deseja adicionar uma ação de SMTP, escolha **Nova etapa**. 

   Para adicionar uma ação entre as etapas, mova o ponteiro sobre a seta entre as etapas. 
   Escolha o sinal de adição (**+**) que aparece e, em seguida, selecione **Adicionar uma ação**.

1. Na caixa de pesquisa, insira "smtp" como filtro. Na lista de ações, selecione a ação desejada.

1. Quando solicitado, forneça essas informações de conexão:

   | Propriedade | Obrigatório | DESCRIÇÃO |
   |----------|----------|-------------|
   | **Nome da Conexão** | SIM | Um nome para a conexão com seu servidor SMTP | 
   | **Endereço do Servidor SMTP** | SIM | O endereço do seu servidor SMTP | 
   | **Nome de usuário** | SIM | O nome de usuário da sua conta SMTP | 
   | **Senha** | SIM | A senha da sua conta SMTP | 
   | **Porta do Servidor SMTP** | Não  | Uma porta específica no servidor SMTP que você deseja usar | 
   | **Habilitar SSL?** | Não  | Ligar ou desligar a criptografia SSL. | 
   |||| 

1. Forneça os detalhes necessários para a ação selecionada. 

1. Salve seu aplicativo lógico ou continuar criando o fluxo de trabalho do aplicativo lógico.

## <a name="connector-reference"></a>Referência de conector

Para obter detalhes técnicos sobre gatilhos, ações e limites, que são explicados na descrição da OpenAPI do conector (anteriormente conhecido como Swagger), veja a [página de referência](/connectors/smtpconnector/) do conector.

## <a name="get-support"></a>Obtenha suporte

* Em caso de dúvidas, visite o [Fórum dos Aplicativos Lógicos do Azure](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ou votar em ideias de recurso, visite o [site de comentários do usuário de Aplicativos Lógicos](https://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre outros [conectores de Aplicativos Lógicos](../connectors/apis-list.md)