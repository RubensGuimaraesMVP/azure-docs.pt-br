---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ed0c387f9785336fbf18b3fd3c0cd9a7b09df633
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279430"
---
Transações principais (Máximo de transações permitidas em 10 segundos, por cofre, por região<sup>1</sup>):

|Tipo de chave|Chave HSM<br>Chave CREATE|Chave HSM<br>Todas as outras transações|Chave de software<br>Chave CREATE|Chave de software<br>Todas as outras transações|
|:---|---:|---:|---:|---:|
|RSA de 2048 bits|5|1000|10|2000|
|RSA de 3072 bits|5|250|10|500|
|RSA de 4096 bits|5|125|10|250|
|ECC P-256|5|1000|10|2000|
|ECC P-384|5|1000|10|2000|
|ECC P-521|5|1000|10|2000|
|ECC SECP256K1|5|1000|10|2000|
|

Transações de cofre, Segredos e Chaves de Conta de Armazenamento Gerenciadas:
| Tipos de transação | Máximo de transações permitidas em 10 segundos, por cofre, por região<sup>1</sup> |
| --- | --- |
| Todas as transações |2000 |
|

Consulte [diretrizes de limitação do Azure Key Vault](../articles/key-vault/key-vault-ovw-throttling.md) para obter informações sobre como lidar com a limitação quando esses limites são excedidos.

<sup>1</sup> Há um limite que afeta toda a assinatura para todos os tipos de transação, que é 5 vezes por limite do cofre de chaves. Por exemplo, HSM- outras transações por assinatura são limitadas a 5000 transações em 10 segundos por assinatura.
