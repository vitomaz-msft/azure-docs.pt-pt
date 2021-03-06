---
title: Restrições e problemas conhecidos na importação de API de gestão de API do Azure | Documentos da Microsoft
description: Detalhes de problemas conhecidos e restrições à importação no usando os formatos de API aberta, WSDL ou WADL de API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: vlvinogr
editor: ''
ms.assetid: 7a5a63f0-3e72-49d3-a28c-1bb23ab495e2
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: apipm
ms.openlocfilehash: a9f4a4ed4a8771f32a4d66aed2457a43abb92a63
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285878"
---
# <a name="api-import-restrictions-and-known-issues"></a>Restrições de importação de API e problemas conhecidos
## <a name="about-this-list"></a>Sobre esta lista
Ao importar uma API, poderá se deparou com algumas restrições ou identificar problemas que precisam de ser retificados antes de poder importá com êxito. Documentos neste artigo, organizadas pelo formato de importação da API.

## <a name="open-api"> </a>OpenAPI/Swagger
Se estiver a receber erros de importar o seu documento OpenAPI, certifique-se de que validar-o-usando o designer no portal do Azure (Design - Front-End - Editor de especificação de OpenAPI,) ou com uma aplicação de terceiros, tais como de ferramenta <a href="http://editor.swagger.io">Swagger Editor</a>.

* É suportado apenas o formato JSON de OpenAPI.
* Parâmetros obrigatórios no caminho e consulta tem de ter nomes exclusivos. (No OpenAPI um nome de parâmetro apenas tem de ser exclusivo dentro de um local, por exemplo, caminho, consulta e cabeçalho.  No entanto, na gestão de API, podemos permitir operações a ser discriminados por parâmetros de caminho e consulta (que não suporta a OpenAPI). Por conseguinte Exigimos que os nomes de parâmetros de ser exclusivo dentro do modelo de URL completo.)
* Esquemas referenciadas usando **$ref** propriedades não podem conter outros **$ref** propriedades.
* **$ref** ponteiros não podem referenciar arquivos externos.
* **x-ms-caminhos** e **x-servers** são as extensões suportadas apenas.
* Extensões personalizadas são ignoradas ao importar e não são guardadas ou preservadas para exportação.

> [!IMPORTANT]
> Veja este [documento](https://blogs.msdn.microsoft.com/apimanagement/2018/04/11/important-changes-to-openapi-import-and-export/) para obter informações importantes e sugestões relacionadas com a importação de OpenAPI.

## <a name="wsdl"> </a>WSDL
Ficheiros WSDL são utilizados para gerar as APIs de pass-through SOAP ou servir como o back-end de uma API de SOAP para REST.
* **Enlaces de SOAP** -enlaces de apenas SOAP do estilo "documento" e a codificação de "literal" são suportados. Não há suporte para o estilo de "rpc" ou a codificação de SOAP.
* **WSDL:import** -este atributo não é suportado. Os clientes devem mesclar as importações num documento.
* **Mensagens com várias partes** -estes tipos de mensagens não são suportados.
* **WCF wsHttpBinding** -serviços SOAP criados com o Windows Communication Foundation devem utilizar a basicHttpBinding, wsHttpBinding não é suportada.
* **MTOM** - serviços usando o MTOM <em>poderá</em> trabalhar. Oficial de suporte não está disponível neste momento.
* **Recursão** -tipos que são definidos recursivamente (por exemplo, consulte a uma matriz por si mesmos) não são suportadas pelo APIM.

## <a name="wadl"> </a>WADL
Atualmente, não existem não existem problemas de importação WADL conhecidos.
