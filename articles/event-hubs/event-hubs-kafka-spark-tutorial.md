---
title: Conectar seu aplicativo Apache Spark aos Hubs de Eventos do Azure habilitados para Kafka | Microsoft Docs
description: Use o Apache Spark com os Hubs de Eventos do Azure para Kafka.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: tutorial
ms.custom: ''
ms.date: 10/30/2018
ms.author: bahariri
ms.openlocfilehash: a2ab9a77728509b794c2f5b810fb939f6d1a16e3
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51286768"
---
# <a name="connect-your-apache-spark-application-with-kafka-enabled-azure-event-hubs"></a>Conectar seu aplicativo Apache Spark aos Hubs de Eventos do Azure habilitados para Kafka
Este tutorial orienta você sobre como conectar seu aplicativo Spark aos Hubs de Eventos habilitados para Kafka para streaming em tempo real. Essa integração permite streaming sem a necessidade de alterar os clientes de protocolo ou executar seus próprios clusters Kafka ou Zookeeper. Este tutorial requer o Apache Spark v2.4+ e o Apache Kafka v2.0+.

> [!NOTE]
> Este exemplo está disponível no [GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/spark/)

Neste tutorial, você aprenderá como:
> [!div class="checklist"]
> * Criar um namespace de Hubs de Eventos
> * Clonar o projeto de exemplo
> * Executar o Spark
> * Ler dos Hubs de Eventos para Kafka
> * Gravar nos Hubs de Eventos para o Kafka

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar este tutorial, verifique se você possui:
-   Assinatura do Azure. Se você não tiver uma, [crie uma conta gratuita](https://azure.microsoft.com/free/).
-   [Apache Spark v2.4](https://spark.apache.org/downloads.html)
-   [Apache Kafka v2.0]( https://kafka.apache.org/20/documentation.html)
-   [Git](https://www.git-scm.com/downloads)

> [!NOTE]
> O adaptador Spark-Kafka foi atualizado para dar suporte ao Kafka v2.0 a partir do Spark v2.4. Em versões anteriores do Spark, o adaptador dava suporte ao Kafka v0.10 e versões posteriores, mas dependia especificamente das APIs do Kafka v0.10. Como os Hubs de Eventos para Kafka não dão suporte ao Kafka v0.10, os adaptadores Spark-Kafka de versões do Spark anteriores à v2.4 não têm suporte pelos Hubs de Eventos para ecossistemas do Kafka.


## <a name="create-an-event-hubs-namespace"></a>Criar um namespace de Hubs de Eventos
É necessário um namespace do Hubs de Eventos para enviar e receber de qualquer serviço de Hub de Eventos. Consulte [Criando um Hub de Eventos habilitado para Kafka](event-hubs-create.md) para obter instruções sobre como obter um ponto de extremidade Kafka nos Hubs de Eventos. Obtenha a cadeia de conexão dos Hubs de Eventos e o FQDN (nome de domínio totalmente qualificado) para uso posterior. Para obter instruções, confira [Obter uma cadeia de conexão dos Hubs de Eventos](event-hubs-get-connection-string.md). 

## <a name="clone-the-example-project"></a>Clonar o projeto de exemplo
Clone o repositório dos Hubs de Eventos do Azure e navegue até a subpasta `tutorials/spark`:

```bash
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/spark
```

## <a name="read-from-event-hubs-for-kafka"></a>Ler dos Hubs de Eventos para Kafka
Com algumas alterações de configuração, você pode começar a ler dos Hubs de Eventos para Kafka. Atualize **BOOTSTRAP_SERVERS** e **EH_SASL** com detalhes de seu namespace e você poderá iniciar o streaming com os Hubs de Eventos como faria com o Kafka. Para ver o código de exemplo completo, consulte o arquivo sparkConsumer.scala no GitHub. 

```scala
//Read from your Event Hub!
val df = spark.readStream
    .format("kafka")
    .option("subscribe", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("kafka.request.timeout.ms", "60000")
    .option("kafka.session.timeout.ms", "30000")
    .option("kafka.group.id", GROUP_ID)
    .option("failOnDataLoss", "false")
    .load()

//Use dataframe like normal (in this example, write to console)
val df_write = df.writeStream
    .outputMode("append")
    .format("console")
    .start()
```

## <a name="write-to-event-hubs-for-kafka"></a>Gravar nos Hubs de Eventos para o Kafka
Você também pode gravar nos Hubs de Eventos da mesma forma que gravaria no Kafka. Não se esqueça de atualizar sua configuração para alterar **BOOTSTRAP_SERVERS** e **EH_SASL** com informações de seu namespace dos Hubs de Eventos.  Para ver o código de exemplo completo, consulte o arquivo sparkProducer.scala no GitHub. 

```scala
df = /**Dataframe**/

//Write to your Event Hub!
df.writeStream
    .format("kafka")
    .option("topic", TOPIC)
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS)
    .option("kafka.sasl.mechanism", "PLAIN")
    .option("kafka.security.protocol", "SASL_SSL")
    .option("kafka.sasl.jaas.config", EH_SASL)
    .option("checkpointLocation", "./checkpoint")
    .start()
```



## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a transmitir usando o conector Spark-Kafka e os Hubs de Eventos para Kafka. Você seguiu estas etapas: 

> [!div class="checklist"]
> * Criar um namespace de Hubs de Eventos
> * Clonar o projeto de exemplo
> * Executar o Spark
> * Ler dos Hubs de Eventos para Kafka
> * Gravar nos Hubs de Eventos para o Kafka

Para saber mais sobre os Hubs de Eventos e Hubs de Eventos para o Kafka, consulte o tópico a seguir:  

- [Saiba sobre os Hubs de Evento](event-hubs-what-is-event-hubs.md)
- [Hubs de Eventos do Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md)
- [Como criar Hubs de Eventos habilitados para Kafka](event-hubs-create-kafka-enabled.md)
- [Transmitir para Hubs de Eventos a partir de seus aplicativos Kafka](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Espelhar um agente do Kafka em um hub de eventos habilitado para Kafka](event-hubs-kafka-mirror-maker-tutorial.md)
- [Conectar o Apache Flink a um hub de eventos habilitado para Kafka](event-hubs-kafka-flink-tutorial.md)
- [Integrar o Kafka Connect a um hub de eventos habilitado para Kafka](event-hubs-kafka-connect-tutorial.md)
- [Conectar o Akka Streams a um hub de eventos habilitado para Kafka](event-hubs-kafka-akka-streams-tutorial.md)
- [Explorar exemplos em nosso GitHub](https://github.com/Azure/azure-event-hubs-for-kafka)
