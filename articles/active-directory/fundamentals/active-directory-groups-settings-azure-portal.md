---
title: Como editar as informações do grupo usando o Azure Active Directory | Microsoft Docs
description: Aprenda como editar as informações de um grupo usando o Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731326"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>Como editar as informações do grupo usando o Azure Active Directory

Usando o Azure Active Directory, é possível editar as configurações de um grupo, incluindo a atualização de nome, descrição ou tipo de associação.

## <a name="to-edit-your-group-settings"></a>Para editar as configurações de grupo
1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta de administrador global para o diretório.

2. Selecione **Azure Active Directory** e, em seguida, selecione **Grupos**.

    A página **Grupos - Todos os grupos** é exibida, mostrando todos os grupos ativos.

3. Na página **Grupos - Todos os grupos**, digite o máximo possível do nome do grupo na caixa **Pesquisar**. Para os fins deste artigo, estamos pesquisando o grupo **Política de MDM - Oeste**.

    Os resultados da pesquisa aparecem na caixa **Pesquisar**, que é atualizada na medida em que você digita mais caracteres.

    ![Página Todos os grupos, com texto de pesquisa na caixa Pesquisar](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. Selecione o grupo **Política de MDM - Oeste** e, em seguida, selecione **Propriedades** na área **Gerenciar**.

    ![Página Visão Geral do Grupo com número e membros e opção Membro destacada](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. Atualize as informações **Configurações gerais** conforme necessário, incluindo:

    ![Configurações de propriedades para um grupo](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **Nome do grupo.** Edite o nome do grupo existente.
    
    - **Descrição do grupo.** Edite a descrição do grupo existente.

    - **Tipo de grupo.** Não será possível alterar o tipo de grupo após ter sido criado. Para alterar o **Tipo de grupo**, é necessário excluir o grupo e criar um novo.
    
    - **Tipo de associação.** Altere o tipo de associação. Para obter mais informações sobre os vários tipos de associações disponíveis, consulte [Como criar um grupo básico e adicionar membros usando o portal do Azure Active Directory](active-directory-groups-create-azure-portal.md)
    
    - **ID do objeto.** Não é possível alterar a ID do objeto, mas é possível copiá-la para usar nos comandos do PowerShell para o grupo. Para obter mais informações sobre o uso de cmdlets do PowerShell, consulte [Cmdlets do Azure Active Directory para definir configurações de grupo](../users-groups-roles/groups-settings-v2-cmdlets.md).

## <a name="next-steps"></a>Próximas etapas
Esses artigos fornecem mais informações sobre o Active Directory do Azure.

- [Exibir grupos e membros](active-directory-groups-view-azure-portal.md)

- [Criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md)

- [Como adicionar ou remover membros de um grupo](active-directory-groups-members-azure-portal.md)

- [Gerenciar regras dinâmicas para usuários em um grupo](../users-groups-roles/groups-create-rule.md)

- [Gerenciar associações de um grupo](active-directory-groups-membership-azure-portal.md)

- [Gerenciar acesso a recursos usando grupos](active-directory-manage-groups.md)

- [Associar ou adicionar uma assinatura do Azure ao Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
