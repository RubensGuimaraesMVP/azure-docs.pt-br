---
title: Práticas recomendadas de validação de pilha do Azure | Microsoft Docs
description: Este artigo descreve as práticas recomendadas para usar a validação como um serviço.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: John.Haskin
ms.openlocfilehash: fc2659fb9bbe043a61f1ad49bb4290b7ccf834f8
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422007"
---
# <a name="create-an-oem-package"></a>Criar um pacote de OEM

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

O pacote de extensão de OEM do Azure Stack é o mecanismo pelo qual OEM conteúdo específico é adicionado à infraestrutura do Azure Stack, para uso na implantação, bem como processos operacionais, como atualização, expansão e substituição de campo.

## <a name="creating-the-package"></a>Criação do pacote

Depois de criado e validado, o pacote de extensão do OEM pode ser usado em VaaS.  Antes de continuar, certifique-se de que você tenha concluído as etapas para [criando um pacote do OEM](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true). O pacote, em seguida, é enviado à Microsoft, juntamente com os resultados de teste VaaS para entrar no fluxo de trabalho de validação do pacote. As etapas a seguir detalham como empacotar os arquivos gerados em um único arquivo zip que VaaS pode consumir.

1. Identificar o conteúdo a seguir para o pacote:
    - Um arquivo executável chamado `<Publisher>-<Model>-<Version>.exe`
    - Um ou mais arquivos de arquivos binários chamados `<Publisher><Model>-<Version>-#.bin`, onde # é um número sequencial, começando com 1. O número de arquivos binários é depende do tamanho total do conteúdo do pacote.
    - Um arquivo de manifesto chamado `oemMetadata.xml`, que deve ser idêntico em conteúdo para o arquivo Metadata. XML na raiz do conteúdo do pacote.

2. Selecione os arquivos de conteúdo e criar um arquivo zip do conteúdo:

    ![Compacte o conteúdo do arquivo](media/vaas-create-oem-package-1.png) ![compactar o conteúdo do item](media/vaas-create-oem-package-2.png)

3. Renomeie o arquivo resultante para que ele é descritivo para que você possa identificá-lo.

## <a name="verifying-the-contents"></a>Verificar o conteúdo

Para validar a estrutura do arquivo zip, inspecione-o e verifique se há sem subpastas. O TLD tem o conteúdo compactado. Abaixo está uma estrutura de pacote válido.
> [!IMPORTANT]
> Compactar a pasta pai, em vez do conteúdo causará falha de assinatura de pacote.

![Conteúdo do pacote compactado corretamente](media/vaas-create-oem-package-3.png)

O arquivo zip agora pode ser carregado VaaS e assinado pela Microsoft no fluxo de trabalho de validação do pacote.

## <a name="next-steps"></a>Próximas etapas

- [Validar um pacote de OEM](azure-stack-vaas-validate-oem-package.md)
