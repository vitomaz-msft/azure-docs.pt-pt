---
title: Copiar dados a partir de um servidor FTP utilizando o Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados de um servidor de FTP para um arquivo de dados dependente suportados através da utilização de uma atividade de cópia no pipeline Azure Data Factory.
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
ms.date: 05/02/2018
ms.author: jingwang
ms.openlocfilehash: 7a3d776acb06d2aa55f71dafb0ddccbc307f394e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37050527"
---
# <a name="copy-data-from-ftp-server-by-using-azure-data-factory"></a>Copiar os dados do servidor FTP utilizando o Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versão 1](v1/data-factory-ftp-connector.md)
> * [Versão atual](connector-ftp.md)

Este artigo descreve como utilizar a atividade de cópia no Azure Data Factory para copiar dados a partir de um servidor FTP. Baseia-se no [copiar descrição geral da atividade](copy-activity-overview.md) artigo que apresenta uma descrição geral da atividade de cópia.

## <a name="supported-capabilities"></a>Capacidades suportadas

Pode copiar dados do servidor FTP para qualquer arquivo de dados suportados sink. Para obter uma lista dos arquivos de dados que são suportados como origens/sinks pela atividade de cópia, consulte o [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats) tabela.

Especificamente, este conector FTP suporta:

- Copiar ficheiros utilizando **básico** ou **anónimo** autenticação.
- Copiar ficheiros como-está ou analisar ficheiros com o [suportado os formatos de ficheiro e compressão codecs](supported-file-formats-and-compression-codecs.md).

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As secções seguintes fornecem detalhes sobre as propriedades que são utilizados para definir entidades do Data Factory específicas para FTP.

## <a name="linked-service-properties"></a>Propriedades de serviço ligado

As seguintes propriedades são suportadas para o serviço FTP ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **FtpServer**. | Sim |
| anfitrião | Especifique o nome ou endereço IP do servidor de FTP. | Sim |
| porta | Especifique a porta em que o servidor FTP está a escutar.<br/>Valores permitidos são: inteiro, valor predefinido é **21**. | Não |
| enableSsl | Especifique se pretende utilizar FTP através de um canal SSL/TLS.<br/>Valores permitidos são: **verdadeiro** (predefinição), **falso**. | Não |
| enableServerCertificateValidation | Especifique se pretende ativar a validação do certificado SSL de servidor quando estiver a utilizar FTP através do canal SSL/TLS.<br/>Valores permitidos são: **verdadeiro** (predefinição), **falso**. | Não |
| authenticationType | Especifique o tipo de autenticação.<br/>Valores permitidos são: **básico**, **anónimo** | Sim |
| userName | Especifique o utilizador que tem acesso ao servidor de FTP. | Não |
| palavra-passe | Especifique a palavra-passe para o utilizador (nome de utilizador). Marcar este campo como um SecureString armazena de forma segura na fábrica de dados, ou [referenciar um segredo armazenado no Cofre de chaves do Azure](store-credentials-in-key-vault.md). | Não |
| connectVia | O [integração Runtime](concepts-integration-runtime.md) para ser utilizado para ligar ao arquivo de dados. Pode utilizar o Runtime de integração do Azure ou o tempo de execução do Self-hosted integração (se o arquivo de dados esteja localizado numa rede privada). Se não for especificado, utiliza a predefinição de Runtime de integração do Azure. |Não |

>[!NOTE]
>O conector FTP suporta o servidor de FTP ao aceder com sem encriptação ou a encriptação de SSL/TLS explícita; não suporta a encriptação de SSL/TLS implícita.

**Exemplo 1: utilizar a autenticação anónima**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Anonymous"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: utilizar a autenticação básica**

