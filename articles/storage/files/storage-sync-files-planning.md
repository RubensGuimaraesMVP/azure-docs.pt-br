---
title: Planejando uma implantação de Sincronização de Arquivos do Azure | Microsoft Docs
description: Saiba o que considerar ao planejar uma implantação de Arquivos do Azure.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 11/26/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 89ab5ecb4e1a6a39e785a51c61e1344631b1f394
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52335173"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Planejando uma implantação da Sincronização de Arquivos do Azure
Use a Sincronização de Arquivos do Azure para centralizar os compartilhamentos de arquivos da sua organização em Arquivos do Azure enquanto mantém a flexibilidade, o desempenho e a compatibilidade de um servidor de arquivos local. A Sincronização de arquivos do Azure transforma o Windows Server em um cache rápido do compartilhamento de arquivos do Azure. Use qualquer protocolo disponível no Windows Server para acessar seus dados localmente, incluindo SMB, NFS e FTPS. Você pode ter tantos caches quantos precisar em todo o mundo.

Este artigo descreve as considerações importantes para uma implantação da Sincronização de Arquivos do Azure. Recomendamos ler também [Planejando uma implantação dos Arquivos do Azure](storage-files-planning.md). 

## <a name="azure-file-sync-terminology"></a>Terminologia da Sincronização de Arquivos do Azure
Antes de entrar nos detalhes do planejamento de uma implantação da Sincronização de Arquivos do Azure, é importante entender a terminologia.

### <a name="storage-sync-service"></a>Serviço de Sincronização de Armazenamento
O Serviço de Sincronização de Armazenamento é o recurso do Azure de nível superior da Sincronização de arquivos do Azure. O recurso Serviço de Sincronização de Armazenamento é um par do recurso de conta de armazenamento e pode, de maneira semelhante, ser implantado em grupos de recursos do Azure. Um recurso de nível superior distinto do recurso de conta de armazenamento é necessário porque o Serviço de Sincronização de Armazenamento pode criar relações de sincronização com várias contas de armazenamento por meio de vários grupos de sincronização. Uma assinatura pode ter vários recursos de Serviço de Sincronização de Armazenamento implantados.

### <a name="sync-group"></a>Grupo de sincronização
Um grupo de sincronização define a topologia de sincronização para um conjunto de arquivos. Os pontos de extremidade em um grupo de sincronização são mantidos em sincronização entre si. Se, por exemplo, você tiver dois conjuntos distintos de arquivos que deseja gerenciar com a Sincronização de arquivos do Azure, crie dois grupos de sincronização e adicione pontos de extremidade diferentes a cada um. Um Serviço de Sincronização de Armazenamento pode hospedar quantos grupos de sincronização forem necessários.  

### <a name="registered-server"></a>Servidor registrado
O objeto de servidor registrado representa uma relação de confiança entre seu servidor (ou cluster) e o Serviço de Sincronização de Armazenamento. Você pode registrar quantos servidores desejar em uma instância do Serviço de Sincronização de Armazenamento. No entanto, um servidor (ou cluster) pode ser registrado apenas em um Serviço de Sincronização de Armazenamento por vez.

### <a name="azure-file-sync-agent"></a>Agente de Sincronização de Arquivo do Azure
O agente de Sincronização de arquivos do Azure é um pacote baixável que permite que o Windows Server seja sincronizado com um compartilhamento de arquivos do Azure. O agente de Sincronização de Arquivos do Azure contém três componentes principais: 
- **FileSyncSvc.exe**: o serviço Windows em segundo plano responsável por monitorar alterações nos pontos de extremidade de Servidor e iniciar as sessões de sincronização com o Azure.
- **StorageSync.sys**: o filtro do sistema de arquivos da Sincronização de Arquivos do Azure, responsável por dispor os arquivos em camadas para os Arquivos do Azure (quando as camadas de nuvem estão habilitadas).
- **Cmdlets de gerenciamento do PowerShell**: cmdlets do PowerShell usados para interagir com o provedor de recursos do Azure Microsoft.StorageSync. Encontre esses cmdlets nos seguintes locais (padrão):
    - C:\Arquivos de Programas\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Arquivos de Programas\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Ponto de extremidade do servidor
