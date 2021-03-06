---
title: Criar uma VM de uma imagem gerenciada no Azure | Microsoft Docs
description: Crie uma máquina virtual do Windows a partir de uma imagem de gerenciada generalizada usando o Azure PowerShell ou o portal do Azure no modelo de implantação do Gerenciador de Recursos.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: 8acbb33b396aa617936eb0333bd68fea60532425
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404649"
---
# <a name="create-a-vm-from-a-managed-image"></a>Criar uma VM por meio de uma imagem gerenciada

Você pode criar várias VMs (máquinas virtuais) de uma imagem de VM gerenciada do Azure usando o PowerShell ou o portal do Azure. Uma imagem de VM gerenciada contém as informações necessárias para criar uma VM, incluindo o sistema operacional e os discos de dados. Os VHDs (discos rígidos virtuais) que formam a imagem, incluindo os discos do sistema operacional e quaisquer discos de dados, são armazenados como discos gerenciados. 

Antes de criar uma nova VM, você precisará [criar uma imagem de VM gerenciada](capture-image-resource.md) para usar como a imagem de origem. 

## <a name="use-the-portal"></a>Usar o portal

1. Abra o [Portal do Azure](https://portal.azure.com).
2. No menu do lado esquerdo, selecione **Todos os recursos**. Você pode classificar os recursos por **Tipo** localizar facilmente as suas imagens.
3. Selecione a imagem que você deseja usar a partir da lista. A imagem da página **Visão geral** será aberta.
4. Clique em **Criar VM** no menu.
5. Insira as informações da máquina virtual. O nome do usuário e a senha inseridos aqui serão usados para fazer logon na máquina virtual. Quando concluir, selecione **OK**. Você pode criar a nova VM em um grupo de recursos existente ou escolher **Criar novo** para criar um novo grupo de recursos para armazenar a VM.
6. Selecione um tamanho para a VM. Para ver mais tamanhos, selecione **Exibir todos os** ou altere o filtro **Tipo de disco com suporte**. 
7. Em **Configurações**, faça as alterações necessárias e selecione **OK**. 
8. Na página de resumo, você deve ver o nome da sua imagem listado como uma **Imagem privada**. Selecione **OK** para iniciar a implantação da máquina virtual.


## <a name="use-powershell"></a>Usar o PowerShell

Você pode usar o PowerShell para criar uma VM por meio de uma imagem usando o conjunto de parâmetro simplificado definido para o novo cmdlet do [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm). A imagem precisa estar no mesmo grupo de recursos no qual você criará a VM.

Este exemplo requer o módulo do AzureRM, versão 5.6.0 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para encontrar a versão. Se você precisa atualizar, consulte [Instalar o módulo do Azure PowerShell](/powershell/azure/install-azurerm-ps).

O parâmetro simplificado definido para [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) requer apenas que você forneça um nome, um grupo de recursos e o nome da imagem para criar uma VM de uma imagem. New-AzureRmVm usará o valor do parâmetro **-Name** como o nome de todos os recursos que cria automaticamente. Neste exemplo, fornecemos nomes mais detalhados para cada recurso, mas permitimos ao cmdlet criá-los automaticamente. Você também pode criar recursos, como a rede virtual, antecipadamente e passar o nome do recurso para o cmdlet. New-AzureRmVm usará os recursos existentes se puder encontrá-los pelo nome.

O exemplo a seguir cria uma máquina virtual chamada *myVMFromImage*, no grupo de recursos *myResourceGroup*, na imagem chamada *myImage*. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Próximas etapas
[Criar e gerenciar VMs do Windows com o módulo do Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

