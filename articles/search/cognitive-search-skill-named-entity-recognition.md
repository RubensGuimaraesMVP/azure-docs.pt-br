---
title: Habilidade de pesquisa cognitiva de Reconhecimento de Entidade Nomeada (Azure Search) | Microsoft Docs
description: Extrai entidades nomeadas para a pessoa, local e a organização de texto em um pipeline de pesquisa cognitivas do Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: luisca
ms.openlocfilehash: f9ff3f66f3a73fbaf1a4c2ca280c85f4bde65444
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52442022"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Habilidade cognitiva de Reconhecimento de Entidade Nomeada

A habilidade do **Reconhecimento de Entidade Nomeada** extrai entidades nomeadas de texto. Entidades disponíveis incluem os tipos `person`, `location`, e `organization`.

> [!NOTE]
> <ul>
> <li>Pesquisa Cognitiva está na visualização pública. A execução do conjunto de habilidades e a extração e normalização de imagem são oferecidas gratuitamente no momento. Posteriormente, os preços dessas funcionalidades serão anunciados. </li>
> <li> Habilidade de reconhecimento de entidade nomeada é considerada "obsoleta" e não serão oficialmente suportada começando a 15 de fevereiro de 2019. Siga as recomendações listadas na página <a href="cognitive-search-skill-deprecated.md">Habilidades de pesquisas cognitivas obsoletas</a> para migrar para uma habilidade com suporte</li>

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registro deve ser 50.000 caracteres conforme medido por `String.Length`. Se você precisar interromper o backup de seus dados antes de enviá-la para o extrator de frases-chave, considere o uso de [habilidade de Texto Dividido](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parâmetros de habilidades

Os parâmetros diferenciam maiúsculas de minúsculas.

| Nome do parâmetro     | DESCRIÇÃO |
|--------------------|-------------|
| Categorias    | Matriz de categorias que devem ser extraídas.  Tipos possíveis de categoria: `"Person"`, `"Location"`, `"Organization"`. Se nenhuma categoria for fornecida, todos os tipos são retornados.|
|defaultLanguageCode |  Código de idioma do texto de entrada. Há suporte para vários idiomas: `de, en, es, fr, it`|
| minimumPrecision  | Um número entre 0 e 1. Se a precisão for menor do que esse valor, a entidade não é retornada. O padrão é 0.|

## <a name="skill-inputs"></a>Entradas de habilidades

| Nome de entrada      | DESCRIÇÃO                   |
|---------------|-------------------------------|
| languageCode  | Opcional. O padrão é `"en"`.  |
| text          | O texto para analisar.          |

## <a name="skill-outputs"></a>Saídas de habilidades

| Nome de saída     | DESCRIÇÃO                   |
|---------------|-------------------------------|
| pessoas      | Uma matriz de cadeias de caracteres onde cada cadeia de caracteres representa o nome de uma pessoa. |
| Locais  | Uma matriz de cadeias de caracteres onde cada cadeia de caracteres representa um local. |
| organizações  | Uma matriz de cadeias de caracteres onde cada cadeia de caracteres representa uma organização. |
| entidades | Uma matriz de tipos complexos. Cada tipo complexo inclui os seguintes campos: <ul><li>categoria (`"person"`, `"organization"`, ou `"location"`)</li> <li>valor (nome de entidade real)</li><li>deslocamento (o local onde ele foi encontrado no texto)</li><li>confiança (um valor entre 0 e 1 que representa essa confiança de que o valor é uma entidade real)</li></ul> |

##  <a name="sample-definition"></a>Definição de exemplo

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
      }
    ]
  }
```
##  <a name="sample-input"></a>Entrada de exemplo

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "This is the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
             "languageCode": "en"
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Saída de exemplo

```json
{
  "values": [
    {
      "recordId": "1",
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Casos de erro
Se o código do idioma do documento não for suportado, um erro será retornado e nenhuma entidade será extraída.

## <a name="see-also"></a>Consulte também

+ [Habilidades predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)
+ [Habilidade de Reconhecimento de Entidade](cognitive-search-skill-entity-recognition.md)
