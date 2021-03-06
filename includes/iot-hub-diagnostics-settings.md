---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 0039c4eff3571a96cba1ab36136e0a588d78eb75
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50159121"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Habilitar registro em log com as configurações de diagnóstico

1. Entre no [portal do Azure](https://portal.azure.com) e navegue até o Hub IoT.

2. Selecionar **configurações de Diagnóstico**.

3. Selecione **Ativar diagnóstico**.

   ![Ativar diagnóstico](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Nomeie as configurações de diagnóstico.

5. Escolha para onde você deseja enviar os logs. Você pode selecionar qualquer combinação das três opções abaixo:

   * Arquivar em uma conta de armazenamento
   * Transmitir para um hub de eventos
   * Enviar para o Log Analytics

6. Escolha as operações que você deseja monitorar e habilite os logs para essas operações. As operações que podem ser informadas em configurações de diagnóstico são:

   * conexões
   * Telemetria de dispositivo
   * Mensagens da nuvem para o dispositivo
   * Operações de identidade do dispositivo
   * Carregamentos de arquivos
   * Roteamento de mensagem
   * Operações de dispositivo gêmeo para nuvem
   * Operações de nuvem gêmea para dispositivo
   * Operações de gêmeos
   * Operações de trabalho
   * Métodos diretos  

6. Salve as novas configurações. 

Se você deseja ativar as configurações de diagnóstico com o PowerShell, use o seguinte código:

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzureRmDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

As novas configurações terão efeito em aproximadamente 10 minutos. Depois disso, os logs aparecerão no destino de arquivamento configurado, na folha **Configurações de diagnóstico**. Para obter mais informações sobre como configurar o diagnóstico, confira [Coletar e consumir dados de log com os recursos do Azure](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).