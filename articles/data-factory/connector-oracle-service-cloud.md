---
title: Copiar dados do Oracle Service Cloud usando o Azure Data Factory (versão prévia) | Microsoft Docs
description: Saiba como copiar dados do Oracle Service Cloud para armazenamentos de dados do coletor compatíveis usando uma atividade de cópia em um pipeline do Azure Data Factory.
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
ms.openlocfilehash: a576b94881e114a97e58bf93515e372221da3346
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095880"
---
# <a name="copy-data-from-oracle-service-cloud-using-azure-data-factory-preview"></a>Copiar dados do Oracle Service Cloud usando o Azure Data Factory (versão prévia)

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados do Oracle Service Cloud. Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

> [!IMPORTANT]
> Atualmente, esse conector está em versão prévia. Você pode experimentá-lo e oferecer comentários. Se você quiser uma dependência de conectores em versão prévia em sua solução, entre em contato com [suporte do Azure](https://azure.microsoft.com/support/).

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados de Oracle Service Cloud em qualquer armazenamento de dados compatível do coletor. Para obter uma lista de armazenamentos de dados com suporte como origens/coletores da atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

Azure Data Factory fornece um driver interno para habilitar a conectividade, portanto, não é necessário instalar manualmente qualquer driver usando esse conector.

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem detalhes sobre as propriedades que são usadas para definir entidades do Data Factory específicas ao conector Oracle Service Cloud.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir são compatíveis com o serviço Oracle Service Cloud vinculado:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade type deve ser definida como: **OracleServiceCloud** | SIM |
| host | A URL da instância do Oracle Service Cloud.  | SIM |
| Nome de Usuário | O nome de usuário que você usa para acessar o serviço Oracle Service Cloud.  | SIM |
| Senha | A senha correspondente ao nome de usuário fornecido na chave do nome de usuário. Você pode optar por este campo marcado como uma SecureString para armazená-la com segurança no ADF ou armazene a senha no Azure Key Vault e permitir que o ADF copiar pull de atividade a partir daí, ao executar a cópia de dados - Saiba mais [armazenar credenciais no cofre de chaves](store-credentials-in-key-vault.md). | SIM |
| useEncryptedEndpoints | Especifica se os endpoints de fonte de dados são criptografados usando HTTPS. O valor padrão é true.  | Não  |
| useHostVerification | Especifica se é necessário o nome do host no certificado do servidor para corresponder ao nome de host do servidor ao se conectar via SSL. O valor padrão é true.  | Não  |
| usePeerVerification | Especifica se deve verificar a identidade do servidor quando se conecta por meio de SSL. O valor padrão é true.  | Não  |

**Exemplo:**

```json
{
    "name": "OracleServiceCloudLinkedService",
    "properties": {
        "type": "OracleServiceCloud",
        "typeProperties": {
            "host" : "<host>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [conjuntos de dados](concepts-datasets-linked-services.md). Esta seção fornece uma lista de propriedades compatíveis com o conjunto de dados do Oracle Service Cloud.

Para copiar dados do Oracle Service Cloud e para ele, defina a propriedade type do conjunto de dados como **OracleServiceCloudObject**. Não há nenhuma propriedade adicional específica do type nesse tipo de conjunto de dados.

**Exemplo**

```json
{
    "name": "OracleServiceCloudDataset",
    "properties": {
        "type": "OracleServiceCloudObject",
        "linkedServiceName": {
            "referenceName": "<OracleServiceCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista de propriedades compatível com a fonte do Oracle Service Cloud.

### <a name="oracle-service-cloud-as-source"></a>Oracle Service Cloud como origem

Para copiar dados do Oracle Service Cloud, defina o tipo de origem na atividade de cópia como **OracleServiceCloudSource**. As propriedades a seguir têm suporte na seção **source** da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| Tipo | A propriedade type da origem da atividade de cópia deve ser definida como: **OracleServiceCloudSource** | SIM |
| query | Utiliza a consulta SQL personalizada para ler os dados. Por exemplo: `"SELECT * FROM MyTable"`. | SIM |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromOracleServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OracleServiceCloud input dataset name>",
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
                "type": "OracleServiceCloudSource",
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
