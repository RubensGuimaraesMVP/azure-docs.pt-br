---
title: Método CalcHistogram - API do Serviço de Exploração de Conhecimento
titlesuffix: Azure Cognitive Services
description: Aprenda a usar o método CalcHistogram na API do Serviço de Exploração de Conhecimento (KES).
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 0ca43d6f6879198b8f80794c1948439e15f312ad
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122749"
---
# <a name="calchistogram-method"></a>Método CalcHistogram
O método *calchistogram* calcula os objetos que correspondam a uma expressão de consulta estruturada e calcula a distribuição dos valores de atributo.

## <a name="request"></a>Solicitação
`http://<host>/calchistogram?expr=<expr>[&options]` 

NOME|Valor|DESCRIÇÃO
----|-----|-----------
expr | Cadeia de caracteres de texto | Expressão de consulta estruturada que especifica as entidades do índice sobre as quais calcular histogramas.
atributos | Cadeia de caracteres de texto (padrão = "") | Lista delimitada por vírgulas de atributo a ser incluído na resposta.
count   | Número (padrão = 10) | Número de resultados para retornar.
deslocamento  | Número (padrão = 0) | Índice do primeiro resultado para retornar.

## <a name="response-json"></a>Resposta (JSON)
JSONPath | DESCRIÇÃO
----|----
$.expr | O parâmetro *expr* da solicitação.
$.num_entities | Número total de entidades correspondentes.
$.histograms |  Matriz de histogramas, uma para cada atributo solicitado.
$.histograms[\*].attribute | Nome do atributo sobre o qual o histograma foi calculado.
$.histograms[\*].distinct_values | Número de valores distintos entre a correspondência de entidades para este atributo.
$.histograms[\*].total_count | Número total de instâncias de valores entre a correspondência de entidades para este atributo.
$.histograms[\*].histogram | Dados de histograma para este atributo.
$.histograms[\*].histogram[\*].value | Valor do atributo.
$.histograms[\*].histogram[\*].logprob  | Probabilidade de log natural total de entidades correspondentes com o valor de atributo.
$.histograms[\*].histogram[\*].count    | Número de entidades correspondentes com este valor do atributo.
$.aborted | Verdadeiro se a solicitação atingiu o tempo limite.

### <a name="example"></a>Exemplo
No exemplo de publicações acadêmicas, o seguinte calcula um histograma de contagens de publicação por ano e por palavra-chave para um determinado autor desde 2013:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

A resposta indica que há 37 documentos que correspondem à expressão de consulta.  Para o atributo *Ano*, há 3 valores distintos, um para cada ano desde 2013.  A contagem total de artigos sobre os 3 valores distintos é 37.  Para cada *Ano*, o histograma mostra o valor, a probabilidade de log natural total e a contagem de entidades correspondentes.     

O histograma para *palavra-chave* mostra que existem 34 palavras-chave distintas. Como um documento pode ser associado a várias palavras-chave, a contagem total (53) pode ser maior que o número de entidades correspondentes.  Embora haja 34 valores distintos, a resposta inclui somente os 4 primeiros devido ao parâmetro contagem = 4.

```json
{
  "expr": "And(Composite(Author.Name=='jaime teevan'),Y>=2013)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -6.894,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -6.927,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -7.082,
          "count": 10
        }
      ]
    },
    {
      "attribute": "Keyword",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -7.142,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -7.389,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -7.623,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -7.674,
          "count": 2
        }
      ]
    }
  ]
}
``` 
