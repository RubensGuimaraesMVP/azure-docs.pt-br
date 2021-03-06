---
title: Categorizando imagens - Visão por Computador
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados à categorização de imagens usando a API do Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 7062d98d40c15f4e9e873038fc12fc1b104c996d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333472"
---
# <a name="categorizing-images"></a>Categorizar imagens

Além de marcação e descrições, o Computer Vision retorna as categorias baseadas em taxonomia definidas nas versões anteriores. Essas categorias são organizadas como uma taxonomia com hierarquias hereditárias de pai/filho. Todas as categorias estão disponíveis em inglês. Eles podem ser usados sozinhos ou com nossos novos modelos de marcação.

## <a name="the-86-category-concept"></a>O conceito de 86 categorias

Com base em uma lista de 86 conceitos vistos no diagrama a seguir, uma imagem pode ser categorizada de ampla a específica. Para obter a taxonomia completa em formato de texto, confira [Taxonomia de categoria](category-taxonomy.md).

![listas agrupadas de todas as categorias na categoria taxonomia](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Exemplos de categorização de imagens

A resposta JSON a seguir ilustra o que a Computer Vision retorna ao categorizar a imagem de exemplo com base em seus recursos visuais.

![Mulher no teto](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

A tabela a seguir ilustra um conjunto de imagens típico e a categoria retornada pelo Computer Vision para cada imagem.

| Imagem | Categoria |
|-------|----------|
| ![Foto de família](./Images/family_photo.png) | people_group |
| ![Cachorro bonito](./Images/cute_dog.png) | animal_dog |
| ![Montanha ao ar livre](./Images/mountain_vista.png) | outdoor_mountain |
| ![Alimentos pão com análise da pesquisa visual](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Próximas etapas

Aprenda conceitos sobre [marcação de imagens](concept-tagging-images.md) e [descrevendo imagens](concept-describing-images.md).