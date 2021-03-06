---
title: Copiar dados de origens de OData com o Azure Data Factory | Documentos da Microsoft
description: Saiba como copiar dados de origens de OData para arquivos de dados de sink suportado através de uma atividade de cópia num pipeline do Azure Data Factory.
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
ms.date: 05/22/2018
ms.author: jingwang
ms.openlocfilehash: c8bee6902fb74cb77c34395fd05c1c861b4f630e
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166139"
---
# <a name="copy-data-from-an-odata-source-by-using-azure-data-factory"></a>Copiar dados de uma origem de OData com o Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-odata-connector.md)
> * [Versão atual](connector-odata.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados a partir de uma origem de OData. O artigo se baseia no [atividade de cópia no Azure Data Factory](copy-activity-overview.md), que apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados a partir de uma origem de OData para qualquer arquivo de dados de sink suportados. Para obter uma lista de dados armazena se a atividade de cópia suporta como origens e sinks, consulte [arquivos de dados e formatos suportados](copy-activity-overview.md#supported-data-stores-and-formats).

Especificamente, oferece suporte a este conector de OData:

- Versão 3.0 e 4.0 do OData.
- Copiar dados utilizando um dos seguintes autenticações: **anónimo**, **básica**, ou **Windows**.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que pode usar para definir entidades do Data Factory que são específicas para um conector de OData.

## <a name="linked-service-properties"></a>Propriedades do serviço ligado

As seguintes propriedades são suportadas para um serviço ligado de OData:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade tem de ser definida como **OData**. |Sim |
| url | O URL de raiz do serviço OData. |Sim |
| authenticationType | O tipo de autenticação utilizado para ligar à origem de OData. Valores permitidos são **anónimo**, **básica**, e **Windows**. OAuth não é suportado. | Sim |
| userName | Especifique **nome de utilizador** se utilizar autenticação básica ou do Windows. | Não |
| palavra-passe | Especifique **palavra-passe** para o utilizador da conta que especificou para **userName**. Marcar esse campo como um **SecureString** tipo armazena de forma segura no Data Factory. Também pode [referenciar um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Não |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a utilizar para ligar ao arquivo de dados. Pode escolher o Runtime de integração do Azure ou um Runtime de integração autoalojado (se o seu armazenamento de dados está localizado numa rede privada). Se não for especificado, é utilizada a predefinição de Runtime de integração do Azure. |Não |

**Exemplo 1: Utilizar a autenticação anónima**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "http://services.odata.org/OData/OData.svc",
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: Usando a autenticação básica**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of OData source>",
            "authenticationType": "Basic",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 3: Utilizar a autenticação do Windows**

```json
{
    "name": "ODataLinkedService",
    "properties": {
        "type": "OData",
        "typeProperties": {
            "url": "<endpoint of on-premises OData source>",
            "authenticationType": "Windows",
            "userName": "<domain>\\<user>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Esta seção fornece uma lista de propriedades que suporta o conjunto de dados do OData.

Para obter uma lista completa de seções e as propriedades que estão disponíveis para definir conjuntos de dados, consulte [conjuntos de dados e serviços ligados](concepts-datasets-linked-services.md). 

Para copiar dados do OData, defina o **tipo** propriedade do conjunto de dados para **ODataResource**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade do conjunto de dados tem de ser definida como **ODataResource**. | Sim |
| caminho | O caminho para o recurso de OData. | Sim |

**Exemplo**

```json
{
    "name": "ODataDataset",
    "properties":
    {
        "type": "ODataResource",
        "linkedServiceName": {
            "referenceName": "<OData linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties":
        {
            "path": "Products"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade copy

Esta seção fornece uma lista de propriedades que suporta de origem do OData.

Para obter uma lista completa de seções e as propriedades que estão disponíveis para a definição de atividades, consulte [Pipelines](concepts-pipelines-activities.md). 

### <a name="odata-as-source"></a>OData como origem

Para copiar dados do OData, defina o **origem** tipo de atividade de cópia para **RelationalSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | O **tipo** propriedade da origem de atividade de cópia tem de ser definida como **RelationalSource**. | Sim |
| consulta | Opções de consulta de OData para filtrar os dados. Exemplo: `"?$select=Name,Description&$top=5"`.<br/><br/>**Tenha em atenção**: o conector de OData a copia dados de URL combinado: `[URL specified in linked service]/[path specified in dataset][query specified in copy activity source]`. Para obter mais informações, consulte [componentes do URL de OData](http://www.odata.org/documentation/odata-version-3-0/url-conventions/). | Não |

**Exemplo**

```json
"activities":[
    {
        "name": "CopyFromOData",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OData input dataset name>",
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
                "type": "RelationalSource",
                "query": "?$select=Name,Description&$top=5"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-odata"></a>Tipo de dados de mapeamento para OData

Quando copiar dados do OData, são utilizados os seguintes mapeamentos entre tipos de dados do OData e tipos de dados intermediárias do Azure Data Factory. Para saber como atividade de cópia mapeia o tipo de esquema e os dados de origem para o sink, veja [mapeamentos de tipo de esquema e dados](copy-activity-schema-and-type-mapping.md).

| Tipo de dados do OData | Tipo de dados intermediárias de fábrica de dados |
|:--- |:--- |
| Edm.Binary | Byte[] |
| Edm.Boolean | Bool |
| Edm.Byte | Byte[] |
| Edm.DateTime | DateTime |
| Edm.Decimal | decimal |
| Edm.Double | Valor de duplo |
| Edm.Single | Único |
| Edm.Guid | GUID |
| Edm.Int16 | Int16 |
| Edm.Int32 | Int32 |
| Edm.Int64 | Int64 |
| Edm.SByte | Int16 |
| Edm.String | Cadeia |
| Edm.Time | Período de tempo |
| Edm.DateTimeOffset | DateTimeOffset |

> [!NOTE]
> Tipos de dados complexos de OData (por exemplo, **objeto**) não são suportados.


## <a name="next-steps"></a>Passos Seguintes

Para obter uma lista dos arquivos de dados que a atividade de cópia suporta como origens e sinks no Azure Data Factory, veja [arquivos de dados e formatos suportados](copy-activity-overview.md##supported-data-stores-and-formats).