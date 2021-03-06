---
title: Referência de linguagem de Monitor Log Analytics do Azure | Documentos da Microsoft
description: Informações de referência de linguagem de consulta do Explorador de dados usada pelo Log Analytics. Inclui elementos adicionais específicos para o Log Analytics e os elementos que não é suportados em consultas do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 65f3388074ff7a84c6e0516a64665019d5883ab1
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913177"
---
# <a name="log-analytics-query-language-differences"></a>Diferenças de linguagem de consulta de análise de registo

Enquanto [do Log Analytics](../log-analytics-queries.md) baseia [Explorador de dados do Azure](/azure//data-explorer) e utiliza o [mesmo idioma de consulta](/azure/kusto/query), a versão da linguagem tem algumas diferenças. Este artigo identifica os elementos que são diferentes entre a versão do idioma usado para o Data Explorer e a versão utilizada para consultas do Log Analytics.

## <a name="data-explorer-elements-not-supported-in-log-analytics"></a>Elementos de Explorador de dados não suportados no Log Analytics
As secções seguintes descrevem os elementos de linguagem de consulta o Data Explorer que não são suportados pelo Log Analytics.

### <a name="statements-not-supported-in-log-analytics"></a>Instruções não são suportadas no Log Analytics

* [Alias](/kusto/query/aliasstatement)
* [Parâmetros de consulta](/azure/kusto/query/queryparametersstatement)

### <a name="functions-not-supported-in-log-analytics"></a>Funções não suportadas no Log Analytics

* [cluster()](/azure/kusto/query/clusterfunction)
* [cursor_after()](/azure/kusto/query/cursorafterfunction)
* [cursor_before_or_at()](/azure/kusto/query/cursorbeforeoratfunction)
* [cursor_current(), current_cursor()](/azure/kusto/query/cursorcurrent)
* [Database()](/azure/kusto/query/databasefunction)
* [current_principal()](/azure/kusto/query/current-principalfunction)
* [extent_id()](/azure/kusto/query/extentidfunction)
* [extent_tags()](/azure/kusto/query/extenttagsfunction)

### <a name="operators-not-supported-in-log-analytics"></a>Operadores não suportadas no Log Analytics

* [Associação cruzada do Cluster](/azure/kusto/query/joincrosscluster)
* [operador de externaldata](/azure/kusto/query/externaldata-operator)

### <a name="plugins-not-supported-in-log-analytics"></a>Plug-ins não suportado no Log Analytics

* [Plug-in do sql_request](/azure/kusto/query/sqlrequestplugin)


## <a name="additional-operators-in-log-analytics"></a>Operadores adicionais no Log Analytics
Os seguintes operadores suportam funcionalidades específicas do Log Analytics e não estão disponíveis fora do Log Analytics.

* [App()](app-expression.md)
* [Workspace()](workspace-expression.md)

## <a name="next-steps"></a>Passos Seguintes

- Obter referências a diferentes [recursos para escrever consultas do Log Analytics](query-language.md).
- Aceder a completa [documentação para a linguagem de consulta do Explorador de dados de referência](/azure/kusto/query/).
