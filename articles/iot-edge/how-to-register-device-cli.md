---
title: Registrar um novo dispositivo do Azure IoT Edge (CLI)| Microsoft Docs
description: Usar a extensão do IoT para CLI do Azure para registrar um novo dispositivo IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 737a2dfe5c3b3382db00785b3465147143b17e9e
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569243"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli"></a>Registrar um novo dispositivo Azure IoT Edge com a CLI do Azure

Antes de poder usar os dispositivos do IoT com Azure IoT Edge, será necessário registrá-los no hub IoT. Após registrar um dispositivo, você receberá uma cadeia de conexão que poderá ser utilizada para configurar o dispositivo para cargas de trabalho do Edge. 

A [CLI do Azure](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) é uma ferramenta de linha de comando de plataforma cruzada de software livre para gerenciar recursos do Azure como o IoT Edge. Isso permite que você gerencie instantaneamente recursos, instâncias de serviço de provisionamento de dispositivos e hubs vinculados do Hub IoT. A nova extensão de IoT enriquece a CLI do Azure com recursos como gerenciamento de dispositivos e funcionalidade completa do IoT Edge.

Este artigo descreve como registrar um novo dispositivo IoT Edge usando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Um [Hub IoT](../iot-hub/iot-hub-create-using-cli.md) na assinatura do Azure. 
* [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) em seu ambiente. No mínimo, a versão da CLI do Azure deve ser 2.0.24 ou superior. Use `az –-version` para validar. Esta versão dá suporte aos comandos da extensão az e introduz a estrutura de comandos Knack. 
* A [extensão de IoT para a CLI do Azure](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>Criar um dispositivo

Use o comando a seguir para criar uma nova identidade de dispositivo no hub IoT: 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

Este comando inclui três parâmetros:
* **device-id**: fornece um nome descritivo exclusivo para o hub IoT.
* **hub-name**: fornece o nome do hub IoT.
* **edge-enabled**: esse parâmetro declara que o dispositivo é para uso com IoT Edge.

   ![Criar dispositivo IoT Edge](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>Exibir todos os dispositivos

Use o comando a seguir para exibir todos os dispositivos no hub IoT:

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

Qualquer dispositivo registrado como um dispositivo do IoT Edge terá a propriedade **capabilities.iotEdge** definida para **true**. 

## <a name="retrieve-the-connection-string"></a>Recuperar a cadeia de conexão

Quando estiver pronto para configurar o dispositivo, você precisará da cadeia de conexão que vincula o dispositivo físico à identidade no hub IoT. Use o comando a seguir para retornar a cadeia de conexão para um único dispositivo:

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

O parâmetro da id do dispositivo diferencia maiúsculas de minúsculas. Não copie as aspas em torno da cadeia de caracteres de conexão. 

## <a name="next-steps"></a>Próximas etapas

Saiba como [Implantar módulos em um dispositivo com a CLI do Azure](how-to-deploy-modules-cli.md)
