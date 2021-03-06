---
title: Tutorial de distribuição global do Azure Cosmos DB para a API do MongoDB
description: Saiba como configurar a distribuição global do Azure Cosmos DB usando a API do MongoDB.
services: cosmos-db
keywords: distribuição global, MongoDB
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 27fa9849c13de151f6922e829514cc8838f295ea
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52874747"
---
# <a name="set-up-azure-cosmos-db-global-distribution-using-the-mongodb-api"></a>Configurar a distribuição global do Azure Cosmos DB usando a API do MongoDB

Neste artigo, mostraremos como usar o Portal do Azure para configurar a distribuição global do Azure Cosmos DB e, depois, conectar-se usando a API do MongoDB.

Este artigo aborda as seguintes tarefas: 

> [!div class="checklist"]
> * Configurar a distribuição global usando o Portal do Azure
> * Configurar a distribuição global usando a [API do MongoDB](mongodb-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]

## <a name="verifying-your-regional-setup-using-the-mongodb-api"></a>Verificar a configuração regional usando a API do MongoDB
A maneira mais simples de verificar sua configuração global dentro de API para MongoDB é executar o comando *isMaster()* no Shell do Mongo.

No Shell do Mongo:

   ```
      db.isMaster()
   ```
   
Exemplos de resultados:

   ```JSON
      {
         "_t": "IsMasterResponse",
         "ok": 1,
         "ismaster": true,
         "maxMessageSizeBytes": 4194304,
         "maxWriteBatchSize": 1000,
         "minWireVersion": 0,
         "maxWireVersion": 2,
         "tags": {
            "region": "South India"
         },
         "hosts": [
            "vishi-api-for-mongodb-southcentralus.documents.azure.com:10255",
            "vishi-api-for-mongodb-westeurope.documents.azure.com:10255",
            "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
         ],
         "setName": "globaldb",
         "setVersion": 1,
         "primary": "vishi-api-for-mongodb-southindia.documents.azure.com:10255",
         "me": "vishi-api-for-mongodb-southindia.documents.azure.com:10255"
      }
   ```

## <a name="connecting-to-a-preferred-region-using-the-mongodb-api"></a>Conectar-se a uma região preferencial usando a API do MongoDB

A API do MongoDB permite que você especifique a preferência de leitura de sua coleção para um banco de dados distribuído globalmente. Para leituras de baixa latência e alta disponibilidade global, recomendamos a definição de sua preferência de leitura da coleção como *mais próximo*. Uma preferência de leitura de *mais próximo* está configurada para ler a região mais próxima.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Nearest));
```

Para aplicativos com uma região de leitura/gravação primária e uma região secundária para cenários de DR (recuperação de desastres), recomendamos a definição da preferência de leitura da coleção como *secundário preferido*. Uma preferência de leitura de *secundário preferido* é configurada para ler a região secundária quando a região primária não está disponível.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.SecondaryPreferred));
```

Por fim, se você quiser especificar manualmente suas regiões de leitura. Defina a marca region dentro de sua preferência de leitura.

```csharp
var collection = database.GetCollection<BsonDocument>(collectionName);
var tag = new Tag("region", "Southeast Asia");
collection = collection.WithReadPreference(new ReadPreference(ReadPreferenceMode.Secondary, new[] { new TagSet(new[] { tag }) }));
```

Assim, concluímos este tutorial. Aprenda a gerenciar a consistência de sua conta globalmente replicada lendo [Níveis de consistência no Azure Cosmos DB](consistency-levels.md). E para saber mais sobre como a replicação de banco de dados global funciona no Azure Cosmos DB, veja [Distribuir dados globalmente com o Azure Cosmos DB](distribute-data-globally.md).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você fez o seguinte:

> [!div class="checklist"]
> * Configurar a distribuição global usando o Portal do Azure
> * Configurar a distribuição global usando as APIs do SQL

Agora você pode prosseguir para o próximo tutorial e aprender a desenvolver localmente usando o emulador local do Azure Cosmos DB.

> [!div class="nextstepaction"]
> [Desenvolver localmente com o emulador](local-emulator.md)
