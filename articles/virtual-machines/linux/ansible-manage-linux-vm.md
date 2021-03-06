---
title: Usar o Ansible para gerenciar uma máquina virtual do Linux no Azure
description: Saiba como usar o Ansible para gerenciar uma máquina virtual do Linux no Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 09/27/2018
ms.openlocfilehash: 14e675097a23b50c8ff193c69c377ac8edfb989f
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317255"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Usar o Ansible para gerenciar uma máquina virtual do Linux no Azure
O Ansible permite que você automatize a implantação e a configuração de recursos em seu ambiente. Você pode usar o Ansible para gerenciar suas máquinas virtuais do Azure, da mesma forma que faria com qualquer outro recurso. Este artigo mostra como usar um guia estratégico do Ansible para iniciar e parar uma máquina virtual do Linux. 

## <a name="prerequisites"></a>Pré-requisitos

- **Assinatura do Azure** - Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)] [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Usar o Ansible para desalocar a máquina de virtual (parar) uma máquina virtual do Azure
Esta seção ilustra como usar o Ansible para desalocar (parar) uma máquina virtual do Azure

1.  Entre no [Portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Abra o [Cloud Shell](/azure/cloud-shell/overview).

1.  Crie um arquivo (para conter seu guia estratégico) chamado `azure-vm-stop.yml`, e abra-o no editor VI da seguinte maneira:

    ```azurecli-interactive
    vi azure-vm-stop.yml
    ```

1.  Entre no modo de inserção selecionando a tecla **I**.

1.  Cole o código de exemplo a seguir no editor:

    ```yaml
    - name: Stop Azure VM
      hosts: localhost
      connection: local
      tasks:
      - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
          resource_group: myResourceGroup
          name: myVM
          allocated: no
    ```

1.  Saia do modo de inserção selecionando a tecla **Esc**.

1.  Salve o arquivo e saia do editor vi, inserindo o comando a seguir:

    ```bash
    :wq
    ```

1.  Execute o guia estratégico de exemplo do Ansible.

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1.  A saída é semelhante ao seguinte exemplo que mostra que a máquina virtual foi desalocada (parada) com êxito:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Use o Ansible para iniciar uma máquina virtual do Azure desalocada (parada)
Esta seção ilustra como usar o Ansible para iniciar uma máquina virtual do Azure desalocada (parada)

1.  Entre no [Portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Abra o [Cloud Shell](/azure/cloud-shell/overview).

1.  Crie um arquivo (para conter seu guia estratégico) chamado `azure-vm-start.yml`, e abra-o no editor VI da seguinte maneira:

    ```azurecli-interactive
    vi azure-vm-start.yml
    ```

1.  Entre no modo de inserção selecionando a tecla **I**.

1.  Cole o código de exemplo a seguir no editor:

    ```yaml
    - name: Start Azure VM
      hosts: localhost
      connection: local
      tasks:
      - name: Start the virtual machine
        azure_rm_virtualmachine:
          resource_group: myResourceGroup
          name: myVM
    ```

1.  Saia do modo de inserção selecionando a tecla **Esc**.

1.  Salve o arquivo e saia do editor vi, inserindo o comando a seguir:

    ```bash
    :wq
    ```

1.  Execute o guia estratégico de exemplo do Ansible.

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1.  A saída é semelhante ao seguinte exemplo que mostra que a máquina virtual foi iniciada com êxito:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Próximas etapas
> [!div class="nextstepaction"] 
> [Use o Ansible para gerenciar seus inventários dinâmicos do Azure](~/articles/ansible/ansible-manage-azure-dynamic-inventories.md)