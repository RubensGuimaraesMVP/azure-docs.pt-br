---
title: Azure Application Insights – coleta automática de dependência | Microsoft Docs
description: O Application Insights coleta e visualiza dependências automaticamente
services: application-insights
documentationcenter: .net
author: nikmd23
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: reference
ms.date: 08/13/2018
ms.reviewer: mbullwin
ms.author: nimolnar
ms.openlocfilehash: f20963f030c9040b696f7d6a33b25bcee2dc517f
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40130267"
---
# <a name="dependency-auto-collection"></a>Coleta automática de dependência

Veja abaixo a lista atualmente com suporte de chamadas de dependência que são detectadas automaticamente como dependências sem a necessidade de qualquer modificação adicional no código do aplicativo. Isso consiste em chamadas de saída para bibliotecas de comunicação, clientes de armazenamento, log e bibliotecas de métricas, bem como chamadas de entrada para estruturas do aplicativo e servidores. Essas dependências são visualizadas nas exibições [Mapa do aplicativo](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) e [Diagnóstico de transação](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) do Application Insights. Se a dependência não estiver na lista abaixo, você ainda poderá acompanhá-la manualmente com um comando [acompanhar chamada de dependência](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackdependency).

## <a name="net"></a>.NET

| Estruturas do aplicativo| Versões |
| ------------------------|----------|
| Web Forms do ASP.NET | 4.5+ |
| ASP.NET MVC | 4+ |
| API Web ASP.NET | 4.5+ |
| ASP.NET Core | 1.1+ |
| <b> Bibliotecas de comunicação</b> |
| [HttpClient](https://www.microsoft.com/net/) | 4.5+, .NET Core 1.1+ |
| [SqlClient](https://www.nuget.org/packages/System.Data.SqlClient) | .NET Core 1.0+, NuGet 4.3.0 |
| [SDK do Cliente dos Hubs de Eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 |
| [SDK do Cliente do Barramento de Serviço](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) | 3.0.0 |
| <b>Clientes de armazenamento</b>|  |
| ADO.NET | 4.5+ |
| <b>Bibliotecas de log</b> |  |
| ILogger | 1.1+ |
| System.Diagnostics.Trace | 4.5+ |
| [nLog](https://www.nuget.org/packages/NLog/) | 4.4.12+ |
| [log4net](https://www.nuget.org/packages/log4net/) | 2.0.8+ no NetStandard 1.3, 2.0.6+ no .NET 4.5+ |

## <a name="java"></a>Java
| Servidores de aplicativo | Versões |
|-------------|----------|
| [Tomcat](https://tomcat.apache.org/) | 7, 8 | 
| [JBoss EAP](https://developers.redhat.com/products/eap/download/) | 6, 7 |
| [Jetty](http://www.eclipse.org/jetty/) | 9 |
| <b>Estruturas do aplicativo </b> |  |
| [Spring](https://spring.io/) | 3.0 |
| [Spring Boot](https://spring.io/projects/spring-boot) | 1.5.9+<sup>*</sup> |
| Servlet Java | 3.1+ |
| <b>Bibliotecas de comunicação</b> |  |
| [Cliente Apache HTTP](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) | 4.3+<sup>†</sup> |
| <b>Clientes de armazenamento</b> | |
| [SQL Server]( https://mvnrepository.com/artifact/com.microsoft.sqlserver/mssql-jdbc) | 1+<sup>†</sup> |
| [Oracle]( http://www.oracle.com/technetwork/database/application-development/jdbc/downloads/index.html) | 1+<sup>†</sup> |
| [MySql]( https://mvnrepository.com/artifact/mysql/mysql-connector-java) | 1+<sup>†</sup> |
| <b>Bibliotecas de log</b> | |
| [Logback](https://logback.qos.ch/) | 1+ |
| [Log4j](https://logging.apache.org/log4j/) | 1.2+ |
| <b>Bibliotecas de métricas</b> |  |
| JMX | 1.0+ |

> [!NOTE]
> *Exceto o suporte reativo de programação.
> <br>†Exige a instalação do [Agente JVM](https://docs.microsoft.com/azure/application-insights/app-insights-java-agent#install-the-application-insights-agent-for-java).

## <a name="nodejs"></a>Node.js

| Bibliotecas de comunicação | Versões |
| ------------------------|----------|
| [HTTP](https://nodejs.org/api/http.html), [HTTPS](https://nodejs.org/api/https.html) | 0.10+ |
| <b>Clientes de armazenamento</b> | |
| [Redis](https://www.npmjs.com/package/redis) | 2. x |
| [MongoDb](https://www.npmjs.com/package/mongodb); [MongoDb Core](https://www.npmjs.com/package/mongodb-core) | 2.0.0 – 2.3.0 |
| [MySQL](https://www.npmjs.com/package/mysql) | 2.0.0 – 2.14.x |
| [PostgreSql](https://www.npmjs.com/package/pg); | 6.x |
| [pg-pool](https://www.npmjs.com/package/pg-pool) | 1.x |
| <b>Bibliotecas de log</b> | |
| [console](https://nodejs.org/api/console.html) | 0.10+ |
| [Bunyan](https://www.npmjs.com/package/bunyan) | 1.x |
| [Winston](https://www.npmjs.com/package/winston) | 2. x |

## <a name="javascript"></a>JavaScript

| Bibliotecas de comunicação | Versões |
| ------------------------|----------|
| [XMLHttpRequest](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) | Todos |

## <a name="next-steps"></a>Próximas etapas

- Configurar o acompanhamento de dependência personalizado para [.NET](app-insights-asp-net-dependencies.md).
- Configurar o acompanhamento de dependência personalizado para [Java](app-insights-java-agent.md).
- [Escrever telemetria de dependência personalizada](app-insights-api-custom-events-metrics.md#trackdependency)
- Consulte [modelo de dados](application-insights-data-model.md) para modelo de dados e tipos do Application Insights.
- Confira as [plataformas](app-insights-platforms.md) com suporte do Application Insights.
