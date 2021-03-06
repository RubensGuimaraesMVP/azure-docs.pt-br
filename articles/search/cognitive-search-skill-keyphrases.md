---
title: Habilidade de pesquisa cognitiva de frase-chave (Azure Search) | Microsoft Docs
description: Avalia o texto não estruturado e para cada registro, retorna uma lista de palavras-chave em um pipeline de enriquecimento do Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 694271115626c652523be34160ad6a07053f6387
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735791"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Habilidade cognitiva de Extração de Frases-Chave

A habilidade de **Extração de Frases-Chave** avalia o texto não estruturado e para cada registro, retorna uma lista de frases-chave.

Esse recurso é útil se você precisar identificar rapidamente os principais pontos de estratégias no registro. Por exemplo, texto de entrada especificado “A comida estava deliciosa e a equipe foi maravilhosa”, o serviço retorna “comida” e “equipe maravilhosa”.

> [!NOTE]
> Pesquisa Cognitiva está na visualização pública. A execução do conjunto de qualificações e a extração e normalização de imagem são oferecidas gratuitamente. Em um momento posterior, os preços desses recursos serão anunciados. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Limites de dados
O tamanho máximo de um registro deve ser 50.000 caracteres conforme medido por `String.Length`. Se você precisar interromper o backup de seus dados antes de enviá-la para o extrator de frases-chave, considere o uso de [habilidade de Texto Dividido](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Parâmetros de habilidades

Os parâmetros diferenciam maiúsculas de minúsculas.
| Entradas                | DESCRIÇÃO |
|---------------------|-------------|
| defaultLanguageCode | (opcional) O código de idioma a ser aplicado a documentos que não especifica explicitamente o idioma.  Se o código de idioma padrão não for especificado, em inglês (en) será usado como o código de idioma padrão. <br/> Consulte [Lista completa dos idiomas com suporte](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (opcional) O número máximo de frases-chave para produzir. |

## <a name="skill-inputs"></a>Entradas de habilidades
| Entradas     | DESCRIÇÃO |
|--------------------|-------------|
| text | O texto a ser analisado.|
| languageCode  |  Uma cadeia de caracteres que indica o idioma dos registros. Se esse parâmetro não for especificado, o código de idioma padrão será usado para analisar os registros. <br/>Consulte [Lista completa dos idiomas com suporte](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

##  <a name="sample-definition"></a>Definição de exemplo

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      },
      {
        "name": "languageCode",
        "source": "/document/languagecode" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
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
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
             "language": "en"
           }
      }
    ]
```


##  <a name="sample-output"></a>Saída de exemplo

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "keyPhrases": 
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
           }
      }
    ]
}
```


## <a name="errors-and-warnings"></a>Erros e avisos
Se você fornecer um código de idioma sem suporte, será gerado um erro e frases-chave não são extraídos.
Se o texto estiver vazio, um aviso será gerado.
Se o texto for maior que 50.000 caracteres, somente os primeiros 50.000 caracteres serão analisados e um aviso será emitido.

## <a name="see-also"></a>Consulte também

+ [Habilidades predefinidas](cognitive-search-predefined-skills.md)
+ [Como definir um conjunto de qualificações](cognitive-search-defining-skillset.md)