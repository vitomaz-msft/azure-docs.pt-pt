---
title: Criar e implementar um modelo de previsão a utilizar o pacote do Azure Machine Learning para previsão.
description: Saiba como criar, formar, testar e implementar um modelo de previsão com o pacote do Azure Machine Learning para previsão.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: mattcon
author: matthewconners
ms.date: 07/13/2018
ROBOTS: NOINDEX
ms.openlocfilehash: d3fac7c6d0a2274813c6ba6d96d8014d6452d28f
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284902"
---
# <a name="build-and-deploy-forecasting-models-with-azure-machine-learning"></a>Criar e implementar modelos de previsão com o Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Neste artigo, saiba como utilizar **pacote do Azure Machine Learning para previsão** (AMLPF) para criar e implementar um modelo de previsão rapidamente. O fluxo de trabalho é o seguinte:

1. Carregar e explorar dados
2. Criar funcionalidades
3. Dar formação e selecionar o melhor modelo
4. Implementar o modelo e consumir o serviço web

Consulte a [documentação de referência do pacote](https://aka.ms/aml-packages/forecasting) para a lista completa de apresentam vários transformadores e modelos, bem como a referência detalhada para cada módulo e uma classe.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As seguintes contas e aplicação tem de ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Uma conta de gestão de modelos do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado 

 Se esses três são ainda não foi criados ou instalados, siga os [instalação manual de início rápido do Azure Machine Learning e Bancada de trabalho](../desktop-workbench/quickstart-installation.md) artigo.

1. Tem de estar instalado o pacote de Aprendizado de máquina do Azure para a previsão. Saiba como [instalar este pacote aqui](https://aka.ms/aml-packages/forecasting).

## <a name="sample-data-and-jupyter-notebook"></a>Dados de exemplo e o bloco de notas do Jupyter

### <a name="sample-workflow"></a>Fluxo de trabalho de exemplo 
O exemplo segue o fluxo de trabalho:
 
1. **Ingestão de dados**: carregar o conjunto de dados e convertê-los TimeSeriesDataFrame. Este pacote de dados é uma série de dados de tempo estrutura fornecida pelo pacote do Azure Machine Learning para a previsão, descritos neste documento denominado **AMLPF**.

2. **Criar funcionalidades**: utilizar vários featurization apresentam vários transformadores fornecidas pelo AMLPF para criar recursos.

3. **Educar e selecionar o melhor modelo**: comparar o desempenho de vários modelos de série de tempo de univariate e modelos de aprendizagem automática. 

4. **Implementar o modelo**: implementar o pipeline de modelo treinado como um serviço web através do Azure Machine Learning Workbench, de modo que podem ser consumidos por outros utilizadores.

### <a name="get-the-jupyter-notebook"></a>Obter o bloco de notas do Jupyter

Download do bloco de notas para executar os exemplos de código aqui descritos por conta própria.

> [!div class="nextstepaction"]
> [Obter o bloco de notas do Jupyter](https://aka.ms/aml-packages/forecasting/notebooks/sales_forecasting)

### <a name="explore-the-sample-data"></a>Explorar os dados de exemplo

O machine learning exemplos no código a seguir exemplos dependem de previsão a [conjunto de dados da Dominick do Universidade de Chicago Foods mais](https://research.chicagobooth.edu/kilts/marketing-databases/dominicks) para prever vendas suco de laranja. Dominick era uma cadeia de acessar na área metropolitana de Chicago.

### <a name="import-any-dependencies-for-this-sample"></a>Importar todas as dependências para este exemplo

Estas dependências têm de ser importadas para os seguintes exemplos de código:


```python
import pandas as pd
import numpy as np
import math
import pkg_resources
from datetime import timedelta
import matplotlib
matplotlib.use('agg')
%matplotlib inline
from matplotlib import pyplot as plt

from sklearn.linear_model import Lasso, ElasticNet
from sklearn.ensemble import RandomForestRegressor, GradientBoostingRegressor
from sklearn.neighbors import KNeighborsRegressor

from ftk import TimeSeriesDataFrame, ForecastDataFrame, AzureMLForecastPipeline
from ftk.ts_utils import last_n_periods_split

from ftk.transforms import TimeSeriesImputer, TimeIndexFeaturizer, DropColumns
from ftk.transforms.grain_index_featurizer import GrainIndexFeaturizer
from ftk.models import Arima, SeasonalNaive, Naive, RegressionForecaster, ETS, BestOfForecaster
from ftk.models.forecaster_union import ForecasterUnion
from ftk.model_selection import TSGridSearchCV, RollingOriginValidator

from azuremltkbase.deployment import AMLSettings
from ftk.operationalization.forecast_webservice_factory import ForecastWebserviceFactory
from ftk.operationalization import ScoreContext

from ftk.data import get_a_year_of_daily_weather_data
print('imports done')
```

    imports done
    

## <a name="load-data-and-explore"></a>Carregar dados e explorar

Este fragmento de código mostra o processo normal de começar com um conjunto de dados não processado, neste caso o [dados de pratos de mais de Dominick](https://research.chicagobooth.edu/kilts/marketing-databases/dominicks).  Também pode utilizar a função de conveniência [load_dominicks_oj_data](/python/api/azuremlftk/ftk.data.dominicks_oj.load_dominicks_oj_data).


```python
# Load the data into a pandas DataFrame
csv_path = pkg_resources.resource_filename('ftk', 'data/dominicks_oj/dominicks_oj.csv')
whole_df = pd.read_csv(csv_path, low_memory = False)
whole_df.head()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store</th>
      <th>marca</th>
      <th>semana</th>
      <th>logmove</th>
      <th>recurso</th>
      <th>preço</th>
      <th>AGE60</th>
      <th>EDUC</th>
      <th>ÉTNICOS</th>
      <th>RENDIMENTO</th>
      <th>HHLARGE</th>
      <th>WORKWOM</th>
      <th>HVAL150</th>
      <th>SSTRDIST</th>
      <th>SSTRVOL</th>
      <th>CPDIST5</th>
      <th>CPWVOL5</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>tropicana</td>
      <td>40</td>
      <td>9.02</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>0.46</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>tropicana</td>
      <td>46</td>
      <td>8.72</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>0.46</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>tropicana</td>
      <td>47</td>
      <td>8.25</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>0.46</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>tropicana</td>
      <td>48</td>
      <td>8.99</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>0.46</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>tropicana</td>
      <td>50</td>
      <td>9.09</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>0.46</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
    </tr>
  </tbody>
</table>



Os dados são compostos por vendas semanais por marca e o arquivo. O logaritmo de quantidade vendida está no _logmove_ coluna. Os dados também incluem alguns recursos de demográficos do cliente. 

Para modelar a série de tempo, terá de extrair os elementos a seguir este pacote de dados: 
+ Um eixo de data/hora 
+ A quantidade de vendas para ser prever


```python
# The sales are contained in the 'logmove' column. 
# Values are logarithmic, so exponentiate and round them to get quantity sold
def expround(x):
    return math.floor(math.exp(x) + 0.5)
whole_df['Quantity'] = whole_df['logmove'].apply(expround)

# The time axis is in the 'week' column
# This is the week offset from the week of 1989-09-07 through 1989-09-13 inclusive
# Create new datetime columns containing the start and end of each week period
weekZeroStart = pd.to_datetime('1989-09-07 00:00:00')
weekZeroEnd = pd.to_datetime('1989-09-13 23:59:59')
whole_df['WeekFirstDay'] = whole_df['week'].apply(lambda n: weekZeroStart + timedelta(weeks=n))
whole_df['WeekLastDay'] = whole_df['week'].apply(lambda n: weekZeroEnd + timedelta(weeks=n))
whole_df[['store','brand','WeekLastDay','Quantity']].head()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Store</th>
      <th>marca</th>
      <th>WeekLastDay</th>
      <th>Quantidade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>tropicana</td>
      <td>06 de 1990-20 23:59:59</td>
      <td>8256</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-01 23:59:59</td>
      <td>6144</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-08 23:59:59</td>
      <td>3840</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-15 23:59:59</td>
      <td>8000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>tropicana</td>
      <td>1990-08-29 23:59:59</td>
      <td>8896</td>
    </tr>
  </tbody>
</table>




```python
nseries = whole_df.groupby(['store', 'brand']).ngroups
print('{} time series in the data frame.'.format(nseries))
```

    249 time series in the data frame.
    

Os dados contêm aproximadamente 250 diferentes combinações de loja e marca num quadro de dados. Cada combinação define sua própria série de tempo de vendas. 

Pode utilizar o [TimeSeriesDataFrame](/python/api/azuremlftk/ftk.time_series_data_frame.timeseriesdataframe) classe para modelar convenientemente várias séries numa estrutura de dados individual utilizando o _detalhamento_. O intervalo de agregação for especificado o `store` e `brand` colunas.

A diferença entre _intervalo de agregação_ e _grupo_ é que o intervalo de agregação sempre é fisicamente significativo no mundo real, enquanto o grupo não tem de ser. As funções do pacote interno utilizam grupo para criar um modelo único de várias séries de tempo se o usuário acredite que este agrupamento ajuda a melhorar o desempenho do modelo. Por predefinição, o grupo está definido para ser igual ao intervalo de agregação, e um único modelo foi concebido para cada intervalo de agregação. 


```python
# Create a TimeSeriesDataFrame
# Use end of period as the time index
# Store and brand combinations label the grain 
# i.e. there is one time series for each unique pair of store and grain
whole_tsdf = TimeSeriesDataFrame(whole_df, 
                                 grain_colnames=['store', 'brand'],
                                 time_colname='WeekLastDay', 
                                 ts_value_colname='Quantity',
                                 group_colnames='store')

whole_tsdf[['Quantity']].head()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Quantidade</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>Store</th>
      <th>marca</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>06 de 1990-20 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8256</td>
    </tr>
    <tr>
      <th>1990-08-01 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>6144</td>
    </tr>
    <tr>
      <th>1990-08-08 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>3840</td>
    </tr>
    <tr>
      <th>1990-08-15 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8000</td>
    </tr>
    <tr>
      <th>1990-08-29 23:59:59</th>
      <th>2</th>
      <th>tropicana</th>
      <td>8896</td>
    </tr>
  </tbody>
</table>



Na representação TimeSeriesDataFrame, o eixo de tempo e o intervalo de agregação agora fazem parte do índice do quadro de dados e permitem o acesso fácil a datetime de pandas funcionalidade de fragmentação.


```python
# sort so we can slice
whole_tsdf.sort_index(inplace=True)

# Get sales of dominick's brand orange juice from store 2 during summer 1990
whole_tsdf.loc[pd.IndexSlice['1990-06':'1990-09', 2, 'dominicks'], ['Quantity']]
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>Quantidade</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>Store</th>
      <th>marca</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>06 de 1990-20 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>10560</td>
    </tr>
    <tr>
      <th>1990-08-01 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>8000</td>
    </tr>
    <tr>
      <th>1990-08-08 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6848</td>
    </tr>
    <tr>
      <th>1990-08-15 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>2880</td>
    </tr>
    <tr>
      <th>1990-08-29 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>1600</td>
    </tr>
    <tr>
      <th>1990-09-05 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>25344</td>
    </tr>
    <tr>
      <th>1990-09-12 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>10752</td>
    </tr>
    <tr>
      <th>1990-09-19 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6656</td>
    </tr>
    <tr>
      <th>1990-09-26 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <td>6592</td>
    </tr>
  </tbody>
</table>

O [TimeSeriesDataFrame.ts_report](/python/api/azuremlftk/ftk.time_series_data_frame.timeseriesdataframe#ts-report) função gera um relatório abrangente da estrutura de dados de série de tempo. O relatório inclui tanto uma descrição geral de dados, bem como estatísticas específicas para os dados de séries de tempo. 

```python
whole_tsdf.ts_report()
```

    --------------------------------  Data Overview  ---------------------------------
    <class 'ftk.time_series_data_frame.TimeSeriesDataFrame'>
    MultiIndex: 28947 entries, (1990-06-20 23:59:59, 2, dominicks) to (1992-10-07 23:59:59, 137, tropicana)
    Data columns (total 17 columns):
    week            28947 non-null int64
    logmove         28947 non-null float64
    feat            28947 non-null int64
    price           28947 non-null float64
    AGE60           28947 non-null float64
    EDUC            28947 non-null float64
    ETHNIC          28947 non-null float64
    INCOME          28947 non-null float64
    HHLARGE         28947 non-null float64
    WORKWOM         28947 non-null float64
    HVAL150         28947 non-null float64
    SSTRDIST        28947 non-null float64
    SSTRVOL         28947 non-null float64
    CPDIST5         28947 non-null float64
    CPWVOL5         28947 non-null float64
    Quantity        28947 non-null int64
    WeekFirstDay    28947 non-null datetime64[ns]
    dtypes: datetime64[ns](1), float64(13), int64(3)
    memory usage: 3.8+ MB
    --------------------------  Numerical Variable Summary  --------------------------
              week  logmove     feat    price    AGE60     EDUC   ETHNIC   INCOME  \
    count 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00 28947.00   
    mean    100.46     9.17     0.24     2.28     0.17     0.23     0.16    10.62   
    std      34.69     1.02     0.43     0.65     0.06     0.11     0.19     0.28   
    min      40.00     4.16     0.00     0.52     0.06     0.05     0.02     9.87   
    25%      70.00     8.49     0.00     1.79     0.12     0.15     0.04    10.46   
    50%     101.00     9.03     0.00     2.17     0.17     0.23     0.07    10.64   
    75%     130.00     9.76     0.00     2.73     0.21     0.28     0.19    10.80   
    max     160.00    13.48     1.00     3.87     0.31     0.53     1.00    11.24   
    
           HHLARGE  WORKWOM  HVAL150  SSTRDIST  SSTRVOL  CPDIST5  CPWVOL5  \
    count 28947.00 28947.00 28947.00  28947.00 28947.00 28947.00 28947.00   
    mean      0.12     0.36     0.34      5.10     1.21     2.12     0.44   
    std       0.03     0.05     0.24      3.47     0.53     0.73     0.22   
    min       0.01     0.24     0.00      0.13     0.40     0.77     0.09   
    25%       0.10     0.31     0.12      2.77     0.73     1.63     0.27   
    50%       0.11     0.36     0.35      4.65     1.12     1.96     0.38   
    75%       0.14     0.40     0.53      6.65     1.54     2.53     0.56   
    max       0.22     0.47     0.92     17.86     2.57     4.11     1.14   
    
           Quantity  
    count  28947.00  
    mean   17312.21  
    std    27477.66  
    min       64.00  
    25%     4864.00  
    50%     8384.00  
    75%    17408.00  
    max   716416.00  
    ------------------------  Non-Numerical Variable Summary  -----------------------
                   WeekFirstDay
    count                 28947
    unique                  121
    top     1992-03-12 00:00:00
    freq                    249
    first   1990-06-14 00:00:00
    last    1992-10-01 00:00:00
    ------------------------------  Time Series Summary  -----------------------------
    Number of time series                 249
    Minimum time                    1990-06-20 23:59:59
    Maximum time                    1992-10-07 23:59:59
    
    Inferred frequencies
    Number of ['store', 'brand']s with frequency W-WED     249
    Use get_frequency_dict() method to explore ['store', 'brand']s with unusual frequency and clean data
    
    Detected seasonalities
    Number of ['store', 'brand']s with seasonality 1         190
    Number of ['store', 'brand']s with seasonality 15        15
    Number of ['store', 'brand']s with seasonality 14        11
    Number of ['store', 'brand']s with seasonality 7         9
    Number of ['store', 'brand']s with seasonality 6         8
    Number of ['store', 'brand']s with seasonality 8         5
    Number of ['store', 'brand']s with seasonality 2         4
    Number of ['store', 'brand']s with seasonality 23        2
    Number of ['store', 'brand']s with seasonality 3         1
    Number of ['store', 'brand']s with seasonality 11        1
    Number of ['store', 'brand']s with seasonality 12        1
    Number of ['store', 'brand']s with seasonality 13        1
    Number of ['store', 'brand']s with seasonality 47        1
    Use get_seasonality_dict() method to explore ['store', 'brand']s with unusual seasonality and clean data
    -----------------------------  Value Column Summary  -----------------------------
    Value column                        Quantity
    Percentage of missing values        0.00
    Percentage of zero values           0.00
    Mean coefficient of variation       31688.52
    Median coefficient of variation     24000.20
    Minimum coefficient of variation    ['store', 'brand'] (48, 'tropicana'): 4475.53
    Maximum coefficient of variation    ['store', 'brand'] (111, 'dominicks'): 193333.55
    ------------------------------  Correlation Matrix  ------------------------------
        week  logmove  feat  price  AGE60  EDUC  ETHNIC  INCOME  HHLARGE  WORKWOM  \
    0   1.00     0.10  0.04  -0.21  -0.01  0.01    0.00    0.00     0.01    -0.00   
    1   0.10     1.00  0.54  -0.43   0.09  0.00    0.06   -0.04    -0.06    -0.08   
    2   0.04     0.54  1.00  -0.29  -0.00  0.00    0.00   -0.00    -0.00     0.00   
    3  -0.21    -0.43 -0.29   1.00   0.04  0.02    0.04   -0.03    -0.04    -0.02   
    4  -0.01     0.09 -0.00   0.04   1.00 -0.31   -0.09   -0.15    -0.32    -0.63   
    5   0.01     0.00  0.00   0.02  -0.31  1.00   -0.34    0.66    -0.39     0.56   
    6   0.00     0.06  0.00   0.04  -0.09 -0.34    1.00   -0.72     0.25    -0.29   
    7   0.00    -0.04 -0.00  -0.03  -0.15  0.66   -0.72    1.00    -0.08     0.40   
    8   0.01    -0.06 -0.00  -0.04  -0.32 -0.39    0.25   -0.08     1.00    -0.28   
    9  -0.00    -0.08  0.00  -0.02  -0.63  0.56   -0.29    0.40    -0.28     1.00   
    10  0.01     0.02  0.00   0.04  -0.11  0.89   -0.42    0.64    -0.48     0.45   
    11  0.01    -0.00  0.00   0.08   0.07 -0.12    0.54   -0.41     0.06    -0.19   
    12 -0.01    -0.09 -0.00   0.03  -0.05 -0.13    0.23   -0.26     0.04    -0.06   
    13 -0.01     0.02 -0.00  -0.06   0.09 -0.20   -0.22    0.21     0.19    -0.13   
    14 -0.00    -0.12 -0.00  -0.01  -0.09  0.28   -0.38    0.36    -0.20     0.37   
    15  0.03     0.76  0.47  -0.36   0.08 -0.04    0.11   -0.08    -0.00    -0.10   
    
        HVAL150  SSTRDIST  SSTRVOL  CPDIST5  CPWVOL5  Quantity  
    0      0.01      0.01    -0.01    -0.01    -0.00      0.03  
    1      0.02     -0.00    -0.09     0.02    -0.12      0.76  
    2      0.00      0.00    -0.00    -0.00    -0.00      0.47  
    3      0.04      0.08     0.03    -0.06    -0.01     -0.36  
    4     -0.11      0.07    -0.05     0.09    -0.09      0.08  
    5      0.89     -0.12    -0.13    -0.20     0.28     -0.04  
    6     -0.42      0.54     0.23    -0.22    -0.38      0.11  
    7      0.64     -0.41    -0.26     0.21     0.36     -0.08  
    8     -0.48      0.06     0.04     0.19    -0.20     -0.00  
    9      0.45     -0.19    -0.06    -0.13     0.37     -0.10  
    10     1.00     -0.17    -0.24    -0.22     0.27     -0.04  
    11    -0.17      1.00     0.17    -0.11    -0.40      0.06  
    12    -0.24      0.17     1.00    -0.05     0.36     -0.02  
    13    -0.22     -0.11    -0.05     1.00     0.02     -0.00  
    14     0.27     -0.40     0.36     0.02     1.00     -0.11  
    15    -0.04      0.06    -0.02    -0.00    -0.11      1.00  
    You may call TimeSeriesDataFrame.plot_scatter_matrix() to get a correlation matrix plot. However, this
    is not recommended if your data is big.
    



![PNG](./media/how-to-build-deploy-forecast-models/output_15_1.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_15_2.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_15_3.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_15_4.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_15_5.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_15_6.png)


## <a name="integrate-with-external-data"></a>Integrar com dados externos

Por vezes, é útil integrar dados externos como funcionalidades adicionais para a previsão. Neste exemplo de código, adere TimeSeriesDataFrame com dados externos relacionados com Meteorologia.


```python
# Load weather data
weather_1990 = get_a_year_of_daily_weather_data(year=1990)
weather_1991 = get_a_year_of_daily_weather_data(year=1991)
weather_1992 = get_a_year_of_daily_weather_data(year=1992)

# Preprocess weather data
weather_all = pd.concat([weather_1990, weather_1991, weather_1992])
weather_all.reset_index(inplace=True)

# Only use a subset of columns
weather_all = weather_all[['TEMP', 'DEWP', 'WDSP', 'PRCP']]

# Compute the WeekLastDay column, in order to merge with sales data
weather_all['WeekLastDay'] = pd.Series(
    weather_all.time_index - weekZeroStart, 
    index=weather_all.time_index).apply(lambda n: weekZeroEnd + timedelta(weeks=math.floor(n.days/7)))

# Resample daily weather data to weekly data
weather_all = weather_all.groupby('WeekLastDay').mean()

# Set WeekLastDay as new time index
weather_all = TimeSeriesDataFrame(weather_all, time_colname='WeekLastDay')

# Merge weather data with sales data
whole_tsdf = whole_tsdf.merge(weather_all, how='left', on='WeekLastDay')
whole_tsdf.head()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th>semana</th>
      <th>logmove</th>
      <th>recurso</th>
      <th>preço</th>
      <th>AGE60</th>
      <th>EDUC</th>
      <th>ÉTNICOS</th>
      <th>RENDIMENTO</th>
      <th>HHLARGE</th>
      <th>WORKWOM</th>
      <th>...</th>
      <th>SSTRDIST</th>
      <th>SSTRVOL</th>
      <th>CPDIST5</th>
      <th>CPWVOL5</th>
      <th>Quantidade</th>
      <th>WeekFirstDay</th>
      <th>TEMP</th>
      <th>DEWP</th>
      <th>WDSP</th>
      <th>PRCP</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>Store</th>
      <th>marca</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="5" valign="top">06 de 1990-20 23:59:59</th>
      <th rowspan="3" valign="top">2</th>
      <th>dominicks</th>
      <td>40</td>
      <td>9.26</td>
      <td>1</td>
      <td>1.59</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
      <td>10560</td>
      <td>06 de 1990-14</td>
      <td>72.00</td>
      <td>61.87</td>
      <td>9.74</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>Minute.maid</th>
      <td>40</td>
      <td>8.41</td>
      <td>0</td>
      <td>3.17</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
      <td>4480</td>
      <td>06 de 1990-14</td>
      <td>72.00</td>
      <td>61.87</td>
      <td>9.74</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>tropicana</th>
      <td>40</td>
      <td>9.02</td>
      <td>0</td>
      <td>3.87</td>
      <td>0.23</td>
      <td>0.25</td>
      <td>0,11</td>
      <td>10.55</td>
      <td>0,10</td>
      <td>0,30</td>
      <td>...</td>
      <td>2.11</td>
      <td>1.14</td>
      <td>1.93</td>
      <td>0.38</td>
      <td>8256</td>
      <td>06 de 1990-14</td>
      <td>72.00</td>
      <td>61.87</td>
      <td>9.74</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">5</th>
      <th>dominicks</th>
      <td>40</td>
      <td>7.49</td>
      <td>1</td>
      <td>1.59</td>
      <td>0,12</td>
      <td>0,32</td>
      <td>0,05</td>
      <td>10.92</td>
      <td>0,10</td>
      <td>0.41</td>
      <td>...</td>
      <td>3.80</td>
      <td>0,68</td>
      <td>1.60</td>
      <td>0,74</td>
      <td>1792</td>
      <td>06 de 1990-14</td>
      <td>72.00</td>
      <td>61.87</td>
      <td>9.74</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>Minute.maid</th>
      <td>40</td>
      <td>8.35</td>
      <td>0</td>
      <td>2.99</td>
      <td>0,12</td>
      <td>0,32</td>
      <td>0,05</td>
      <td>10.92</td>
      <td>0,10</td>
      <td>0.41</td>
      <td>...</td>
      <td>3.80</td>
      <td>0,68</td>
      <td>1.60</td>
      <td>0,74</td>
      <td>4224</td>
      <td>06 de 1990-14</td>
      <td>72.00</td>
      <td>61.87</td>
      <td>9.74</td>
      <td>0.19</td>
    </tr>
  </tbody>
</table>


## <a name="preprocess-data-and-impute-missing-values"></a>Pré-processar dados e impute valores em falta

Comece por divisão dos dados em conjunto de treinamento e um teste definido com o [last_n_periods_split](/python/api/azuremlftk/ftk.ts_utils#last-n-periods-split) função de utilitário. O conjunto de teste resultante contém as último 40 observações de cada série de tempo. 


```python
train_tsdf, test_tsdf = last_n_periods_split(whole_tsdf, 40)
```

Modelos de série de tempo básicas necessitam de séries de tempo contínuo. Certifique-se a série é regular, o que significa que têm um índice de tempo como amostra em intervalos regulares, utilizando o [check_regularity_by_grain](/python/api/azuremlftk/ftk.time_series_data_frame.timeseriesdataframe#check-regularity-by-grain) função.


```python
ts_regularity = train_tsdf.check_regularity_by_grain()
print(ts_regularity[ts_regularity['regular'] == False])
```

                                              problems  regular
    store brand                                                
    2     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    5     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    8     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    9     dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    12    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    14    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    18    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    21    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    28    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    33    dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    ...                                            ...      ...
    119   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    121   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    123   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    126   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    128   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    129   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    130   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    131   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    134   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    137   dominicks    [Irregular datetime gaps exist]    False
          minute.maid  [Irregular datetime gaps exist]    False
          tropicana    [Irregular datetime gaps exist]    False
    
    [213 rows x 2 columns]
    

Pode ver que a maior parte da série (213 de 249) é irregulares. Uma [transformação de imputation](/python/api/azuremlftk/ftk.transforms.time_series_imputer.timeseriesimputer) é necessário para preencher na ausência de valores de quantidade de vendas. Embora existam muitas opções de imputation, o código de exemplo seguinte utiliza uma interpolação linear.


```python
# Use a TimeSeriesImputer to linearly interpolate missing values
imputer = TimeSeriesImputer(input_column='Quantity', 
                            option='interpolate',
                            method='linear',
                            freq='W-WED')

train_imputed_tsdf = imputer.transform(train_tsdf)
```

Depois do código de imputation é executado, a série de todos os tem uma frequência normal:


```python
ts_regularity_imputed = train_imputed_tsdf.check_regularity_by_grain()
print(ts_regularity_imputed[ts_regularity_imputed['regular'] == False])
```

    Empty DataFrame
    Columns: [problems, regular]
    Index: []
    

## <a name="univariate-time-series-models"></a>Modelos de série de tempo de Univariate

Agora que tem de limpar os dados, pode começar a Modelagem.  Comece criando três modelos de univariate: o modelo de "ingênua", o modelo de "ingênua sazonais" e um modelo de "ARIMA".
* O algoritmo de previsão Bayesiana utiliza o valor da variável destino real do último período como o valor previsto do período atual.

* O algoritmo sazonais Bayesiana utiliza o valor da variável destino real do mesmo ponto no tempo a época anterior, como o valor previsto do ponto de tempo atual. Alguns exemplos incluem a utilizar o valor real do mesmo mês do ano passado para prever meses do ano atual; Utilize a mesma hora do passado para prever as horas de hoje. 

* O algoritmo de suavização (ETS) exponencial gera previsões pela computação as médias ponderadas de observações sobre o passado, com os pesos em decadência exponencialmente à medida as observações se mais antigas. 

* O algoritmo de regressão automática integrada mover média (ARIMA) captura autocorrelation nos dados de séries de tempo. Para obter mais informações sobre ARIMA, consulte [esta ligação](https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average)

Comece por definir determinados parâmetros de modelo com base em sua exploração de dados. 


```python
oj_series_freq = 'W-WED'
oj_series_seasonality = 52
```

### <a name="initialize-models"></a>Inicializar modelos


```python
# Initialize naive model.
naive_model = Naive(freq=oj_series_freq)

# Initialize seasonal naive model. 
seasonal_naive_model = SeasonalNaive(freq=oj_series_freq, 
                                     seasonality=oj_series_seasonality)

# Initialize ETS model.
ets_model = ETS(freq=oj_series_freq, seasonality=oj_series_seasonality)

# Initialize ARIMA(p,d,q) model.
arima_order = [2, 1, 0]
arima_model = Arima(oj_series_freq, arima_order)
```

### <a name="combine-multiple-models"></a>Combinar vários modelos

O [ForecasterUnion](/python/api/azuremlftk/ftk.models.forecaster_union.forecasterunion) estimator permite-lhe combinar vários avaliadores e ajuste/prever neles usando uma linha de código.

```python
forecaster_union = ForecasterUnion(
    forecaster_list=[('naive', naive_model), ('seasonal_naive', seasonal_naive_model), 
                     ('ets', ets_model), ('arima', arima_model)]) 
```

### <a name="fit-and-predict"></a>Ajustar e prever

Os avaliadores no AMLPF siga a mesma API como scikit-saiba estimadores: um método adequado para a preparação de modelos e um método de predict para gerar as previsões. 

**Preparação de modelos**  
Uma vez que estes modelos são todos os modelos de univariate, um modelo é adequado para cada intervalo de agregação dos dados. Utilizar AMLPF, todos os modelos de 249 podem ajustar com chamada de função apenas um.


```python
forecaster_union_fitted = forecaster_union.fit(train_imputed_tsdf)
```

**Previsão de vendas em dados de teste**  
Assim como o método adequado, pode criar previsões de indisponibilidade para todas as 249 séries no conjunto de dados de teste com uma chamada para o `predict` função. 


```python
forecaster_union_prediction = forecaster_union_fitted.predict(test_tsdf, retain_feature_column=True)
```

**Avaliar o desempenho do modelo**   

Agora pode calcular os erros de previsão no conjunto de teste. Aqui pode usar o erro de percentagem absoluto mean (MAPE). MAPE é o erro de percentagem absoluto mean em relação aos valores de vendas reais. O ```calc_error``` função fornece algumas funções internas para métricas de erro mais usados. Também pode definir nossa função de erro personalizada para calcular MedianAPE e passá-lo para o argumento de err_fun.


```python
def calc_median_ape(y_true, y_pred):
    y_true = np.array(y_true).astype(float)
    y_pred = np.array(y_pred).astype(float)
    y_true_rm_na = y_true[~(np.isnan(y_true) | np.isnan(y_pred))]
    y_pred_rm_na = y_pred[~(np.isnan(y_true) | np.isnan(y_pred))]
    y_true = y_true_rm_na
    y_pred = y_pred_rm_na
    if len(y_true) == 0:
        # if there is no entries left after removing na data, return np.nan
        return(np.nan)
    y_true_rm_zero = y_true[y_true != 0]
    y_pred_rm_zero = y_pred[y_true != 0]
    if len(y_true_rm_zero) == 0:
        # if all values are zero, np.nan will be returned.
        return(np.nan)
    ape = np.abs((y_true_rm_zero - y_pred_rm_zero) / y_true_rm_zero) * 100
    median_ape = np.median(ape)
    return median_ape
```


```python
forecaster_union_MAPE = forecaster_union_prediction.calc_error(err_name='MAPE',
                                                               by='ModelName')
forecaster_union_MedianAPE = forecaster_union_prediction.calc_error(err_name='MedianAPE', 
                                                                    err_fun=calc_median_ape,
                                                                    by='ModelName')

univariate_model_errors = forecaster_union_MAPE.merge(forecaster_union_MedianAPE, on='ModelName')
univariate_model_errors
```



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>modelName</th>
      <th>MAPE</th>
      <th>MedianAPE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>arima</td>
      <td>126.57</td>
      <td>66.49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ETS</td>
      <td>187.89</td>
      <td>75.73</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ingênua</td>
      <td>103.57</td>
      <td>59.14</td>
    </tr>
    <tr>
      <th>3</th>
      <td>seasonal_naive</td>
      <td>180.54</td>
      <td>65.99</td>
    </tr>
  </tbody>
</table>



## <a name="build-machine-learning-models"></a>Criar modelos de machine learning

Além dos modelos de univariate tradicional, Azure Machine Learning pacote para a previsão também permite-lhe criar modelos de aprendizagem automática.

Para esses modelos, comece por criar recursos.

### <a name="feature-engineering"></a>Engenharia de funcionalidades

**Apresentam vários transformadores**   
O pacote fornece apresentam vários transformadores muitas para dados de séries de tempo pré-processamento e featurization. Os exemplos que se seguem demonstram algumas das funcionalidades de pré-processamento e featurization.


```python
# DropColumns: Drop columns that should not be included for modeling. `logmove` is the log of the number of 
# units sold, so providing this number would be cheating. `WeekFirstDay` would be 
# redundant since we already have a feature for the last day of the week.
columns_to_drop = ['logmove', 'WeekFirstDay', 'week']
column_dropper = DropColumns(columns_to_drop)

# TimeSeriesImputer: Fill missing values in the features
# First, we need to create a dictionary with key as column names and value as values used to fill missing 
# values for that column. We are going to use the mean to fill missing values for each column.
columns_with_missing_values = train_imputed_tsdf.columns[pd.DataFrame(train_imputed_tsdf).isnull().any()].tolist()
columns_with_missing_values = [c for c in columns_with_missing_values if c not in columns_to_drop]
missing_value_imputation_dictionary = {}
for c in columns_with_missing_values:
    missing_value_imputation_dictionary[c] = train_imputed_tsdf[c].mean()
fillna_imputer = TimeSeriesImputer(option='fillna', 
                                   input_column=columns_with_missing_values,
                                   value=missing_value_imputation_dictionary)

# TimeIndexFeaturizer: extract temporal features from timestamps
time_index_featurizer = TimeIndexFeaturizer(correlation_cutoff=0.1, overwrite_columns=True)

# GrainIndexFeaturizer: create indicator variables for stores and brands
grain_featurizer = GrainIndexFeaturizer(overwrite_columns=True, ts_frequency=oj_series_freq)
```

**Pipelines**   
Objetos do pipeline facilitam a salvar um conjunto de passos, para que podem ser aplicadas repetidamente para objetos diferentes. Além disso, os objetos do pipeline podem ser pickled para torná-los facilmente transportável para outras máquinas para a implementação. Pode encadear todos apresentam vários transformadores que criou até agora com um pipeline. 


```python
pipeline_ml = AzureMLForecastPipeline([('drop_columns', column_dropper), 
                                       ('fillna_imputer', fillna_imputer),
                                       ('time_index_featurizer', time_index_featurizer),
                                       ('grain_featurizer', grain_featurizer)
                                      ])


train_feature_tsdf = pipeline_ml.fit_transform(train_imputed_tsdf)
test_feature_tsdf = pipeline_ml.transform(test_tsdf)

# Let's get a look at our new feature set
print(train_feature_tsdf.head())
```

    F1 2018-06-14 23:10:03,472 INFO azureml.timeseries - pipeline fit_transform started. 
    F1 2018-06-14 23:10:07,317 INFO azureml.timeseries - pipeline fit_transform finished. Time elapsed 0:00:03.845078
    F1 2018-06-14 23:10:07,317 INFO azureml.timeseries - pipeline transforms started. 
    F1 2018-06-14 23:10:16,499 INFO azureml.timeseries - pipeline transforms finished. Time elapsed 0:00:09.182314
                                           feat  price  AGE60  EDUC  ETHNIC  \
    WeekLastDay         store brand                                           
    1990-06-20 23:59:59 2     dominicks    1.00   1.59   0.23  0.25    0.11   
                              minute.maid  0.00   3.17   0.23  0.25    0.11   
                              tropicana    0.00   3.87   0.23  0.25    0.11   
                        5     dominicks    1.00   1.59   0.12  0.32    0.05   
                              minute.maid  0.00   2.99   0.12  0.32    0.05   
    
                                           INCOME  HHLARGE  WORKWOM  HVAL150  \
    WeekLastDay         store brand                                            
    1990-06-20 23:59:59 2     dominicks     10.55     0.10     0.30     0.46   
                              minute.maid   10.55     0.10     0.30     0.46   
                              tropicana     10.55     0.10     0.30     0.46   
                        5     dominicks     10.92     0.10     0.41     0.54   
                              minute.maid   10.92     0.10     0.41     0.54   
    
                                           SSTRDIST     ...       CPWVOL5  \
    WeekLastDay         store brand                     ...                 
    1990-06-20 23:59:59 2     dominicks        2.11     ...          0.38   
                              minute.maid      2.11     ...          0.38   
                              tropicana        2.11     ...          0.38   
                        5     dominicks        3.80     ...          0.74   
                              minute.maid      3.80     ...          0.74   
    
                                           Quantity  TEMP  DEWP  WDSP  PRCP  year  \
    WeekLastDay         store brand                                                 
    1990-06-20 23:59:59 2     dominicks    10560.00 72.00 61.87  9.74  0.19  1990   
                              minute.maid   4480.00 72.00 61.87  9.74  0.19  1990   
                              tropicana     8256.00 72.00 61.87  9.74  0.19  1990   
                        5     dominicks     1792.00 72.00 61.87  9.74  0.19  1990   
                              minute.maid   4224.00 72.00 61.87  9.74  0.19  1990   
    
                                           day  grain_brand  grain_store  
    WeekLastDay         store brand                                       
    1990-06-20 23:59:59 2     dominicks     20    dominicks            2  
                              minute.maid   20  minute.maid            2  
                              tropicana     20    tropicana            2  
                        5     dominicks     20    dominicks            5  
                              minute.maid   20  minute.maid            5  
    
    [5 rows x 22 columns]
    

 **RegressionForecaster**

O [RegressionForecaster](/python/api/azuremlftk/ftk.models.regression_forecaster.regressionforecaster) função encapsula sklearn avaliadores de regressão, de modo a que pode ser treinados em TimeSeriesDataFrame. O forecaster encapsulada coloca também cada grupo, esse armazenamento caso, o mesmo modelo. O forecaster pode aprender um modelo para um grupo de série que foram considerada semelhantes e pode ser agrupado em conjunto. Um modelo para um grupo de série, muitas vezes, usa os dados da série de mais tempo para melhorar as previsões de série curta. Pode substituir estes modelos para os outros modelos na biblioteca, que oferecem suporte a regressão. 


```python
lasso_model = RegressionForecaster(estimator=Lasso(),
                                   make_grain_features=False)
elastic_net_model = RegressionForecaster(estimator=ElasticNet(),
                                         make_grain_features=False)
knn_model = RegressionForecaster(estimator=KNeighborsRegressor(),
                                 make_grain_features=False)
random_forest_model = RegressionForecaster(estimator=RandomForestRegressor(),
                                           make_grain_features=False)
boosted_trees_model = RegressionForecaster(estimator=GradientBoostingRegressor(),
                                           make_grain_features=False)

ml_union = ForecasterUnion(forecaster_list=[
    ('lasso', lasso_model), 
    ('elastic_net', elastic_net_model), 
    ('knn', knn_model), 
    ('random_forest', random_forest_model), 
    ('boosted_trees', boosted_trees_model)
]) 
```


```python
ml_union.fit(train_feature_tsdf, y=train_feature_tsdf.ts_value)
ml_results = ml_union.predict(test_feature_tsdf, retain_feature_column=True)
```


```python
ml_model_MAPE = ml_results.calc_error(err_name='MAPE', by='ModelName')
ml_model_MedianAPE = ml_results.calc_error(err_name='MedianAPE', 
                                           err_fun=calc_median_ape,
                                           by='ModelName')
ml_model_errors = ml_model_MAPE.merge(ml_model_MedianAPE, on='ModelName')
all_errors = pd.concat([univariate_model_errors, ml_model_errors])
all_errors.sort_values('MedianAPE')
```





<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>modelName</th>
      <th>MAPE</th>
      <th>MedianAPE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>random_forest</td>
      <td>78.82</td>
      <td>42.81</td>
    </tr>
    <tr>
      <th>0</th>
      <td>boosted_trees</td>
      <td>78.46</td>
      <td>45.37</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ingênua</td>
      <td>103.57</td>
      <td>59.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>knn</td>
      <td>129.85</td>
      <td>65.37</td>
    </tr>
    <tr>
      <th>1</th>
      <td>elastic_net</td>
      <td>125.11</td>
      <td>65.59</td>
    </tr>
    <tr>
      <th>3</th>
      <td>seasonal_naive</td>
      <td>180.54</td>
      <td>65.99</td>
    </tr>
    <tr>
      <th>0</th>
      <td>arima</td>
      <td>126.57</td>
      <td>66.49</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lasso</td>
      <td>112.87</td>
      <td>67.92</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ETS</td>
      <td>187.89</td>
      <td>75.73</td>
    </tr>
  </tbody>
</table>



Alguns modelos de machine learning eram capazes de tirar partido de mais funcionalidades e as semelhanças entre séries para obter uma melhor precisão de previsão.

### <a name="cross-validation-parameter-and-model-sweeping"></a>Validação cruzada, parâmetro e varrimento de modelo    

O pacote adapta-se algumas funções para um aplicativo de previsão de aprendizagem tradicional.  [RollingOriginValidator](/python/api/azuremlftk/ftk.model_selection.cross_validation.rollingoriginvalidator) faz a validação cruzada-, respeitando o que seria e não deve ser conhecido numa estrutura de previsão. 

A figura abaixo, cada quadrado representa dados de um ponto de tempo. Quadrados azuis na representam o treinamento e quadrados laranja representam testes em cada subconjuntos de validação. Dados de testes deverão ser provenientes os pontos de tempo após o ponto de tempo de treinamento maior. Caso contrário, os dados futuros for vazando sobre os dados de treinamento, fazendo com que a avaliação de modelo para se tornar inválido. 
![PNG](./media/how-to-build-deploy-forecast-models/cv_figure.PNG)

**Varrimento de parâmetro**  
O [TSGridSearchCV](/python/api/azuremlftk/ftk.model_selection.search.tsgridsearchcv) classe exaustivamente pesquisa sobre os valores de parâmetro especificado e utiliza `RollingOriginValidator` para avaliar o desempenho de parâmetro para encontrar os melhores parâmetros.


```python
# Set up the `RollingOriginValidator` to do 2 folds of rolling origin cross-validation
rollcv = RollingOriginValidator(n_splits=2)
randomforest_model_for_cv = RegressionForecaster(estimator=RandomForestRegressor(),
                                                 make_grain_features=False)

# Set up our parameter grid and feed it to our grid search algorithm
param_grid_rf = {'estimator__n_estimators': np.array([10, 50, 100])}
grid_cv_rf = TSGridSearchCV(randomforest_model_for_cv, param_grid_rf, cv=rollcv)

# fit and predict
randomforest_cv_fitted= grid_cv_rf.fit(train_feature_tsdf, y=train_feature_tsdf.ts_value)
print('Best paramter: {}'.format(randomforest_cv_fitted.best_params_))
```

    Best paramter: {'estimator__n_estimators': 100}
    

**Varrimento de modelo**  
O `BestOfForecaster` classe seleciona o modelo com o melhor desempenho de uma lista de modelos tendo em conta. Semelhante à `TSGridSearchCV`, ele também usa RollingOriginValidator para cruzada avaliação de validação e o desempenho.  
Aqui podemos passar uma lista de dois modelos para demonstrar a utilização de `BestOfForecaster`


```python
best_of_forecaster = BestOfForecaster(forecaster_list=[('naive', naive_model), 
                                                       ('random_forest', random_forest_model)])
best_of_forecaster_fitted = best_of_forecaster.fit(train_feature_tsdf,
                                                   validator=RollingOriginValidator(n_step=20, max_horizon=40))
best_of_forecaster_prediction = best_of_forecaster_fitted.predict(test_feature_tsdf)
best_of_forecaster_prediction.head()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th>PointForecast</th>
      <th>DistributionForecast</th>
      <th>Quantidade</th>
    </tr>
    <tr>
      <th>WeekLastDay</th>
      <th>Store</th>
      <th>marca</th>
      <th>ForecastOriginTime</th>
      <th>modelName</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1992-01-08 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <th>1992-01-01 23:59:59</th>
      <th>random_forest</th>
      <td>9299.20</td>
      <td>&lt;scipy.Stats._distn_infrastructure.rv_frozen s...</td>
      <td>11712.00</td>
    </tr>
    <tr>
      <th>1992-01-15 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <th>1992-01-01 23:59:59</th>
      <th>random_forest</th>
      <td>10259.20</td>
      <td>&lt;scipy.Stats._distn_infrastructure.rv_frozen s...</td>
      <td>4032.00</td>
    </tr>
    <tr>
      <th>1992-01-22 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <th>1992-01-01 23:59:59</th>
      <th>random_forest</th>
      <td>6828.80</td>
      <td>&lt;scipy.Stats._distn_infrastructure.rv_frozen s...</td>
      <td>6336.00</td>
    </tr>
    <tr>
      <th>1992-01-29 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <th>1992-01-01 23:59:59</th>
      <th>random_forest</th>
      <td>16633.60</td>
      <td>&lt;scipy.Stats._distn_infrastructure.rv_frozen s...</td>
      <td>13632.00</td>
    </tr>
    <tr>
      <th>1992-02-05 23:59:59</th>
      <th>2</th>
      <th>dominicks</th>
      <th>1992-01-01 23:59:59</th>
      <th>random_forest</th>
      <td>12774.40</td>
      <td>&lt;scipy.Stats._distn_infrastructure.rv_frozen s...</td>
      <td>45120.00</td>
    </tr>
  </tbody>
</table>



**Criar o pipeline final**   
Agora que identificou o melhor modelo, pode criar e ajustar o seu pipeline final com apresentam vários transformadores todos os e o melhor modelo. 


```python
random_forest_model_final = RegressionForecaster(estimator=RandomForestRegressor(100),make_grain_features=False)
pipeline_ml.add_pipeline_step('random_forest_estimator', random_forest_model_final)
pipeline_ml_fitted = pipeline_ml.fit(train_imputed_tsdf)
final_prediction = pipeline_ml_fitted.predict(test_tsdf)
final_median_ape = final_prediction.calc_error(err_name='MedianAPE', err_fun=calc_median_ape)
print('Median of APE of final pipeline: {0}'.format(final_median_ape))
```

    F1 2018-05-04 11:07:04,108 INFO azureml.timeseries - pipeline fit started. 
    F1 2018-05-04 11:07:43,121 INFO azureml.timeseries - pipeline fit finished. Time elapsed 0:00:39.012880
    F1 2018-05-04 11:07:43,136 INFO azureml.timeseries - pipeline predict started. 
    F1 2018-05-04 11:08:03,564 INFO azureml.timeseries - pipeline predict finished. Time elapsed 0:00:20.428147
    Median of APE of final pipeline: 42.54336821266968
    

## <a name="visualization"></a>Visualização
O `ForecastDataFrame` classe fornece funções de desenho para visualizar e analisar os resultados de previsão. Utilize os gráficos comumente usados com os seus dados. Consulte o bloco de notas do exemplo abaixo sobre funções de desenho para todas as funções disponíveis. 

O `show_error` representa a função de métricas de desempenho agregadas por uma coluna arbitrária. Por predefinição, o `show_error` função agrega pela `grain_colnames` do `ForecastDataFrame`. Muitas vezes é útil identificar os detalhamento/grupos com o desempenho melhor ou pior, especialmente quando tem um grande número de série de tempo. O `performance_percent` argumento de `show_error` permite-lhe especificar um intervalo de desempenho e desenhar o erro de um subconjunto de Detalhamento/grupos.

Desenhar o detalhamento com o desempenho de 5% da parte inferior, ou seja, principais 5% MedianAPE


```python
fig, ax = best_of_forecaster_prediction.show_error(err_name='MedianAPE', err_fun=calc_median_ape, performance_percent=(0.95, 1))
```

![PNG](./media/how-to-build-deploy-forecast-models/output_59_0.png)


Desenhar o detalhamento com o % 5 principal de desempenho, ou seja, inferior a 5% MedianAPE.


```python
fig, ax = best_of_forecaster_prediction.show_error(err_name='MedianAPE', err_fun=calc_median_ape, performance_percent=(0, 0.05))
```


![PNG](./media/how-to-build-deploy-forecast-models/output_61_0.png)


Depois de ter uma ideia do desempenho global, pode querer explorar detalhamento individuais, especialmente aquelas que fosse ruim. O `plot_forecast_by_grain` método desenha previsão versus real de Detalhamento especificado. Aqui, vamos desenhar o intervalo de agregação com o melhor desempenho e o intervalo de agregação com o pior desempenho detetados no `show_error` plotagem.


```python
fig_ax = best_of_forecaster_prediction.plot_forecast_by_grain(grains=[(33, 'tropicana'), (128, 'minute.maid')])
```


![PNG](./media/how-to-build-deploy-forecast-models/output_63_0.png)



![PNG](./media/how-to-build-deploy-forecast-models/output_63_1.png)



## <a name="additional-notebooks"></a>Blocos de notas adicionais
Para obter uma descrição mais aprofundada sobre as principais funcionalidades da AMLPF, consulte os seguintes blocos de notas com mais detalhes e exemplos de cada funcionalidade:  
[Bloco de notas no TimeSeriesDataFrame](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Introduction_to_TimeSeriesDataFrames.ipynb)  
[Bloco de notas na preparação de dados](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Data_Wrangling_Sample.ipynb)  
[Bloco de notas no apresentam vários transformadores](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Forecast_Package_Transforms.ipynb)  
[Bloco de notas em modelos](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/AMLPF_models_sample_notebook.ipynb)  
[Bloco de notas no cruzada validação](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Time_Series_Cross_Validation.ipynb)  
[Bloco de notas no desfasamento transformador e OriginTime](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Constructing_Lags_and_Explaining_Origin_Times.ipynb)  
[Bloco de notas sobre as funções de desenho](https://azuremlftkrelease.blob.core.windows.net/samples/feature_notebooks/Plotting_Functions_in_AMLPF.ipynb)

## <a name="operationalization"></a>Operacionalização

Nesta secção, implemente um pipeline como um serviço web do Azure Machine Learning e seu uso para preparação e classificação.
Atualmente, não estejam equipados lá pipelines apenas são suportadas para implementação. O serviço web implementado de classificação retrains o modelo e gera as previsões sobre novos dados.

### <a name="set-model-deployment-parameters"></a>Definir parâmetros de implementação do modelo

Altere os parâmetros seguintes para seus próprios valores. Certifique-se de que o seu ambiente do Azure Machine Learning, a conta de gestão de modelos e o grupo de recursos estão localizados na mesma região.


```python
azure_subscription = '<subscription name>'

# Two deployment modes are supported: 'local' and 'cluster'. 
# 'local' deployment deploys to a local docker container.
# 'cluster' deployment deploys to a Azure Container Service Kubernetes-based cluster
deployment_type = '<deployment mode>'

# The deployment environment name. 
# This could be an existing environment or a new environment to be created automatically.
aml_env_name = '<deployment env name>'

# The resource group that contains the Azure resources related to the AML environment.
aml_env_resource_group = '<env resource group name>'

# The location where the Azure resources related to the AML environment are located at.
aml_env_location = '<env resource location>'

# The AML model management account name. This could be an existing model management account a new model management 
# account to be created automatically. 
model_management_account_name = '<model management account name>'

# The resource group that contains the Azure resources related to the model management account.
model_management_account_resource_group = '<model management account resource group>'

# The location where the Azure resources related to the model management account are located at.
model_management_account_location = '<model management account location>'

# The name of the deployment/web service.
deployment_name = '<web service name>'

# The directory to store deployment related files, such as pipeline pickle file, score script, 
# and conda dependencies file. 
deployment_working_directory = '<local working directory>'
```

### <a name="define-the-azure-machine-learning-environment-and-deployment"></a>Definir o ambiente do Azure Machine Learning e a implantação


```python
aml_settings = AMLSettings(azure_subscription=azure_subscription,
                     env_name=aml_env_name, 
                     env_resource_group=aml_env_resource_group,
                     env_location=aml_env_location, 
                     model_management_account_name=model_management_account_name, 
                     model_management_account_resource_group=model_management_account_resource_group,
                     model_management_account_location=model_management_account_location,
                     cluster=deployment_type)

random_forest_model_deploy = RegressionForecaster(estimator=RandomForestRegressor(),make_grain_features=False)
pipeline_deploy = AzureMLForecastPipeline([('drop_columns', column_dropper), 
                                           ('fillna_imputer', fillna_imputer),
                                           ('time_index_featurizer', time_index_featurizer),
                                           ('random_forest_estimator', random_forest_model_deploy)
                                          ])

aml_deployment = ForecastWebserviceFactory(deployment_name=deployment_name,
                                           aml_settings=aml_settings, 
                                           pipeline=pipeline_deploy,
                                           deployment_working_directory=deployment_working_directory,
                                           ftk_wheel_loc='https://azuremlftkrelease.blob.core.windows.net/dailyrelease/azuremlftk-0.1.18165.29a1-py3-none-any.whl')
```

### <a name="create-the-web-service"></a>Criar o serviço web


```python
# This step can take 5 to 20 minutes
aml_deployment.deploy()
```

### <a name="score-the-web-service"></a>O serviço web de pontuação

Para classificar um pequeno conjunto de dados, utilize o [pontuação](/python/api/azuremlftk/ftk.operationalization.forecast_web_service.forecastwebservice#score) chamada de método para submeter um serviço da web para todos os dados.


```python
# Need to add empty prediction columns to the validation data frame and create a ForecastDataFrame.
# The scoring API will be updated in later versions to take TimeSeriesDataFrame directly. 
validate_tsdf = test_tsdf.assign(PointForecast=0.0, DistributionForecast=np.nan)
validate_fcast = ForecastDataFrame(validate_tsdf, pred_point='PointForecast', pred_dist='DistributionForecast')

# Define Score Context
score_context = ScoreContext(input_training_data_tsdf=train_imputed_tsdf,
                             input_scoring_data_fcdf=validate_fcast, 
                             pipeline_execution_type='train_predict')

# Get deployed web service
aml_web_service = aml_deployment.get_deployment()

# Score the web service
results = aml_web_service.score(score_context=score_context)
```

Para classificar um conjunto de dados grandes, utilize o [classificação paralela](/python/api/azuremlftk/ftk.operationalization.forecast_web_service.forecastwebservice#score-parallel) modo para submeter o serviço web de vários chama, uma para cada grupo de dados.

```python
results = aml_web_service.score(score_context=score_context, method='parallel')
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o pacote do Azure Machine Learning para previsão nestes artigos:

+ Leitura a [descrição geral do pacote](https://aka.ms/aml-packages/forecasting).

+ Explore os [referenciar docs](https://aka.ms/aml-packages/forecasting) para este pacote.

+ Saiba mais sobre [outros pacotes de Python para o Azure Machine Learning](reference-python-package-overview.md).
