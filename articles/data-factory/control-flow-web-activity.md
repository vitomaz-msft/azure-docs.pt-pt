---
title: Web atividade de Azure Data Factory | Microsoft Docs
description: Saiba como pode utilizar a atividade de Web, uma das atividades de fluxo de controlo suportadas pela fábrica de dados, para invocar um ponto final de REST de um pipeline.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: shlo
ms.openlocfilehash: 71e89828645cadbbbf60527fca9968fd8ed568ff
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059222"
---
# <a name="web-activity-in-azure-data-factory"></a>Atividade de Web no Azure Data Factory
A atividade Web pode ser utilizada para chamar um ponto final REST personalizado a partir de um pipeline do Data Factory. Pode transmitir conjuntos de dados e serviços ligados aos quais a atividade tem acesso e que pode consumir. 

## <a name="syntax"></a>Sintaxe

```json
{  
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{  
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{  
         "Content-Type":"application/json"
      },
      "authentication":{  
         "type":"ClientCertificate",  
         "pfx":"****",
         "password":"****"
      },
      "datasets":[  
         {  
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{  
               ...
            }
         }
      ],
      "linkedServices":[  
         {  
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Propriedades do tipo

Propriedade | Descrição | Valores permitidos | Necessário
-------- | ----------- | -------------- | --------
nome | Nome da atividade de web | Cadeia | Sim
tipo | Tem de ser definido como **WebActivity**. | Cadeia | Sim
método | Método de REST API para o ponto final de destino. | cadeia. <br/><br/>Tipos suportados: "GET", "Publicar", "Colocar" | Sim
url | Ponto final de destino e o caminho | Cadeia (ou expressão com o resultType da cadeia). A atividade será tempo limite em 1 minuto com um erro se não receber uma resposta do ponto final. | Sim
cabeçalhos | Cabeçalhos que são enviados para o pedido. Por exemplo, para definir o idioma e o tipo de um pedido: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Cadeia (ou expressão com o resultType da cadeia) | Sim, é necessário o cabeçalho Content-type. `"headers":{ "Content-Type":"application/json"}`
corpo | Representa o payload de que é enviado para o ponto final.  | Cadeia (ou expressão com o resultType da cadeia). <br/><br/>Consulte o esquema do payload de pedido no [esquema de payload de pedido](#request-payload-schema) secção. | Necessário para métodos POST/PUT.
autenticação | Método de autenticação utilizado para chamar o ponto final. Os tipos suportados são "Basic ou ClientCertificate." Para obter mais informações, consulte [autenticação](#authentication) secção. Se não for necessária a autenticação, exclua esta propriedade. | Cadeia (ou expressão com o resultType da cadeia) | Não
Conjuntos de dados | Lista de conjuntos de dados transmitido para o ponto final. | Matriz de referências de conjunto de dados. Pode ser uma matriz vazia. | Sim
linkedServices | Lista de serviços ligados transmitido para o ponto final. | Matriz de referências de serviço ligado. Pode ser uma matriz vazia. | Sim

> [!NOTE]
> Pontos finais REST, que invoca a atividade de web tem de devolver uma resposta de tipo de JSON. A atividade será tempo limite em 1 minuto com um erro se não receber uma resposta do ponto final.

A tabela seguinte mostra os requisitos para o conteúdo JSON:

| Tipo de valor | Corpo do pedido | Corpo da resposta |
|---|---|---|
|Objeto JSON | Suportadas | Suportadas |
|Matriz JSON | Suportadas <br/>(Atualmente, matrizes JSON não funcionam em resultado de erros. Uma correção está em curso.) | Não suportado |
| Valor JSON | Suportadas | Não suportado |
| Tipo de JSON não | Não suportado | Não suportado |
||||

## <a name="authentication"></a>Autenticação

### <a name="none"></a>Nenhuma
Se não for necessária a autenticação, não inclua a propriedade de "autenticação".

### <a name="basic"></a>Básica
Especifique o nome de utilizador e palavra-passe para utilizar com a autenticação básica. 

```json
"authentication":{  
   "type":"Basic,
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Certificado de cliente
Especifique com codificação base64 conteúdo de um ficheiro PFX e a palavra-passe. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```
## <a name="request-payload-schema"></a>Esquema de payload de pedido
Quando utiliza o método POST/PUT, a propriedade body representa o payload de que é enviado para o ponto final. Pode passar conjuntos de dados e serviços ligados como parte do payload. Segue-se o esquema para o payload de: 

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
} 
```

## <a name="example"></a>Exemplo
Neste exemplo, a atividade de web no pipeline chama um ponto final REST. Transmite um serviço ligado SQL do Azure e um conjunto de dados SQL do Azure para o ponto final. O ponto final REST utiliza a cadeia de ligação de SQL do Azure para ligar ao servidor SQL do Azure e devolve o nome da instância do SQL server. 

### <a name="pipeline-definition"></a>Definição de pipeline

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>Valores de parâmetros de pipeline

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Código de ponto final de serviço Web

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Passos Seguintes
Outras atividades de fluxo de controlo suportadas pela fábrica de dados, consulte: 

- [Atividade Executar Pipeline](control-flow-execute-pipeline-activity.md)
- [Para cada atividade](control-flow-for-each-activity.md)
- [Atividade Obter Metadados](control-flow-get-metadata-activity.md)
- [Atividade de Pesquisa](control-flow-lookup-activity.md)
