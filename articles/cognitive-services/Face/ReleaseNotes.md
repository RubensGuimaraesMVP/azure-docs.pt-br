---
title: Notas de lançamento - Face API Service
titleSuffix: Azure Cognitive Services
description: Notas de versão para o Serviço de API de Detecção Facial incluem um histórico das alterações para as várias versões.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6fd3d33d40b0ed142127e46dd7c9173de39947c7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121984"
---
# <a name="face-api-release-notes"></a>Notas de versão da API de Detecção Facial

Este artigo refere-se ao Face API Service versão 1.0.

### <a name="release-changes-in-may-2018"></a>Alterações de versão em maio de 2018

* Atributo `gender` aprimorado significativamente e também atributos `age`, `glasses`, `facialHair`, `hair`, `makeup` aprimorados. Use-os por meio do parâmetro [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`. 

* Limite de tamanho do arquivo de imagem de entrada maior, passando 4 MB para 6 MB em [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [Pessoa de PersonGroup – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) e [Pessoa de LargePersonGroup – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Alterações de versão em março de 2018

* Adicionado o Contêiner de Escala de Milhões: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) e [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Mais detalhes sobre [Como usar o recurso de larga escala](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Parâmetro [Face – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` aumentado de [1, 5] para [1, 100] e o padrão para 10.

### <a name="release-changes-in-may-2017"></a>Alterações de versão em maio de 2017

* Adicionados os atributos `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure` e `noise` no parâmetro [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`.

* Suporte para dez mil pessoas em um PersonGroup e [Face – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Paginação compatível em [PersonGroup pessoa – Lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) com parâmetros opcionais: `start` e `top`.

* Simultaneidade compatível na adição/exclusão de faces com relação a diferentes FaceLists e diferentes pessoas em PersonGroup.

### <a name="release-changes-in-march-2017"></a>Alterações de versão em março de 2017
* Adicionado o atributo `emotion` no parâmetro [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes`.

* Corrigida face que não podia ser detectada com o retângulo retornado de [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como `targetFace` em [FaceList – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [Pessoa de PersonGroup – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Corrigido tamanho de face detectável garantir que fique estritamente entre 36 x 36 a 4.096 x 4.096 pixels.

### <a name="release-changes-in-november-2016"></a>Alterações de versão em novembro de 2016
* Adicionada assinatura Armazenamento Facial Padrão para armazenar faces persistentes adicionais ao usar [Pessoa de PersonGroup – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ou [FaceList – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) para correspondência de similaridade ou identificação. As imagens armazenadas são cobradas a US$ 0,5 por mil faces, e essa taxa é dividida proporcionalmente por dia. As assinaturas da camada gratuita continuam limitadas a mil pessoas no total.

### <a name="release-changes-in-october-2016"></a>Alterações de versão em outubro de 2016
* Alterada a mensagem de erro de mais de uma face em targetFace, passando de 'Existem mais de uma face na imagem' para 'Existe mais de uma face na imagem' em [FaceList – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [Pessoa de PersonGroup – Adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Alterações de versão de julho de 2016
* Face compatível para autenticação de objeto em [Face – Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Adicionado o parâmetro `mode` opcional possibilitando a seleção de dois modos de trabalho: `matchPerson` e `matchFace`, em [Face – Localizar Semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e o padrão é `matchPerson`.

* Adicionado o parâmetro `confidenceThreshold` opcional para o usuário definir o limite de se uma face pertence a um objeto de Pessoa em [Face – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Adicionados os parâmetros `start` e `top` opcionais em [PersonGroup – Listar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) para permitir que o usuário especifique o ponto inicial e o número total de PersonGroups para a lista.

### <a name="v10-changes-from-v0"></a>Alterações à V1.0 da V0
* Atualizado o ponto de extremidade da raiz do serviço de ```https://westus.api.cognitive.microsoft.com/face/v0/``` para ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Alterações aplicadas a: [Face – Detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [Face – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face – Localizar Semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [Face – Agrupar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Atualizado o tamanho mínimo face detectável para 36 x 36 pixels. Faces menores que 36 x 36 pixels não serão detectadas.

* Preteridos os dados de PersonGroup e Pessoa de Detecção Facial V0. Esses dados não podem ser acessados com o serviço Detecção Facial V1.0.

* Preterido o ponto de extremidade V0 da API de Detecção Facial em 30 de junho de 2016.
