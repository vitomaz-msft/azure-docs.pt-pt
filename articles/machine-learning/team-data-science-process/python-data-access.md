---
title: Aceder a conjuntos de dados com a biblioteca de cliente Python do Machine Learning | Documentos da Microsoft
description: Instalar e utilizar a biblioteca de cliente Python para aceder e gerir dados do Azure Machine Learning com segurança a partir de um ambiente local do Python.
services: machine-learning
documentationcenter: python
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath
ms.openlocfilehash: 496ffdcb5cc2e7b208a53dc8c1d8ba4c0818945f
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44719975"
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Utilizar a biblioteca de clientes de Python do Azure Machine Learning para aceder a conjuntos de dados com Python
A pré-visualização da biblioteca de clientes do Microsoft Azure Machine Learning Python pode permitir o acesso seguro aos conjuntos de dados do Azure Machine Learning de um ambiente de Python local e permite a criação e gestão de conjuntos de dados numa área de trabalho.

Este tópico fornece instruções sobre como:

* instalar a biblioteca de cliente Python do Machine Learning 
* aceder e carregar conjuntos de dados, incluindo instruções sobre como obter autorização para aceder a conjuntos de dados do Azure Machine Learning do seu ambiente local do Python
* conjuntos de dados intermediários do acesso de experimentações
* Utilize a biblioteca de cliente de Python para enumerar conjuntos de dados, aceder aos metadados, ler o conteúdo de um conjunto de dados, criar novos conjuntos de dados e atualizar conjuntos de dados existentes

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Pré-requisitos
A biblioteca de cliente Python foi testada em seguintes ambientes:

* Windows, Mac e Linux
* Python 2.7, 3.3 e 3.4

Ele tem uma dependência nos seguintes pacotes:

* pedidos
* python-dateutil
* pandas

