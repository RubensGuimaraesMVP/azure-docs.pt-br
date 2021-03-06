---
title: Tutorial – Rotear tráfego para pontos de extremidade ponderados usando o Gerenciador de Tráfego do Azure | Microsoft Docs
description: Este artigo de tutorial descreve como rotear o tráfego para pontos de extremidade ponderados usando o Gerenciador de Tráfego.
services: traffic-manager
author: KumudD
Customer intent: As an IT Admin, I want to distribute traffic based on the weight assigned to a website endpoint so that I can control the user traffic to a given website.
ms.service: traffic-manager
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: kumud
ms.openlocfilehash: 0f5b1f3525ab75f8c14f7921e23b30a1c58e8c73
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158815"
---
# <a name="tutorial-control-traffic-routing-with-weighted-endpoints-by-using-traffic-manager"></a>Tutorial: Controlar o roteamento de tráfego com pontos de extremidade ponderados usando o Gerenciador de Tráfego 

Este tutorial descreve como usar o Gerenciador de Tráfego do Azure para controlar o roteamento de tráfego do usuário entre pontos de extremidade usando o método de roteamento ponderado. Nesse método de roteamento de tráfego, você atribui um peso a cada ponto de extremidade na configuração do perfil do Gerenciador de Tráfego. O tráfego do usuário é roteado com base no peso atribuído a cada ponto de extremidade. O peso é um inteiro de 1 a 1.000. Quanto maior o valor do peso atribuído a um ponto de extremidade, mais alta é sua prioridade.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar duas VMs em execução em um site básico no IIS.
> * Criar duas VMs de teste para ver o Gerenciador de Tráfego em ação.
> * Configurar um nome DNS para as VMs que executam IIS.
> * Criar um perfil do Gerenciador de Tráfego.
> * Adicionar pontos de extremidade de VM no perfil do Gerenciador de Tráfego.
> * Veja o Gerenciador de Tráfego em ação.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para ver o Gerenciador de Tráfego em ação, implante o seguinte para este tutorial:
- Duas instâncias de sites básicos em execução em diferentes regiões do Azure: Leste dos EUA e Europa Ocidental.
- Duas VMs de teste para testar o Gerenciador de Tráfego: uma VM no Leste dos EUA e outra na Europa Ocidental. As VMs de teste são usadas para ilustrar como o Gerenciador de Tráfego roteia o tráfego de usuário para um site que tem o peso mais alto atribuído ao seu ponto de extremidade.

### <a name="sign-in-to-azure"></a>Entrar no Azure 

