---
title: Otimizar hiperparâmetros para o modelo com o Azure Machine Learning
description: Otimize eficientemente hiperparâmetros para o modelo de aprendizagem profunda de aprendizagem automática / máquina com o serviço Azure Machine Learning. Aprenderá como definir o espaço de pesquisa de parâmetro, especifique uma métrica primária para otimizar e a terminar no início mau desempenho é executado.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: e66dcac1d83c71174ad5d7c3fdcd2310143f8e01
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50140811"
---
# <a name="tune-hyperparameters-for-your-model"></a>Otimizar hiperparâmetros para o modelo

Otimize eficientemente hiperparâmetros para o modelo com o serviço Azure Machine Learning.  A otimização de hiper-parâmetros inclui os seguintes passos:

* Definir o espaço de pesquisa de parâmetro
* Especifique uma métrica primária para otimizar  
* Especificar critérios de cessação antecipada de mau desempenho execuções
* Alocar recursos para a otimização de hiper-parâmetros
* Inicie uma experiência com a configuração acima
* Visualize as execuções de preparação
* Selecionar o melhor desempenho de configuração para o modelo

## <a name="what-are-hyperparameters"></a>Quais são hiperparâmetros?

Hiperparâmetros são parâmetros ajustável que escolher para preparar um modelo que regem o processo de treinamento em si. Por exemplo, para preparar uma rede neural profunda, decidir o número de camadas ocultas na rede e o número de nós em cada camada antes do modelo de formação. Estes valores, normalmente, mantenha-se constante durante o processo de treinamento.

Em cenários de aprendizagem profunda de aprendizagem automática / máquinas, desempenho de modelos depende muito os valores de hiper-parâmetros selecionados. O objetivo de exploração de hiper-parâmetros é pesquisar em várias configurações de hiper-parâmetros para encontrar uma configuração que resulta em melhor desempenho. Normalmente, o processo de exploração de hiper-parâmetros é extremamente manual, uma vez que o espaço de pesquisa é grande e avaliação de cada configuração pode ser cara.

O Azure Machine Learning permite-lhe automatizar hiper-parâmetros exploração de uma forma eficiente, economizando tempo significativo e recursos. Especifique o intervalo de valores de hiper-parâmetros e executa um número máximo de treinamento. O sistema, em seguida, inicia várias execuções simultâneas com configurações de parâmetros diferentes e automaticamente localiza a configuração que resulta em melhor desempenho, medido pela métrica que escolher. Execuções de preparação com mau desempenho são automaticamente antecipadas terminada, a redução de desperdício de recursos de computação. Estes recursos em vez disso, são utilizados para explorar outras configurações de hiper-parâmetros.

>[!NOTE]
> Código neste artigo foi testado com o Azure Machine Learning SDK versão 0.168 

## <a name="define-search-space"></a>Definir o espaço de pesquisa

Ajuste automaticamente hiperparâmetros ao explorar o intervalo de valores definidos para cada hiper-parâmetros.

### <a name="types-of-hyperparameters"></a>Tipos de hiperparâmetros

Cada hiper-parâmetros podem ser discretos ou contínua.

#### <a name="discrete-hyperparameters"></a>Discretos hiperparâmetros 

Hiperparâmetros discretos especificados como uma `choice` entre valores discretos. `choice` pode ser:

* um ou mais valores separados por vírgulas
* um `range` objeto
* qualquer arbitrário `list` objeto


