---
title: Habilitar o registro em log de diagnóstico para aplicativos Web no Serviço de Aplicativo do Azure
description: Saiba como habilitar o log de diagnóstico e adicionar instrumentação ao seu aplicativo, bem como acessar as informações registradas pelo Azure.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.openlocfilehash: 8a58f8722b41944a7be02254e0f00682575c1bbb
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636950"
---
# <a name="enable-diagnostics-logging-for-web-apps-in-azure-app-service"></a>Habilitar o registro em log de diagnóstico para aplicativos Web no Serviço de Aplicativo do Azure
## <a name="overview"></a>Visão geral
O Azure oferece diagnóstico integrado para ajudar na depuração de um [aplicativo Web do Serviço de Aplicativo](https://go.microsoft.com/fwlink/?LinkId=529714). Neste artigo, você saberá como habilitar o registro em log de diagnóstico e adicionar instrumentação ao seu aplicativo, bem como acessar as informações registradas pelo Azure.

Este artigo usa o [portal do Azure](https://portal.azure.com) e a CLI do Azure para trabalhar com logs de diagnóstico. Para saber mais sobre como trabalhar com logs de diagnóstico usando o Visual Studio, confira [Solucionando problemas do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="whatisdiag"></a>Diagnóstico de servidor Web e diagnóstico de aplicativos
Os aplicativos Web do Serviço de Aplicativo oferecem funcionalidade de diagnóstico para registro em log tanto de informações do servidor Web quanto do aplicativo Web. Estes estão logicamente separados em **diagnóstico de servidor Web** e **diagnóstico de aplicativos**.

### <a name="web-server-diagnostics"></a>Diagnóstico de servidor Web
Você pode habilitar ou desabilitar os seguintes tipos de logs:

* **Registro em Log Detalhado de Erros** - informações detalhadas de erros para códigos de status HTTP que indiquem uma falha (código de status 400 ou superior). Pode conter informações que podem ajudar a determinar por que o servidor retornou o código de erro.
* **Falha no Rastreamento de Solicitação** - informações detalhadas sobre solicitações com falha, incluindo um rastreamento dos componentes IIS usados para processar a solicitação e o tempo levado em cada componente. É útil se você está tentando melhorar o desempenho do site ou isolar o que está causando o retorno de um erro específico de HTTP.
* **Registro em Log de Servidor Web** - informações sobre transações HTTP usando o [formato de arquivo de log estendido W3C](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). É útil para determinar as métricas gerais do site, como o número de solicitações manipuladas e quantas solicitações existem vindas de um endereço IP específico.

### <a name="application-diagnostics"></a>Diagnóstico de aplicativo
O diagnóstico de aplicativo permite que você capture informações produzidas por um aplicativo da Web. Os aplicativos ASP.NET podem usar a classe [Rastreamento.de.Diagnóstico.de.Sistema](https://msdn.microsoft.com/library/36hhw2t6.aspx) para registrar informações no log de diagnóstico do aplicativo. Por exemplo: 

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Em tempo de execução, você pode recuperar esses logs para ajudar na solução de problemas. Para obter mais informações, consulte [Solucionando problemas de aplicativos Web do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md).

Os aplicativos Web do Serviço de Aplicativo também registram informações de implantação quando você publica o conteúdo em um aplicativo Web. Acontece automaticamente e não há definições de configuração para log de implantação. O log de implantação permite que você determine por que uma implantação falhou. Por exemplo, se está usando um script de implantação personalizado, você poderá usar o log de implantação para determinar por que o script está falhando.

## <a name="enablediag"></a>Como habilitar o diagnóstico
Para habilitar o diagnóstico no [Portal do Azure](https://portal.azure.com), vá até a página de seu aplicativo Web e clique em **Configurações > Logs de diagnóstico**.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Parte de logs](./media/web-sites-enable-diagnostic-log/logspart.png)

Ao habilitar o **diagnóstico de aplicativos**, você também escolhe o **Nível**. Essa configuração permite que você filtre as informações capturadas como **informativas**, de **aviso** ou de **erro**. Configurar para **detalhado** fará o registro de toda informação produzida pelo aplicativo.

> [!NOTE]
> Diferentemente de alterar o arquivo web.config, habilitar o diagnóstico de aplicativos ou alterar os níveis de log do diagnóstico não recicla o domínio do aplicativo em que este é executado.
>
>

Para **Log do aplicativo**, você pode ativar a opção do sistema de arquivos temporariamente para fins de depuração. Esta opção é desativada automaticamente em 12 horas. Você também pode ativar a opção de armazenamento de blob para selecionar um contêiner de blob para gravar logs.

Para **Log do servidor Web**, você pode selecionar **armazenamento** ou **sistema de arquivos**. Selecionar **armazenamento** permite que você selecione uma conta de armazenamento e, em seguida, um contêiner de blob onde os logs estejam gravados. 

Se você armazenar logs no sistema de arquivos, os arquivos poderão ser acessados por FTP ou baixados como um arquivo Zip usando a CLI do Azure.

Por padrão, os logs não são excluídos automaticamente (com exceção do **Log do aplicativo (Filesystem)**). Para excluir automaticamente os logs, defina o campo **Período de retenção (dias)**.

> [!NOTE]
> Se você [regenerar as chaves de acesso de sua conta de armazenamento](../storage/common/storage-create-storage-account.md), será necessário redefinir a respectiva configuração de log para usar as chaves atualizadas. Para fazer isso:
>
> 1. Na guia **Configurar**, defina o respectivo recurso de log como **Desativado**. Salve sua configuração.
> 2. Habilite o registro no blob da conta de armazenamento novamente. Salve sua configuração.
>
>

Qualquer combinação de sistema de arquivos ou armazenamento de blobs pode ser habilitada ao mesmo tempo e ter configurações de nível de log individuais. Por exemplo, você pode desejar gravar erros e avisos no armazenamento de blob como uma solução de log a longo prazo, habilitando o log de sistema de arquivos em um nível detalhado.

Enquanto ambos os locais de armazenamento fornecem as mesmas informações básicas para eventos registrados, o **armazenamento de blobs** registra informações adicionais como a ID da instância, ID do thread e um carimbo de data/hora mais granular (formato de tique) do que o registro no **sistema de arquivos**.

> [!NOTE]
> As informações armazenadas no **armazenamento de blobs** só podem ser acessadas usando um cliente de armazenamento ou um aplicativo que possa trabalhar diretamente com esses sistemas de armazenamento. Por exemplo, o Visual Studio 2013 contém um Gerenciador de Armazenamento que pode ser usado para explorar o armazenamento de blobs, e o HDInsight pode acessar os dados armazenados no armazenamento de blobs. Você também pode gravar um aplicativo que acesse o Armazenamento do Azure usando um dos [SDKs do Azure](https://azure.microsoft.com/downloads/).
>

## <a name="download"></a> Como baixar logs
Informações de diagnóstico armazenadas no sistema de arquivos do aplicativo Web podem ser diretamente acessadas usando FTP. Além disso, pode ser baixado como um arquivo Zip usando a CLI do Azure.

A estrutura de diretórios onde os logs estão armazenados é a seguinte:

* **Logs do aplicativo** - /LogFiles/Application/. Essa pasta contém um ou mais arquivos de texto que contêm informações produzidas pelo log do aplicativo.
* **Rastreamento de Solicitação Falha** - /LogFiles/W3SVC#########/. Esta pasta contém um arquivo XSL e um ou mais arquivos XML. Baixe o arquivo XSL no mesmo diretório que o(s) arquivo(s) XML, pois o arquivo XSL fornece funcionalidade para formatar e filtrar o conteúdo do(s) arquivo(s) XML quando visualizado(s) no Internet Explorer.
* **Logs de erro do aplicativo** - /LogFiles/DetailedErrors/. Esta pasta contém um ou mais arquivos .htm que fornecem informações exaustivas para quaisquer erros de HTTP.
* **Logs do Web Server** - /LogFiles/http/RawLogs. Esta pasta contém um ou mais arquivos de texto que foram formatados usando o [formato W3C estendido de arquivo de log](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx).
* **Logs de implantação** - /LogFiles/Git. Esta pasta contém logs gerados pelo processo interno de implantação usado pelos aplicativos Web do Azure, assim como logs para implantações do Git. Você também pode encontrar os logs de implantação em D:\home\site\deployments.

### <a name="ftp"></a>FTP

Para abrir uma conexão FTP ao servidor FTP do seu aplicativo, consulte [Implantar aplicativo no Serviço de Aplicativo do Azure usando FTP/S](app-service-deploy-ftp.md).

Uma vez conectado ao servidor FTP/S do seu aplicativo web, abra a pasta **LogFiles**, onde os arquivos de log estão armazenados.

### <a name="download-with-azure-cli"></a>Baixar com a CLI do Azure
Para baixar os arquivos de log usando a Interface da Linha de Comando do Azure, abra uma nova sessão de prompt de comando, PowerShell, Bash ou Terminal e insira o seguinte comando:

    az webapp log download --resource-group resourcegroupname --name webappname

Esse comando salva os logs para o aplicativo Web chamado 'webappname' em um arquivo chamado **diagnostics.zip** no diretório atual.

> [!NOTE]
> Se você não instalou a CLI do Azure ou não a configurou para usar sua Assinatura do Azure, consulte [Como usar a CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Como exibir os logs de rastreamento de Java no Application Insights
O Application Insights do Visual Studio fornece ferramentas para filtrar e pesquisar logs e para correlacioná-los com solicitações e outros eventos.

1. Adicionar o SDK do Application Insights ao projeto no Visual Studio.
   * Em Gerenciador de Soluções, clique com o botão direito no seu projeto e escolha Adicionar Application Insights. A interface guia você pelas etapas que incluem a criação de um recurso do Application Insights. [Saiba mais](../application-insights/app-insights-asp-net.md)
2. Adicionar o pacote do Ouvinte de Rastreamento ao seu projeto.
   * Clique com o botão direito do mouse em seu projeto e escolha Gerenciar Pacotes NuGet. Selecione `Microsoft.ApplicationInsights.TraceListener` [Saiba mais](../application-insights/app-insights-asp-net-trace-logs.md)
3. Carregue seu projeto e execute-o para gerar dados de log.
4. No [Portal do Azure](https://portal.azure.com/), navegue até o novo recurso do Application Insights e abra **Pesquisa**. Você verá os dados de log, a solicitação, o uso e outras telemetrias. Algumas telemetrias podem levar alguns minutos para aparecer: clique em Atualizar. [Saiba mais](../application-insights/app-insights-diagnostic-search.md)

[Saiba mais sobre desempenho de rastreamento com o Application Insights](../application-insights/app-insights-azure-web-apps.md)

## <a name="streamlogs"></a> Como transmitir logs
Ao desenvolver um aplicativo, é sempre útil visualizar informações de registro em log realizado em tempo quase real. É possível transmitir informações de registro para o ambiente de desenvolvimento usando a CLI do Azure.

> [!NOTE]
> Alguns tipos de buffer de log gravam no arquivo de log, o que pode resultar em eventos com problemas na transmissão. Por exemplo, uma entrada para log de aplicativo, que ocorre quando um usuário visita uma página, pode ser exibida durante a transmissão antes da entrada de log HTTP correspondente para a solicitação da página.
>
> [!NOTE]
> O streaming de log também transmitirá informações gravadas em qualquer arquivo de texto armazenado na pasta **D:\\home\\LogFiles\\**.
>
>

### <a name="streaming-with-azure-cli"></a>Streaming com CLI do Azure
Para transmitir informações de log, abra uma nova sessão de prompt de comando, PowerShell, Bash ou Terminal e insira o seguinte comando:

    az webapp log tail --name webappname --resource-group myResourceGroup

Isto realiza a conexão ao aplicativo Web chamado 'webappname' e inicia o streaming de informações para a janela, enquanto eventos de log ocorrem no aplicativo Web. Qualquer informação escrita para os arquivos com terminação .txt ou .htm que sejam armazenadas no diretório /LogFiles (d:/home/LogFiles) será transmitida à console local.

Para filtrar eventos específicos como erros, use o parâmetro **-Filtro** . Por exemplo: 

    az webapp log tail --name webappname --resource-group myResourceGroup --filter Error

Para filtrar tipos específicos de log como HTTP, use o parâmetro **-Caminho** . Por exemplo: 

    az webapp log tail --name webappname --resource-group myResourceGroup --path http

> [!NOTE]
> Se você não instalou a CLI do Azure ou não a configurou para usar sua Assinatura do Azure, consulte [Como usar a CLI do Azure](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a> Como compreender os logs de diagnóstico
### <a name="application-diagnostics-logs"></a>Logs de diagnóstico de aplicativo
O diagnóstico de aplicativo armazena informações em um formato específico para aplicativos .NET, dependendo se você armazena logs no sistema de arquivos ou no armazenamento de blobs. 

O conjunto de dados armazenados de base é o mesmo em ambos os tipos de armazenamento - a data e hora em que o evento ocorreu, a ID do processo que produziu o evento, o tipo de evento (informação, aviso ou erro) e a mensagem do evento. Usar o sistema de arquivos para armazenamento de log será útil quando você precisar de acesso imediato para solucionar um problema porque os arquivos de log são atualizados quase instantaneamente. O armazenamento de blobs é usado para fins de arquivamento porque os arquivos são armazenados em cache e, em seguida, liberados para o contêiner de armazenamento em um agendamento.

**Sistema de Arquivos**

Cada linha registrada no sistema de arquivos ou recebida via streaming estará no seguinte formato:

    {Date}  PID[{process ID}] {event type/level} {message}

Por exemplo, um evento de erro deve aparecer semelhante ao seguinte exemplo:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

O registro em sistema de arquivos fornece a informação mais básica dos três métodos disponíveis, fornecendo apenas a hora, o ID do processo, o nível de evento e a mensagem.

**Armazenamento de Blobs**

Ao registrar o log no armazenamento de blob, os dados serão armazenados em um formato de valores separados por vírgulas (CSV). Campos adicionais são registrados para fornecer informações mais detalhadas sobre o evento. As seguintes propriedades são usadas para cada linha no CSV:

| Nome da propriedade | Valor/formato |
| --- | --- |
| Data |A data e hora em que o evento ocorreu |
| Nível |Nível de evento (por exemplo, erro, aviso, informação) |
| ApplicationName |O nome do aplicativo Web |
| InstanceId |A instância do aplicativo Web onde o evento ocorreu |
| EventTickCount |A data e hora em que o evento ocorreu, em formato de escala (maior precisão) |
| EventId |A ID deste evento<p><p>terá como padrão 0 caso nenhuma seja especificada |
| Pid |ID do Processo |
| Tid |A ID do thread que produziu o evento |
| Mensagem |A mensagem com detalhes do evento |

Os dados armazenados em um blob deverão ser semelhantes ao seguinte exemplo:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> Para o ASP.NET Core, o registro é realizado usando o provedor [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) e esse provedor deposita arquivos de log adicionais no contêiner de blob. Para obter mais informações, consulte [Registro do ASP.NET Core no Azure](/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#logging-in-azure).
>
>

### <a name="failed-request-traces"></a>Rastreamento de Solicitação Falha
Os rastreamentos de solicitações com falha são armazenados em arquivos XML chamados **fr######.xml**. Para facilitar a exibição das informações registradas, uma folha de estilos XSL chamada **freb.xsl** é fornecida no mesmo diretório dos arquivos XML. Se você abrir um dos arquivos XML no Internet Explorer, ele usará a folha de estilos XSL para fornecer uma exibição formatada da informação rastreada, similar ao seguinte exemplo:

![solicitação falha visualizada no navegador](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

### <a name="detailed-error-logs"></a>Logs de erro do aplicativo
Logs detalhados de erro são documentos HTML que fornecem informações mais detalhadas sobre erros HTTP que tenham ocorrido. Como são simplesmente documentos HTML, eles podem ser visualizados usando um navegador da Web.

### <a name="web-server-logs"></a>Logs do Web Server
Os logs do servidor da Web são formatados usando o [formato W3C estendido de arquivo de log](https://msdn.microsoft.com/library/windows/desktop/aa814385.aspx). Esta informação pode ser lida usando um editor de texto ou analisada usando ferramentas como o [Analisador de Log](https://go.microsoft.com/fwlink/?LinkId=246619).

> [!NOTE]
> Os logs gerados por aplicativos Web do Azure não dão suporte aos campos **s-computername**, **s-ip** ou **cs-version**.
>
>

## <a name="nextsteps"></a> Próximas etapas
* [Como monitorar aplicativos Web](web-sites-monitor.md)
* [Solucionando problemas de aplicativos Web do Azure no Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md)
* [Analisar logs de aplicativos Web no HDInsight](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)

> [!NOTE]
> Se você deseja começar com o Serviço de Aplicativo do Azure antes de se inscrever em uma conta do Azure, acesse [Experimentar o Serviço de Aplicativo](https://azure.microsoft.com/try/app-service/), em que você pode criar imediatamente um aplicativo Web inicial de curta duração no Serviço de Aplicativo. Nenhum cartão de crédito é exigido, sem compromissos.
>
>
