---
title: Implantar um Workspace do Machine Learning com o Azure Resource Manager | Microsoft Docs
description: Como implantar um workspace para o Azure Machine Learning usando o modelo do Azure Resource Manager
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/05/2018
ms.openlocfilehash: 82a9cfec0efe60e3a0cdf16efa373e68efebee59
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307973"
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Implantar Workspace do Machine Learning usando o Azure Resource Manager

Usar um modelo de implantação do Azure Resource Manager poupa tempo fornecendo a você uma maneira escalonável de implantar componentes interconectados com um mecanismo de validação e repetição. Para configurar Workspace do Azure Machine Learning, por exemplo, você precisa configurar uma conta de armazenamento do Azure e implantar seu workspace. Imagine fazer isso manualmente para centenas de workspaces. Uma alternativa mais fácil é usar um modelo do Azure Resource Manager para implantar um Workspace do Azure Machine Learning e todas as suas dependências. Este artigo guia você pelo passo a passo desse processo. Para obter uma excelente visão geral do Azure Resource Manager, confira [Visão geral do Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Passo a passo: criar um Workspace do Machine Learning
Criaremos um grupo de recursos do Azure, implantaremos uma nova conta de armazenamento do Azure e um novo Workspace do Azure Machine Learning usando um modelo do Resource Manager. Assim que a implantação estiver concluída, imprimiremos informações importantes sobre os workspaces que foram criados (a chave primária, a workspaceID e a URL para o workspace).

### <a name="create-an-azure-resource-manager-template"></a>Criar um modelo do Azure Resource Manager
Um Workspace do Machine Learning exige uma conta de armazenamento do Azure para armazenar o conjunto de dados vinculado a ele.
O modelo a seguir usa o nome do grupo de recursos para gerar o nome da conta de armazenamento e o nome do workspace.  Ele também usa o nome da conta de armazenamento como uma propriedade ao criar o workspace.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Salve esse modelo como arquivo mlworkspace.json em c:\temp\.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Implantar o grupo de recursos, com base no modelo
* Abrir o PowerShell
* Instale módulos do Azure Resource Manager e do Gerenciamento de Serviços do Azure  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Estas etapas baixam e instalam os módulos necessários para concluir as etapas restantes. Isso só precisa ser feito uma vez no ambiente em que você está executando os comandos do PowerShell.   

* Autenticar-se no Azure  

```
# Authenticate (enter your credentials in the pop-up window)
Connect-AzureRmAccount
```
Esta etapa precisa ser repetida para cada sessão. Uma vez autenticado, as informações da sua assinatura devem ser exibidas.

![Conta do Azure][1]

Agora que temos acesso ao Azure, podemos criar o grupo de recursos.

* Criar um grupo de recursos

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Verifique se o grupo de recursos está provisionado corretamente. **ProvisioningState** deve ser "Succeeded".
O nome do grupo de recursos é usado pelo modelo para gerar o nome da conta de armazenamento. O nome da conta de armazenamento deve ter entre 3 e 24 caracteres, usar números e apenas letras minúsculas.

![Grupo de recursos][2]

* Usando a implantação do grupo de recursos, implante um novo Workspace do Machine Learning.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Depois que a implantação for concluída, é fácil acessar as propriedades do workspace que você implantou. Por exemplo, você pode acessar o Token de Chave Primária.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Outra maneira de recuperar tokens de um workspace existente é usar o comando Invoke-AzureRmResourceAction. Por exemplo, você pode listar os tokens primário e secundário de todos os workspaces.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Depois que o workspace estiver provisionado, você também poderá automatizar muitas tarefas do Azure Machine Learning Studio usando o [Módulo do PowerShell para Azure Machine Learning](https://aka.ms/amlps).

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre [como criar modelos do Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 
* Examine o [repositório de modelos de início rápido do Azure](https://github.com/Azure/azure-quickstart-templates). 
* Assista a este vídeo sobre o [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ./media/deploy-with-resource-manager-template/azuresubscription.png
[2]: ./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
