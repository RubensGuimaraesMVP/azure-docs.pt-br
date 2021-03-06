---
title: Instalar e executar contêineres
titleSuffix: Text Analytics - Cognitive Services - Azure
description: Como baixar, instalar e executar contêineres para Análise de Texto neste tutorial passo a passo.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 11/14/2018
ms.author: diberry
ms.openlocfilehash: 99bdb42d9a0d86d0d2acc4a6272e0c802042e6b5
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634869"
---
# <a name="install-and-run-containers"></a>Instalar e executar contêineres

O uso de contêineres é uma abordagem de distribuição de software em que um aplicativo ou serviço é empacotado como uma imagem de contêiner. A configuração e as dependências para o aplicativo ou serviço são incluídas na imagem de contêiner. A imagem de contêiner pode ser implantada em um host de contêiner com pouca ou nenhuma modificação. Os contêineres são isolados entre si e o sistema operacional subjacente, com um volume menor do que uma máquina virtual. Contêineres podem ser criados com base em imagens de contêiner para tarefas de curto prazo e removidos quando não são mais necessários.

A Análise de Texto fornece o seguinte conjunto de contêineres do Docker, cada um deles contendo um subconjunto da funcionalidade:

| Contêiner| DESCRIÇÃO |
|----------|-------------|
|Extração de Frases-Chave | Extrai as frases-chave para identificar os principais pontos. Por exemplo, para o texto de entrada "A comida estava deliciosa e a equipe era maravilhosa", a API retorna os principais pontos de discussão: "comida" e "equipe maravilhosa". |
|Detecção de Idioma | Para até 120 idiomas, detecta e relata em qual idioma o texto de entrada é escrito. O contêiner relata um código de idioma único para cada documento incluído na solicitação. O código de idioma é emparelhado com uma pontuação que indica a intensidade da pontuação. |
|Análise de Sentimento | Analisa o texto bruto para obter pistas sobre o sentimento positivo ou negativo. Essa API retorna uma pontuação de sentimento entre 0 e 1 para cada documento, em que 1 é a mais positiva. Os modelos de análise são pré-treinados usando um amplo corpo de texto e tecnologias de idioma natural da Microsoft. Para [idiomas selecionados](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages.md), a API pode analisar e pontuar qualquer texto bruto que você forneça, retornando diretamente os resultados ao aplicativo responsável pela chamada. |

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="preparation"></a>Preparação

Você deve cumprir os seguintes pré-requisitos antes de usar os contêineres de Análise de Texto:

