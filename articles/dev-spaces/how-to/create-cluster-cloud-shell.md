---
title: Como criar um cluster do Kubernetes habilitado para Azure Dev Spaces usando o Azure Cloud Shell | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: article
description: Aprenda a criar rapidamente um cluster do Kubernetes habilitado para Azure Dev Spaces diretamente do navegador sem instalar nada.
keywords: Docker, Kubernetes, Azure, AKS, Serviço do Kubernetes do Azure, contêineres
ms.openlocfilehash: 61a466efdacb56889a7e437d84d3c9de43927e65
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705745"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Criar um cluster do Kubernetes usando o Azure Cloud Shell

É possível usar o [Azure Cloud Shell](/azure/cloud-shell) para criar um cluster do Azure Dev Spaces usando o botão **Experimentar** desta página. Se não estiver conectado, siga os prompts para entrar com uma conta do Azure e, em seguida, digite os comandos no prompt do Azure Cloud Shell quando for exibido.

## <a name="create-the-cluster"></a>Criar o cluster

Primeiro, crie o grupo de recursos. Use uma das regiões atualmente suportadas (EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral ou CanadaEast).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Crie um cluster Kubernetes com o seguinte comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --kubernetes-version 1.11.3 --enable-addons http_application_routing
```

São necessários alguns minutos para criar o cluster.  Quando concluída, a saída será mostrada no formato JSON. Procure `provisioningState` e verifique se é `Succeeded`.

## <a name="next-steps"></a>Próximas etapas

Consulte [Azure Dev Spaces](/azure/dev-spaces/) para obter links e ter acesso a tutoriais completos.
