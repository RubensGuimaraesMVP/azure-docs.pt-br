---
title: Monitoramento e ajuste de desempenho – Banco de Dados SQL do Azure | Microsoft Docs
description: Dicas de ajuste de desempenho no Banco de Dados SQL por meio de avaliação e melhoria.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 10/23/2018
ms.openlocfilehash: 0d728d81a29c5520938c8553c026727c0f94cc43
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49956996"
---
# <a name="monitoring-and-performance-tuning"></a>Monitoramento e ajuste de desempenho

O Banco de Dados SQL do Azure é um serviço de dados flexível e gerenciado automaticamente no qual você pode facilmente monitorar o uso, adicionar ou remover recursos (CPU, memória, E/S), encontrar recomendações que podem melhorar o desempenho do banco de dados ou permitir que o banco de dados se adapte à carga de trabalho e otimize o desempenho automaticamente.

## <a name="monitoring-database-performance"></a>Monitorando o desempenho do banco de dados

O monitoramento do desempenho de um banco de dados SQL no Azure começa com o monitoramento da utilização de recursos em relação ao nível de desempenho de banco de dados escolhido. O Banco de Dados SQL do Azure permite identificar oportunidades para melhorar e otimizar o desempenho de consultas sem alterar recursos examinando as [recomendações de ajuste de desempenho](sql-database-advisor.md). Índices ausentes e consultas precárias são motivos comuns para um desempenho ruim do banco de dados. Aplique essas recomendações de ajuste para melhorar o desempenho de sua carga de trabalho. Você também pode permitir que o banco de dados SQL do Azure [otimize o desempenho das consultas automaticamente](sql-database-automatic-tuning.md), aplicando todas as recomendações identificadas e verificando se elas melhoram o desempenho do banco de dados.

Você tem as seguintes opções para monitorar e solucionar problemas de desempenho do banco de dados:

- No [portal do Azure](https://portal.azure.com), clique em **Bancos de dados SQL**, selecione o banco de dados e, depois, use o gráfico de Monitoramento para procurar recursos que estão atingindo o limite. O consumo de DTU é exibido por padrão. Clique em **Editar** para alterar o intervalo de tempo e os valores mostrados.
- Use a [Análise de Desempenho de Consultas](sql-database-query-performance.md) para identificar as consultas que gastam a maioria dos recursos.
- Use o [Assistente do Banco de Dados SQL](sql-database-advisor-portal.md) para exibir recomendações para criar e remover índices, parametrizar consultas e corrigir problemas de esquema.
- Use [Insights inteligentes do SQL do Azure](sql-database-intelligent-insights.md) para monitoramento automático do desempenho do banco de dados. Quando um problema de desempenho é detectado, um log de diagnóstico é gerado com detalhes e RCA (Análise da Causa Raiz) do problema. Recomendação de melhoria de desempenho é fornecida quando possível.
- [Habilite o ajuste automático](sql-database-automatic-tuning-enable.md) e permita que o banco de dados SQL do Azure corrija os problemas de desempenho identificados automaticamente.
- Use [exibições de gerenciamento dinâmico (DMVs)](sql-database-monitoring-with-dmvs.md), [eventos estendidos](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr) e a [Loja de Consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) para solucionar problemas mais detalhados de desempenho.

> [!TIP]
> Consulte [orientação de desempenho](sql-database-performance-guidance.md) para encontrar técnicas que você pode usar para melhorar o desempenho do Banco de Dados SQL do Azure depois de identificar o problema de desempenho usando um ou mais dos métodos acima.

## <a name="monitor-databases-using-the-azure-portal"></a>Monitorar bancos de dados usando o Portal do Azure

No [Portal do Azure](https://portal.azure.com/), você pode monitorar a utilização de um único banco de dados selecionando seu banco de dados e clicando no gráfico **Monitoramento**. Isso abre uma janela **Métrica** que pode ser alterada clicando no botão **Editar gráfico**. Adicione as seguintes métricas:

- Percentual de CPU
- Porcentagem de DTU
- Porcentagem de E/S de dados
- Percentual de tamanho do banco de dados

Depois de adicionar essas métricas, você pode continuar a visualizá-las no gráfico **Monitoramento** com mais informações na janela **Métrica**. Todas as quatro métricas mostram o percentual médio de utilização relativo à **DTU** do seu banco de dados. Consulte os artigos [Modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [Modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) para obter mais informações sobre as camadas de serviço.  

![Monitoramento da camada de serviço do desempenho do banco de dados.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

Você também pode configurar alertas nas métricas de desempenho. Clique no botão **Adicionar alerta** na janela **Métrica**. Siga o Assistente para configurar o alerta. Você tem a opção de alerta se a métrica exceder um limite determinado ou se ficar abaixo de um limite determinado.

Por exemplo, se você espera que a carga de trabalho em seu banco de dados cresça, você poderá configurar um alerta por email sempre que seu banco de dados atinge 80% em qualquer uma das métricas de desempenho. Você pode usar isso como um aviso antecipado para decidir quando precisará alternar para o próximo tamanho mais alto de computação.

As métricas de desempenho podem ajudá-lo a determinar se você pode fazer downgrade para um tamanho de computação inferior. Suponha que você está usando um banco de dados Standard S2 e todas as métricas de desempenho mostram que o banco de dados em média não usa mais de 10% a qualquer momento. É provável que o banco de dados funcione bem em Standard S1. No entanto, tome cuidado com cargas de trabalho que apresentam picos ou oscilam antes de tomar a decisão de migrar para um tamanho de computação inferior.

## <a name="troubleshoot-performance-issues"></a>Solucionar problemas de desempenho

Para diagnosticar e resolver problemas de desempenho, comece Noções básicas sobre o estado de cada consulta ativa e as condições que causam problemas de desempenho relevantes para cada estado da carga de trabalho. Para melhorar o desempenho do Banco de Dados SQL do Microsoft Azure, entenda que cada solicitação de consulta ativa do seu aplicativo está em execução ou em um estado de espera. Ao solucionar um problema de desempenho no banco de dados SQL, tenha o gráfico a seguir em mente que você leia este artigo para diagnosticar e resolver problemas de desempenho.

![Estados da carga de trabalho](./media/sql-database-monitor-tune-overview/workload-states.png)

Para uma carga de trabalho com problemas de desempenho, o problema de desempenho pode ser devido à contenção de CPU (uma condição **relacionada à execução)** ou as consultas individuais estão aguardando algo (uma condição**relacionada à espera**).

## <a name="running-related-performance-issues"></a>Problemas de desempenho relacionados à execução

Como diretriz geral, se a utilização da CPU estiver consistentemente ou acima de 80%, você tem um problema de desempenho relacionados à execução. Se você tiver um problema de execução, ele pode ser causado por recursos insuficientes de CPU ou ele pode estar relacionado a uma das seguintes condições:

- Número excessivo de consultas em execução
- Número excessivo de consultas em compilação
- Uma ou mais consultas em execução estão usando um plano de consulta ideal

Se você determinar que você tem um problema de desempenho relacionados à execução, seu objetivo é identificar o problema exato usando um ou mais métodos. Os métodos mais comuns para identificar problemas de execução são:

- Use o [portal do Azure](#monitor-databases-using-the-azure-portal) para monitorar a utilização de percentual de CPU.
- Use as [exibições de gerenciamento dinâmico a seguir](sql-database-monitoring-with-dmvs.md):

  - [sys.DM db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) retorna o consumo de CPU, memória e e/s para um Banco de Dados SQL do Microsoft Azure. Existe uma linha para cada 15 segundos, mesmo se não houver nenhuma atividade no banco de dados. Dados históricos são mantidos por uma hora.
  - [sys. resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) retorna uso de CPU e dados de armazenamento para um Banco de Dados SQL do Microsoft Azure. Os dados são coletados e agregados em intervalos de cinco minutos.

> [!IMPORTANT]
> Para um conjunto de consultas do T-SQL usando esses DMVs para solucionar problemas de utilização da CPU, consulte [Identificar problemas de desempenho da CPU](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

Depois de identificar o problema, você pode ajustar as consultas problemáticas ou atualizar o tamanho do computador ou a camada de serviço para aumentar a capacidade do banco de dados SQL do Azure para absorver os requisitos da CPU. Para obter informações sobre dimensionamento de recursos para bancos de dados únicos, consulte [Recursos de banco de dados único de escala no Banco de Dados SQL do Azure](sql-database-single-database-scale.md) e para dimensionar recursos para conjuntos elásticos, consulte [Recursos do conjunto elástico de escala no Banco de Dados SQL do Azure](sql-database-elastic-pool-scale.md). Para obter informações sobre o dimensionamento de uma instância gerenciada, consulte [Limites de recursos no nível da instância](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).

## <a name="waiting-related-performance-issues"></a>Problemas de desempenho relacionados à espera

Quando tiver certeza de que você não está enfrentando um problema de desempenho relacionado à execução de CPU alto, você está enfrentando um problema de desempenho relacionado à espera. Ou seja, os recursos da CPU não estão sendo usados eficientemente porque a CPU está aguardando algum outro recurso. Nesse caso, o próximo passo é identificar quais recursos da sua CPU estão aguardando. Os métodos mais comuns para mostrar as principais categorias de tipo de espera:

- O [Repositório de Consultas](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store) fornece estatísticas de esperta por consulta ao longo do tempo. No Repositório de Consultas, aguarde os tipos de espera combinados em categorias de espera. O mapeamento das categorias de espera para tipos de espera está disponível em [sys. query_store_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql?view=sql-server-2017#wait-categories-mapping-table).
- [sys.dm_db_wait_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) retorna informações sobre todas as esperas encontradas por conversas executadas durante a operação. Você pode usar essa exibição agregada para diagnosticar problemas de desempenho com o Banco de Dados SQL do Microsoft Azure e também com consultas e lotes específicos.
- [DM os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) retorna informações sobre a fila de espera de tarefas que estão esperando algum recurso.

Conforme mostrado no gráfico anterior, as esperas mais comuns são:

- Bloqueios (bloqueio)
- E/S
- `tempdb`-relacionados de contenção
- Esperas de concessão de memória

> [!IMPORTANT]
> Para um conjunto um consultas T-SQL usando esses DMVs para solucionar esses problemas relacionados a espera, consulte:
>
> - [Identificar problemas de desempenho de e/s](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar `tempdb` problemas de desempenho](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identificar as esperas de concessão de memória](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)

## <a name="improving-database-performance-with-more-resources"></a>Melhorando o desempenho do banco de dados com mais recursos

Por fim, se não houver nenhum item acionável que possa melhorar o desempenho do banco de dados, você poderá alterar a quantidade de recursos disponíveis no Banco de Dados SQL do Azure. É possível atribuir mais recursos alterando a [camada de serviço DTU](sql-database-service-tiers-dtu.md) de um banco de dados individual ou aumentar as eDTUs de um pool elástico a qualquer momento. Como alternativa, se estiver usando o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md), você poderá alterar a camada de serviço ou aumentar os recursos alocados para o banco de dados.

1. Para bancos de dados individuais, é possível [alterar as camadas de serviço](sql-database-service-tiers-dtu.md) ou os [recursos de computação](sql-database-service-tiers-vcore.md) sob demanda para melhorar o desempenho do banco de dados.
2. Para vários bancos de dados, considere o uso de [pools elásticos](sql-database-elastic-pool-guidance.md) para dimensionar os recursos automaticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Ajustar e refatorar o código do aplicativo ou do banco de dados

Você pode alterar o código do aplicativo para usar o banco de dados de forma mais otimizada, alterar índices, forçar planos ou usar dicas para adaptar manualmente o banco de dados à sua carga de trabalho. Encontre algumas diretrizes e dicas para o ajuste manual e a reescrita do código no artigo [tópico de diretrizes de desempenho](sql-database-performance-guidance.md).

## <a name="next-steps"></a>Próximas etapas

- Para habilitar o ajuste automático no Banco de Dados SQL do Azure e permitir que o recurso de ajuste automático gerencie a carga de trabalho por completo, consulte [Habilitar o ajuste automático](sql-database-automatic-tuning-enable.md).
- Para usar o ajuste manual, examine as [Recomendações de ajuste no portal do Azure](sql-database-advisor-portal.md) e aplique manualmente aquelas que melhoram o desempenho de suas consultas.
- Altere os recursos que estão disponíveis no banco de dados alterando as [camadas de serviço do Banco de Dados SQL do Azure](sql-database-performance-guidance.md)