Um ponto de extremidade do servidor representa um local específico em um servidor registrado, como uma pasta em um volume do servidor. Vários pontos de extremidade do servidor podem existir no mesmo volume se seus namespaces não forem sobrepostos (por exemplo, `F:\sync1` e `F:\sync2`). Você pode configurar políticas de disposição em camada de nuvem individualmente para cada ponto de extremidade de servidor. 

Você pode criar um ponto de extremidade do servidor por meio de um ponto de montagem. Observe que os pontos de montagem no ponto de extremidade do servidor são ignorados.  

Você pode criar um ponto de extremidade do servidor no volume do sistema, mas haverá duas limitações, se você fizer isso:
* A disposição em camadas de nuvem não pode ser habilitada.
* A restauração rápida de namespace (em que o sistema rapidamente desativa o namespace inteiro e, em seguida, começa a recuperar o conteúdo) não é executada.


> [!Note]  
> Apenas volumes não removíveis são compatíveis.  Não há suporte para unidades mapeadas de um compartilhamento remoto para um caminho de ponto de extremidade do servidor.  Além disso, um ponto de extremidade do servidor pode estar localizado no volume do sistema do Windows, embora a disposição em camadas de nuvem não seja compatível com o volume do sistema.

Se você adicionar uma localização de servidor que tem um conjunto existente de arquivos como um ponto de extremidade de servidor a um grupo de sincronização, esses arquivos serão mesclados com os outros arquivos que já estão em outros pontos de extremidade no grupo de sincronização.

### <a name="cloud-endpoint"></a>Ponto de extremidade da nuvem
Um ponto de extremidade da nuvem é um compartilhamento de arquivos do Azure que faz parte de um grupo de sincronização. Todo o compartilhamento de arquivos do Azure é sincronizado e um compartilhamento de arquivos do Azure pode ser membro de apenas um ponto de extremidade da nuvem. Portanto, um compartilhamento de arquivos do Azure pode ser membro de apenas um grupo de sincronização. Se você adicionar um compartilhamento de arquivos do Azure que tem um conjunto existente de arquivos como um ponto de extremidade da nuvem a um grupo de sincronização, os arquivos existentes serão mesclados com os outros arquivos que já estão em outros pontos de extremidade no grupo de sincronização.

