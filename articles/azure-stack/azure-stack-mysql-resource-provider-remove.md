---
title: Removendo o provedor de recursos do MySQL no Azure Stack | Microsoft Docs
description: Saiba como você pode remover o provedor de recursos MySQL da sua implantação do Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 2740da5a51e95a327a868734a7f009dddf40219a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964927"
---
# <a name="remove-the-mysql-resource-provider"></a>Remover o provedor de recursos do MySQL

Antes de remover o provedor de recursos do MySQL, você deve remover todas as dependências do provedor. Você também precisará de uma cópia do pacote de implantação que foi usado para instalar o provedor de recursos.

> [!NOTE]
> Você pode encontrar os links de download para o recurso de instaladores de provedor na [implantar os pré-requisitos do provedor de recursos](./azure-stack-mysql-resource-provider-deploy.md#prerequisites).

Remover o provedor de recursos MySQL não exclui os bancos de dados de locatário de servidores de hospedagem.

## <a name="dependency-cleanup"></a>Limpeza de dependência

Há várias tarefas de limpeza antes de executar o script DeployMySqlProvider.ps1 para remover o provedor de recursos.

O operador de pilha do Azure é responsável para as seguintes tarefas de limpeza:

* Exclua todos os planos que referenciam o adaptador do MySQL.
* Exclua todas as cotas que estão associadas com o adaptador do MySQL.

## <a name="to-remove-the-mysql-resource-provider"></a>Para remover o provedor de recursos do MySQL

1. Verifique se você removeu todas as MySQL recurso provedor dependências existentes.

   > [!NOTE]
   > Desinstalar o provedor de recursos MySQL continuará mesmo que os recursos dependentes estiver atualmente usando o provedor de recursos.
  
2. Obtenha uma cópia do pacote de instalação do provedor de recursos do MySQL e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário.
3. Abra uma janela de console novo com privilégios elevada do PowerShell e altere o diretório onde você extraiu os arquivos de instalação do provedor de recursos MySQL.
4. Execute o script DeployMySqlProvider.ps1 usando os seguintes parâmetros:
    - **Desinstalar**. Remove o provedor de recursos e todos os recursos associados.
    - **PrivilegedEndpoint**. O endereço IP ou nome DNS do ponto de extremidade com privilégios.
    - **AzureEnvironment**. O ambiente do Azure usado para implantar o Azure Stack. Necessário apenas para implantações do AD do Azure.
    - **CloudAdminCredential**. A credencial do administrador da nuvem, necessária para acessar o ponto de extremidade com privilégios.
    - **DirectoryTenantID**
    - **AzCredential**. A credencial para a conta de administrador de serviço do Azure Stack. Use as mesmas credenciais que você usou para implantar o Azure Stack.

## <a name="next-steps"></a>Próximas etapas

[Oferta de serviços de aplicativo como PaaS](azure-stack-app-service-overview.md)