**Mecanismo do Docker**: você deve ter o mecanismo do Docker instalado localmente. O Docker fornece pacotes que configuram o ambiente do Docker em [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms) e [Windows](https://docs.docker.com/docker-for-windows/). No Windows, o Docker deve ser configurado para dar suporte a contêineres do Linux. Contêineres do Docker também podem ser implantados diretamente para o [Serviço de Kubernetes do Azure](/azure/aks/), [Instâncias de Contêiner do Azure](/azure/container-instances/), ou um cluster do [Kubernetes](https://kubernetes.io/) implantado no [Azure Stack](/azure/azure-stack/). Para obter mais informações sobre a implantação do Kubernetes para o Azure Stack, confira [Implantar Kubernetes para o Azure Stack](/azure/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).

O Docker deve ser configurado para permitir que os contêineres se conectem com o Azure e enviem dados de cobrança para ele.

**Familiaridade com o Registro de Contêiner da Microsoft e o Docker**: você deve ter uma compreensão básica de ambos os conceitos de Registro de Contêiner da Microsoft e do Docker, como registros, repositórios, contêineres e imagens de contêiner, bem como conhecimento de comandos `docker` básicos.  

Para instruções sobre conceitos básicos do Docker e de contêiner, confira [Visão geral do Docker](https://docs.docker.com/engine/docker-overview/).

### <a name="server-requirements-and-recommendations"></a>Recomendações e requisitos do servidor

A tabela a seguir descreve os núcleos de CPU mínimos e recomendados, pelo menos 2,6 gigahertz (GHz) ou mais rápidos, bem como a memória, em gigabytes (GB), para alocar para cada contêiner de Análise de Texto.

| Contêiner | Mínimo | Recomendadas |
|-----------|---------|-------------|
|Extração de Frases-Chave | 1 núcleo, 2 GB de memória | 1 núcleo, 4 GB de memória |
|Detecção de Idioma | 1 núcleo, 2 GB de memória | 1 núcleo, 4 GB de memória |
|Análise de Sentimento | 1 núcleo, 8 GB de memória | 1 núcleo, 8 GB de memória |

## <a name="download-container-images-from-microsoft-container-registry"></a>Baixar imagens de contêiner do Registro de Contêiner da Microsoft

Imagens de contêiner para Análise de Texto estão disponíveis no Registro de Contêiner da Microsoft. A tabela a seguir lista os repositórios disponíveis do Registro de Contêiner da Microsoft para os contêineres de Análise de Texto. Cada repositório contém uma imagem de contêiner, que deve ser baixada para executar o contêiner localmente.

| Contêiner | Repositório |
|-----------|------------|
|Extração de Frases-Chave | `mcr.microsoft.com/azure-cognitive-services/keyphrase` |
|Detecção de Idioma | `mcr.microsoft.com/azure-cognitive-services/language` |
|Análise de Sentimento | `mcr.microsoft.com/azure-cognitive-services/sentiment` |

Use o comando [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner de um repositório. Por exemplo, para baixar a imagem de contêiner de Extração de Frases-chave mais recente do repositório, use o seguinte comando:

```Docker
docker pull mcr.microsoft.com/azure-cognitive-services/keyphrase:latest
```

Para obter uma descrição completa de marcas disponíveis para os contêineres de Análise de Texto, confira o seguintes contêineres no Hub do Docker:

* [Extração de Frases-chave](https://go.microsoft.com/fwlink/?linkid=2018757)
* [Detecção de Idioma](https://go.microsoft.com/fwlink/?linkid=2018759)
* [Análise de Sentimento](https://go.microsoft.com/fwlink/?linkid=2018654)

> [!TIP]
> Você pode usar o comando [docker images](https://docs.docker.com/engine/reference/commandline/images/) comando para listar as imagens de contêiner baixadas. Por exemplo, o comando a seguir lista a ID, o repositório e a tag de cada imagem de contêiner baixada, formatada como uma tabela:
>
>  ```Docker
>  docker images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}"
>  ```
>

## <a name="instantiate-a-container-from-a-downloaded-container-image"></a>Criar uma instância de um contêiner com base em uma imagem de contêiner baixada

Use o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para criar uma instância de um contêiner de uma imagem de contêiner baixada. Por exemplo, o seguinte comando:

* Cria uma instância de um contêiner com base em uma imagem de contêiner de Análise de Sentimento
* Aloca um núcleo de CPU e 8 gigabytes (GB) de memória
* Expõe a porta TCP 5000 e aloca um PSEUDO-TTY para o contêiner
* Remove automaticamente o contêiner após ele sair

```Docker
docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/sentiment Eula=accept Billing=https://westus.api.cognitive.microsoft.com/text/analytics/v2.0 ApiKey=0123456789

```

> [!IMPORTANT]
> As opções de linha de comando `Eula`, `Billing` e `ApiKey` deverão ser especificadas para instanciar o contêiner; caso contrário, o contêiner não iniciará.  Para obter mais informações, confira [Cobrança](#billing).

Quando instanciado, você pode chamar operações do contêiner usando o URI de host do contêiner. Por exemplo, o seguinte URI de host representa o contêiner de Análise de Sentimento instanciado no exemplo anterior:

```http
http://localhost:5000/
```

> [!TIP]
> Você pode acessar a [Especificação de OpenAPI](https://swagger.io/docs/specification/about/) (anteriormente conhecida como a especificação de Swagger), que descreve as operações compatíveis com um contêiner instanciado, por meio do URI relativo `/swagger` para esse contêiner. Por exemplo, o seguinte URI fornece acesso à especificação OpenAPI para o contêiner de Análise de Sentimento instanciado no exemplo anterior:
>
>  ```http
>  http://localhost:5000/swagger
>  ```

Você pode [chamar as operações da API REST](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-call-api) disponíveis do seu contêiner ou usar a biblioteca de clientes do [SDK de Análise de Texto dos Serviços Cognitivos do Azure](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics) para chamar essas operações.  
> [!IMPORTANT]
> Você precisará ter o SDK de Análise de Texto dos Serviços Cognitivos do Azure versão 2.1.0 ou posterior se você quiser usar a biblioteca de clientes com o seu contêiner.

A única diferença entre chamar uma operação específica do contêiner e chamar a mesma operação de um serviço correspondente no Azure é que, para chamar a operação, você poderá usar o URI do host do contêiner, em vez do URI de host de uma região do Azure. Por exemplo, se você quisesse usar uma instância da Análise de Texto em execução na região Oeste dos EUA do Azure, você chamaria a operação de API REST a seguir:

```http
POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
```

Se você quisesse usar um contêiner de Extração de Frases-chave em execução no computador local na respectiva configuração padrão, você chamaria a operação de API REST a seguir:

```http
POST http://localhost:5000/text/analytics/v2.0/keyPhrases
```

### <a name="billing"></a>Cobrança

Os contêineres de Análise de Texto enviam informações de cobrança do Azure, usando um recurso de Análise de Texto correspondente em sua conta do Azure. As opções de linha de comando a seguir são usadas pelos contêineres de Análise de Texto para fins de cobrança:

| Opção | DESCRIÇÃO |
|--------|-------------|
| `ApiKey` | A chave de API do recurso de Análise de Texto usado para rastrear informações de cobrança.<br/>O valor dessa opção deve ser definido como uma chave de API para o recurso de Análise de Texto do Azure provisionado especificado em `Billing`. |
| `Billing` | O ponto de extremidade do recurso de Análise de Texto usado para rastrear informações de cobrança.<br/>O valor dessa opção precisa ser definido para o URI de ponto de extremidade de um recurso de Análise de Texto do Azure provisionado.|
| `Eula` | Indica que você aceitou a licença para o contêiner.<br/>O valor dessa opção deve ser definido para `accept`. |

> [!IMPORTANT]
> Todas as três opções deverão ser especificadas com os valores válidos, caso contrário o contêiner não iniciará.

Para obter mais informações sobre essas opções, confira [Configurar os contêineres](../text-analytics-resource-container-config.md).

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu conceitos e fluxo de trabalho para baixar, instalar e executar os contêineres de Análise de Texto. Em resumo:

* A Análise de Texto fornece três contêineres do Linux para o Docker, encapsulando a Extração de Frases-chave, Detecção de Idioma e Análise de Sentimento.
* Imagens de contêiner são baixadas de um registro de contêiner privado no Azure.
* Imagens de contêiner são executadas no Docker.
* Você pode usar a API REST ou o SDK para chamar as operações em contêineres de análise de texto especificando o URI do host do contêiner.
* Você deve especificar as informações de cobrança ao criar uma instância de um contêiner.
* ** Contêineres de Serviços Cognitivos não são licenciados para execução sem estarem conectados ao Azure para fins de medição. Os clientes precisam habilitar os contêineres comunicarem informações de cobrança com o serviço de medição em todos os momentos. Contêineres dos Serviços Cognitivos não enviam dados do cliente (por exemplo, a imagem ou texto que está sendo analisado) para a Microsoft.  

## <a name="next-steps"></a>Próximas etapas

* Examinar [Configurar contêineres](../text-analytics-resource-container-config.md) para saber mais sobre as definições de configuração
* Examinar [Visão geral da análise de texto](../overview.md) para saber mais sobre Análise de Sentimento, Detecção de Idioma e Detecção de Frases-chave  
* Veja a [API de Análise de Texto](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) para obter detalhes sobre os métodos compatíveis com o contêiner.
* Veja as [Perguntas frequentes](../text-analytics-resource-faq.md) para resolver problemas relacionados à funcionalidade de Pesquisa Visual Computacional.