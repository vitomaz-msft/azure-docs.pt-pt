---
title: Consultas de pesquisa do Log Analytics | Documentos da Microsoft
description: Este artigo fornece um tutorial para começar a trabalhar escrever consultas de pesquisa do Log Analytics.
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
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 250eddb043ccf9fa0b1bb92a298900f8ad820140
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992274"
---
# <a name="search-queries-in-log-analytics"></a>Consultas de pesquisa do Log Analytics

> [!NOTE]
> Deve efetuar [introdução às consultas no Log Analytics](get-started-queries.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Consultas do Log Analytics do Azure podem começar com um nome de tabela ou um comando de pesquisa. Este tutorial abrange as consultas baseadas em pesquisa. Há vantagens em cada método.

As consultas baseadas em tabela comece pela consulta de âmbito e, portanto, tendem a ser mais eficiente do que as consultas de pesquisa. Consultas de pesquisa são que menos estruturado que torna a melhor escolha quando procurar por um valor específico em colunas ou tabelas. **pesquisa** pode analisar todas as colunas numa determinada tabela ou em todas as tabelas, para o valor especificado. A quantidade de dados a serem processados pode ser enorme, que é a razão pela qual estas consultas podem demorar mais tempo a concluir e podem devolver conjuntos de resultados muito grandes.

## <a name="search-a-term"></a>Um termo de pesquisa
O **pesquisa** comando é normalmente utilizado para procurar um termo específico. No exemplo seguinte, todas as colunas em todas as tabelas são analisadas para o termo "error":

```Kusto
search "error"
| take 100
```

Enquanto eles são fáceis de usar, unscoped consultas como aquele mostrado acima não são eficientes e provavelmente devolver muitos resultados irrelevantes. Uma prática recomendada seria procurar na tabela relevante, ou até mesmo uma coluna de específica.

### <a name="table-scoping"></a>Tabela de âmbito
Para um termo de pesquisa numa tabela específica, adicione `in (table-name)` imediatamente após o **pesquisa** operador:

```Kusto
search in (Event) "error"
| take 100
```

ou em várias tabelas:
```Kusto
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tabela e coluna de âmbito
Por predefinição, **pesquisa** avaliará a todas as colunas no conjunto de dados. Para pesquisar apenas uma coluna de específica, utilize esta sintaxe:

```Kusto
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Se usar `==` em vez de `:`, os resultados incluem registos em que o *origem* coluna tem o valor exato "error", neste caso exato. Usando ":" não irá incluir registos em que *origem* tem valores como "código de erro 404" ou "Erro".

## <a name="case-sensitivity"></a>Sensibilidade
Por predefinição, a pesquisa de termo é maiúsculas de minúsculas, portanto, pesquisar "dns", poderia resultar em resultados como "DNS", "dns" ou "Dns". Para fazer a pesquisa diferenciando maiúsculas de minúsculas, use o `kind` opção:

```Kusto
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Utilizar carateres universais
O **pesquisa** comando suporta carateres universais, no início, fim ou no meio de um período.

Para pesquisar termos que começam por "win":
```Kusto
search in (Event) "win*"
| take 100
```

Para pesquisar termos que terminam com ".com":
```Kusto
search in (Event) "*.com"
| take 100
```

Para pesquisar termos que contêm "www":
```Kusto
search in (Event) "*www*"
| take 100
```

Termos de pesquisa que começa com "corp" e termina em ".com", por exemplo, "corp.mydomain.com" "

```Kusto
search in (Event) "corp*.com"
| take 100
```

Também pode obter tudo numa tabela ao utilizar apenas um caráter universal: `search in (Event) *`, mas isso seria o mesmo que escrever apenas `Event`.

> [!TIP]
> Embora seja possível usar `search *` para obter todas as colunas de cada tabela, recomenda-se que, sempre definir o âmbito suas consultas para tabelas específicas. Consultas unscoped podem demorar algum tempo a concluir e podem devolver demasiados resultados.

## <a name="add-and--or-to-search-queries"></a>Adicione *e* / *ou* para procurar consulta
Uso **e** para procurar registos que contêm vários termos:

```Kusto
search in (Event) "error" and "register"
| take 100
```

Uso **ou** para obter os registos que contêm pelo menos um termos:

```Kusto
search in (Event) "error" or "register"
| take 100
```

Se tiver várias condições de pesquisa, pode combiná-los para a mesma consulta com parênteses:

```Kusto
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

Os resultados deste exemplo seria registos que contenham o termo "error" e também contêm "Registar" ou algo que começa com "marshal".

## <a name="pipe-search-queries"></a>Consultas de pesquisa de pipe
Assim como qualquer outro comando **pesquisa** pode ser enviada por pipe para que os resultados da pesquisa podem ser filtrados, ordenados e agregados. Por exemplo, para obter o número de *evento* registos que contêm "win":

```Kusto
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Passos Seguintes

- Ver mais tutoriais sobre o [site de linguagem de consulta do Log Analytics](https://aka.ms/LogAnalyticsLanguage).