---
title: Estenda sua experiência com o R - Azure Machine Learning Studio | Microsoft Docs
description: Como estender a funcionalidade do Azure Machine Learning Studio por meio da linguagem R usando o módulo Executar o Script R.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: df6453f1cf6e8b372239d528067a7258d645518d
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311001"
---
# <a name="azure-machine-learning-studio-extend-your-experiment-with-r"></a>Microsoft Azure Machine Learning Studio: estenda seu experimento com o R 
Você pode estender a funcionalidade do Machine Learning Studio do Azure por meio da linguagem R, usando o módulo [Executar Script R][execute-r-script].

Esse módulo aceita vários conjuntos de dados de entrada e produz um único conjunto de dados como saída. Você pode digitar um script R no parâmetro **Script R** do módulo [Executar Script R][execute-r-script].

Você pode acessar cada porta de entrada do módulo usando código semelhante ao seguinte:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Listando todos os pacotes instalados no momento
A lista de pacotes instalados pode mudar. Uma lista de pacotes instalados atualmente pode ser encontrada em [R Packages Suportados pelo Azure Machine Learning Studio](https://msdn.microsoft.com/library/azure/mt741980.aspx).

Obtenha também uma lista completa e atual dos pacotes instalados, digitando o seguinte código no módulo [Executar Script R][execute-r-script]:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Isso envia a lista de pacotes para a porta de saída do módulo [Executar Script R][execute-r-script].
Para exibir a lista de pacotes, conecte um módulo de conversão, como o [Converter para CSV][convert-to-csv] na saída à esquerda do módulo [Executar Script R][execute-r-script], execute o experimento e, então, clique na saída do módulo de conversão e selecione **Baixar**. 

![Baixar saída do módulo “Converter para CSV”](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importando pacotes
Você pode importar pacotes ainda não instalados usando os seguintes comandos no módulo [Executar Script R][execute-r-script] :

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

onde o arquivo `my_favorite_package.zip` contém o pacote.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
