---
title: Matriz de suporte para recuperação de desastre de VMs do Hyper-V locais para o Azure | Microsoft Docs
description: Resume os componentes compatíveis e os requisitos para recuperação de desastre de VM Hyper-V para o Azure com o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/28/2018
ms.author: raynew
ms.openlocfilehash: e389f37448211afc35fb98572161be4fcaea7556
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50210713"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Matriz de suporte para recuperação de desastre de VMs do Hyper-V locais para o Azure


Este artigo resume os componentes compatíveis e as configurações de recuperação de desastres de VMs Hyper-V locais para o Azure usando o [Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Cenários com suporte

**Cenário** | **Detalhes**
--- | ---
Hyper-V com Virtual Machine Manager | Você pode executar a recuperação de desastres para o Azure para VMs em execução em hosts Hyper-V que são gerenciados na malha do System Center Virtual Machine Manager.<br/><br/> É possível implantar este cenário no portal do Azure ou usando o PowerShell.<br/><br/> Quando hosts Hyper-V são gerenciadas pelo Virtual Machine Manager, você também pode executar a recuperação de desastres em um site local secundário. Para saber mais sobre este cenário, leia [este tutorial](hyper-v-vmm-disaster-recovery.md).
Hyper-V sem Virtual Machine Manager | Você pode executar a recuperação de desastres para o Azure para VMs em execução em hosts Hyper-V que não são gerenciados pelo Virtual Machine Manager.<br/><br/> É possível implantar este cenário no portal do Azure ou usando o PowerShell.


## <a name="on-premises-servers"></a>Servidores locais

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
Hyper-V (executando sem Virtual Machine Manager) | Windows Server 2016 (incluindo a instalação Server Core), Windows Server 2012 R2 com as últimas atualizações | Quando você configura um site do Hyper-V no Site Recovery, a mistura de hosts que executam o Windows Server 2016 e o 2012 R2 não tem suporte.<br/><br/> Para VMs localizadas em um host executando o Windows Server 2016, a recuperação para um local alternativo não tem suporte.
Hyper-V (executando sem Virtual Machine Manager) | Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Se o Virtual Machine Manager for usado, os hosts Windows Server 2016 devem ser gerenciadas no Virtual Machine Manager 2016.<br/><br/> No momento, uma nuvem de Virtual Machine Manager que misturam hosts Hyper-V executados no Windows Server 2016 e no 2012 R2 não são compatíveis.<br/><br/> Ambientes que incluem uma atualização de um servidor 2012 R2 existente do Virtual Machine Manager para o 2016 não são compatíveis.


## <a name="replicated-vms"></a>VMs replicadas


A tabela a seguir resume o suporte de VMs. O Site Recovery é compatível com qualquer carga de trabalho em execução em um sistema operacional compatível.

 **Componente** | **Detalhes**
--- | ---
Configuração da VM | VMs que são replicadas para o Azure devem atender aos [requisitos do Azure](#azure-vm-requirements).
Sistema operacional convidado | Qualquer SO convidado [com suporte para Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases).<br/><br/> O Windows Server 2016 Nano Server não é compatível.


## <a name="vmdisk-management"></a>Gerenciamento de VM/disco

**Ação** | **Detalhes**
--- | ---
Redimensionar o disco na VM replicada do Hyper-V | Sem suporte. Desative a replicação, faça a alteração e ative novamente a replicação para a VM.
Adicionar disco na VM replicada do Hyper-V | Sem suporte. Desative a replicação, faça a alteração e ative novamente a replicação para a VM.

## <a name="hyper-v-network-configuration"></a>Configuração de rede Hyper-V

**Componente** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
Rede do host: Agrupamento NIC | SIM | SIM
Rede do host: VLAN | SIM | SIM
Rede do host: IPv4 | SIM | SIM
Rede do host: IPv6 | Não  | Não 
Rede da VM convidada: Agrupamento NIC | Não  | Não 
Rede da VM convidada: IPv4 | SIM | SIM
Rede da VM convidada: IPv6 | Não  | SIM
Rede da VM convidada: IP estático (Windows) | SIM | SIM
Rede da VM convidada: IP estático (Linux) | Não  | Não 
Rede da VM convidada: Multi-NIC | SIM | SIM



## <a name="azure-vm-network-configuration-after-failover"></a>Configuração de rede da VM do Azure (após failover)

**Componente** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | SIM | SIM
ILB | SIM | SIM
ELB | SIM | SIM
Gerenciador de Tráfego do Azure | SIM | SIM
NIC múltipla | SIM | SIM
IP Reservado | SIM | SIM
IPv4 | SIM | SIM
Manter endereço IP de origem | SIM | SIM
Pontos de extremidade de serviço de Rede Virtual do Azure<br/> (sem Firewalls de Armazenamento do Azure) | SIM | SIM
Rede Acelerada | Não  | Não 


## <a name="hyper-v-host-storage"></a>Armazenamento de host do Hyper-V

**Armazenamento** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | --- | ---
NFS | ND | ND
SMB 3.0 | SIM | SIM
SAN (ISCSI) | SIM | SIM
Múltiplos caminhos (MPIO). Testado com:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM para CLARiiON | SIM | SIM

## <a name="hyper-v-vm-guest-storage"></a>Armazenamento de convidado da VM do Hyper-V

**Armazenamento** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
VMDK | ND | ND
VHD/VHDX | SIM | SIM
VM geração 2 | SIM | SIM
EFI/UEFI| SIM | SIM
Disco de cluster compartilhado | Não  | Não 
Disco criptografado | Não  | Não 
NFS | ND | ND
SMB 3.0 | Não  | Não 
RDM | ND | ND
Disco >1 TB | Sim, até 4.095 GB | Sim, até 4.095 GB
Disco: setor de lógica e física de 4K | Não compatível: Gen 1/Gen 2 | Não compatível: Gen 1/Gen 2
Disco: setor de lógica e física de 4K e 512 bytes | SIM |  SIM
Gerenciamento de volumes lógicos (LVM). Há suporte para o LVM para discos de dados somente. As VMs do Azure tem apenas um único disco de sistema operacional. | SIM | SIM
Volume com discos distribuídos >1 TB | SIM | SIM
Espaços de Armazenamento | SIM | SIM
Adição/remoção de disco a quente | Não  | Não 
Exclusão de disco | SIM | SIM
Múltiplos caminhos (MPIO) | SIM | SIM

## <a name="azure-storage"></a>Armazenamento do Azure

**Componente** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
Armazenamento com redundância local | SIM | SIM
Armazenamento com redundância geográfica | SIM | SIM
Armazenamento com redundância geográfica com acesso de leitura | SIM | SIM
Armazenamento frio | Não  | Não 
Armazenamento quente| Não  | Não 
Blobs de bloco | Não  | Não 
Criptografia em repouso (SSE)| SIM | SIM
Armazenamento Premium | SIM | SIM
Serviço de importação/exportação | Não  | Não 
Firewalls de armazenamento do Azure para redes virtuais configurados na conta de armazenamento de cache/armazenamento de destino (usada para armazenar dados de replicação) | Não  | Não 


## <a name="azure-compute-features"></a>Recursos de computação do Azure

**Recurso** | **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
Conjuntos de disponibilidade | SIM | SIM
HUB | SIM | SIM  
Discos gerenciados | Sim, para failover.<br/><br/> O failback de discos gerenciados não é compatível. | Sim, para failover.<br/><br/> O failback de discos gerenciados não é compatível.

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

VMs locais que são replicados para o Azure devem atender aos requisitos de VM do Azure resumidos nesta tabela.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
Sistema operacional convidado | O Site Recovery é compatível com todos os sistemas operacionais que têm [suporte do Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | A verificação de pré-requisitos falha quando não há suporte para ela.
Arquitetura do sistema operacional convidado | 64 bits | A verificação de pré-requisitos falha quando não há suporte para ela.
Tamanho do disco do sistema operacional | Até 2.048 GB para máquinas virtuais de Geração 1.<br/><br/> Até 300 GB para máquinas virtuais de Geração 2.  | A verificação de pré-requisitos falha quando não há suporte para ela.
Contagem do disco do sistema operacional | 1 | A verificação de pré-requisitos falha quando não há suporte para ela.
Contagem de disco de dados | 16 ou menos  | A verificação de pré-requisitos falha quando não há suporte para ela.
Tamanho do VHD do disco de dados | Até 4.095 GB | A verificação de pré-requisitos falha quando não há suporte para ela.
Adaptadores de rede | Há suporte para vários adaptadores |
VHD compartilhado | Sem suporte | A verificação de pré-requisitos falha quando não há suporte para ela.
Disco FC | Sem suporte | A verificação de pré-requisitos falha quando não há suporte para ela.
Formato de disco rígido | VHD  <br/><br/> VHDX | O Site Recovery converterá automaticamente o VHDX em VHD ao realizar o failover para o Azure. Quando você executa o failback para o local, as máquinas virtuais continuam a usar o formato VHDX.
BitLocker | Sem suporte | O Bitlocker deve ser desabilitado antes de habilitar a replicação para uma VM.
Nome da VM | Entre 1 e 63 caracteres. Restrito a letras, números e hifens. O nome da VM deve começar e terminar com uma letra ou um número. | Atualize o valor nas propriedades da VM no Site Recovery.
Tipo de VM | Geração 1<br/><br/> Geração 2--Windows | VMs da Geração 2 com um tipo de disco básico de SO (que inclui um ou dois volumes de dados formatados como VHDX) e suporte para menos de 300 GB de espaço em disco.<br></br>Não há suporte para VMs Linux da Geração 2. [Saiba mais](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Ações de cofre dos Serviços de Recuperação

**Ação** |  **Hyper-V com Virtual Machine Manager** | **Hyper-V sem Virtual Machine Manager**
--- | --- | ---
Mover cofre entre grupos de recursos<br/><br/> Dentro e entre as assinaturas | Não  | Não 
Mover armazenamento, rede, VMs do Azure entre grupos de recursos<br/><br/> Dentro e entre as assinaturas | Não  | Não 


## <a name="provider-and-agent"></a>Provedor e agente

Para certificar-se de que a implantação seja compatível com as configurações neste artigo, verifique se você está executando as versões de agente e provedor mais recentes.

**Nome** | **Descrição** | **Detalhes**
--- | --- | --- | --- | ---
Provedor do Azure Site Recovery | Coordena as comunicações entre servidores locais e o Azure <br/><br/> Hyper-V com o Virtual Machine Manager: instalados nos servidores do Virtual Machine Manager<br/><br/> Hyper-V sem o Virtual Machine Manager: instalado nos hosts Hyper-V| Versão mais recente: 5.1.2700.1 (disponível no portal do Azure)<br/><br/> [Recursos e correções mais recentes](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Agente dos Serviços de Recuperação do Microsoft Azure | Coordena a replicação entre VMs Hyper-V e o Azure<br/><br/> Instalado em servidores Hyper-V locais (com ou sem Virtual Machine Manager) | Agente mais recente disponível no portal






## <a name="next-steps"></a>Próximas etapas
Saiba como [preparar o Azure](tutorial-prepare-azure.md) para a recuperação de desastre de VMs locais do Hyper-V.
