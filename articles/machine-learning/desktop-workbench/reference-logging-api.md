---
title: Referência da API de registo de ML Azure | Documentos da Microsoft
description: Referência da API de registo.
services: machine-learning
author: akshaya-a
ms.author: akannava
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/25/2017
ROBOTS: NOINDEX
ms.openlocfilehash: f7dac645ef0b732b7f3087a06c74385871d5b1c9
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238341"
---
# <a name="logging-api-reference"></a>Referência da API de registo

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Biblioteca de registo do Azure ML permite que o programa emitir métricas e ficheiros que são controlados pelo serviço do histórico para análise posterior. 

## <a name="uploading-metrics"></a>A carregar métricas

```python
# import logging API package
from azureml.logging import get_azureml_logger

# initialize a logger object
logger = get_azureml_logger()

# log "scalar" metrics
logger.log("simple integer value", 7)
logger.log("simple float value", 3.141592)
logger.log("simple string value", "this is a string metric")

# log a list of numerical values. 
# this automatically creates a chart in the Run History details page
logger.log("chart data points", [1, 3, 5, 10, 6, 4])
```

Por predefinição, todas as métricas são submetidas forma assíncrona, para que o envio não impedir a execução do programa. Isso pode causar problemas de ordenação, quando várias métricas são enviadas em casos extremos. Um exemplo disso seria duas métricas com sessão iniciadas ao mesmo tempo, mas por algum motivo, que o utilizador prefere a ordem exata ser preservados. Outro caso é quando a métrica tem de ser controlada antes de executar um código que é conhecido por falhem rapidamente. Em ambos os casos, a solução é _aguarde_ até que a métrica é completamente registrada antes de continuar:

```python
# blocking call
logger.log("my metric 1", 1).wait()
logger.log("my metric 2", 2).wait()
```

## <a name="consuming-metrics"></a>Métricas de consumo

As métricas são armazenadas pelo serviço do histórico e associadas para a execução que produziu-los. O separador de histórico de execuções e o comando da CLI abaixo permitem-lhe obtê-las (e artefactos abaixo), depois de uma execução estar concluída.

```azurecli
# show the last run
$ az ml history last

# list all past runs
$ az ml history list 

# show a paritcular run
$ az ml history info -r <runid>
```

## <a name="artifacts-files"></a>Artefatos (ficheiros)

Além de métricas, AzureML permite ao utilizador controlar os ficheiros também. Por predefinição, todos os ficheiros escritos no `outputs` pasta relativo ao diretório de trabalho do programa (a pasta de projeto no contexto de computação) são carregados para o serviço de histórico e controlado para análise posterior. A limitação é que o tamanho de arquivo individual tem de ser menor do que 512 MB.


```Python
# Log content as an artifact
logger.upload("artifact/path", "This should be the contents of artifact/path in the service")
```

## <a name="consuming-artifacts"></a>Consumo de artefactos

Para imprimir o conteúdo de um artefato que tem sido controlados, utilizador pode utilizar o separador de histórico de execuções para a execução de determinado para **baixe** ou **promover** o artefacto, ou utilize o abaixo comandos da CLI para conseguir o mesmo efeito.

```azurecli
# show all artifacts generated by a run
$ az ml history info -r <runid> -a <artifact/path>

# promote a particular artifact
$ az ml history promote -r <runid> -ap <artifact/prefix> -n <name of asset to create>
```
## <a name="next-steps"></a>Passos Seguintes
- Percorrer a [classificar íris, tutorial, parte 2](tutorial-classifying-iris-part-2.md) para ver a API de log em ação.
- Revisão [como histórico de execuções de utilização e métricas de modelo no Azure Machine Learning Workbench](how-to-use-run-history-model-metrics.md) para compreender melhor como APIs de Registro pode ser usada no histórico de execuções.