Entre no [Portal do Azure](https://portal.azure.com).

### <a name="create-websites"></a>Criar sites

Nesta seção, você criará duas instâncias de site que fornecem os dois pontos de extremidade para o perfil do Gerenciador de Tráfego em duas regiões do Azure. Para criar os dois sites, conclua as seguintes etapas:
1. Crie duas VMs para executar um site básico: uma no Leste dos EUA e outra na Europa Ocidental.
2. Instale um servidor IIS em cada VM. Atualize a página da Web padrão que descreve o nome da VM à qual um usuário está conectado ao visitar o site.

#### <a name="create-vms-for-running-websites"></a>Criar VMs para a execução de sites
Nesta seção, você criará duas VMs (*myIISVMEastUS* e *myIISVMWEurope*) nas regiões do Azure Leste dos EUA e Europa Ocidental.

1. No canto superior esquerdo do portal do Azure, selecione **Criar um recurso** > **Computação** > **VM do Windows Server 2016**.
2. Insira ou selecione as seguintes informações em **Noções Básicas**. Aceite os padrões para as outras configurações e selecione **Criar**.

    |Configuração|Valor|
    |---|---|
    |NOME|Insira **myIISVMEastUS**.|
    |Nome de usuário| Insira um nome de usuário de sua escolha.|
    |Senha| Insira uma senha de sua escolha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **Novo**e insira **myResourceGroupTM1**.|
    |Local padrão| Selecione **Leste dos EUA**.|
    |||
4. Selecione um tamanho da VM em **Escolher um tamanho**.
5. Selecione os seguintes valores para **Configurações** e selecione **OK**:
    
    |Configuração|Valor|
    |---|---|
    |Rede virtual| Selecione **Rede virtual**. Em **Criar rede virtual**, como **Nome**, insira **myVNet1**. Em **Sub-rede**, insira **mySubnet**.|
    |Grupo de Segurança de Rede|Selecione **Basic**. Na lista suspensa **Selecionar portas de entrada públicas**, selecione **HTTP** e **RDP**. |
    |Diagnóstico de inicialização|Selecione **Desabilitado**.|
    |||
6. Em **Criar**, em **Resumo**, selecione **Criar** para iniciar a implantação da VM.

7. Conclua as etapas 1 a 6 novamente, com as seguintes alterações:

    |Configuração|Valor|
    |---|---|
    |Grupo de recursos | Selecione **Novo**e insira **myResourceGroupTM2**.|
    |Local padrão|Insira **Europa Ocidental**.|
    |Nome da VM | Insira **myIISVMWEurope**.|
    |Rede virtual | Selecione **Rede virtual**. Em **Criar rede virtual**, como **Nome**, insira **myVNet2**. Em **Sub-rede**, insira **mySubnet**.|
    |||
8. As VMs podem levar alguns minutos para serem criadas. Não continue com outras etapas até que ambas as VMs tenham sido criadas.

![Criar uma máquina virtual](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-webpage"></a>Instalar o IIS e personalizar a página da Web padrão

Nesta seção, você deve instalar o servidor IIS em duas VMs&mdash;myIISVMEastUS e myIISVMWEurope&mdash; e, em seguida, atualizar a página do site padrão. A página do site personalizada mostra o nome da VM à qual você está se conectando ao visitar o site em um navegador da Web.

1. Selecione **Todos os recursos** no menu à esquerda. Na lista de recursos, selecione **myIISVMEastUS** no grupo de recursos **myResourceGroupTM1**.
2. Na página **Visão Geral**, selecione **Conectar**. Em **Conectar-se à máquina virtual**, selecione **Baixar arquivo RDP**. 
3. Abra o arquivo .rdp baixado. Se solicitado, selecione **Conectar**. Insira o nome de usuário e a senha que você especificou quando criou a VM. Talvez seja necessário selecionar **Mais opções** > **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM. 
4. Selecione **OK**.
5. Você pode receber um aviso de certificado durante o processo de entrada. Se você receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a conexão.
6. Na área de trabalho do servidor, procure **Ferramentas Administrativas do Windows** > **Gerenciador do Servidor**.
7. Abra o Windows PowerShell na VM1. Use os seguintes comandos para instalar o servidor IIS e atualizar o arquivo .htm padrão.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default .htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom .htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Instalar o IIS e personalizar a página da Web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)

8. Feche a conexão RDP com **myIISVMEastUS**.
9. Repita as etapas de 1 a 8. Crie uma conexão RDP com a VM **myIISVMWEurope** no grupo de recursos **myResourceGroupTM2** para instalar o IIS e personalizar sua página da Web padrão.

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurar nome DNS para as VMs que executam IIS

O Gerenciador de Tráfego roteia o tráfego de usuário com base no nome DNS dos pontos de extremidade de serviço. Nesta seção, você configura os nomes DNS para os servidores IIS myIISVMEastUS e myIISVMWEurope.

1. Selecione **Todos os recursos** no menu à esquerda. Na lista de recursos, selecione **myIISVMEastUS** no grupo de recursos **myResourceGroupTM1**.
2. Na página **Visão Geral**, em **Nome DNS**, selecione **Configurar**.
3. Na página **Configuração**, em rótulo de nome DNS, adicione um nome exclusivo. Em seguida, selecione **Salvar**.
4. Repita as etapas de 1 a 3 para a VM denominada **myIISVMWEurope** no grupo de recursos **myResourceGroupTM1**.

### <a name="create-a-test-vm"></a>Criar uma VM de teste

Nesta seção, você cria a VM *mVMEastUS*. Você usará essa VM para testar como o Gerenciador de Tráfego roteia o tráfego para o ponto de extremidade de site que tem o valor de peso mais alto.

1. No canto superior esquerdo do portal do Azure, selecione **Criar um recurso** > **Computação** > **VM do Windows Server 2016**.
2. Insira ou selecione as seguintes informações em **Noções Básicas**. Aceite os padrões para as outras configurações e selecione **Criar**:

    |Configuração|Valor|
    |---|---|
    |NOME|Insira **myVMEastUS**.|
    |Nome de usuário| Insira um nome de usuário de sua escolha.|
    |Senha| Insira uma senha de sua escolha. A senha deve ter no mínimo 12 caracteres e atender a [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **Usar existente** e **myResourceGroupTM1**.|
    |||

4. Selecione um tamanho da VM em **Escolher um tamanho**.
5. Selecione os seguintes valores para **Configurações** e selecione **OK**:
    |Configuração|Valor|
    |---|---|
    |Rede virtual| Selecione **Rede virtual**. Em **Criar rede virtual**, como **Nome**, insira **myVNet3**. Em Sub-rede, insira **mySubnet**.|
    |Grupo de Segurança de Rede|Selecione **Basic**. Na lista suspensa **Selecionar portas de entrada públicas**, selecione **HTTP** e **RDP**. |
    |Diagnóstico de inicialização|Selecione **Desabilitado**.|
    |||

6. Em **Criar**, em **Resumo**, selecione **Criar** para iniciar a implantação da VM.
8. A VM demora alguns minutos para criar. Não continue com outras etapas até que a VM tenha sido criada.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gerenciador de Tráfego
Criar um perfil do Gerenciador de Tráfego com base no método de roteamento **Ponderado**.

1. No canto superior esquerdo da tela, selecione **Criar um recurso** > **Rede** > **Perfil do Gerenciador de Tráfego**  >  **Criar**.
2. No **Criar perfil do Gerenciador de Tráfego**, insira ou selecione as informações a seguir. Aceite os padrões para as outras configurações e selecione **Criar**.

    | Configuração                 | Valor                                              |
    | ---                     | ---                                                |
    | NOME                   | Insira um nome exclusivo na zona trafficmanager.net. Isso resulta no nome DNS trafficmanager.net, que é usado para acessar seu perfil do Gerenciador de Tráfego.                                   |
    | Método de roteamento          | Selecione o método de roteamento **Ponderado**.                                       |
    | Assinatura            | Selecione sua assinatura.                          |
    | Grupo de recursos          | Selecione **Usar existente** e **myResourceGroupTM1**. |
    |        |   |

    ![Criar um perfil do Gerenciador de Tráfego](./media/tutorial-traffic-manager-weighted-endpoint-routing/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Adicionar pontos de extremidade do Gerenciador de Tráfego

Adicione as duas VMs executando os servidores IIS, myIISVMEastUS e myIISVMWEurope, a fim de rotear o tráfego de usuário para eles.

1. Na barra de pesquisa do portal, procure o nome do perfil do Gerenciador de Tráfego criado na seção anterior. Selecione o perfil nos resultados exibidos.
2. Em **Perfil do Gerenciador de Tráfego**, na seção **Configurações**, selecione **Pontos de Extremidade** > **Adicionar**.
3. Insira ou selecione as seguintes informações. Aceite os padrões para as outras configurações e selecione **OK**.

    | Configuração                 | Valor                                              |
    | ---                     | ---                                                |
    | Tipo                    | Insira o ponto de extremidade do Azure.                                   |
    | NOME           | Insira **myEastUSEndpoint**.                                        |
    | Tipo de recurso de destino           | Selecione **Endereço IP público**.                          |
    | Recurso de destino          | Escolha um endereço IP Público para mostrar a lista de recursos com endereços IP públicos na mesma assinatura. Em **Recursos**, selecione o endereço IP público denominado **myIISVMEastUS-ip**. Isso é o endereço IP público do servidor IIS VM no Leste dos EUA.|
    |  Peso      | Insira **100**.        |
    |        |           |

4. Repita as etapas 2 e 3 para adicionar outro ponto de extremidade chamado **myWestEuropeEndpoint** ao endereço IP público **myIISVMWEurope-ip**. Esse endereço é associado à VM do servidor IIS chamada myIISVMWEurope. Em **Peso**, insira **25**. 
5.  Quando a adição de ambos os pontos de extremidade estiver concluída, eles serão exibidos em Perfil do Gerenciador de Tráfego com status de monitoramento **Online**.

## <a name="test-the-traffic-manager-profile"></a>Testar o perfil do Gerenciador de Tráfego
Para ver o Gerenciador de Tráfego em ação, conclua as seguintes etapas:
1. Determine o nome DNS do seu perfil do Gerenciador de Tráfego.
2. Veja o Gerenciador de Tráfego em ação.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Determinar o nome DNS do Perfil do Gerenciador de Tráfego
Neste tutorial, para manter a simplicidade, você usará o nome DNS do perfil do Gerenciador de Tráfego do Microsoft Azure para visitar os sites da Web. 

Você pode determinar o nome DNS do seu perfil do Gerenciador de Tráfego conforme a seguir:

1.  Na barra de pesquisa do portal, procure o nome do perfil do Gerenciador de Tráfego criado na seção anterior. Nos resultados exibidos, selecione o perfil do Gerenciador de Tráfego.
1. Selecione **Visão geral**.
2. O perfil do Gerenciador de Tráfego exibe seu nome DNS. Nas implantações de produção, configure um nome de domínio intuitivo para apontar para o nome de domínio do Gerenciador de Tráfego usando um registro CNAME de DNS.

   ![Nome DNS do Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Ver o Gerenciador de Tráfego em ação
Nesta seção, você pode ver o Gerenciador de Tráfego em ação. 

1. Selecione **Todos os recursos** no menu à esquerda. Na lista de recursos, selecione **myVMEastUS** no grupo de recursos **myResourceGroupTM1**.
2. Na página **Visão Geral**, selecione **Conectar**. Em **Conectar-se à máquina virtual**, selecione **Baixar arquivo RDP**. 
3. Abra o arquivo .rdp baixado. Se solicitado, selecione **Conectar**. Insira o nome de usuário e senha que você especificou ao criar a VM. Talvez seja necessário selecionar **Mais opções** > **Usar uma conta diferente** para especificar as credenciais inseridas durante a criação da VM. 
4. Selecione **OK**.
5. Você pode receber um aviso de certificado durante o processo de entrada. Se você receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a conexão. 
6. Em um navegador da Web na VM myVMEastUS, insira o nome do DNS do seu perfil do Gerenciador de Tráfego para visualizar seu site. Você será encaminhado para o site hospedado no servidor IIS myIISVMEastUS porque ele recebeu um peso mais alto, **100**. O servidor IIS myIISVMWEurope recebe um valor de peso de ponto de extremidade mais baixo, **25**.

   ![Testar perfil de Gerenciador de Tráfego](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Excluir o perfil do Gerenciador de Tráfego
Quando você não precisar mais dos recursos criados neste tutorial, poderá excluí-los. Para fazer isso, selecione o grupo de recursos (**ResourceGroupTM1** ou **ResourceGroupTM2**) e, em seguida, selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Rotear o tráfego para pontos de extremidade específicos com base na localização geográfica do usuário](traffic-manager-configure-geographic-routing-method.md)


