---
title: Redefinição de TCP do Balanceador de Carga quando ocioso | Microsoft Docs
description: Load Balancer com pacotes TCP RST bidirecionais em tempo limite de ociosidade
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/08/2018
ms.author: kumud
ms.openlocfilehash: 9aa3811eb03d38a4c6ab8203512f3e6699098122
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883628"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Balanceador de Carga com Redefinição de TCP quando ocioso (Versão Prévia Pública)

Você pode usar o [Standard Load Balancer](load-balancer-standard-overview.md) para criar um aplicativo comportamento mais previsível para seus cenários com Redefinições de TCP bidirecionais (pacote TCP RST) para cada tempo limite de ociosidade configurável.  O comportamento de padrão do Load Balancer é remover fluxos silenciosamente quando o tempo limite de ociosidade de um fluxo for atingido.

![Redefinição de TCP do Balanceador de Carga](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>A funcionalidade de Load Balancer com tempo limite de ociosidade para redefinição do TCP está disponível como Visualização Pública no momento e disponibilizada em um conjunto limitado de [regiões](#regions). Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos. Veja os [Termos de Uso Adicionais para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter detalhes.
 
Alterar esse comportamento padrão e habilitar envio de Redefinições de TCP no tempo limite de ociosidade em regras NAT de entrada, regras de balanceamento de carga e [regras de saída](https://aka.ms/lboutboundrules).  Quando habilitado por regra, o Load Balancer enviará Redefinições de TCP (pacotes TCP RST) para os pontos de extremidade do cliente e do servidor no momento do tempo limite de ociosidade para todos os fluxos correspondentes.

Os pontos de extremidade que recebem pacotes TCP RST fecham imediatamente o soquete correspondente. Isso fornece uma notificação imediata para os pontos de extremidade que a liberação da conexão ocorreu e qualquer comunicação futura na mesma conexão TCP falhará.  Aplicativos podem limpar conexões quando o soquete é fechado e restabelecer as conexões conforme necessário, sem esperar o tempo limite eventual da conexão TCP.

Para muitos cenários, isso pode reduzir a necessidade de enviar keepalives de TCP (ou de camada de aplicativo) para atualizar o tempo limite de um fluxo. 

Se as durações ociosas excederem aquelas permitidas pela configuração ou seu aplicativo apresentar um comportamento indesejado com Redefinições de TCP habilitadas, você ainda precisará usar keepalives de TCP (ou keepalives da camada de aplicativo) para monitorar a atividade das conexões TCP.  Além disso, keepalives também podem ser úteis para quando a conexão é transmitida com proxy em algum lugar no caminho, particularmente em keepalives de camada de aplicativo.  

Examine cuidadosamente todo o cenário de ponta a ponta para decidir se é vantagem habilitar Redefinições de TCP, ajustando o tempo limite de ociosidade, e se podem ser necessárias etapas adicionais para garantir o comportamento do aplicativo desejado.

## <a name="enabling-tcp-reset-on-idle-timeout"></a>Habilitar Redefinição de TCP após tempo limite de ociosidade

Usando a API versão 2018-07-01, você pode habilitar o envio de Redefinições de TCP bidirecionais após tempo limite de ociosidade em uma base por regra:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a>Disponibilidade de região

Esse parâmetro está efetivo atualmente nas seguintes regiões.  Em regiões não listadas aqui, o parâmetro não tem nenhum efeito.

| Região |
|---|
| Sudeste da Ásia |
| Europa Ocidental |
| Leste dos EUA 2 |
| Norte do Reino Unido |
| Oeste dos EUA |

Essa tabela será atualizada conforme a versão prévia for expandida para outras regiões.  

## <a name="limitations"></a>Limitações

- [Disponibilidade de região](#regions) limitada.
- O portal não pode ser usado para configurar ou exibir a Redefinição de TCP.  Em vez disso, use modelos, API REST, Az CLI 2.0 ou PowerShell.

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Standard Load Balancer](load-balancer-standard-overview.md).
- Saiba mais sobre [regras de saída](load-balancer-outbound-rules-overview.md).
