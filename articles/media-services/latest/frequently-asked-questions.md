---
title: Perguntas frequentes sobre os Serviços de Mídia do Azure v3 | Microsoft Docs
description: Este artigo fornece respostas às perguntas frequentes sobre os Serviços de Mídia do Azure v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 26661a213b5df5424bf9ab9ad799345ae35620ea
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036760"
---
# <a name="azure-media-services-v3-frequently-asked-questions"></a>Perguntas frequentes sobre os serviços de mídia do Azure v3

Este artigo fornece respostas às perguntas frequentes sobre os Serviços de Mídia do Azure (AMS) v3.

## <a name="can-i-use-the-azure-portal-to-manage-v3-resources"></a>Eu posso usar o portal do Azure para gerenciar recursos da v3?

Ainda não. Você pode usar um dos SDKs com suporte. Consulte os tutoriais e exemplos neste conjunto de documentos.

## <a name="is-there-an-api-for-configuring-media-reserved-units"></a>Há uma API para configurar Unidades Reservadas para Mídia?

Atualmente, você precisa usar APIs do AMS v2 para configurar unidades reservadas de mídia (conforme descrito em [Dimensionar o processamento de mídia](../previous/media-services-scale-media-processing-overview.md). 

Ao usar **VideoAnalyzerPreset** e/ou **AudioAnalyzerPreset**, configure sua conta de Serviços de Mídia para 10 Unidades Reservadas para Mídia S3.

## <a name="does-v3-asset-have-no-assetfile-concept"></a>O Ativo V3 não possui um conceito AssetFile?

Os AssetFiles foram removidos da API do AMS para separar os Serviços de Mídia das dependências do SDK do Armazenamento. Agora o Armazenamento, não os Serviços de Mídia, mantém as informações que pertencem ao Armazenamento. 

## <a name="where-did-client-side-storage-encryption-go"></a>Onde está a criptografia de armazenamento do lado do cliente?

Agora, é recomendável a criptografia de armazenamento do lado do servidor (que está ativada por padrão). Para obter mais informações, consulte [Criptografia do Serviço de Armazenamento do Microsoft Azure para dados inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

## <a name="what-is-the-recommended-upload-method"></a>Qual é o método de carregamento recomendado?

É recomendável o uso de ingestões HTTP(s). Para obter mais informações, consulte [Ingestão HTTP(s)](job-input-from-http-how-to.md).

## <a name="how-does-pagination-work"></a>Como a paginação funciona?

Os Serviços de Mídia dão suporte para $top para recursos que dão suporte para OData, mas o valor passado para $top deve ser menor que 1000 (por exemplo, o tamanho da página para paginação).

Isso permite que você obtenha uma pequena amostra de itens usando $top (por exemplo, os 100 itens mais recentes) ou paginar todos os itens usando paginação. 

Os Serviços de Mídia não dão suporte para paginação pelos dados com um tamanho de página especificado pelo usuário.

Para obter mais informações, consulte [Filtragem, classificação, paginação](assets-concept.md#filtering-ordering-paging)

## <a name="how-to-retrieve-an-entity-in-media-services-v3"></a>Como recuperar uma entidade nos Serviços de Mídia v3?

A v3 é baseada em uma superfície de API unificada que expõe a funcionalidade de operações e gerenciamento compilada no **Azure Resource Manager**. De acordo com o **Azure Resource Manager**, os nomes dos recursos são sempre exclusivos. Desse modo, é possível usar qualquer cadeia de caracteres de identificador exclusivo (por exemplo, GUIDs) para os nomes dos recursos. 

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Visão geral dos Serviços de Mídia v3](media-services-overview.md)
