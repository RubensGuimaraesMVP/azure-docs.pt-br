---
title: 'Interoperabilidade em recursos de conectividade de back-end do Azure: análise do plano de controle | Microsoft Docs'
description: Este artigo fornece a análise do plano de controle da configuração do teste que é possível utilizar para analisar a interoperabilidade entre o ExpressRoute, uma VPN site a site e o emparelhamento de rede virtual no Azure.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: 37f5399426bebd375200bbc18dae7ed83f4fde3f
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614661"
---
# <a name="interoperability-in-azure-back-end-connectivity-features-control-plane-analysis"></a>Interoperabilidade em recursos de conectividade de back-end do Azure: análise do plano de controle

Este artigo descreve a análise do plano de controle da [configuração do teste][Setup]. Também é possível examinar a [configuração do teste][Configuration] e a [análise do plano de dados][Data-Analysis] da configuração do teste.

A análise do plano de controle examina essencialmente as rotas que são trocadas entre redes dentro de uma topologia. A análise do plano de controle pode ajudá-lo a reconhecer como diferentes redes exibem a topologia.

## <a name="hub-and-spoke-vnet-perspective"></a>Perspectiva de VNet de hub e spoke

A figura a seguir ilustra a rede a partir da perspectiva de uma VNet (rede virtual) de hub e de uma VNet de spoke (destacada em azul). A figura também mostra o ASN (número do sistema autônomo) de diferentes redes e rotas que são trocadas entre diferentes redes: 

[![1]][1]

O ASN do gateway do Azure ExpressRoute da VNet é diferente do ASN dos MSEEs (Microsoft Enterprise Edge Routers). Um gateway do ExpressRoute usa um ASN privado (um valor de **65515**) e os MSEEs usam um ASN público (um valor de **12076**) globalmente. Ao configurar o emparelhamento de ExpressRoute, porque MSEE é o par, você usará **12076** como o ASN do par. No lado do Azure, o MSEE estabelece o emparelhamento de eBGP com o gateway do ExpressRoute. O emparelhamento de eBGP duplo que o MSEE estabelece para cada emparelhamento do ExpressRoute é transparente no nível do plano de controle. Portanto, quando você exibe uma tabela de rotas do ExpressRoute, verá o ASN do gateway do ExpressRoute da VNet para os prefixos da VNet. 

A figura a seguir mostra uma tabela de rotas do ExpressRoute: 

[![5]][5]

No Azure, o ASN é significativo apenas a partir de uma perspectiva de emparelhamento. Por padrão, o ASN do gateway do ExpressRoute e do gateway da VPN no Gateway de VPN do Azure é **65515**.

## <a name="on-premises-location-1-and-the-remote-vnet-perspective-via-expressroute-1"></a>Localização 1 local e a perspectiva remota da VNet via ExpressRoute 1

Ambas a Localização 1 local e a VNet remota são conectadas à VNet do hub via ExpressRoute 1. Elas compartilham a mesma perspectiva da topologia, conforme mostrado no diagrama a seguir:

[![2]][2]

## <a name="on-premises-location-1-and-the-branch-vnet-perspective-via-a-site-to-site-vpn"></a>Localização 1 local e a perspectiva da VNet de branch via uma VPN site a site

Ambas as Localização 1 local e a VNet de branch estão conectadas a um gateway de VPN da VNet central por meio de uma conexão VPN site a site. Elas compartilham a mesma perspectiva da topologia, conforme mostrado no diagrama a seguir:

[![3]][3]

## <a name="on-premises-location-2-perspective"></a>Perspectiva de Localização 2 local

A Localização 2 local é conectada a uma VNet de hub via emparelhamento privado do ExpressRoute 2: 

[![4]][4]

## <a name="expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Conectividade do ExpressRoute e VPN site a site em conjunto

###  <a name="site-to-site-vpn-over-expressroute"></a>VPN site a site pelo ExpressRoute

É possível configurar uma VPN site a site, usando emparelhamento do Microsoft ExpressRoute para trocar dados em modo privado entre a rede local e as VNets do Azure. Com essa configuração, é possível trocar dados com confidencialidade, autenticidade e integridade. A troca de dados também é antirreprodução. Para obter mais informações sobre como configurar uma VPN IPsec site a site no modo de túnel usando emparelhamento do ExpressRoute da Microsoft, consulte [VPN site a site sobre emparelhamento do Microsoft Azure ExpressRoute][S2S-Over-ExR]. 

