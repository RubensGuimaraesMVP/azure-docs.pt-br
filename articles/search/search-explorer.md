---
title: Search Explorer para consultar índices no Azure Search | Microsoft Docs
description: Saiba como usar o Search Explorer para consultar índices no Azure Search.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.openlocfilehash: 520d9e7b1899c54d922ff6fb77e0901f9609b029
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004126"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Como usar o Search Explorer para consultar índices no Azure Search 

Este artigo mostra como consultar um índice existente do Azure Search usando o **Search Explorer** no portal do Azure. Você pode usar o Search Explorer para enviar cadeias de caracteres de consulta Lucene simples ou completas para qualquer índice existente em seu serviço.

## <a name="open-the-service-dashboard"></a>Abrir o painel de serviço
1. Clique em **Todos os recursos** no menu à esquerda do [portal do Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Selecione o serviço Azure Search.

## <a name="select-an-index"></a>Selecionar um índice

Selecione o índice que você deseja pesquisar no bloco **Índices**.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Abrir o Search Explorer

Clique no bloco do Search Explorer para deslizar e abrir a barra de pesquisa e o painel de resultados.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Iniciar a pesquisa

Ao usar o Search Explorer, você pode especificar [parâmetros de consulta](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para formular a consulta.

1. Em **Cadeia de caracteres de consulta**, digite uma consulta e então pressione **Pesquisar**. 

   A cadeia de caracteres de consulta é analisada automaticamente para a URL de solicitação adequada para enviar uma solicitação HTTP em relação à API REST do Azure Search.   
   
   Você pode usar qualquer sintaxe de consulta Lucene simples ou completa válida para criar a solicitação. O caractere `*` é equivalente a uma pesquisa vazia ou não especificada que retorna todos os documentos em nenhuma ordem específica.

2. Em **Resultados**, os resultados da consulta são apresentados em JSON bruto, idêntico à carga retornada em um Corpo de Resposta HTTP ao emitir solicitações de forma programática.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Próximas etapas

Os recursos a seguir fornecem exemplos e informações de sintaxe de consulta adicionais.

 + [Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Sintaxe de consulta Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Exemplos de sintaxe de consulta Lucene](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Sintaxe de expressão do filtro OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 