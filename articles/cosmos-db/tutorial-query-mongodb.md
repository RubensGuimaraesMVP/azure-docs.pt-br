---
title: 'Azure Cosmos DB: como consultar usando a API do MongoDB?'
description: Aprenda a consultar com a API do MongoDB para o Azure Cosmos DB
services: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/29/2018
ms.openlocfilehash: 8c865ab7fa997b169e481612ce791d960688da56
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52841499"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-mongodb-api"></a>Tutorial: Consultar o Azure Cosmos DB usando a API do MongoDB

A [API para MongoDB](mongodb-introduction.md) do Azure Cosmos DB oferece suporte a [consultas do shell do MongoDB](https://docs.mongodb.com/manual/tutorial/query-documents/). 

Este artigo aborda as seguintes tarefas: 

> [!div class="checklist"]
> * Consultar dados com o MongoDB

Você pode começar usando os exemplos neste documento e assistindo ao vídeo [Consultar o Azure Cosmos DB com o shell do MongoDB](https://azure.microsoft.com/resources/videos/query-azure-cosmos-db-data-by-using-the-mongodb-shell/).

## <a name="sample-document"></a>Exemplo de documento

As consultas neste artigo usam o seguinte exemplo de documento.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a id="examplequery1"></a>Exemplo de consulta 1 

Com base no exemplo de documento de família acima, a consulta a seguir retorna os documentos cujo campo de id corresponde a `WakefieldFamily`.

**Consulta**
    
    db.families.find({ id: "WakefieldFamily"})

**Resultados**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a id="examplequery2"></a>Exemplo de consulta 2 

A próxima consulta retorna todos os filhos da família. 

**Consulta**
    
    db.families.find( { id: "WakefieldFamily" }, { children: true } )

**Resultados**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a id="examplequery3"></a>Exemplo de consulta 3 

A próxima consulta retorna todas as famílias registradas. 

**Consulta**
    
    db.families.find( { "isRegistered" : true })
**Resultados** Nenhum documento retornará. 

## <a id="examplequery4"></a>Exemplo de consulta 4

A próxima consulta retorna todas as famílias não registradas. 

**Consulta**
    
    db.families.find( { "isRegistered" : false })
**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery5"></a>Exemplo de consulta 5

A próxima consulta retorna todas as famílias não registradas e cujo estado seja NY. 

**Consulta**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a id="examplequery6"></a>Exemplo de consulta 6

A próxima consulta retorna todas as famílias das quais as notas dos filhos seja 8.

**Consulta**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Resultados**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a id="examplequery7"></a>Exemplo de consulta 7

A próxima consulta retorna todas as famílias das quais o tamanho da matriz de filhos seja três.

**Consulta**
  
      db.Family.find( {children: { $size:3} } )

**Resultados**

Nenhum resultado é retornado, pois não há famílias com mais de dois filhos. Somente quando o parâmetro for dois essa consulta será bem-sucedida e retornará o documento completo.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você fez o seguinte:

> [!div class="checklist"]
> * Aprendeu a consultar usando o MongoDB 

Agora você pode prosseguir para o próximo tutorial e aprender a distribuir seus dados globalmente.

> [!div class="nextstepaction"]
> [Distribuir os dados globalmente](tutorial-global-distribution-sql-api.md)