A principal limitação da configuração de uma VPN site a site que usa o emparelhamento da Microsoft é a taxa de transferência. A taxa de transferência no túnel IPsec é limitada pela capacidade do gateway de VPN. A taxa de transferência do gateway de VPN é menor que a taxa de transferência do ExpressRoute. Nesse cenário, o uso do túnel IPsec para tráfego altamente seguro e o uso de emparelhamento privado para todos os outros tráfegos ajuda a otimizar a utilização da largura de banda do ExpressRoute.

### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN site a site como um caminho de failover seguro para o ExpressRoute

O ExpressRoute funciona como um par de circuitos redundantes para garantir alta disponibilidade. Você pode configurar a conectividade de ExpressRoute geo-redundante em diferentes regiões do Azure. Além disso, conforme demonstrado em nossa configuração do teste, em uma região do Azure, é possível usar uma VPN site a site para criar um caminho de failover para a conectividade do ExpressRoute. Quando os mesmos prefixos são anunciados sobre o ExpressRoute e uma VPN site a site, o Azure prioriza o ExpressRoute. Para evitar roteamento assimétrico entre o ExpressRoute e a VPN site a site, a configuração de rede local também deverá retribuir usando a conectividade do ExpressRoute antes de usar a conectividade VPN site a site.

Para obter mais informações sobre como configurar conexões coexistentes para o ExpressRoute e uma VPN site a site, consulte [ExpressRoute e coexistência site a site][ExR-S2S-CoEx].

## <a name="extend-back-end-connectivity-to-spoke-vnets-and-branch-locations"></a>Estender conectividade de back-end para VNets do spoke e localizações de branch

### <a name="spoke-vnet-connectivity-by-using-vnet-peering"></a>Conectividade de VNet do spoke usando emparelhamento VNet

A arquitetura da VNet do hub e do spoke é amplamente usada. O hub é uma VNet no Azure que funciona como um ponto central de conectividade entre as VNets do spoke e a rede local. Os spokes são VNets que emparelham-se com o hub e que podem ser utilizadas para isolar as cargas de trabalho. O tráfego flui entre o datacenter local e o hub por meio de uma conexão de VPN ou ExpressRoute. Para obter mais informações sobre a arquitetura, confira [Implementar uma topologia de rede hub-spoke no Azure][Hub-n-Spoke].

Em emparelhamento VNet dentro de uma região, as VNets do spoke podem usar gateways de VNet do hub (ambos gateways de VPN e ExpressRoute) para comunicarem-se com redes remotas.

### <a name="branch-vnet-connectivity-by-using-site-to-site-vpn"></a>Conectividade da VNet do branch usando VPN site a site

Talvez você queira que as VNets de branch, que estão em regiões diferentes, e as redes locais comuniquem-se entre si por meio de uma VNet de hub. A solução nativa do Azure para essa configuração é a conectividade de VPN site a site usando uma VPN. Uma alternativa é usar uma NVA (solução de virtualização de rede) para roteamento no hub.

Para obter mais informações, consulte [O que é Gateway de VPN? ][VPN] e [Implantar um NVA altamente disponível][Deploy-NVA].

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre a [análise de plano de dados][Data-Analysis] da configuração do teste e das exibições do recurso de monitoramento de rede do Azure.

Consulte as [Perguntas frequentes sobre o ExpressRoute][ExR-FAQ] para:
-   Saber quantos circuitos do ExpressRoute podem ser conectados a um gateway do ExpressRoute.
-   Saiba quantos gateways do ExpressRoute podem ser conectados a um circuito do ExpressRoute.
-   Saber mais sobre outros limites de escala do ExpressRoute.


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "Perspectiva da topologia de VNet de hub e spoke"
[2]: ./media/backend-interoperability/Loc1ExRView.png "Perspectiva da topologia de VNet remota e Localização 1 via ExpressRoute 1"
[3]: ./media/backend-interoperability/Loc1VPNView.png "Perspectiva da topologia de VNet de branch e Localização 1 via VPN site a site"
[4]: ./media/backend-interoperability/Loc2View.png "Perspectiva da topologia de Localização 2"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "Tabela de rotas do ExpressRoute 1"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering


