---
title: Solução de falhas de failover para o Azure | Microsoft Docs
description: Este artigo descreve maneiras de solucionar erros comuns no failover para o Azure
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 1e7486dc646843c473cfb355445e194893934a1a
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447139"
---
# <a name="troubleshoot-errors-when-failing-over-a-virtual-machine-to-azure"></a>Solução de erros durante o failover de uma máquina virtual para o Azure

Você poderá receber um dos erros a seguir ao fazer failover de uma máquina virtual para o Azure. Para solucionar problemas, use as etapas descritas para cada condição de erro.

## <a name="failover-failed-with-error-id-28031"></a>Falha no failover com a ID de Erro 28031

O Site Recovery não pôde criar uma máquina virtual com failover no Azure. Isso pode ocorrer devido a um dos seguintes motivos:

* Não há cota suficiente disponível para criar a máquina virtual: verifique a cota disponível acessando Assinatura -> Uso + cotas. Abra uma [nova solicitação de suporte](http://aka.ms/getazuresupport) para aumentar a cota.

* Você está tentando fazer failover em máquinas virtuais de famílias de tamanho diferentes no mesmo conjunto de disponibilidade. Escolha a mesma família de tamanho para todas as máquinas virtuais no mesmo conjunto de disponibilidade. Altere o tamanho acessando as configurações Computação e Rede da máquina virtual e, em seguida, repita o failover.

* Há uma política na assinatura que impede a criação de uma máquina virtual. Altere a política para permitir a criação de uma máquina virtual e, em seguida, repita o failover.

## <a name="failover-failed-with-error-id-28092"></a>Falha no failover com a ID de Erro 28092

O Site Recovery não pôde criar um adaptador de rede para a máquina virtual com failover. Verifique se você tem uma cota suficiente disponível para criar adaptadores de rede na assinatura. Verifique a cota disponível acessando Assinatura -> Uso + cotas. Abra uma [nova solicitação de suporte](http://aka.ms/getazuresupport) para aumentar a cota. Se você tem uma cota suficiente, isso pode ser um problema intermitente. Repita a operação. Se o problema persistir mesmo após várias tentativas, deixe um comentário ao final deste documento.  

## <a name="failover-failed-with-error-id-70038"></a>Falha no failover com a ID de Erro 70038

O Site Recovery não pôde criar uma máquina virtual Clássica com failover no Azure. Isso pode ocorre porque:

* Um dos recursos, como uma rede virtual que é necessária para a criação da máquina virtual, não existe. Crie a rede virtual, conforme fornecido nas configurações Computação e Rede da máquina virtual ou modifique a configuração para uma rede virtual que já existe e, em seguida, repita o failover.

## <a name="failover-failed-with-error-id-170010"></a>Falha no failover com a ID de Erro 170010

O Site Recovery não pôde criar uma máquina virtual com failover no Azure. Pode acontecer porque uma atividade interna de hidratação falhou para a máquina virtual no local.

Para exibir qualquer computador no Azure, o ambiente do Azure requer que alguns dos drivers em inicialização iniciem estado e serviços conforme o DHCP esteja no estado de inicialização automática. Assim, a atividade de hidratação, no momento do failover, converte o tipo de inicialização **atapi, intelide, storflt, vmbus, and storvsc drivers** para iniciar. Ele também converte o tipo de inicialização de alguns serviços, como o DHCP para iniciar automaticamente. Essa atividade pode falhar devido a problemas específicos do ambiente. Para alterar manualmente o tipo de inicialização de drivers, siga as etapas a seguir:

1. [Baixar](http://download.microsoft.com/download/5/D/6/5D60E67C-2B4F-4C51-B291-A97732F92369/Script-no-hydration.ps1) o script sem hidratação e executar conforme a seguir. Esse script verifica se a VM requer hidratação.

    `.\Script-no-hydration.ps1`

    Fornece o seguinte resultado se for necessária a hidratação:

        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0

        This system doesn't meet no-hydration requirement.

    No caso de VM atender o requisito de n´~ao hidratação, o script fornecerá o resultado "esse sistema atende aos requisitos não-hidratação". Nesse caso, todos os drivers e serviços estão no estado conforme exigido pelo Azure e hidratação na VM não é necessária.

2. Execute o script de nenhum conjunto de hidratação da seguinte maneira, se a VM não atender o requisito de não hidratação.

    `.\Script-no-hydration.ps1 -set`
    
    Isso irá converter o tipo de inicialização dos drivers e fornecerá o resultado como abaixo:
    
        REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc           start =  3 expected value =  0 

        Updating registry:  REGISTRY::HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\storvsc   start =  0 

        This system is now no-hydration compatible. 

## <a name="unable-to-connectrdpssh-to-the-failed-over-virtual-machine-due-to-grayed-out-connect-button-on-the-virtual-machine"></a>Não é possível conectar/RDP/SSH para a máquina virtual com failover devido ao botão Conectar esmaecido na máquina virtual

Se o botão **Conectar** da VM com failover no Azure estiver esmaecido e você não estiver conectado ao Azure por meio de uma conexão VPN Site a Site ou do Express Route,

1. Vá para **Máquina Virtual** > **Rede** e clique no nome da interface de rede necessária.  ![network-interface](media/site-recovery-failover-to-azure-troubleshoot/network-interface.PNG)
2. Navegue até **Configurações de Ip** e clique no campo de nome da configuração de IP necessária. ![IPConfigurations](media/site-recovery-failover-to-azure-troubleshoot/IpConfigurations.png)
3. Para habilitar o endereço IP público, clique em **Habilitar**. ![Habilitar 3D](media/site-recovery-failover-to-azure-troubleshoot/Enable-Public-IP.png)
4. Clique em **Definir as configurações necessárias** > **Criar novo**. ![Criar novo](media/site-recovery-failover-to-azure-troubleshoot/Create-New-Public-IP.png)
5. Insira o nome do endereço público, escolha as opções padrão para **SKU** e **atribuição** e clique em **OK**.
6. Agora, para salvar as alterações feitas, clique em **Salvar**.
7. Feche os painéis e navegue até a seção **Visão geral** de máquina virtual para conectar-se/RDP.

## <a name="unable-to-connectrdpssh---vm-connect-button-available"></a>Não é possível conectar/RDP/SSH – Botão Conectar da VM disponível

Se o botão **Conectar** da VM com failover no Azure estiver disponível (não esmaecido), verifique o **Diagnóstico de inicialização** em sua máquina virtual e verifique se há erros conforme listado [neste artigo](../virtual-machines/windows/boot-diagnostics.md).

1. Se a máquina virtual não foi iniciada, tente fazer o failover para um ponto de recuperação mais antigo.
2. Se o aplicativo na máquina virtual não responder, tente o failover para um ponto de recuperação consistente do aplicativo.
3. Se a máquina virtual estiver ingressada no domínio, verifique se esse controlador de domínio está funcionando corretamente. Isso pode ser feito seguindo as etapas fornecidas abaixo:

     a. Criar uma nova máquina virtual na mesma rede.

    b.  Verifique se ele é capaz de ingressar no mesmo domínio no qual a máquina virtual com failover deve aparecer.

    c. Se o controlador de domínio **não** estiver funcionando corretamente, tente fazer logon na máquina virtual com failover usando uma conta do administrador local.
4. Se você estiver usando um servidor DNS personalizado, verifique se ele é acessível. Isso pode ser feito seguindo as etapas fornecidas abaixo:

     a. Crie uma nova máquina virtual na mesma rede e

    b. Verifique se a máquina virtual é capaz de executar a resolução de nome usando o servidor DNS personalizado

>[!Note]
>Habilitar qualquer configuração diferente do Diagnóstico de Inicialização exigiria um Agente de VM do Azure instalado na máquina virtual antes do failover

## <a name="unexpected-shutdown-message-event-id-6008"></a>Mensagem de desligamento inesperado (ID de evento 6008)

Ao inicializar uma VM do Windows após o failover, se você receber uma mensagem de desligamento inesperada na VM recuperada, ela indicará que um estado de desligamento da VM não foi capturado no ponto de recuperação usado para failover. Isso acontece quando você recupera para um ponto em que a VM não havia sido completamente desligada.

Normalmente, isso não é motivo de preocupação e geralmente pode ser ignorado para failovers não planejados. No caso de um failover planejado, certifique-se de que a VM seja desligada corretamente antes do failover e forneça tempo suficiente para que os dados da replicação pendente localmente sejam enviados para o Azure. Em seguida, use a opção **Mais recente** na [Tela de failover](site-recovery-failover.md#run-a-failover) para que todos os dados pendentes no Azure sejam processados em um ponto de recuperação, que é usado para failover da VM.

## <a name="retaining-drive-letter-after-failover"></a>Retenção da letra da unidade após failover
Para manter a letra da unidade em máquinas virtuais após failover, você pode definir a **Política SAN** para a máquina virtual local como **OnlineAll**. [Leia mais](https://support.microsoft.com/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).

## <a name="next-steps"></a>Próximas etapas
- Solucionar problemas da [conexão de RDP para a VM Windows](../virtual-machines/windows/troubleshoot-rdp-connection.md)
- Solucionar problemas da [conexão de SSH para VM Linux](../virtual-machines/linux/detailed-troubleshoot-ssh-connection.md)

Se precisar de mais ajuda, poste sua consulta no [fórum do Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr) ou deixe um comentário ao final deste documento. Temos uma comunidade ativa que deve conseguir ajudá-lo.
