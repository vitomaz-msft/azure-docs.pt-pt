---
title: Python de exemplo para efetuar a derivação de novas colunas na preparação de dados do Azure Machine Learning | Documentos da Microsoft
description: Este documento fornece exemplos de código do Python para criar novas colunas na preparação de dados do Azure Machine Learning.
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 43a66efde2623a848065bbde2be4b56fd6c7fc8b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969313"
---
# <a name="sample-of-custom-column-transforms-python"></a>Exemplo de transformações de coluna personalizada (Python) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


É o nome dessa transformação no menu **Adicionar coluna (Script)**.

Antes de ler este apêndice, leia [visão geral da extensibilidade de Python](data-prep-python-extensibility-overview.md).

## <a name="test-equivalence-and-replace-values"></a>Testar a equivalência e substituir valores 
Se o valor no Col1 for inferior a 4, em seguida, a nova coluna deve ter um valor de 1. Se o valor Col1 é mais do que 4, a nova coluna tem o valor 2. 

```python
    1 if row["Col1"] < 4 else 2
```
## <a name="current-date-and-time"></a>Data e hora atuais 

```python
    datetime.datetime.now()
```
## <a name="typecasting"></a>Typecasting 
```python
    float(row["Col1"]) / float(row["Col2"] - 1)
```
## <a name="evaluate-for-nullness"></a>Avaliar para nullness 
Se Col1 contiver um valor nulo, em seguida, marcar a nova coluna como **ruim**. Se não estiver, marque-a como **boa.** 

```python
    'Bad' if pd.isnull(row["Col1"]) else 'Good'
```
## <a name="new-computed-column"></a>Nova coluna calculada 
```python
    np.log(row["Col1"])
```
## <a name="epoch-computation"></a>Computação de "Epoch" 
Número de segundos desde a época de Unix (Col1 supondo que já é uma data): 
```python
    row["Col1"] - datetime.datetime.utcfromtimestamp(0)).total_seconds()
```

## <a name="hash-a-column-value-into-a-new-column"></a>Um valor de coluna de hash para uma nova coluna
```python
    import hashlib
    hash(row["MyColumnToHashCol1"])

```