Recomendamos que utilize uma distribuição do Python, tal como [Anaconda](http://continuum.io/downloads#all) ou [Canopy](https://store.enthought.com/downloads/), que vêm com o Python, IPython e instalado de três pacotes listados acima. Embora o IPython não é estritamente necessário, é um ótimo ambiente para manipular e visualizar dados de forma interativa.

### <a name="installation"></a>Como instalar a biblioteca de cliente Python do Azure Machine Learning
A biblioteca de cliente Python do Azure Machine Learning também tem de estar instalada para concluir as tarefas descritas neste tópico. Está disponível a partir da [índice de pacote do Python](https://pypi.python.org/pypi/azureml). Para instalá-lo no seu ambiente de Python, execute o seguinte comando do seu ambiente de Python local:

    pip install azureml

Em alternativa, pode transferir e instalar a partir das fontes de no [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).

    python setup.py install

Se tiver o git instalado no seu computador, pode utilizar o pip para instalar diretamente a partir do repositório de git:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <a name="datasetAccess"></a>Utilizar fragmentos de código do Studio para aceder a conjuntos de dados
A biblioteca de cliente Python oferece acesso programático a seus conjuntos de dados existentes de experimentações que tenham sido executadas.

A partir da interface de web do Studio, pode gerar trechos de código que incluem todas as informações necessárias para transferir e anular a serialização de conjuntos de dados como objetos de pacote de dados de pandas no seu computador local.

### <a name="security"></a>Segurança para acesso a dados
Os fragmentos de código fornecidos pelo Studio para utilização com a biblioteca de cliente Python inclui o id de área de trabalho e a autorização token. Estes disponibilizar acesso total à sua área de trabalho e têm de ser protegidos, como uma palavra-passe.

Por motivos de segurança, a funcionalidade de trechos de código só está disponível para os utilizadores que têm a sua função definida como **proprietário** para a área de trabalho. Sua função é apresentada no Azure Machine Learning Studio no **USUÁRIOS** página sob **definições**.

![Segurança][security]

Se a sua função não está definida como **proprietário**, pode de qualquer pedido para ser novamente convidado como um proprietário ou peça ao proprietário da área de trabalho para fornecer a o trecho de código.

Para obter o token de autorização, pode fazer o seguinte:

* Pedir um token de um proprietário. Os proprietários podem acessar os tokens de autorização da página de definições do seu espaço de trabalho no Studio. Selecione **configurações** no painel esquerdo e clique **TOKENS de autorização** para ver os tokens primários e secundários.  Embora principal ou tokens de autorização secundário podem ser usados no trecho de código, recomenda-se que os proprietários de partilham apenas tokens de autorização secundário.

![Tokens de autorização](./media/python-data-access/ml-python-access-settings-tokens.png)

* Peça para ser promovido a função de proprietário.  Para fazer isso, um proprietário atual do espaço de trabalho precisa primeiro remover a área de trabalho, em seguida, voltar convidá-lo a ele como proprietário.

Assim que os desenvolvedores de obteve o id de área de trabalho e a autorização token, eles são capazes de acessar a área de trabalho usando o trecho de código, independentemente da respetiva função.

Tokens de autorização são geridos na **TOKENS de autorização** página sob **definições**. Pode voltar a gerá-los, mas este procedimento revoga o acesso ao token anterior.

### <a name="accessingDatasets"></a>Conjuntos de dados de acesso de um aplicativo local do Python
1. No Machine Learning Studio, clique em **conjuntos de dados** na barra de navegação à esquerda.
2. Selecione o conjunto de dados que pretende aceder. Pode selecionar qualquer um dos conjuntos de dados do **conjuntos de dados de meu** lista ou a partir da **exemplos** lista.
3. A partir da barra de ferramentas na parte inferior, clique em **gerar código de acesso a dados**. Se os dados estão num formato incompatível com a biblioteca de cliente de Python, este botão está desativado.
   
    ![Conjuntos de dados][datasets]
4. Selecione o fragmento de código a partir da janela que aparece e copie-o para a área de transferência.
   
    ![Código de acesso][dataset-access-code]
5. Cole o código no bloco de notas da sua aplicação Python local.
   
    ![Bloco de notas][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Conjuntos de dados intermediários do acesso de experimentações de Machine Learning
Após a execução uma experimentação no Machine Learning Studio, é possível acessar os conjuntos de dados intermediários de nós de saída de módulos. Conjuntos de dados intermediários são dados que foi criados e utilizados para obter os passos intermédios quando uma ferramenta de modelo tiver sido executada.

Conjuntos de dados intermediários podem ser acedidos, desde que o formato de dados é compatível com a biblioteca de cliente Python.

São suportados os seguintes formatos (constantes para estes estão no `azureml.DataTypeIds` classe):

* Texto sem formatação
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

Pode determinar o formato passando o mouse sobre um nó de saída do módulo. É apresentado junto com o nome de nó, numa descrição.

Alguns dos módulos, como o [Split] [ split] módulo, para um formato com o nome de saída `Dataset`, que não é suportado pela biblioteca de cliente Python.

![Formato de conjunto de dados][dataset-format]

Tem de utilizar um módulo de conversão, tal como [converter para CSV][convert-to-csv], para obter um resultado para um formato suportado.

![Formato de GenericCSV][csv-format]

Os passos seguintes mostram um exemplo que cria uma experimentação, executa-o e acessa o conjunto de dados intermediário.

1. Crie uma nova experimentação.
2. Inserir um **conjunto de dados de classificação de binário para adultos recenseamento receitas** módulo.
3. Inserir um [Split] [ split] módulo e ligue-se a entrada para a saída do módulo de conjunto de dados.
4. Inserir um [converter para CSV] [ convert-to-csv] módulo e ligue-se a entrada para um do [Split] [ split] saídas do módulo.
5. Guardar a experimentação, executá-la e esperar que ele termine a execução.
6. Clique no nó de saída no [converter para CSV] [ convert-to-csv] módulo.
7. Quando for apresentado o menu de contexto, selecione **gerar código de acesso a dados**.
   
    ![Menu de Contexto][experiment]
8. Selecione o fragmento de código e copiá-lo para a área de transferência a partir da janela que aparece.
   
    ![Código de acesso][intermediate-dataset-access-code]
9. Cole o código no seu bloco de notas.
   
    ![Bloco de notas][ipython-intermediate-dataset]
10. É possível visualizar os dados usando matplotlib. Isto é apresentado num histograma da coluna de idade:
    
    ![Histograma][ipython-histogram]

## <a name="clientApis"></a>Utilize a biblioteca de cliente Python do Machine Learning para aceder, ler, criar e gerir conjuntos de dados
### <a name="workspace"></a>Área de trabalho
A área de trabalho é o ponto de entrada para a biblioteca de cliente Python. Fornecer o `Workspace` classe com o id de área de trabalho e a autorização para criar uma instância do token:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Enumerar conjuntos de dados
Para enumerar todos os conjuntos de dados numa determinada área de trabalho:

    for ds in ws.datasets:
        print(ds.name)

Para enumerar apenas os criados pelo utilizador conjuntos de dados:

    for ds in ws.user_datasets:
        print(ds.name)

Para enumerar apenas os conjuntos de dados de exemplo:

    for ds in ws.example_datasets:
        print(ds.name)

Pode aceder a um conjunto de dados por nome (o que diferencia maiúsculas de minúsculas):

    ds = ws.datasets['my dataset name']

Ou pode acessá-lo pelo índice:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metadados
Conjuntos de dados tem metadados, além de conteúdo. (Conjuntos de dados intermediários são uma exceção a esta regra e não tem quaisquer metadados).

Alguns valores de metadados são atribuídos pelo usuário no momento de criação:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Outras são valores atribuídos pelo Azure ML:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Consulte o `SourceDataset` classe para obter mais informações sobre os metadados disponíveis.

### <a name="read-contents"></a>Ler o conteúdo
Os fragmentos de código fornecidos pelo Machine Learning Studio automaticamente transferir e anular a serialização de conjunto de dados para um objeto do pandas DataFrame. Isso é feito com o `to_dataframe` método:

    frame = ds.to_dataframe()

Se preferir transferir os dados não processados e efetuar a desserialização por conta própria, que é uma opção. No momento, esta é a única opção para formatos como "ARFF', não é possível anular a serialização da biblioteca de cliente Python.

Para ler o conteúdo como texto:

    text_data = ds.read_as_text()

Para ler o conteúdo como binário:

    binary_data = ds.read_as_binary()

Pode também simplesmente abrir um fluxo para o conteúdo:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Criar um novo conjunto de dados
A biblioteca de cliente Python permite-lhe carregar conjuntos de dados de seu programa de Python. Estes conjuntos de dados, em seguida, estão disponíveis para utilização na sua área de trabalho.

Se tiver os seus dados num pandas DataFrame, utilize o seguinte código:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Se os dados já são serializados, pode usar:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

A biblioteca de cliente Python é capaz de serializar uma pandas DataFrame para os seguintes formatos (constantes para estes estão no `azureml.DataTypeIds` classe):

* Texto sem formatação
* GenericCSV
* GenericTSV
* GenericCSVNoHeader
* GenericTSVNoHeader

### <a name="update-an-existing-dataset"></a>Atualizar um conjunto de dados existente
Se tentar carregar um novo conjunto de dados com um nome que corresponde a um conjunto de dados existente, obterá um erro de conflito.

Para atualizar um conjunto de dados existente, tem primeiro de obter uma referência ao conjunto de dados existente:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Em seguida, utilizar `update_from_dataframe` para serializar e substitua o conteúdo do conjunto de dados no Azure:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Se desejar serializar os dados num formato diferente, especifique um valor para o opcional `data_type_id` parâmetro.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Opcionalmente, pode definir uma nova descrição ao especificar um valor para o `description` parâmetro.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Opcionalmente, pode definir um novo nome, especificando um valor para o `name` parâmetro. De agora em diante, irá obter o conjunto de dados com o novo nome. O código a seguir atualiza os dados, nome e descrição.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

O `data_type_id`, `name` e `description` parâmetros são opcionais e predefinido para o valor anterior. O `dataframe` sempre é necessário o parâmetro.

Se os dados já são serializados, utilize `update_from_raw_data` em vez de `update_from_dataframe`. Se passar apenas `raw_data` em vez de `dataframe`, funciona de forma semelhante.

<!-- Images -->
[security]:./media/python-data-access/security.png
[dataset-format]:./media/python-data-access/dataset-format.png
[csv-format]:./media/python-data-access/csv-format.png
[datasets]:./media/python-data-access/datasets.png
[dataset-access-code]:./media/python-data-access/dataset-access-code.png
[ipython-dataset]:./media/python-data-access/ipython-dataset.png
[experiment]:./media/python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