```Python
    {
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

Neste caso, `batch_size` assume a um dos valores [16, 32, 64, 128] e `number_of_hidden_layers` assume um dos valores [1, 2, 3, 4].

Hiperparâmetros discretos avançados também podem ser especificados com uma distribuição. Há suporte para as distribuições seguintes:

* `quniform(low, high, q)` -Devolve um valor como round (uniforme (inferior, superior) / p) * p
* `qloguniform(low, high, q)` -Devolve um valor como round (exp (uniforme (inferior, superior)) / p) * p
* `qnormal(mu, sigma, q)` -Devolve um valor como round (normal (mu, sigma) / p) * p
* `qlognormal(mu, sigma, q)` -Devolve um valor como round (exp (normal (mu, sigma)) / p) * p

#### <a name="continuous-hyperparameters"></a>Hiperparâmetros contínuos 

Hiperparâmetros contínuos são especificados como uma distribuição através de um intervalo contínuo de valores. Distribuições suportadas incluem:

* `uniform(low, high)` -Devolve um valor de distribuídas de maneira uniforme entre baixa e alta
* `loguniform(low, high)` -Devolve um valor desenhado, de acordo com o exp (uniforme (inferior, superior)), para que o logaritmo de valor de retorno é distribuído uniformemente
* `normal(mu, sigma)` -Devolve um valor real que normalmente é distribuído com mean sigma mu e desvio padrão
* `lognormal(mu, sigma)` -Devolve um valor desenhado, de acordo com o exp (normal (mu, sigma)), para que o logaritmo de valor de retorno é normalmente distribuído

Um exemplo de uma definição de espaço de parâmetro:

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Esse código define um espaço de pesquisa com dois parâmetros - `learning_rate` e `keep_probability`. `learning_rate` tem uma distribuição normal com o valor médio 10 e um desvio-padrão de 3. `keep_probability` tem uma distribuição uniforme com um valor mínimo de 0,05 e um valor máximo de 0,1.

### <a name="sampling-the-hyperparameter-space"></a>O espaço de hiper-parâmetros de amostragem

Também pode especificar o método de amostragem de parâmetro a utilizar a definição de espaço de hiper-parâmetros. O serviço do Azure Machine Learning suporta amostragem aleatória, a amostragem de grade e Bayesianos amostragem.

#### <a name="random-sampling"></a>Amostragem aleatória

Numa amostragem aleatória, os valores de hiper-parâmetros são selecionadas aleatoriamente entre o espaço de pesquisa definido. Amostragem aleatória permite que o espaço de pesquisa incluir hiperparâmetros discretos e contínuos.

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Amostragem de grelha

Amostragem de grade executa uma pesquisa de grade simples ao longo de todos os valores viáveis no espaço de pesquisa definido. Só pode ser utilizado com especificados por meio de hiperparâmetros `choice`. Por exemplo, o seguinte espaço tem um total de seis amostras:

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Amostragem de Bayesianos

Amostragem de Bayesianos baseia-se no algoritmo de otimização Bayesianos e faz escolhas inteligentes os valores de hiper-parâmetros para o exemplo a seguir. Ele escolhe o exemplo com base em como os exemplos anteriores efetuada, como, por exemplo que o novo exemplo melhora a métrica primária comunicada.

Quando utiliza Bayesianos amostragem, o número de execuções simultâneas tem um impacto sobre a eficácia do processo de otimização. Normalmente, um número menor de execuções simultâneas pode levar a convergência de amostragem melhor, uma vez que o menor grau de paralelismo aumenta o número de execuções que tiram partido de execuções concluídas anteriormente.

Amostragem de Bayesianos só suporta `choice` e `uniform` distribuições sobre o espaço de pesquisa. 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Amostragem de Bayesianos não suporta qualquer política de cessação antecipada (consulte [especificar uma política de cessação antecipada](#specify-early-termination-policy)). Ao utilizar a amostragem de parâmetro Bayesianos, defina `early_termination_policy = None`, ou deixe de fora o `early_termination_policy` parâmetro.

<a name='specify-primary-metric-to-optimize'/>

## <a name="specify-primary-metric"></a>Especifique a métrica primária

Especifique a métrica principal que pretende que a experimentação para otimizar a otimização de hiper-parâmetros. Cada execução de treinamento é avaliada para a métrica primária. Mau desempenho execuções (em que a métrica primária não cumpre os critérios definidos pela política de cessação antecipada) será terminada. Além do nome de métrica principal, também especificar o objetivo da otimização - se maximizar ou minimizar a métrica primária.

* `primary_metric_name`: O nome da métrica primário para otimizar. O nome da métrica primário tem de corresponder exatamente ao nome da métrica registado pelo script de treinamento. Ver [iniciar sessão métricas para a otimização de hiper-parâmetros](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: Pode ser qualquer um `PrimaryMetricGoal.MAXIMIZE` ou `PrimaryMetricGoal.MINIMIZE` e determina se a métrica primária será maximizada ou minimizada ao avaliar as execuções. 

```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```

Otimize as execuções para maximizar a "precisão".  Lembre-se de que este valor de registo no seu script de treinamento.

<a name='log-metrics-for-hyperparameter-tuning'/>

### <a name="log-metrics-for-hyperparameter-tuning"></a>Métricas de registo para a otimização de hiper-parâmetros

O script de treinamento para o seu modelo tem de iniciar as métricas relevantes durante a preparação de modelos. Ao configurar a otimização de hiper-parâmetros, especifique a métrica primária a utilizar para avaliar o desempenho de execução. (Consulte [especifique uma métrica primária para otimizar](#specify-primary-metric-to-optimize).)  No seu script de treinamento, tem de iniciar esta métrica, de modo está disponível para o processo de otimização de hiper-parâmetros.

Esta métrica de registo no seu script de treinamento com o seguinte fragmento de exemplo:

```Python
from azureml.core.run import Run
run_logger = Run.get_context()
run_logger.log("accuracy", float(val_accuracy))
```

O script de treinamento calcula o `val_accuracy` e regista-lo como "precisão", que é utilizada como a métrica primária. Sempre que a métrica é registada é recebida, o serviço de otimização de hiper-parâmetros. Cabe ao desenvolvedor de modelo para determinar a frequência de relatório esta métrica.

<a name='specify-early-termination-policy'/>

## <a name="specify-early-termination-policy"></a>Especificar Política de cessação antecipada

Termine mau desempenho é executado automaticamente com uma política de cessação antecipada. Cessação reduz o desperdício de recursos e em vez disso, utiliza estes recursos para explorar outras configurações de parâmetro.

Quando utilizar uma política de cessação antecipada, pode configurar os seguintes parâmetros que controlam quando uma política é aplicada:

* `evaluation_interval`: a frequência para aplicar a política. Sempre que o script de treinamento registos a métrica primária é contabilizado como um intervalo. Portanto, um `evaluation_interval` 1 aplicará a diretiva sempre que o script de treinamento reporta a métrica primária. Um `evaluation_interval` de 2 irá aplicar a política de todas as outras vezes o script de treinamento reporta a métrica primária. Se não for especificado, `evaluation_interval` está definido como 1, por predefinição.
* `delay_evaluation`: atrasa a primeira avaliação de política para um número especificado de intervalos. É um parâmetro opcional que permite que todas as configurações ser executado por um número mínimo inicial de intervalos, evitando o encerramento prematuro de treinamento é executado. Se for especificado, a política aplica-se cada múltiplo de evaluation_interval é maior que ou igual a delay_evaluation.

Serviço de Machine Learning do Azure suporta as seguintes políticas de cessação antecipada.

### <a name="bandit-policy"></a>Política de bandit

Bandit é uma política de terminação com base num intervalo de quantidade e a avaliação do slack fator/slack. A política desde o início termina todas as execuções em que a métrica de principal não está no fator slack especificado / run do slack quantidade em relação ao melhor desempenho de treinamento. Ele usa os seguintes parâmetros de configuração:

* `slack_factor` ou `slack_amount`: slack permitido em relação ao melhor desempenho de treinamento ser executado. `slack_factor` Especifica o slack permitido como um rácio. `slack_amount` Especifica o slack permitido como um valor absoluto, em vez de uma taxa.

    Por exemplo, considere uma política de Bandit a ser aplicada neste intervalo de 10. Partem do princípio de que a execução melhor executar no intervalo de 10 comunicado uma métrica primária 0,8 com o objetivo de maximizar a métrica primária. Se a política foi especificada com um `slack_factor` de 0,2, é executada qualquer treinamento, cuja melhor métrica no intervalo de 10 é menor que 0.66 (0,8 / (1 +`slack_factor`)) será terminada. Se em vez disso, a política foi especificada com um `slack_amount` de 0,2, é executada qualquer treinamento, cuja melhor métrica no intervalo de 10 é menor que 0.6 (0,8 - `slack_amount`) será terminada.
* `evaluation_interval`: a frequência para aplicar a política (parâmetro opcional).
* `delay_evaluation`: atrasa a primeira avaliação de política para um número especificado de intervalos (parâmetro opcional).


```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

