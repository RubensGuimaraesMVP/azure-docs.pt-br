---
title: Copiar dados do Google AdWords usando o Azure Data Factory (versão prévia) | Microsoft Docs
description: Saiba como copiar dados do Google AdWords para armazenamentos de dados de coletor compatível usando uma atividade de cópia em um pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: jingwang
ms.openlocfilehash: 402d4fb0c1eb7c6760f800bdd408e9a4d8161ccc
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095877"
---
# <a name="copy-data-from-google-adwords-using-azure-data-factory-preview"></a>Copiar dados do Google AdWords usando o Azure Data Factory (versão prévia)

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados do Google AdWords. Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

> [!IMPORTANT]
> Atualmente, esse conector está em versão prévia. Você pode experimentá-lo e oferecer comentários. Se você quiser uma dependência de conectores em versão prévia em sua solução, entre em contato com [suporte do Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados de um Google AdWords para qualquer armazenamento de dados de coletor compatível. Para obter uma lista de armazenamentos de dados com suporte como origens/coletores da atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory fornece um driver interno para habilitar a conectividade, portanto, não é necessário instalar manualmente qualquer driver usando esse conector.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções que se seguem fornecem detalhes sobre as propriedades que são usadas para definir entidades do Data Factory específicas ao conector do Google AdWords.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir são compatíveis com o serviço vinculado do Google AdWords:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade type deve ser definida como **GoogleAdWords** | SIM |
| clientCustomerID | A ID do cliente da conta do AdWords para a qual você deseja buscar dados do relatório.  | SIM |
| developerToken | O token de desenvolvedor associado à conta de gerenciador que você usa para permitir acesso à API do AdWords.  Você pode optar por este campo marcado como uma SecureString para armazená-la com segurança no ADF ou armazene a senha no Azure Key Vault e permitir que o ADF copiar pull de atividade a partir daí, ao executar a cópia de dados - Saiba mais [armazenar credenciais no cofre de chaves](store-credentials-in-key-vault.md). | SIM |
| authenticationType | O mecanismo de autenticação OAuth 2.0 usado para autenticação. ServiceAuthentication só pode ser usado em IR auto-hospedado. <br/>Os valores permitidos são: **ServiceAuthentication**, **UserAuthentication** | SIM |
| refreshToken | O token de atualização obtido do Google para autorizar o acesso ao AdWords para UserAuthentication. Você pode optar por este campo marcado como uma SecureString para armazená-la com segurança no ADF ou armazene a senha no Azure Key Vault e permitir que o ADF copiar pull de atividade a partir daí, ao executar a cópia de dados - Saiba mais [armazenar credenciais no cofre de chaves](store-credentials-in-key-vault.md). | Não  |
| clientId | A ID do cliente do aplicativo Google usado para adquirir o token de atualização. Você pode optar por este campo marcado como uma SecureString para armazená-la com segurança no ADF ou armazene a senha no Azure Key Vault e permitir que o ADF copiar pull de atividade a partir daí, ao executar a cópia de dados - Saiba mais [armazenar credenciais no cofre de chaves](store-credentials-in-key-vault.md). | Não  |
| clientSecret | O segredo do cliente do aplicativo Google usado para adquirir o token de atualização. Você pode optar por este campo marcado como uma SecureString para armazená-la com segurança no ADF ou armazene a senha no Azure Key Vault e permitir que o ADF copiar pull de atividade a partir daí, ao executar a cópia de dados - Saiba mais [armazenar credenciais no cofre de chaves](store-credentials-in-key-vault.md). | Não  |
| email | A ID de e-mail da conta de serviço que é usada para ServiceAuthentication e que só pode ser usada em IR auto-hospedado.  | Não  |
| keyFilePath | O caminho completo para o arquivo chave .p12 que é usado para autenticar o endereço de e-mail da conta de serviço e que só pode ser usado em IR auto-hospedado.  | Não  |
| trustedCertPath | O caminho completo do arquivo .pem que contém certificados de autoridade de certificação confiáveis para verificar o servidor ao se conectar via SSL. Essa propriedade só pode ser definida ao usar o SSL em IR auto-hospedado. O valor padrão é o arquivo de cacerts.pem instalado com o IR.  | Não  |
| useSystemTrustStore | Especifica se deve usar um certificado de autoridade de certificação do repositório de confiança de sistema ou de um arquivo PEM especificado. O valor padrão é falso.  | Não  |

**Exemplo:**

```json
{
    "name": "GoogleAdWordsLinkedService",
    "properties": {
        "type": "GoogleAdWords",
        "typeProperties": {
            "clientCustomerID" : "<clientCustomerID>",
            "developerToken": {
                 "type": "SecureString",
                 "value": "<developerToken>"
            },
            "authenticationType" : "ServiceAuthentication",
            "refreshToken": {
                 "type": "SecureString",
                 "value": "<refreshToken>"
            },
            "clientId": {
                 "type": "SecureString",
                 "value": "<clientId>"
            },
            "clientSecret": {
                 "type": "SecureString",
                 "value": "<clientSecret>"
            },
            "email" : "<email>",
            "keyFilePath" : "<keyFilePath>",
            "trustedCertPath" : "<trustedCertPath>",
            "useSystemTrustStore" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [conjuntos de dados](concepts-datasets-linked-services.md). Esta seção fornece uma lista das propriedades compatíveis com o conjunto de dados do Google AdWords.

Para copiar dados do Google AdWords, defina a propriedade type do conjunto de dados para **GoogleAdWordsObject**. Não há nenhuma propriedade adicional específica do type nesse tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "GoogleAdWordsDataset",
    "properties": {
        "type": "GoogleAdWordsObject",
        "linkedServiceName": {
            "referenceName": "<GoogleAdWords linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades compatível com a origem do Google AdWords.

### <a name="google-adwords-as-source"></a>Google AdWords como origem

Para copiar dados do Google AdWords, defina o tipo de origem na atividade de cópia como **GoogleAdWordsSource**. As propriedades a seguir têm suporte na seção **source** da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade type da origem da atividade de cópia deve ser definida como: **GoogleAdWordsSource** | SIM |
| query | Utiliza a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | SIM |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromGoogleAdWords",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<GoogleAdWords input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "GoogleAdWordsSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Próximas etapas
Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, consulte [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).