> [!Important]  
> A Sincronização de Arquivos do Azure dá suporte a alterações diretamente no compartilhamento de arquivos do Azure. No entanto, as alterações feitas no compartilhamento de arquivos do Azure precisam primeiro ser descobertas por um trabalho de detecção de alteração da Sincronização de Arquivos do Azure. Um trabalho de detecção de alteração é iniciado para um ponto de extremidade da nuvem apenas uma vez a cada 24 horas. Além disso, as alterações feitas em um compartilhamento de arquivos do Azure através do protocolo REST não atualizarão a hora da última modificação do SMB e não serão vistas como uma alteração de sincronização. Para obter mais informações, consulte [Perguntas frequentes sobre os Arquivos do Azure](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Disposição em camadas de nuvem 
A camada de nuvem é um recurso opcional da Sincronização de Arquivos do Azure em que arquivos acessados frequentemente são armazenados em cache localmente no servidor, enquanto todos os outros arquivos são organizados em camadas para Arquivos do Azure com base nas configurações de política. Confira mais informações em [Noções básicas sobre camadas de nuvem](storage-sync-cloud-tiering.md).

## <a name="azure-file-sync-system-requirements-and-interoperability"></a>Requisitos de sistema e interoperabilidade da Sincronização de Arquivos do Azure 
Esta seção aborda os requisitos de sistema e a interoperabilidade do agente de Sincronização de Arquivos do Azure com soluções de terceiros, funções e recursos do Windows Server.

### <a name="evaluation-tool"></a>Ferramenta de avaliação
Antes de implantar a Sincronização de Arquivos do Azure, você precisa avaliar se ela é compatível com seu sistema usando a ferramenta de avaliação da Sincronização de Arquivos do Azure. Essa ferramenta é um cmdlet do AzureRM PowerShell que verifica se há possíveis problemas com seu sistema de arquivos e conjunto de dados, como caracteres sem suporte ou uma versão de sistema operacional sem suporte. Observe que suas verificações abrangem a maioria dos recursos mencionados abaixo, mas não todos eles. É recomendável que você leia o restante desta seção com cuidado para garantir que sua implantação seja perfeita. 

#### <a name="download-instructions"></a>Instruções de download
1. Certifique-se de que você tem a versão mais recente do PackageManagement e do PowerShellGet instalada (isso permite que você instale módulos na versão prévia)
    
    ```PowerShell
        Install-Module -Name PackageManagement -Repository PSGallery -Force
        Install-Module -Name PowerShellGet -Repository PSGallery -Force
    ```
 
2. Reiniciar o PowerShell
3. Instalar os módulos
    
    ```PowerShell
        Install-Module -Name AzureRM.StorageSync -AllowPrerelease
    ```

#### <a name="usage"></a>Uso  
Você pode invocar a ferramenta de avaliação de algumas maneiras diferentes: você pode executar verificações de sistema, verificações de conjunto de dados ou ambas. Para executar verificações de sistema e de conjunto de dados: 

```PowerShell
    Invoke-AzureRmStorageSyncCompatibilityCheck -Path <path>
```

Para testar apenas o conjunto de dados:
```PowerShell
    Invoke-AzureRmStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Para testar somente os requisitos do sistema:
```PowerShell
    Invoke-AzureRmStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
Para exibir os resultados em CSV:
```PowerShell
    $errors = Invoke-AzureRmStorageSyncCompatibilityCheck […]
    $errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="system-requirements"></a>Requisitos do Sistema
- Um servidor executando o Windows Server 2012 R2, o Windows Server 2016 ou o Windows Server 2019:

    | Versão | SKUs com suporte | Opções de implantação com suporte |
    |---------|----------------|------------------------------|
    | Windows Server 2019 | Datacenter e Standard | Completo (servidor com uma interface do usuário) |
    | Windows Server 2016 | Datacenter e Standard | Completo (servidor com uma interface do usuário) |
    | Windows Server 2012 R2 | Datacenter e Standard | Completo (servidor com uma interface do usuário) |

    Versões futuras do Windows Server serão adicionadas à medida que forem liberadas. Versões anteriores do Windows podem ser adicionadas de acordo com os comentários do usuário.

    > [!Important]  
    > Recomendamos manter todos os servidores usados com a Sincronização de Arquivos do Azure atualizados com as últimas atualizações do Windows Update. 

- Um servidor com um mínimo de 2 GiB de memória.

    > [!Important]  
    > Se o servidor estiver sendo executado em uma máquina virtual com memória dinâmica ativada, a VM deverá ser configurada com um mínimo de 2048 MiB de memória.
    
- Um volume anexado localmente formatado com o sistema de arquivos NTFS.

### <a name="file-system-features"></a>Recursos do sistema de arquivos
| Recurso | Status de suporte | Observações |
|---------|----------------|-------|
| ACLs (listas de controle de acesso) | Com suporte total | As ACLs do Windows são preservadas pela Sincronização de arquivos do Azure e são impostas pelo Windows Server nos pontos de extremidade do servidor. Não há suporte (ainda) para as ACLs do Windows nos Arquivos do Azure, caso os arquivos sejam acessados diretamente na nuvem. |
| Links físicos | Ignorado | |
| Links simbólicos | Ignorado | |
| Pontos de montagem | Com suporte parcial | Pontos de montagem podem ser a raiz de um Ponto de Extremidade de Servidor, mas serão ignorados se estiverem contidos no namespace de um ponto de extremidade de servidor. |
| Junções | Ignorado | Por exemplo, as pastas DFSRoots e DfrsrPrivate do Sistema de Arquivos Distribuído. |
| Pontos de nova análise | Ignorado | |
| Compactação NTFS | Com suporte total | |
| Arquivos esparsos | Com suporte total | Os arquivos esparsos são sincronizados (não são bloqueados), mas são sincronizados com a nuvem como um arquivo completo. Se o conteúdo do arquivo for alterado na nuvem (ou em outro servidor), o arquivo não será mais esparso quando a alteração for baixada. |
| ADS (Fluxos de Dados Alternativos) | Preservados, mas não sincronizados | Por exemplo, as marcas de classificação criadas pela Infraestrutura de classificação de arquivos não são sincronizadas. As marcas de classificação em arquivos existentes em cada um dos pontos de extremidade do servidor são mantidas. |

> [!Note]  
> Há suporte para apenas os volumes NTFS. ReFS, FAT, FAT32 e outros sistemas de arquivos não têm suporte.

### <a name="files-skipped"></a>Arquivos ignorados
| Arquivo/pasta | Observação |
|-|-|
| Desktop.ini | Arquivo específico do sistema |
| ethumbs.db$ | Arquivo temporário para miniaturas |
| ~$\*.\* | Arquivo temporário do Office |
| \*.tmp | Arquivo temporário |
| \*.laccdb | Arquivo de bloqueio do banco de dados do Access|
| 635D02A9D91C401B97884B82B3BCDAEA.* | Arquivo de sincronização interno|
| \\Informações de Volume do Sistema | Pasta específica do volume |
| $RECYCLE.BIN| Pasta |
| \\SyncShareState | Pasta para sincronização |

### <a name="failover-clustering"></a>Clustering de failover
O clustering de failover do Windows Server tem suporte pela Sincronização de Arquivo do Azure para a opção de implantação “Servidor de Arquivos de uso geral”. Não há suporte para o Clustering de Failover em “SOFS (Servidor de Arquivos de Escalabilidade Horizontal) para dados de aplicativos” ou CSVs (Volumes Compartilhados Clusterizados).

> [!Note]  
> O agente de Sincronização de Arquivos do Azure deve ser instalado em cada nó em um Cluster de Failover para a sincronização funcionar corretamente.

### <a name="data-deduplication"></a>Eliminação de duplicação de dados
Para volumes que não têm a disposição em camadas de nuvem habilitada, a Sincronização de Arquivos do Azure dá suporte à Eliminação de Duplicação de Dados do Windows Server habilitada no volume. Atualmente, não damos suporte à interoperabilidade entre a Sincronização de Arquivos do Azure com a disposição em camadas de nuvem habilitada e a Eliminação de Duplicação de Dados.

### <a name="distributed-file-system-dfs"></a>DFS (Sistema de Arquivos Distribuído)
A Sincronização de arquivos do Azure fornece suporte para interoperabilidade com DFS-N (Namespaces de DFS) e DFS-R (Replicação do DFS) iniciando com o [agente 1.2 de Sincronização de arquivos do Azure](https://go.microsoft.com/fwlink/?linkid=864522).

**DFS-N (Namespaces de DFS)**: a Sincronização de arquivos do Azure é totalmente suportada em servidores DFS-N. É possível instalar o agente Sincronização de arquivos do Azure em um ou mais membros DFS-N para sincronizar dados entre os pontos de extremidade de servidor e o ponto de extremidade da nuvem. Para obter mais informações, consulte [visão geral de Namespaces de DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS-R (Replicação do DFS)**: quando tanto DFS-R como a Sincronização de arquivos do Azure forem soluções de replicação, na maioria dos casos é recomendável substituir DFS-R pela Sincronização de arquivos do Azure. No entanto, existem vários cenários em que você deseja usar DFS-R e Sincronização de arquivos do Azure em conjunto:

- Você está migrando de uma implantação de DFS-R para uma implantação de Sincronização de arquivos do Azure. Para obter mais informações, consulte [Migrar uma implantação de DFS-R (Replicação do DFS) para Sincronização de arquivos do Azure](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync).
- Nem todo servidor local que precisa de uma cópia dos dados do seu arquivo pode ser conectado diretamente à internet.
- Os servidor de ramificação consolidam dados em um servidor de hub único, para o qual você gostaria de usar a Sincronização de arquivos do Azure.

Para Sincronização de arquivos do Azure e DFS-R trabalharem lado a lado:

1. A camada de nuvem da Sincronização de arquivos do Azure deve ser desabilitada em volumes com pastas replicadas DFS-R.
2. Os pontos de extremidade de servidor não devem ser configurados em pastas de replicação somente leitura do DFS-R.

Para obter mais informações, consulte [Visão geral da Replicação do DFS](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
Usando o sysprep em um servidor que possua o agente Sincronização de Arquivos do Azure instalado e isso pode levar a resultados inesperados. A instalação do agente e o registro do servidor devem ocorrer depois da implantação da imagem do servidor e da conclusão da mini-instalação do sysprep.

### <a name="windows-search"></a>Windows Search
Se a opção de camadas em nuvem estiver habilitada em um ponto de extremidade do servidor, os arquivos que estão em camadas serão ignorados e não serão indexados pelo Windows Search. Arquivos sem camadas são indexados corretamente.

### <a name="antivirus-solutions"></a>Soluções de antivírus
Como os antivírus funcionam com o exame de arquivos em busca de códigos mal-intencionados conhecidos, um antivírus pode causar o recall de arquivos em camadas. Nas versões 4.0 e acima do agente de Sincronização de Arquivo do Azure, arquivos em camadas têm o conjunto FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS de atributo seguro do Windows. Recomendamos consultar o fornecedor do software para saber como configurar a solução para ignorar a leitura de arquivos com esse conjunto de atributos (muitos fazem isso automaticamente).

As soluções antivírus internas da Microsoft, o Windows Defender e o System Center Endpoint Protection (SCEP), ignoram automaticamente a leitura de arquivos que possuem esse atributo definido. Nós os testamos e identificamos um problema menor: quando você adiciona um servidor a um grupo de sincronização existente, os arquivos com menos de 800 bytes são recuperados (feitos o download) no novo servidor. Esses arquivos permanecerão no novo servidor e não serão colocados em camadas, pois não atendem ao requisito de tamanho em camadas (> 64kb).

### <a name="backup-solutions"></a>Soluções de backup
Como as soluções de antivírus, as soluções de backup podem causar o recall de arquivos em camadas. Recomendamos o uso de uma solução de backup de nuvem para fazer backup do compartilhamento do arquivos do Azure, em vez de um produto de backup local.

Se você estiver usando uma solução de backup local, os backups deverão ser executados em um servidor no grupo de sincronização que possui a camada de nuvem desabilitada. Ao executar uma restauração, use as opções de restauração no nível do volume ou no nível do arquivo. Os arquivos restaurados usando a opção de restauração no nível do arquivo serão sincronizados com todos os pontos de extremidade no grupo de sincronização e os arquivos existentes serão substituídos pela versão restaurada do backup.  As restaurações no nível de volume não substituirão as versões de arquivo mais recentes no compartilhamento de arquivos do Azure ou em outros pontos de extremidade do servidor.

> [!Note]  
> A restauração bare-metal (BMR) pode causar resultados inesperados e não é atualmente suportada.

### <a name="encryption-solutions"></a>Soluções de criptografia
O suporte para soluções de criptografia depende de como elas são implementadas. A Sincronização de Arquivo do Azure é conhecida por funcionar com:

- Criptografia de BitLocker
- Proteção de Informações do Azure, Azure Rights Management Services (Azure RMS) e Active Directory RMS

A Sincronização de Arquivo do Azure é conhecida por não funcionar com:

- EFS (Sistema de Arquivos com Criptografia) NTFS

Em geral, a Sincronização de Arquivos do Azure deve dar suporte à interoperabilidade com soluções de criptografia que ficam abaixo do sistema de arquivos, como o BitLocker, e com soluções implementadas no formato de arquivo, como a Proteção de Informações do Azure. Não foi feita nenhuma interoperabilidade especial para soluções que ficam acima do sistema de arquivos (como NTFS EFS).

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Outras soluções de HSM (Gerenciamento de Armazenamento Hierárquico)
Nenhuma outra solução de HSM deve ser usada com a Sincronização de Arquivos do Azure.

## <a name="region-availability"></a>Disponibilidade de região
A Sincronização de Arquivos do Azure está disponível apenas nas seguintes regiões:

| Região | Localização do Datacenter |
|--------|---------------------|
| Leste da Austrália | Nova Gales do Sul |
| Sudeste da Austrália | Vitória |
| Canadá Central | Toronto |
| Leste do Canadá | Cidade de Quebec |
| Índia Central | Pune |
| Centro dos EUA | Iowa |
| Ásia Oriental | Hong Kong |
| Leste dos EUA | Virgínia |
| Leste dos EUA 2 | Virgínia |
| Centro-Norte dos EUA | Illinois |
| Norte da Europa | Irlanda |
| Centro-Sul dos Estados Unidos | Texas |
| Sul da Índia | Chennai |
| Sudeste Asiático | Singapura |
| Sul do Reino Unido | Londres |
| Oeste do Reino Unido | Cardiff |
| Europa Ocidental | Países Baixos |
| Oeste dos EUA | Califórnia |

A Sincronização de Arquivos do Azure é compatível apenas com um compartilhamento de arquivo do Azure que esteja na mesma região que o Serviço de Sincronização de Armazenamento.

### <a name="azure-disaster-recovery"></a>Recuperação de desastre do Azure
Para proteger-se contra a perda de uma região do Azure, a Sincronização de Arquivos do Azure integra-se com a opção de GRS ([redundância de armazenamento com redundância geográfica](../common/storage-redundancy-grs.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)). O armazenamento GRS funciona usando a replicação de bloco assíncrono entre o armazenamento na região primária, com o qual você normalmente interage, e o armazenamento na região secundária emparelhada. Em caso de desastre que faça uma região do Azure ficar temporária ou permanentemente offline, a Microsoft fará failover do armazenamento para a região emparelhada. 

Para dar suporte à integração de failover entre o armazenamento com redundância geográfica e Sincronização de Arquivos do Azure, todas as regiões de Sincronização de Arquivos do Azure são emparelhadas com uma região secundária que corresponde à região secundária usada pelo armazenamento. Esses pares são os seguintes:

| Região primária      | Região emparelhada      |
|---------------------|--------------------|
| Leste da Austrália      | Sudeste da Austrália |
| Sudeste da Austrália | Leste da Austrália     |
| Canadá Central      | Leste do Canadá        |
| Leste do Canadá         | Canadá Central     |
| Índia Central       | Sul da Índia        |
| Centro dos EUA          | Leste dos EUA 2          |
| Ásia Oriental           | Sudeste Asiático     |
| Leste dos EUA             | Oeste dos EUA            |
| Leste dos EUA 2           | Centro dos EUA         |
| Norte da Europa        | Europa Ocidental        |
| Centro-Norte dos EUA    | Centro-Sul dos Estados Unidos   |
| Sul da Índia         | Índia Central      |
| Sudeste Asiático      | Ásia Oriental          |
| Sul do Reino Unido            | Oeste do Reino Unido            |
| Oeste do Reino Unido             | Sul do Reino Unido           |
| Europa Ocidental         | Norte da Europa       |
| Oeste dos EUA             | Leste dos EUA            |

## <a name="azure-file-sync-agent-update-policy"></a>Política de atualização do agente de Sincronização de Arquivo do Azure
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Próximas etapas
* [Considere as configurações de firewall e proxy](storage-sync-files-firewall-and-proxy.md)
* [Planejando uma implantação de Arquivos do Azure](storage-files-planning.md)
* [Implantar os Arquivos do Azure](storage-files-deployment-guide.md)
* [Implantar a Sincronização de Arquivos do Azure](storage-sync-files-deployment-guide.md)