Neste exemplo, é aplicada a política de cessação antecipada em cada intervalo quando métricas são comunicadas, começando no intervalo de avaliação de 5. Qualquer execução cuja melhor métrica é menor que (1/(1+0.1) ou 91% do melhor desempenho será execução terminada.

### <a name="median-stopping-policy"></a>Política de parar mediano

A parar mediana é uma política de cessação antecipada, com base em médias de métricas principais comunicadas pelas execuções de execução. Esta política computa médias em execução em todas as execuções de preparação e termina execuções cujo desempenho é pior do que o valor mediano das médias em execução. Esta política tem os seguintes parâmetros de configuração:
* `evaluation_interval`: a frequência para aplicar a política (parâmetro opcional).
* `delay_evaluation`: atrasa a primeira avaliação de política para um número especificado de intervalos (parâmetro opcional).


```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

Neste exemplo, é aplicada a política de cessação antecipada em cada intervalo que começa no intervalo de avaliação de 5. Uma execução será terminada em intervalo 5 se sobre intervalos 1:5, sua melhor métrica primária é pior do que o valor mediano das médias em execução em todas as execuções de preparação.

### <a name="truncation-selection-policy"></a>Política de seleção de truncamento

Cancela a seleção truncamento que uma determinada percentagem da execução mais baixa é executada a cada intervalo de avaliação. Execuções são comparadas com base no respetivo desempenho na métrica primária e a mais baixa X % está terminada. Ele usa os seguintes parâmetros de configuração:

* `truncation_percentage`: a percentagem da execução mais baixa é executada para terminar a cada intervalo de avaliação. Especifique um valor inteiro entre 1 e 99.
* `evaluation_interval`: a frequência para aplicar a política (parâmetro opcional).
* `delay_evaluation`: atrasa a primeira avaliação de política para um número especificado de intervalos (parâmetro opcional).


```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

Neste exemplo, é aplicada a política de cessação antecipada em cada intervalo que começa no intervalo de avaliação de 5. Uma execução será terminada no intervalo de 5, se estiver seu desempenho no intervalo de 5 a 20% mais baixo de desempenho de todas as execuções no intervalo de 5.

### <a name="no-termination-policy"></a>Nenhuma política de terminação

Se quiser que todas as execuções de preparação para conclusão, defina a política como None. Isso terá o efeito de não aplicar qualquer política de cessação antecipada.

```Python
policy=None
```

### <a name="default-policy"></a>Política predefinida

Não se for especificada nenhuma política, o serviço de otimização de hiper-parâmetros permitirá que todas as execuções de preparação realizada até à conclusão.

>[!NOTE] 
>Se estiver procurando por uma política conservador que proporciona poupanças sem terminar tarefas promissoras, pode utilizar uma política de parar de mediana com `evaluation_interval` 1 e `delay_evaluation` 5. Estas são definições conservadoras, que podem fornecer aproximadamente 25% - 35% de poupanças sem perda em métrica primária (com base nos nossos dados de avaliação).

## <a name="allocate-resources"></a>Alocar recursos

Controle o orçamento de recursos para a sua experimentação, especificando o número máximo de total de execuções de preparação de otimização de hiper-parâmetros.  Opcionalmente, especifica a duração máxima para a sua experimentação de otimização de hiper-parâmetros.

* `max_total_runs`: Número total máximo de execuções de preparação que será criada. Limite superior - pode haver menos execuções, por exemplo, se o espaço de hiper-parâmetros é finito e tem menos de exemplos. Tem de ser um número entre 1 e 1000.
* `max_duration_minutes`: Duração máxima em minutos o experimentação de otimização de hiper-parâmetros. Parâmetro é opcional e, se estiver presente, todas as execuções que estariam sendo executados depois desta duração automaticamente foram canceladas.

>[!NOTE] 
>Se os dois `max_total_runs` e `max_duration_minutes` forem especificados, o experimentação de otimização de hiper-parâmetros termina quando o primeiro desses dois limites for atingido.

Além disso, especifique o número máximo de treinamento é executado para serem executadas em simultâneo durante sua pesquisa de otimização de hiper-parâmetros.

* `max_concurrent_runs`: Número máximo de execuções para executar simultaneamente em determinado momento. Se especificado nenhum, todos `max_total_runs` será iniciado em paralelo. Se for especificado, tem de ser um número entre 1 e 100.

>[!NOTE] 
>O número de execuções simultâneas é Check controlado nos recursos disponíveis no destino de computação especificado. Por este motivo, terá de garantir que o destino de computação tem os recursos disponíveis para a simultaneidade pretendido.

Alocar recursos para a otimização de hiper-parâmetros:

```Python
max_total_runs=20,
max_concurrent_runs=4
```

Esse código configura o experimentação para poder utilizar um máximo de 20 execuções total, executado 4 configurações de cada vez de otimização de hiper-parâmetros.

## <a name="configure-experiment"></a>Configurar experimentação

Configure a sua experiência com o espaço de pesquisa de hiper-parâmetros definidos, política de cessação antecipada, métrica primária e alocação de recursos das secções acima de otimização de hiper-parâmetros. Além disso, fornecer um `estimator` que será chamado com os amostras hiperparâmetros. O `estimator` descreve o script de treinamento, executar, os recursos por tarefa (única ou múltipla gpu) e o destino de computação para utilizar. Uma vez que a simultaneidade para a sua experimentação de otimização de hiper-parâmetros é Check controlado nos recursos disponíveis, certifique-se de que o destino de computação especificado no `estimator` tem recursos suficientes para a simultaneidade pretendido. (Para obter mais informações sobre os avaliadores, consulte [como a criar modelos](how-to-train-ml-models.md).)

Configure a sua experimentação de otimização de hiper-parâmetros:

```Python
from azureml.train.hyperdrive import HyperDriveRunConfig
hyperdrive_run_config = HyperDriveRunConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-experiment"></a>Submeter a experimentação

Depois de definir o configuração de otimização de hiper-parâmetros, submeta uma experimentação:

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

`experiment_name` é o nome a atribuir a sua experimentação, a otimização de hiper-parâmetros e `workspace` é a área de trabalho no qual pretende criar a experimentação (para obter mais informações sobre experiências, consulte [como serviço Azure Machine Learning funciona?](concept-azure-machine-learning-architecture.md))

## <a name="visualize-experiment"></a>Visualizar a experimentação

O SDK do Azure Machine Learning fornece um widget de bloco de notas que apresenta o progresso do seu treinamento é executado. O fragmento seguinte visualiza a todos os seus hiper-parâmetros otimização é executada num único local num bloco de notas do Jupyter:

```Python
from azureml.train.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Esse código exibe uma tabela com detalhes sobre as execuções de preparação para cada uma das configurações de hiper-parâmetros.

![tabela de otimização de hiper-parâmetros](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Também é possível visualizar o desempenho de cada uma das execuções à medida que progride de treinamento. 

![desenho de otimização de hiper-parâmetros](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Além disso, pode identificar visualmente a correlação entre desempenho e os valores de hiperparâmetros individuais com um paralelo as coordenadas de desenho. 

![coordenadas paralelas de otimização de hiper-parâmetros](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

É possível visualizar todos os seus hiper-parâmetros otimização é executada no portal web do Azure. Para obter mais informações sobre como visualizar uma experimentação no web portal, consulte [como controlar experimentações](how-to-track-experiments.md#view-the-experiment-in-the-web-portal).

![portal de otimização de hiper-parâmetros](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-best-model"></a>Encontrar o melhor modelo

Depois de concluíram todas os execuções de otimização de hiper-parâmetros, identifique o melhor desempenho, configuração e os valores de hiper-parâmetros correspondente:

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Bloco de notas de exemplo
Consulte a 
* [Training/03.Train-hyperparameter-Tune-Deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow) para obter um tutorial sobre ajuste de hiperparâmetros para um modelo do Tensorflow. 

Obter este bloco de notas:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Passos Seguintes
* [Controlar uma experimentação](how-to-track-experiments.md)
* [Implementar um modelo preparado](how-to-deploy-and-where.md)