```json
{
    "name": "FTPLinkedService",
    "properties": {
        "type": "FtpServer",
        "typeProperties": {
            "host": "<ftp server>",
            "port": 21,
            "enableSsl": true,
            "enableServerCertificateValidation": true,
            "authenticationType": "Basic",
            "userName": "<username>",
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

Para uma lista completa das secções e propriedades disponíveis para definir os conjuntos de dados, consulte o artigo de conjuntos de dados. Esta secção fornece uma lista de propriedades suportadas por conjunto de dados do FTP.

Para copiar dados de FTP, defina a propriedade de tipo do conjunto de dados para **FileShare**. São suportadas as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do conjunto de dados tem de ser definida: **partilha de ficheiros** |Sim |
| folderPath | Caminho para a pasta. Filtro de caráter universal não é suportado. Por exemplo: pasta/subpasta / |Sim |
| fileName | **Filtro de nome ou o caráter universal** para os ficheiros em especificado "folderPath". Se não especificar um valor para esta propriedade, o conjunto de dados aponta para todos os ficheiros na pasta. <br/><br/>Para o filtro, permitidos são de carateres universais: `*` (corresponde a zero ou mais carateres) e `?` (corresponde a zero ou único caráter).<br/>-Exemplo 1: `"fileName": "*.csv"`<br/>-Exemplo 2: `"fileName": "???20180427.txt"`<br/>Utilize `^` para o escape se o nome de ficheiro real tem carateres universais ou este caráter de escape no interior. |Não |
| Formato | Se pretender **copiar ficheiros como-é** entre arquivos baseados em ficheiros (cópia binário), ignorar a secção de formato em ambas as definições do conjunto de dados de entrada e de saída.<br/><br/>Se pretender analisar ficheiros com um formato específico, são suportados os seguintes tipos de formato de ficheiro: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Definir o **tipo** propriedade de formato para um destes valores. Para obter mais informações, consulte [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato Json](supported-file-formats-and-compression-codecs.md#json-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Orc formato](supported-file-formats-and-compression-codecs.md#orc-format), e [Parquet formato](supported-file-formats-and-compression-codecs.md#parquet-format) secções. |Nenhum (apenas para cenário de cópia binário) |
| compressão | Especifique o tipo e o nível de compressão de dados. Para obter mais informações, consulte [suportado os formatos de ficheiro e compressão codecs](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Tipos suportados são: **GZip**, **Deflate**, **BZip2**, e **ZipDeflate**.<br/>Níveis suportados são: **Optimal** e **Fastest**. |Não |
| useBinaryTransfer | Especifique se pretende utilizar o modo de transferência de binários. Os valores são verdadeiras para modo binário (predefinição) e como falso para ASCII. |Não |

>[!TIP]
>Para copiar todos os ficheiros numa pasta, especifique **folderPath** apenas.<br>Para copiar um único ficheiro com um nome específico, especifique **folderPath** com parte de pasta e **fileName** com o nome de ficheiro.<br>Para copiar um subconjunto de ficheiros de uma pasta, especifique **folderPath** com parte de pasta e **fileName** com filtro de caráter universal.

>[!NOTE]
>Se estava a utilizar a propriedade de "fileFilter" para o filtro de ficheiro, ainda é suportado como-é, enquanto são sugeridos para utilizar a nova capacidade de filtro adicionada ao "fileName" passa.

**Exemplo:**

```json
{
    "name": "FTPDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<FTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade Copy

Para uma lista completa das secções e propriedades disponíveis para definir as atividades, consulte o [Pipelines](concepts-pipelines-activities.md) artigo. Esta secção fornece uma lista de propriedades suportado pela origem FTP.

### <a name="ftp-as-source"></a>Como origem de FTP

Para copiar dados de FTP, defina o tipo de origem na atividade de cópia para **FileSystemSource**. As seguintes propriedades são suportadas na atividade de cópia **origem** secção:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo da origem de atividade de cópia tem de ser definida: **FileSystemSource** |Sim |
| recursiva | Indica se os dados é lida a recursivamente partir das pastas sub ou apenas a partir da pasta especificada. Nota Quando recursiva está definida como VERDADEIRO e o sink é baseado em ficheiros de arquivo, vazia pasta/sub-folder não serão copiados/criado no sink.<br/>Valores permitidos são: **verdadeiro** (predefinição), **false** | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromFTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<FTP input dataset name>",
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
                "type": "FileSystemSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md##supported-data-stores-and-formats).
