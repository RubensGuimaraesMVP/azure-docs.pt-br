---
title: Usar o PowerShell para integrar a Central de Segurança do Azure e proteger sua rede | Microsoft Docs
description: Este documento orienta você pelo processo de integração da Central de Segurança do Azure usando os cmdlets do PowerShell.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e400fcbf-f0a8-4e10-b571-5a0d0c3d0c67
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/2/2018
ms.author: rkarlin
ms.openlocfilehash: 650c767d6f8ef495bb19886980b6d45bfe53b32a
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311170"
---
# <a name="automate-onboarding-of-azure-security-center-using-powershell"></a>Automatizar integração da Central de Segurança do Azure usando o PowerShell

Você pode proteger suas cargas de trabalho do Azure programaticamente usando o módulo do PowerShell da Central de Segurança do Azure.
O uso do PowerShell permite automatizar tarefas e evitar o erro humano, que ocorrem com as tarefas manuais. Isso é especialmente útil em implantações em grande escala que envolvem dezenas de assinaturas com centenas e milhares de recursos. Tudo isso deve ser protegido desde o início.

A integração da Central de Segurança do Azure através do PowerShell permite que você automatize programaticamente a integração e o gerenciamento de seus recursos do Azure e adicione os controles de segurança necessários.

Este artigo fornece um exemplo de script do PowerShell que pode ser modificado e usado em seu ambiente para implantar a Central de Segurança em suas assinaturas. 

Neste exemplo, habilitaremos a Central de Segurança em uma assinatura com a ID: d07c0080-170c-4c24-861d-9c817742786c e aplicaremos as configurações recomendadas que fornecem um alto nível de proteção, com a implementação da camada Padrão da Central de Segurança que oferece proteção avançada contra ameaças e recursos de detecção de ameaças:

1. Defina o [nível de proteção ASC padrão](https://azure.microsoft.com/pricing/details/security-center/). 
 
2. Defina a área de trabalho do Log Analytics para a qual o Microsoft Monitoring Agent enviará os dados coletados nas VMs associadas à assinatura. Neste exemplo, uma área de trabalho definida pelo usuário existente (myWorkspace).

3. Ative o provisionamento de agente automático da Central de Segurança que [implanta o Microsoft Monitoring Agent](security-center-enable-data-collection.md#auto-provision-mma).

5. Defina o [CISO da organização como o contato de segurança para alertas do ASC e eventos notáveis](security-center-provide-security-contact-details.md).

6. Atribua as [políticas de segurança padrão da](security-center-azure-policy.md) Central de Segurança.

## <a name="prerequisites"></a>Pré-requisitos

Essas etapas devem ser realizadas antes de executar os cmdlets da Central de Segurança:

1.  Execute o PowerShell como administrador.
2.  Execute os seguintes comandos no PowerShell:
      
        Install-Module -Name PowerShellGet -Force
        Set-ExecutionPolicy -ExecutionPolicy AllSigned
        Import-Module PowerShellGet
6.  Reiniciar o PowerShell

7. No PowerShell, execute os seguintes comandos:

         Install-Module -Name AzureRM.Security -AllowPrerelease -Force

## <a name="onboard-security-center-using-powershell"></a>Integrar a Central de Segurança usando o PowerShell

1.  Registre suas assinaturas no Provedor de recursos da Central de Segurança:

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.Security' 

2.  Opcional: defina o nível de cobertura (tipo de preço) das assinaturas (se não estiver definido, o tipo de preço será definido como Gratuito):

        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
        Set-AzureRmSecurityPricing -Name "default" -PricingTier "Standard"

3.  Configure um espaço de trabalho do Log Analytics no qual os agentes irão reportar. Você deve ter um espaço de trabalho já criado do Log Analytics ao qual as VMs da assinatura se reportarão. Você pode definir várias assinaturas para reportar ao mesmo espaço de trabalho. Se não estiver definido, o espaço de trabalho padrão será usado.

        Set-AzureRmSecurityWorkspaceSetting -Name "default" -Scope
        "/subscriptions/d07c0080-170c-4c24-861d-9c817742786c" -WorkspaceId"/subscriptions/d07c0080-170c-4c24-861d-9c817742786c/resourceGroups/myRg/providers/Microsoft.OperationalInsights/workspaces/myWorkspace"

4.  Instalação do provisionamento automático do Microsoft Monitoring Agent em suas VMs do Azure:
    
        Set-AzureRmContext -Subscription "d07c0080-170c-4c24-861d-9c817742786c"
    
        Set-AzureRmSecurityAutoProvisioningSetting -Name "default" -EnableAutoProvision

    > [!NOTE]
    > É recomendável habilitar o provisionamento automático para garantir que suas máquinas virtuais do Azure sejam automaticamente protegidas pela Central de Segurança do Azure.
    >

5.  Opcional: é altamente recomendável que você defina os detalhes do contato de segurança para as assinaturas que você integrou. Eles serão usados como destinatários de alertas e notificações gerados pela Central de Segurança:

        Set-AzureRmSecurityContact -Name "default1" -Email "CISO@my-org.com" -Phone "2142754038" -AlertsAdmin -NotifyOnAlert 

6.  Atribua a iniciativa de política padrão da Central de Segurança:

        Register-AzureRmResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
        $Policy = Get-AzureRmPolicySetDefinition -Name ' [Preview]: Enable Monitoring in Azure Security Center'
        New-AzureRmPolicyAssignment -Name 'ASC Default <d07c0080-170c-4c24-861d-9c817742786c>' -DisplayName 'Security Center Default <subscription ID>' -PolicySetDefinition $Policy -Scope '/subscriptions/d07c0080-170c-4c24-861d-9c817742786c'

Você já integrou corretamente a Central de Segurança do Azure ao PowerShell!

Agora você pode usar esses cmdlets do PowerShell com scripts de automação para iterar programaticamente entre assinaturas e recursos. Isso economiza tempo e reduz a probabilidade de erro humano. Você pode usar este [exemplo de script](https://github.com/Microsoft/Azure-Security-Center/blob/master/quickstarts/ASC-Samples.ps1) como referência.






## <a name="see-also"></a>Consulte também
Para saber mais sobre como você pode usar o PowerShell para automatizar a integração à Central de Segurança, confira o artigo a seguir:

* [AzureRM.Security](https://www.powershellgallery.com/packages/AzureRM.Security/0.2.0-preview).

Para saber mais sobre a Central de Segurança, confira o seguinte artigo:

* [Configurando políticas de segurança na Central de Segurança do Azure](security-center-azure-policy.md) : saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Gerenciando e respondendo a alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md) : aprenda a gerenciar e a responder a alertas de segurança.
* [Perguntas frequentes da Central de Segurança do Azure](security-center-faq.md) : encontre as perguntas frequentes sobre como usar o serviço.
