---
title: Depurar e analisar serviços do Apache Hadoop com despejos de heap – Azure
description: Colete despejos de heap automaticamente para serviços do Apache Hadoop e coloque dentro a conta de armazenamento de Blobs do Azure para depuração e análise.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 1b4ca22faf8ef01cab4b2e7231fea8ed49f0fcb3
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52494582"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-apache-hadoop-services"></a>Coletar despejos de heap no armazenamento de Blob para depurar e analisar serviços do Apache Hadoop
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Despejos de heap contêm um instantâneo da memória do aplicativo, incluindo os valores das variáveis no momento em que o despejo foi criado. Portanto, eles são úteis para diagnosticar problemas que ocorrem no tempo de execução. Despejos de heap dos serviços [Apache Hadoop](https://hadoop.apache.org/) podem ser coletados automaticamente e colocados na conta de armazenamento de Blob do Azure de um usuário em HDInsightHeapDumps/.

A coleção de despejos de heap para vários serviços deve ser habilitada para serviços em clusters individuais. O padrão para esse recurso deve ser desativado para um cluster. Esses despejos de heap podem ser grandes; portanto, é aconselhável monitorar a conta de armazenamento de Blob na qual eles são salvos após habilitar a coleta.

> [!IMPORTANT]
> O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para obter mais informações, confira [baixa do HDInsight no Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). As informações neste artigo aplicam-se apenas ao HDInsight baseado no Windows. Para obter informações sobre o HDInsight baseado em Linux, consulte [Habilitar despejos de heap para serviços do Apache Hadoop no HDInsight baseado em Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Serviços qualificados para despejos de heap
Você pode habilitar o despejo de heap para os seguintes serviços:

* **Apache hcatalog** -tempelton
* **Hive Apache** -hiveserver2, metastore, derbyserver
* **mapreduce** - jobhistoryserver
* **Yarn Apache** -resourcemanager, nodemanager, timelineserver
* **Apache hdfs** -datanode, secondarynamenode namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Elementos de configuração que habilitam despejos de heap
Para habilitar despejos de heap para um serviço, você precisa definir os elementos de configuração apropriados na seção desse serviço, especificado por **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

O valor de **service_name** pode ser qualquer um dos serviços listados aqui: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode ou namenode.

## <a name="enable-using-azure-powershell"></a>Habilitar usando o Azure PowerShell
Por exemplo, para ativar despejos de heap para jobhistoryserver usando o Azure PowerShell, você pode usar o seguinte script:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Habilitar o uso do SDK do .NET
Por exemplo, para ativar despejos de heap para jobhistoryserver usando o SDK .NET do Azure HDInsight, você pode usar o seguinte código:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
