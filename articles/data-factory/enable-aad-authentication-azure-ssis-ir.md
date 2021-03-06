---
title: Habilitar a autenticação do Azure Active Directory para o tempo de execução de integração do Azure SSIS | Microsoft Docs
description: Este artigo descreve como configurar o tempo de execução de integração do Azure SSIS para habilitar conexões que usam a autenticação do Azure Active Directory.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: 3c829819748309ecbca248afe35cd59f54b202a6
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085403"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Habilitar a autenticação do Azure Active Directory para o tempo de execução de integração do Azure SSIS

Este artigo mostra como criar um IR do Azure-SSIS com a identidade do serviço do Azure Data Factory. Você pode usar autenticação do Microsoft Azure Active Directory com a identidade gerenciada do Azure Data Factory em vez da autenticação SQL para criar um tempo de execução de integração do Azure-SSIS.

Para obter mais informações sobre a identidade gerenciada para o ADF, consulte [identidade de serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Se você já tiver criado um tempo de execução de integração do Azure-SSIS com a autenticação do SQL, não poderá reconfigurar o IR para usar a autenticação do Azure AD com o PowerShell.

## <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-adf-a-member-of-the-group"></a>Crie um grupo no Azure Active Directory e verifique a identidade gerenciada para o seu ADF, membro de um grupo

Você pode usar um grupo do Azure AD existente ou criar um novo usando o PowerShell do Azure AD.

1.  Instale o módulo do [PowerShell do Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2).

2.  Entre usando `Connect-AzureAD` e execute o comando a seguir para criar o grupo e salvá-lo em uma variável:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    A saída é semelhante ao seguinte exemplo, que também examina o valor da variável:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  Adicione a identidade gerenciada para o seu ADF ao grupo. Você pode seguir a [identidade do serviço do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) para obter a iD da IDENTIDADE DE SERVIÇO (por exemplo, 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, mas não usa a ID DO APLICATIVO DE SERVIÇO para esta finalidade).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Você também pode examinar a associação do grupo posteriormente.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Habilitar o Azure AD no Banco de Dados SQL do Azure

O Banco de Dados SQL do Azure oferece suporte à criação de um banco de dados com um usuário do Azure AD. Como resultado, você pode definir um usuário do Azure AD como o administrador do Active Directory e, em seguida, fazer logon no SSMS com o usuário do Azure AD. Em seguida, crie um usuário independente para o grupo do Microsoft Azure Active Directory, a fim de permitir que o IR crie o catálogo do SSIS (SQL Server Integration Services) no servidor.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Habilitar a autenticação do Azure AD para o Banco de Dados SQL do Azure

Você pode [configurar a autenticação do Azure Active Directory para o Banco de Dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) usando estas etapas:

1.  No portal do Azure, selecione **Todos os serviços** -> **Servidores SQL** na navegação à esquerda.

2.  Selecione o Banco de Dados SQL que poderá usar a autenticação do Azure AD.

3.  Na seção **Configuraçõess** da folha, selecione  **Administrador do Active Directory**.

4.  Na barra de comandos, selecione **Definir administrador**.

5.  Selecione uma conta de usuário do Microsoft Azure Active Directory para ser administradora do servidor e selecione **Selecionar.**

6.  Na barra de comandos, selecione **Salvar.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Crie um usuário contido no banco de dados que representa o grupo do Azure AD

Para esta próxima etapa, você precisará [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Inicie o SQL Server Management Studio.

2.  Na caixa de diálogo **Conectar ao servidor** , insira o nome do servidor SQL no campo **Nome do servidor** .

3.  No campo **Autenticação** , selecione **Active Directory – Universal com suporte MFA**. (Você também pode usar dois outros tipos de autenticação do Active Directory. Confira [Configurar e gerenciar a autenticação do Azure Active Directory com o Banco de Dados SQL, a Instância Gerenciada](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  No campo **nome de usuário** , insira o nome da conta do Azure Active Directory que você definir como o administrador do servidor - por exemplo, testuser@xxxonline.com.

5.  Selecione **Conectar**. Conclua o processo de conexão.

6.  Em  **Pesquisador de Objetos**, expanda a pasta  **Bancos de Dados** -> Bancos de Dados do Sistema.

7.  Use o botão direito para selecionar o banco de dados**mestre** e selecione  **Nova consulta**.

8.  Na janela de consulta, insira a linha a seguir e selecione **Executar** na barra de ferramentas:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    O comando deve ser concluído com êxito, criando o usuário independente para o grupo.

9.  Limpe a janela de consulta, insira a linha a seguir e selecione **Executar** na barra de ferramentas:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    O comando deve ser concluído com êxito, concedendo ao usuário independente a capacidade de criar o banco de dados.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Habilitar o Azure AD na Instância Gerenciada do Banco de Dados SQL do Azure

A Instância Gerenciada do Banco de Dados SQL do Azure não dá suporte à criação de um banco de dados com qualquer usuário do Azure AD que não seja administrador do AD. Como resultado, você deve definir o grupo do Azure AD como administrador do Active Directory. Você não precisa criar o usuário independente.

Você pode [configurar a autenticação do Azure Active Directory para a Instância Gerenciada do Banco de Dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) usando estas etapas:

7.  No portal do Azure, selecione **Todos os serviços** -> **Servidores SQL** na navegação à esquerda.

8.  Selecione o SQL Server a ser habilitado para autenticação do Azure AD.

9.  Na seção **Configuraçõess** da folha, selecione  **Administrador do Active Directory**.

10. Na barra de comandos, selecione **Definir administrador**.

11. Procure e selecione o Grupo do Azure Active Directory (por exemplo, SSISIrGroup) e selecione **Selecionar.**

12. Na barra de comandos, selecione **Salvar.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Provisionar o IR do Azure-SSIS no Portal do Azure

Quando você provisiona o IR do Azure-SSIS Integration Runtime com o portal do Azure, na página **Configurações do SQL**, marque a opção "Usar a autenticação do AAD com a identidade gerenciada para o ADF". (A captura de tela a seguir mostra as configurações do IR com o Banco de Dados SQL do Azure. Para o IR com a instância gerenciada, a propriedade "Camada de serviço de banco de dados do catálogo" não está disponível. outras configurações são as mesmas.)

Para saber mais sobre como criar um tempo de execução de integração do Azure-SSIS, confira [Criar um tempo de execução de integração do Azure-SSIS no Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Configurações do tempo de execução de integração do Azure-SSIS](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Provisionar o IR do Azure-SSIS com o PowerShell

Para provisionar seu IR do Azure-SSIS com o PowerShell, faça o seguinte:

1.  Instale o módulo do [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) .

2.  Em seu script, não defina o parâmetro *CatalogAdminCredential*. Por exemplo: 

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
