---
title: Usar um cache externo no Gerenciamento de API do Azure|Microsoft Docs
description: Saiba como configurar e usar um cache externo no Gerenciamento de API do Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: f57b6b35ffff85aad4d970cf9aa908d2a80eadf1
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620877"
---
# <a name="use-an-external-redis-cache-in-azure-api-management"></a>Usar um cache Redis no Gerenciamento de API do Azure

Além de utilizar o cache interno, o Gerenciamento de API do Azure também permite armazenar respostas em cache em um cache Redis externo.

Usar um cache externo permite superar algumas limitações do cache interno. É especialmente útil se você quiser:

* Evitar que o cache fique periodicamente desmarcado durante as atualizações de Gerenciamento de API
* Ter mais controle sobre sua configuração de cache
* Armazenar mais dados do que seu nível de Gerenciamento de API permite
* Usar o armazenamento em cache com a camada Consumo do Gerenciamento de API

Para saber mais sobre o cache, veja [Políticas de armazenamento em cache do Gerenciamento de API](api-management-caching-policies.md) e [Armazenamento em cache personalizado no Gerenciamento de API do Azure](api-management-sample-cache-by-key.md).

![Traga seu próprio cache para o APIM](media/api-management-howto-cache-external/overview.png)

O que você aprenderá:

> [!div class="checklist"]
> * Adicionar um cache externo no Gerenciamento de API

## <a name="availability"></a>Disponibilidade

> [!NOTE]
> Atualmente, esse recurso está disponível apenas na camada **Consumo** do Gerenciamento de API do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisará:

+ [Criar uma instância do Gerenciamento de API do Azure](get-started-create-service-instance.md)
+ Entender o [cache no Gerenciamento de API do Azure](api-management-howto-cache.md)

## <a name="create-cache"> </a> Criar um cache Redis do Azure

Esta seção explica como criar um cache Redis do Azure. Se você já tiver um cache Redis, dentro ou fora do Azure, <a href="#add-external-cache">ignore</a> essa seção e siga para a próxima.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Adicionar um cache externo

Siga as etapas abaixo para adicionar um cache Redis no Gerenciamento de API do Azure.

![Traga seu próprio cache para o APIM](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> A configuração **Usado no** especifica qual implantação regional de Gerenciamento de API se comunicará com o cache configurado no caso de uma configuração multi-regional de Gerenciamento de API. Os caches especificados como **Padrão** serão substituídos por caches com um valor regional.
>
> Por exemplo, se o Gerenciamento de API estiver hospedado nas regiões: Leste dos EUA, Sudeste Asiático e Europa Ocidental, haverá dois caches configurados, um para **Padrão** e um para **Sudeste Asiático**. O Gerenciamento de API que está em **Sudeste Asiático** usará seu próprio cache, enquanto as outras duas regiões usarão a entrada de cache **Padrão**.

### <a name="add-an-azure-redis-cache-from-the-same-subscription"></a>Adicionar um Cache Redis do Azure da mesma assinatura

1. Navegue até sua instância de Gerenciamento de API no portal do Azure.
2. Escolha a guia **Cache externo** no menu à esquerda.
3. Clique no botão **+ Adicionar**.
4. Escolha seu cache no campo suspenso **Instância de cache**.
5. Escolha **Padrão** ou especifique a região desejada no campo suspenso **Usado no**.
6. Clique em **Salvar**.

### <a name="add-a-redis-cache-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Adicionar um cache Redis hospedado fora da assinatura atual do Azure ou do Azure em geral

1. Navegue até sua instância de Gerenciamento de API no portal do Azure.
2. Escolha a guia **Cache externo** no menu à esquerda.
3. Clique no botão **+ Adicionar**.
4. Escolha **Personalizar** no campo suspenso **Instância de cache**.
5. Escolha **Padrão** ou especifique a região desejada no campo suspenso **Usado no**.
6. Forneça a cadeia de conexão do cache Redis no campo **cadeia de conexão**.
7. Clique em **Salvar**.

## <a name="use-the-external-cache"></a>Usar o cache externo

Depois que o cache externo é configurado no Gerenciamento de API do Azure, ele pode ser usado pelas políticas de cache. Confira [Adicionar cache para melhorar o desempenho no Gerenciamento de API do Azure](api-management-howto-cache.md) para ver etapas detalhadas.

## <a name="next-steps"> </a>Próximas etapas
* Para saber mais sobre as políticas de cache, veja [Políticas de cache][Caching policies] na [Referência de política do Gerenciamento de API][API Management policy reference].
* Para saber mais sobre itens de cache por chave usando expressões de política, confira [Cache personalizado no Gerenciamento de API do Azure](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx