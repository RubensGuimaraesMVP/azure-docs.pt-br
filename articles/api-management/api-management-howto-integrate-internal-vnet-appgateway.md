---
title: Como usar o Gerenciamento de API do Azure na rede virtual com Gateway de Aplicativo | Microsoft Docs
description: Saiba como instalar e configurar o gerenciamento de API do Azure na rede virtual interna com o Gateway de Aplicativo (WAF) como front-end
services: api-management
documentationcenter: ''
author: solankisamir
manager: kjoshi
editor: vlvinogr
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: sasolank
ms.openlocfilehash: 6356d930b5bf909f1b209272e7367f5e2dcd5a13
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444608"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>Como integrar o gerenciamento de API em uma VNET interna com o gateway de aplicativo

## <a name="overview"> </a> Visão geral

O serviço de Gerenciamento de API pode ser configurado em uma rede virtual no modo interno, o que o torna acessível apenas de dentro da Rede Virtual. O Gateway de Aplicativo do Aplicativo Azure AD é um Serviço PAAS que fornece um balanceador de carga de Camada 7. Ele atua como um serviço de proxy reverso e fornece também um firewall do aplicativo Web (WAF).

Combinar o Gerenciamento de API provisionado em uma rede virtual interna com o front-end do Gateway de Aplicativo permite os seguintes cenários:

* Usar o mesmo recurso de Gerenciamento de API para consumo por clientes internos e externos.
* Usar um único recurso de Gerenciamento de API e ter uma sub-rede de APIs definida no Gerenciamento de API disponível para consumidores externos.
* Fornecer uma maneira rápida de ativar e desativar o acesso ao Gerenciamento de API da Internet pública.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para seguir as etapas descritas neste artigo, você precisa ter:

* Uma assinatura ativa do Azure.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Certificados - pfx e cer para o nome do host de API e o pfx para o nome de host do portal do desenvolvedor.

## <a name="scenario"> </a> Cenário

Este descreve como usar um único serviço do Gerenciamento de API para consumidores internos e externos e como fazê-lo atuar como um único front-end para APIs locais e na nuvem. Você também verá como expor somente um subconjunto de suas APIs (destacadas em verde no exemplo) para Consumo Externo usando a funcionalidade de roteamento disponível no Gateway de Aplicativo.

No primeiro exemplo de instalação, todas as suas APIs são gerenciadas somente de dentro da sua Rede Virtual. Os consumidores internos (destacados em laranja) podem acessar todas as APIs internas e externas. O tráfego nunca segue para a Internet, um alto desempenho é entregue via circuitos de Rota Expressa.

![roteamento de url](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"> </a> Antes de começar

* Use a versão mais recente do Azure PowerShell. Há mais informações disponíveis em [Como usar o Windows PowerShell com o Gerenciador de Recursos](https://docs.microsoft.com/azure/azure-resource-manager/powershell-azure-resource-manager).

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>O que é necessário para criar uma integração entre o gerenciamento de API e o gateway de aplicativo?

* **Pool de servidor de back-end:** o endereço IP virtual interno do serviço de gerenciamento de API.
* **Configurações do pool de servidores back-end:** cada pool tem configurações como porta, protocolo e afinidade baseada em cookie. Essas configurações são aplicadas a todos os servidores no pool.
* **Porta front-end:** essa porta é a porta pública aberta no gateway de aplicativo. Ao ser atingindo, o tráfego é redirecionado para um dos servidores de back-end.
* **Ouvinte:** o ouvinte tem uma porta front-end, um protocolo (HTTP ou HTTPS, esses valores diferenciam maiúsculas de minúsculas) e o nome do certificado SSL (caso esteja configurando o descarregamento SSL).
* **Regra:** a regra vincula um ouvinte para um pool de servidor de back-end.
* **Investigação de Integridade Personalizada:** o gateway de aplicativo, por padrão, usa investigações baseadas em endereço IP para entender quais servidores no BackendAddressPool estão ativos. O serviço de Gerenciamento de API responde apenas às solicitações que têm o cabeçalho de host correto, portanto, as investigações padrão falham. Uma investigação de integridade personalizada precisa ser definida para ajudar o gateway de aplicativo a determinar que o serviço está ativo e, assim, encaminhar as solicitações.
* **Certificado de domínio personalizado:** para acessar o Gerenciamento de API da Internet, você precisa fazer um mapeamento do nome do host de CNAME para o nome DNS de front-end do Gateway de Aplicativo. Isso garante que o certificado e o cabeçalho do nome do host enviados ao Gateway de Aplicativo que são encaminhados ao Gerenciamento de API possam ser reconhecidos e validados pelo APIM. Neste exemplo, usaremos dois certificados – para o back-end e para o portal do desenvolvedor.  

## <a name="overview-steps"> </a> Etapas necessárias para integrar o Gerenciamento de API e o Gateway de Aplicativo

1. Criar um grupo de recursos para o Gerenciador de Recursos.
2. Crie uma rede virtual, uma sub-rede e um IP público para o gateway de aplicativo. Crie outra sub-rede para o gerenciamento de API.
3. Crie um serviço de Gerenciamento de API dentro da sub-rede da VNET criada acima e não deixe de usar o Modo interno.
4. Configure o nome de domínio personalizado no serviço de Gerenciamento de API.
5. Crie um objeto de configuração do Gateway de Aplicativo.
6. Crie um recurso de Gateway de Aplicativo.
7. Crie um CNAME do nome DNS público do Gateway de Aplicativo para o nome do host do proxy de Gerenciamento de API.

## <a name="exposing-the-developer-portal-externally-through-application-gateway"></a>Expondo o portal do desenvolvedor externamente por meio do Gateway de Aplicativo

Neste guia, também irá expor o **portal do desenvolvedor** ao público externo por meio do Gateway de Aplicativo. Ele requer etapas adicionais para criar o ouvinte, teste, configurações e regras do portal do desenvolvedor. Todos os detalhes são fornecidos nas respectivas etapas.

> [!WARNING]
> Na configuração descrita do portal do desenvolvedor que está sendo acessado por meio do Gateway de Aplicativo, você pode enfrentar problemas com a autenticação de terceiros e do AAD.

## <a name="create-a-resource-group-for-resource-manager"></a>Criar um grupo de recursos para o Gerenciador de Recursos

### <a name="step-1"></a>Etapa 1

Fazer logon no Azure

```powershell
Login-AzureRmAccount
```

Como autenticar com suas credenciais.

### <a name="step-2"></a>Etapa 2

Selecione a assinatura desejada.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000" # GUID of your Azure subscription
Get-AzureRmSubscription -Subscriptionid $subscriptionId | Select-AzureRmSubscription
```

### <a name="step-3"></a>Etapa 3

Crie um grupo de recursos (ignore esta etapa se você estiver usando um grupo de recursos existente).

```powershell
$resGroupName = "apim-appGw-RG" # resource group name
$location = "West US"           # Azure region
New-AzureRmResourceGroup -Name $resGroupName -Location $location
```

O Azure Resource Manager requer que todos os grupos de recursos especifiquem um local. Ele é usado como o local padrão para os recursos do grupo de recursos em questão. Verifique se todos os comandos para criar um Gateway de Aplicativo usam o mesmo grupo de recursos.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Crie uma rede virtual e uma sub-rede para o gateway de aplicativo

O exemplo a seguir mostra como criar uma rede virtual usando o Resource Manager.

### <a name="step-1"></a>Etapa 1

Ao criar uma rede virtual, atribua o intervalo de endereço 10.0.0.0/24 à variável de sub-rede a ser usada para o Gateway de Aplicativo.

```powershell
$appgatewaysubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>Etapa 2

Ao criar uma rede virtual, atribua o intervalo de endereço 10.0.1.0/24 à variável de sub-rede a ser usada para o gerenciamento de API.

```powershell
$apimsubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>Etapa 3

Crie uma Rede Virtual do Microsoft Azure **appgwvnet** no grupo de recursos **apim-appGw-RG** para a região Oeste dos EUA. Use o prefixo 10.0.0.0/16 com as sub-redes 10.0.0.0/24 e 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name "appgwvnet" -ResourceGroupName $resGroupName -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>Etapa 4

Atribua uma variável de sub-rede para as próximas etapas

```powershell
$appgatewaysubnetdata = $vnet.Subnets[0]
$apimsubnetdata = $vnet.Subnets[1]
```

## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Crie um serviço de Gerenciamento de API dentro de uma Vnet configurada no modo interno

O exemplo a seguir mostra como criar um serviço de Gerenciamento de API em uma Vnet configurada somente para acesso interno.

### <a name="step-1"></a>Etapa 1

Crie um objeto de rede virtual de gerenciamento de API usando a sub-rede $apimsubnetdata criada acima.

```powershell
$apimVirtualNetwork = New-AzureRmApiManagementVirtualNetwork -Location $location -SubnetResourceId $apimsubnetdata.Id
```

### <a name="step-2"></a>Etapa 2

Crie um serviço de gerenciamento de API dentro da rede virtual.

```powershell
$apimServiceName = "ContosoApi"       # API Management service instance name
$apimOrganization = "Contoso"         # organization name
$apimAdminEmail = "admin@contoso.com" # administrator's email address
$apimService = New-AzureRmApiManagement -ResourceGroupName $resGroupName -Location $location -Name $apimServiceName -Organization $apimOrganization -AdminEmail $apimAdminEmail -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```

Depois que o comando acima for bem-sucedido, confira a [Configuração DNS necessária para acessar o serviço de Gerenciamento de API da VNET interna](api-management-using-with-internal-vnet.md#apim-dns-configuration) para acessá-lo. Esta etapa pode levar mais de meia hora.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>Configure um nome de domínio personalizado no Gerenciamento de API

### <a name="step-1"></a>Etapa 1

Carregue o certificado com a chave privada para o domínio. Neste exemplo, usaremos `api.contoso.net` e `portal.contoso.net`.  

```powershell
$gatewayHostname = "api.contoso.net"                 # API gateway host
$portalHostname = "portal.contoso.net"               # API developer portal host
$gatewayCertCerPath = "C:\Users\Contoso\gateway.cer" # full path to api.contoso.net .cer file
$gatewayCertPfxPath = "C:\Users\Contoso\gateway.pfx" # full path to api.contoso.net .pfx file
$portalCertPfxPath = "C:\Users\Contoso\portal.pfx"   # full path to portal.contoso.net .pfx file
$gatewayCertPfxPassword = "certificatePassword123"   # password for api.contoso.net pfx certificate
$portalCertPfxPassword = "certificatePassword123"    # password for portal.contoso.net pfx certificate

$certUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName $resGroupName -Name $apimServiceName -HostnameType "Proxy" -PfxPath $gatewayCertPfxPath -PfxPassword $gatewayCertPfxPassword -PassThru
$certPortalUploadResult = Import-AzureRmApiManagementHostnameCertificate -ResourceGroupName $resGroupName -Name $apimServiceName -HostnameType "Proxy" -PfxPath $portalCertPfxPath -PfxPassword $portalCertPfxPassword -PassThru
```

### <a name="step-2"></a>Etapa 2

Depois que os certificados forem carregados, crie nome de host objetos de configuração para o proxy e para o portal.  

```powershell
$proxyHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certUploadResult.Thumbprint -Hostname $gatewayHostname
$portalHostnameConfig = New-AzureRmApiManagementHostnameConfiguration -CertificateThumbprint $certPortalUploadResult.Thumbprint -Hostname $portalHostname
$result = Set-AzureRmApiManagementHostnames -Name $apimServiceName -ResourceGroupName $resGroupName –PortalHostnameConfiguration $portalHostnameConfig -ProxyHostnameConfiguration $proxyHostnameConfig
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Criar um endereço IP público para a configuração de front-end

Criar um recurso IP público **publicIP01** no grupo de recursos.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName $resGroupName -name "publicIP01" -location $location -AllocationMethod Dynamic
```

Um endereço IP é atribuído ao gateway de aplicativo quando o serviço é iniciado.

## <a name="create-application-gateway-configuration"></a>Criar uma configuração do gateway de aplicativo

Todos os itens de configuração devem estar configurados antes de criar o gateway de aplicativo. As etapas a seguir criam os itens de configuração necessários para um recurso de gateway de aplicativo.

### <a name="step-1"></a>Etapa 1

Crie uma configuração de IP do gateway de aplicativo chamada **gatewayIP01**. Quando o Gateway de Aplicativo é iniciado, ele escolhe um endereço IP na sub-rede configurada e no tráfego de rede da rota para os endereços IP no pool de IPs de back-end. Tenha em mente que cada instância usa um endereço IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>Etapa 2

Configure a porta IP front-end para o ponto de extremidade IP público. Essa porta é a porta à qual os usuários finais se conectam.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "port01"  -Port 443
```

### <a name="step-3"></a>Etapa 3

Configure o IP front-end com o ponto de extremidade IP público.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>Etapa 4

Configure o certificado para o Gateway de Aplicativo usado para descriptografar e criptografar novamente o tráfego.

```powershell
$certPwd = ConvertTo-SecureString $gatewayCertPfxPassword -AsPlainText -Force
$cert = New-AzureRmApplicationGatewaySslCertificate -Name "cert01" -CertificateFile $gatewayCertPfxPath -Password $certPwd
$certPortalPwd = ConvertTo-SecureString $portalCertPfxPassword -AsPlainText -Force
$certPortal = New-AzureRmApplicationGatewaySslCertificate -Name "cert02" -CertificateFile $portalCertPfxPath -Password $certPortalPwd
```

### <a name="step-5"></a>Etapa 5

Crie o ouvinte HTTP para o Gateway de Aplicativo. Atribua a configuração do IP, a porta e o certificado SSL do front-end a ser usado.

```powershell
$listener = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert -HostName $gatewayHostname -RequireServerNameIndication true
$portalListener = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $certPortal -HostName $portalHostname -RequireServerNameIndication true
```

### <a name="step-6"></a>Etapa 6

Crie uma investigação personalizada para o ponto de extremidade de domínio do proxy `ContosoApi` do serviço de Gerenciamento de API. O caminho `/status-0123456789abcdef` é um ponto de extremidade de integridade padrão hospedado em todos os serviços de Gerenciamento de API. Defina `api.contoso.net` como um nome de host de investigação personalizado para protegê-lo com o certificado SSL.

> [!NOTE]
> O nome do host `contosoapi.azure-api.net` é o nome do host do proxy padrão configurado quando um serviço `contosoapi` é criado no Azure público.
>

```powershell
$apimprobe = New-AzureRmApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName $gatewayHostname -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
$apimPortalProbe = New-AzureRmApplicationGatewayProbeConfig -Name "apimportalprobe" -Protocol "Https" -HostName $portalHostname -Path "/signin" -Interval 60 -Timeout 300 -UnhealthyThreshold 8
```

### <a name="step-7"></a>Etapa 7

Carregue o certificado a ser usado nos recursos do pool de back-end habilitado para SSL. Esse é o mesmo certificado que você forneceu na etapa 4 acima.

```powershell
$authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile $gatewayCertCerPath
```

### <a name="step-8"></a>Etapa 8

Defina configurações de back-end HTTP para o Gateway de Aplicativo. Isso inclui a definição de um tempo limite para solicitação de back-end após o qual elas serão canceladas. Esse valor é diferente do tempo limite de investigação.

```powershell
$apimPoolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
$apimPoolPortalSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "apimPoolPortalSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimPortalProbe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>Etapa 9

Configure o pool de endereços IP de back-end **apimbackend** com o endereço IP virtual interno para o serviço de gerenciamento de API criado acima.

```powershell
$apimProxyBackendPool = New-AzureRmApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.PrivateIPAddresses[0]
```

### <a name="step-10"></a>Etapa 10

Crie regras para o Gateway de Aplicativo para usar o roteamento básico.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType Basic -HttpListener $listener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType Basic -HttpListener $portalListener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolPortalSetting
```

> [!TIP]
> Altere o RuleType - e roteamento, para restringir o acesso a determinadas páginas do portal do desenvolvedor.

### <a name="step-11"></a>Etapa 11

Configure o número de instâncias e o tamanho do gateway de aplicativo. Neste exemplo, estamos usando o [WAF SKU](../application-gateway/application-gateway-webapplicationfirewall-overview.md) para aumentar a segurança do recurso de Gerenciamento de API.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-12"></a>Etapa 12

Configure WAF para ativar o modo de "Prevenção".

```powershell
$config = New-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Criar um Gateway de Aplicativo

Crie um Gateway de Aplicativo com todos os objetos de configuração das etapas anteriores.

```powershell
$appgwName = "apim-app-gw"
$appgw = New-AzureRmApplicationGateway -Name $appgwName -ResourceGroupName $resGroupName -Location $location -BackendAddressPools $apimProxyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $apimPoolPortalSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener, $portalListener -RequestRoutingRules $rule01, $rule02 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert, $certPortal -AuthenticationCertificates $authcert -Probes $apimprobe, $apimPortalProbe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>Dê um CNAME ao nome do host do proxy do Gerenciamento de API para o nome DNS público do recurso de Gateway de Aplicativo

Depois de criar o gateway, a próxima etapa será configurar o front-end para comunicação. Ao usar um IP público, o Gateway de Aplicativo requer um nome DNS atribuído dinamicamente, o que não é amigável.

O nome DNS do gateway de aplicativo deve ser usado para criar um registro CNAME, que aponta o nome de host do proxy APIM (conforme `api.contoso.net` mostrado no exemplo anterior) para esse nome DNS. Para configurar o registro CNAME de IP de front-end, recupere os detalhes do Gateway de Aplicativo e seu nome DNS/IP associado usando o elemento PublicIPAddress. O uso de registros A não é recomendável, pois o VIP pode mudar na reinicialização do gateway.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resGroupName -Name "publicIP01"
```

## <a name="summary"> </a> Resumo
O Gerenciamento de API do Azure configurado em uma VNET fornece uma interface de gateway única para todas as APIs configuradas, independentemente de estarem hospedados no local ou na nuvem. Integrar o Gateway de Aplicativo com o Gerenciamento de API fornece a flexibilidade de ativar seletivamente as APIs específicas que serão acessíveis na Internet, além de fornecer um Firewall do Aplicativo Web como um front-end para sua instância de Gerenciamento de API.

##<a name="next-steps"> </a> Próximas etapas
* Saiba mais sobre o Gateway de Aplicativo do Azure
  * [Visão geral do Gateway de Aplicativo](../application-gateway/application-gateway-introduction.md)
  * [Firewall do Aplicativo Web do Gateway de Aplicativo (visualização)](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Gateway de Aplicativo usando Roteamento com base em Caminho](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* Saiba mais sobre o gerenciamento de API nas VNETs
  * [Usando o Gerenciamento de API disponível somente na VNET](api-management-using-with-internal-vnet.md)
  * [Uso do Gerenciamento de API na rede virtual](api-management-using-with-vnet.md)
