---
title: Exemplos da CLI do Azure – Serviços de Mídia do Azure | Microsoft Docs
description: Exemplos da CLI do Azure para os Serviços de Mídia do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 5559f9055da3a2a852427c0f27d367159cdc7655
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388481"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Exemplos da CLI do Azure para os Serviços de Mídia do Azure

A tabela a seguir inclui links para exemplos da CLI do Azure para os Serviços de Mídia do Azure.

|  |  |
|---|---|
|**Conta**||
| [Criar uma conta dos Serviços de Mídia](./scripts/cli-create-account.md) | Cria uma conta dos Serviços de Mídia do Azure. Além disso, cria uma entidade de serviço que pode ser usada para acessar APIs e gerenciar a conta de forma programática. |
| [Redefinir credenciais de conta](./scripts/cli-reset-account-credentials.md)|Redefine as credenciais de conta e obtém as configurações de app.config de volta.|
|**Ativos**||
| [Criar ativos](./scripts/cli-create-asset.md)|Cria um ativo de Serviços de Mídia para carregar o conteúdo.|
| [Carregar um arquivo](./scripts/cli-upload-file-asset.md)|Carrega um arquivo local para um contêiner de armazenamento.|
| [Publicar um ativo](./scripts/cli-publish-asset.md)| Cria um localizador de Streaming e obtém as URLs de Streaming novamente. |
| **Transformações** e **Trabalhos**||
| [Criar transformações](./scripts/cli-create-transform.md)|Mostra como criar transformações. Transformações descreve um fluxo de trabalho simples de tarefas para processar seus arquivos de áudio ou vídeos (também conhecidos como “receita”).<br/> Você sempre deve verificar se uma Transformação com o nome desejado e "receita" já existe. Em caso afirmativo, reutilize-a. |
| [Criar trabalhos](./scripts/cli-create-jobs.md)|Enviar um trabalho para uma Transformação de codificação simples usando a URL HTTPs.|
| [Criar EventGrid](./scripts/cli-create-event-grid.md)|Cria uma assinatura de Grade de Eventos de nível de conta para Alterações de Estado do Trabalho.|

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
