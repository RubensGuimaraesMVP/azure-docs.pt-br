---
title: Conectar uma VM do Windows Server | Microsoft Docs
description: Saiba como se conectar e fazer logon em uma VM do Windows usando o portal do Azure e o modelo de implantação do Gerenciador de Recursos.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: cynthn
ms.openlocfilehash: b9cce5658b705e9d3255d2662b2a0157a2e548c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47409013"
---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Como se conectar e fazer logon em uma máquina virtual do Azure executando o Windows
Você usará o botão **Conectar** no portal do Azure para iniciar uma sessão da Área de Trabalho Remota (RDP) a partir de uma área de trabalho do Windows. Primeiramente, conecte-se à máquina virtual, em seguida, faça logon.

Para conectar-se a uma VM do Windows por meio de um Mac, será necessário instalar um cliente do RDP para Mac, como a [Área de Trabalho Remota da Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Conectar-se à máquina virtual
1. Se ainda não tiver feito isso, entre no [portal do Azure](https://portal.azure.com/).
2. No menu à esquerda, selecione **Máquinas Virtuais**.
3. Selecione a máquina virtual na lista.
4. Na parte superior da página da máquina virtual, selecione **Conectar**.
2. Na página **Conectar à máquina virtual**, selecione as opções apropriadas e selecione **Baixar arquivo RDP**.
2. Abra o arquivo RDP baixado e selecione **Conectar** quando solicitado. 
2. Você receberá um aviso de que o arquivo .rdp é proveniente de um editor desconhecido. Isso é esperado. Na janela **Conexão de Área de Trabalho Remota**, selecione **Conectar** para continuar.
   
    ![Captura de tela de um aviso sobre um editor desconhecido.](./media/connect-logon/rdp-warn.png)
3. Na janela **Segurança do Windows**, selecione **Mais opções** e **Usar uma conta diferente**. Digite as credenciais de uma conta na máquina virtual e selecione **OK**.
   
     **Conta local**: normalmente, são o nome de usuário e a senha da conta local especificados por você no momento da criação da máquina virtual. Nesse caso, o domínio é o nome da máquina virtual e é inserido como *nomedavm*&#92;*nome de usuário*.  
   
    **VM Ingressada no domínio**: se a VM pertencer a um domínio, digite o nome de usuário no formato *Domínio*&#92;*Nome de usuário*. A conta precisa estar no grupo Administradores ou ter privilégios de acesso remoto concedidos à VM.
   
    **Controlador de domínio**: se a VM for um controlador de domínio, digite o nome de usuário e a senha da conta de administrador para esse domínio.
4. Selecione **Sim** para verificar a identidade da máquina virtual e concluir o logon.
   
   ![Captura de tela mostrando uma mensagem sobre como verificar a identidade da VM.](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Se o botão **Conectar** no portal estiver esmaecido e você não estiver conectado ao Azure por meio de uma [Express Route](../../expressroute/expressroute-introduction.md) ou de uma conexão [VPN Site a Site](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), será necessário criar e atribuir à VM um endereço IP público antes de usar o RDP. Para obter mais informações, consulte [Endereços IP públicos no Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 


## <a name="next-steps"></a>Próximas etapas
Se você tiver dificuldade para se conectar, consulte [Solucionar problemas de conexões de Área de Trabalho Remota](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

