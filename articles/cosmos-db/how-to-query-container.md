---
title: Consultar contêineres no Azure Cosmos DB
description: Aprenda a consultar contêineres no Azure Cosmos DB
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 11/06/2018
ms.author: mjbrown
ms.openlocfilehash: 5d64aa8b50cdde23d1bb8980510cfac202204f9a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262447"
---
# <a name="query-containers-in-azure-cosmos-db"></a>Consultar contêineres no Azure Cosmos DB

Este artigo explica como consultar um contêiner (coleção, grafo, tabela) no Azure Cosmos DB.

## <a name="in-partition-query"></a>Consulta na partição

Quando você consulta dados de contêineres, o Azure Cosmos DB encaminha automaticamente a consulta para as partições que correspondem aos valores de chave de partição especificados no filtro (se houver). Por exemplo, esta consulta é roteada para apenas a partição que contém a chave de partição "XMS-0001".

```csharp
// Query using partition key into a class called, DeviceReading
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```

## <a name="cross-partition-query"></a>Consulta entre partições

A consulta a seguir não tem um filtro na chave de partição (DeviceId) e é distribuída para todas as partições em que ela é executada no índice da partição. Para executar uma consulta entre partições, defina **EnableCrossPartitionQuery** como true (ou x-ms-documentdb-query-enablecrosspartition na API REST).

```csharp
// Query across partition keys into a class called, DeviceReading
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

O Cosmos DB dá suporte a funções de agregação COUNT, MIN, MAX e AVG em contêineres usando o SQL. As funções de agregação de contêineres começa na versão 1.12.0 do SDK e posteriores. As consultas devem incluir um único operador de agregação e um único valor na projeção.

## <a name="parallel-cross-partition-query"></a>Consulta entre partições em paralelo

Os SDKs do Cosmos DB 1.9.0 e versões superiores dão suporte a opções de execução de consultas paralelas.  Consultas entre partições em paralelo permitem que você execute consultas entre partições de baixa latência. Por exemplo, a consulta a seguir é configurada para ser executada paralelamente entre partições.

```csharp
// Cross-partition Order By Query with parallel execution
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("myDatabaseName", "myCollectionName"),  
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```

Você pode gerenciar a execução de consulta paralela ajustando os seguintes parâmetros:

- **MaxDegreeOfParallelism**: define o número máximo de conexões de rede simultâneas com as partições do contêiner. Se você definir essa propriedade como -1, o grau de paralelismo será gerenciado pelo SDK. Se o MaxDegreeOfParallelism não for especificado nem definido como 0, que é o valor padrão, haverá uma única conexão de rede com as partições do contêiner.

- **MaxBufferedItemCount**: negocia a latência da consulta em comparação com a utilização de memória do lado do cliente. Se a opção for omitida ou definida como -1, o número de itens armazenados em buffer durante a execução da consulta paralela será gerenciado pelo SDK.

Tendo o mesmo estado da coleção, uma consulta paralela retornará resultados na mesma ordem de uma execução serial. Ao executar uma consulta entre partições que inclui operadores de classificação (ORDER BY e/ou TOP), o SDK do Azure Cosmos DB emite a consulta paralelamente entre partições e mescla os resultados parcialmente classificados no lado do cliente para produzir resultados ordenados globalmente.

## <a name="next-steps"></a>Próximas etapas

Consulte os seguintes artigos para saber mais sobre particionamento no Cosmos DB:

- [Particionamento no Azure Cosmos DB](partitioning-overview.md)
- [Chaves de partição sintética no Azure Cosmos DB](synthetic-partition-keys.md)
