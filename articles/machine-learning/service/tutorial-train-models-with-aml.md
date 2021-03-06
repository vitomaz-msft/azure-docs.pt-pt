---
title: 'Tutorial: Preparar um modelo de classificação de imagens com o serviço do Azure Machine Learning'
description: Este tutorial mostra como utilizar o serviço do Azure Machine Learning para preparar um modelo de classificação de imagens com scikit-learn num bloco de notas do Jupyter em Python. Este tutorial é a primeira parte de uma série composta por duas partes.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: hning86
ms.author: haining
ms.reviewer: sgilley
ms.date: 11/21/2018
ms.openlocfilehash: 067a8deb935fb8a49d72c6ce441e8d9760c5390c
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52283660"
---
# <a name="tutorial-1-train-an-image-classification-model-with-azure-machine-learning-service"></a>Tutorial 1: Preparar um modelo de classificação de imagens com o serviço do Azure Machine Learning

Neste tutorial, vai preparar um modelo de machine learning, quer localmente, quer em recursos de computação remotos. Vai utilizar o fluxo de trabalho de preparação e implementação do serviço do Azure Machine Learning (pré-visualização) num bloco de notas do Jupyter em Python.  Depois, pode utilizar o bloco de notas como um modelo para preparar o seu próprio modelo de machine learning com os seus dados. Este tutorial é a **primeira parte de uma série composta por duas partes**.  

O tutorial utiliza o conjunto de dados [MNIST](http://yann.lecun.com/exdb/mnist/) e [scikit-learn](http://scikit-learn.org) com o serviço do Azure Machine Learning para preparar uma regressão logística simples.  O MNIST é um conjunto de dados popular que consiste em 70 000 imagens em tons de cinzento. Cada imagem é um dígito escrito à mão de 28 x 28 pixéis e representam um número de 0 a 9. O objetivo é criar um classificador multiclasses para identificar o dígito que uma determinada imagem representa. 

Aprenda a:

> [!div class="checklist"]
> * Configurar o ambiente de desenvolvimento
> * Aceder e examinar os dados
> * Utilizar a popular biblioteca de machine learning de scikit-learn para preparar uma regressão logística simples localmente 
> * Preparar vários modelos num cluster remoto
> * Rever os resultados da preparação e registar o melhor modelo

Na [parte dois deste tutorial](tutorial-deploy-models-with-aml.md), mais adiante, vai aprender a selecionar um modelo e implementá-lo. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://aka.ms/AMLfree) antes de começar.

>[!NOTE]
> Código neste artigo foi testado com o Azure Machine Learning SDK versão 0.1.79

## <a name="get-the-notebook"></a>Obter o bloco de notas

Para sua comodidade, este tutorial está disponível como [bloco de notas do Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb). Execute o bloco de notas `01.train-models.ipynb` no Azure Notebooks ou no seu próprio servidor Jupyter Notebook.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

>[!NOTE]
> Este tutorial foi testado com o Azure Machine Learning SDK versão 0.1.74 

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Toda a configuração para o seu trabalho de desenvolvimento pode ser feita num bloco de notas Python.  A configuração inclui:

* Importar pacotes Python
* Ligar a uma área de trabalho para permitir a comunicação entre o computador local e os recursos remotos
* Criar uma experimentação para acompanhar todas as execuções
* Criar um destino de computação remoto a utilizar na preparação

### <a name="import-packages"></a>Importar pacotes

Importe os pacotes Python de que precisa nesta sessão. Apresente também a versão do SDK do Azure Machine Learning.

```python
%matplotlib inline
import numpy as np
import matplotlib
import matplotlib.pyplot as plt

import azureml
from azureml.core import Workspace, Run

# check core SDK version number
print("Azure ML SDK Version: ", azureml.core.VERSION)
```

### <a name="connect-to-workspace"></a>Ligar à área de trabalho

Crie um objeto de área de trabalho a partir da área de trabalho existente. `Workspace.from_config()` lê o ficheiro **config.json** e carrega os detalhes para um objeto com o nome `ws`.

```python
# load workspace configuration from the config.json file in the current folder.
ws = Workspace.from_config()
print(ws.name, ws.location, ws.resource_group, ws.location, sep = '\t')
```

### <a name="create-experiment"></a>Criar a experimentação

Crie uma experimentação para acompanhar as execuções na sua área de trabalho. Uma área de trabalho pode ter várias experimentações. 

```python
experiment_name = 'sklearn-mnist'

from azureml.core import Experiment
exp = Experiment(workspace=ws, name=experiment_name)
```

### <a name="create-remote-compute-target"></a>Criar o destino de computação remoto

O Azure ML geridos computação é um serviço gerido que permite que os cientistas de dados preparar modelos de machine learning em clusters de máquinas virtuais do Azure, incluindo VMs com suporte GPU.  Neste tutorial, vai criar um cluster de computação do Azure geridos como o seu ambiente de treinamento. Se o cluster ainda não existir no seu ambiente, este código cria um por si. 

 **A criação do cluster demora, aproximadamente, cinco minutos.** Se o cluster já estiver na área de trabalho, este código utiliza-o e ignora o processo de criação.


```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
compute_name = os.environ.get("BATCHAI_CLUSTER_NAME", "cpucluster")
compute_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 0)
compute_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 4)

# This example uses CPU VM. For using GPU VM, set SKU to STANDARD_NC6
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_D2_V2")


if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('found compute target. just use it. ' + compute_name)
else:
    print('creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size,
                                                                min_nodes = compute_min_nodes, 
                                                                max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Tem agora os pacotes e os recursos de computação necessários para preparar um modelo na cloud. 

## <a name="explore-data"></a>Explorar dados

Antes de preparar um modelo, tem de compreender os dados utilizados na preparação.  Também tem de os copiar para a cloud, de modo a que o seu ambiente de preparação na cloud possa aceder aos mesmos.  Nesta secção, vai aprender a:

* Transferir o conjunto de dados MNIST
* Apresentar algumas imagens de exemplo
* Carregar os dados para a cloud

### <a name="download-the-mnist-dataset"></a>Transferir o conjunto de dados MNIST

Transfira o conjunto de dados MNIST e guarde os ficheiros num diretório `data` localmente.  São transferidas imagens e etiquetas da preparação e do teste.


```python
import os
import urllib.request

os.makedirs('./data', exist_ok = True)

urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename='./data/train-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename='./data/train-labels.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename='./data/test-images.gz')
urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename='./data/test-labels.gz')
```

### <a name="display-some-sample-images"></a>Apresentar algumas imagens de exemplo

Carregue os ficheiros comprimidos para matrizes `numpy`. Em seguida, utilize `matplotlib` para desenhar 30 imagens aleatórias do conjunto de dados com as respetivas etiquetas acima das mesmas. Tenha em atenção de que este passo requer uma `load_data` função que está incluída num `util.py` ficheiro. Este ficheiro está incluído na pasta de exemplo. Confirme que está na mesma pasta que este bloco de notas. O `load_data` função simplesmente analisa os arquivos compactados em matrizes de numpy.



```python
# make sure utils.py is in the same directory as this code
from utils import load_data

# note we also shrink the intensity values (X) from 0-255 to 0-1. This helps the model converge faster.
X_train = load_data('./data/train-images.gz', False) / 255.0
y_train = load_data('./data/train-labels.gz', True).reshape(-1)

X_test = load_data('./data/test-images.gz', False) / 255.0
y_test = load_data('./data/test-labels.gz', True).reshape(-1)

# now let's show some randomly chosen images from the traininng set.
count = 0
sample_size = 30
plt.figure(figsize = (16, 6))
for i in np.random.permutation(X_train.shape[0])[:sample_size]:
    count = count + 1
    plt.subplot(1, sample_size, count)
    plt.axhline('')
    plt.axvline('')
    plt.text(x=10, y=-10, s=y_train[i], fontsize=18)
    plt.imshow(X_train[i].reshape(28, 28), cmap=plt.cm.Greys)
plt.show()
```

Um exemplo aleatório de imagens apresenta:

![exemplo aleatório de imagens](./media/tutorial-train-models-with-aml/digits.png)

Agora, tem uma ideia do aspeto destas imagens e do resultado previsto da predição.

### <a name="upload-data-to-the-cloud"></a>Carregar os dados para a cloud

Agora, vamos tornar os dados acessíveis remotamente mediante o carregamento dos mesmos do seu computador local para o Azure, de modo a ser possível aceder aos mesmos para a preparação remota. O arquivo de dados é uma construção prática associada à sua área de trabalho e que lhe permite carregar/transferir dados e interagir com os mesmos a partir dos seus destinos de computação remotos. É suportado pela conta de armazenamento de blobs do Azure.

Os ficheiros MNIST são carregados para um diretório denominado `mnist`, na raiz do arquivo de dados.

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)

ds.upload(src_dir='./data', target_path='mnist', overwrite=True, show_progress=True)
```
Tem agora tudo aquilo de que precisa para começar a preparar um modelo. 

## <a name="train-a-local-model"></a>Preparar um modelo de local

Preparar um modelo de regressão logística simples usando scikit-saiba localmente.

**A preparação local pode demorar alguns minutos**, consoante a configuração do seu computador.

```python
%%time
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
```

Em seguida, utilize o conjunto de testes para fazer predições e calcule a precisão. 

```python
y_hat = clf.predict(X_test)
print(np.average(y_hat == y_test))
```

A precisão do modelo local apresenta:

`0.9202`

Com apenas algumas linhas de código, tem uma precisão de 92%.

## <a name="train-on-a-remote-cluster"></a>Preparar num cluster remoto

Agora, pode criar um modelo com uma taxa de regularização diferente para expandir este modelo simples. Desta vez, o modelo vai ser preparado num recurso remoto.  

Para esta tarefa, submeta o trabalho para o cluster de preparação remoto que configurou anteriormente.  Para submeter um trabalho, tem de:
* Criar um diretório
* Criar um script de preparação
* Criar um objeto de Calculadora
* Submeter o trabalho 

### <a name="create-a-directory"></a>Criar um diretório

Crie um diretório para fornecer o código necessário do computador para o recurso remoto.

```python
import os
script_folder = './sklearn-mnist'
os.makedirs(script_folder, exist_ok=True)
```

### <a name="create-a-training-script"></a>Criar um script de preparação

Para submeter o trabalho para o cluster, crie primeiro um script de preparação. Execute o código seguinte para criar o script de preparação denominado `train.py` no diretório que acabou de criar. Esta preparação adiciona uma taxa de regularização ao algoritmo de preparação, pelo que produz um modelo ligeiramente diferente do da versão local.

```python
%%writefile $script_folder/train.py

import argparse
import os
import numpy as np

from sklearn.linear_model import LogisticRegression
from sklearn.externals import joblib

from azureml.core import Run
from utils import load_data

# let user feed in 2 parameters, the location of the data files (from datastore), and the regularization rate of the logistic regression model
parser = argparse.ArgumentParser()
parser.add_argument('--data-folder', type=str, dest='data_folder', help='data folder mounting point')
parser.add_argument('--regularization', type=float, dest='reg', default=0.01, help='regularization rate')
args = parser.parse_args()

data_folder = os.path.join(args.data_folder, 'mnist')
print('Data folder:', data_folder)

# load train and test set into numpy arrays
# note we scale the pixel intensity values to 0-1 (by dividing it with 255.0) so the model can converge faster.
X_train = load_data(os.path.join(data_folder, 'train-images.gz'), False) / 255.0
X_test = load_data(os.path.join(data_folder, 'test-images.gz'), False) / 255.0
y_train = load_data(os.path.join(data_folder, 'train-labels.gz'), True).reshape(-1)
y_test = load_data(os.path.join(data_folder, 'test-labels.gz'), True).reshape(-1)
print(X_train.shape, y_train.shape, X_test.shape, y_test.shape, sep = '\n')

# get hold of the current run
run = Run.get_context()

print('Train a logistic regression model with regularizaion rate of', args.reg)
clf = LogisticRegression(C=1.0/args.reg, random_state=42)
clf.fit(X_train, y_train)

print('Predict the test set')
y_hat = clf.predict(X_test)

# calculate accuracy on the prediction
acc = np.average(y_hat == y_test)
print('Accuracy is', acc)

run.log('regularization rate', np.float(args.reg))
run.log('accuracy', np.float(acc))

os.makedirs('outputs', exist_ok=True)
# note file saved in the outputs folder is automatically uploaded into experiment record
joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')
```

Repare que o script obtém dados e guarda modelos:

+ O script de preparação lê um argumento para localizar o diretório que contém os dados.  Quando submete o trabalho posteriormente, tem de apontar para o arquivo de dados deste argumento: `parser.add_argument('--data-folder', type=str, dest='data_folder', help='data directory mounting point')`
    
+ O script de preparação guarda o modelo num diretório denominado “outputs”. <br/>
`joblib.dump(value=clf, filename='outputs/sklearn_mnist_model.pkl')`<br/>
Tudo o que for escrito neste diretório é carregado automaticamente para a sua área de trabalho. Mais à frente no tutorial, vai aceder ao seu modelo a partir deste diretório.

O ficheiro `utils.py` é referenciado a partir do script de preparação para carregar o conjunto de dados devidamente.  Copie este script para a pasta de scripts, de modo a que possa ser acedido, juntamente com o script de preparação, no recurso remoto.


```python
import shutil
shutil.copy('utils.py', script_folder)
```


### <a name="create-an-estimator"></a>Criar simulador

Para submeter a execução, é utilizado um objeto de simulador.  Para criar o seu simulador, execute o código seguinte para definir:

* O nome do objeto de simulador, `est`
* O diretório que contém os seus scripts. Todos os ficheiros neste diretório são carregados para os nós do cluster, para execução. 
* O destino de computação.  Neste caso, vai utilizar o cluster do Batch AI que criou
* O nome do script de preparação, train.py
* Os parâmetros necessários do script de preparação 
* Os pacotes Python necessários para a preparação

Neste tutorial, esse destino é o cluster do Batch AI. Todos os ficheiros na pasta de script são carregados os nós de cluster para execução. data_folder é definida para utilizar o arquivo de dados (`ds.as_mount()`).

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

est = Estimator(source_directory=script_folder,
                script_params=script_params,
                compute_target=compute_target,
                entry_script='train.py',
                conda_packages=['scikit-learn'])
```


### <a name="submit-the-job-to-the-cluster"></a>Submeter o trabalho para o cluster

Submeta o objeto de simulador para executar a experimentação.

```python
run = exp.submit(config=est)
run
```

Uma vez que a chamada é assíncrona, devolve o estado **A preparar** ou **Em execução**, assim que o trabalho for iniciado.

## <a name="monitor-a-remote-run"></a>Monitorizar uma execução remota

No total, a primeira execução demora **aproximadamente dez minutos**. Contudo, nas execuções subsequentes, na condição de que as dependências do script não se alterem, a mesma imagem é reutilizada, pelo que o arranque do contentor é muito mais rápido.

Eis o que está a acontecer enquanto espera:

- **Criação da imagem**: é criada uma imagem do Docker que corresponde ao ambiente Python especificado pelo simulador. A imagem é carregada para a área de trabalho. A criação e o carregamento da imagem demora **cerca de cinco minutos**. 

  Esta fase acontece uma vez para cada ambiente Python, pois o contentor é colocado em cache nas execuções seguintes.  Durante a criação da imagem, os registos são transmitidos para o histórico de execuções. Pode utilizar esses registos para monitorizar o progresso da criação da imagem.

- **Dimensionamento**: se o cluster remoto precisar de mais nós para realizar a execução que está disponível atualmente, são adicionados mais nós de forma automática. Regra geral, o dimensionamento **demora cerca de cinco minutos.**

- **Execução**: nesta fase, os scripts e os ficheiros necessários são enviados para o destino de computação, depois os arquivos de dados são montados/copiados e, por fim, é executado entry_script. Enquanto o trabalho está em execução, stdout e o diretório ./logs são transmitidos para o histórico de execuções. Pode utilizar esses registos para monitorizar o progresso da execução.

- **Pós-processamento**: o diretório ./outputs da execução é copiado para o histórico de execuções na área de trabalho, para que possa aceder a esses resultados.


Pode verificar o progresso de um trabalho em execução de várias formas. Este tutorial utiliza um widget Jupyter, bem como um método `wait_for_completion`. 

### <a name="jupyter-widget"></a>Widget Jupyter

Veja o progresso da execução com um widget Jupyter.  Tal como a submissão da execução, o widget é assíncrono e disponibiliza atualizações dinâmicas a cada 10 a 15 segundos até à conclusão do trabalho.


```python
from azureml.widgets import RunDetails
RunDetails(run).show()
```

Segue-se um instantâneo estático do widget mostrado na parte final da preparação:

![widget de bloco de notas](./media/tutorial-train-models-with-aml/widget.png)

### <a name="get-log-results-upon-completion"></a>Obter resultados do registo após a conclusão

A preparação e a monitorização do modelo são feitas em segundo plano. Aguarde até que o modelo tenha concluído a preparação antes de executar mais código. Utilize `wait_for_completion` para ver quando a preparação do modelo estiver concluída. 


```python
run.wait_for_completion(show_output=False) # specify True for a verbose log
```

### <a name="display-run-results"></a>Apresentar os resultados da execução

Tem agora um modelo preparado num cluster remoto.  Obtenha a precisão do modelo:

```python
print(run.get_metrics())
```
A saída mostra que a precisão do modelo remoto é ligeiramente superior à do modelo local, devido à adição da taxa de regularização durante a preparação.  

`{'regularization rate': 0.8, 'accuracy': 0.9204}`

O próximo tutorial irá explorar esse modelo em mais detalhes.

## <a name="register-model"></a>Registar o modelo

O último passo no script de preparação escreveu o ficheiro `outputs/sklearn_mnist_model.pkl` num diretório denominado `outputs` na VM do cluster no qual o trabalho é executado. `outputs` é um diretório especial no sentido em que todos os conteúdos no mesmo são carregados automaticamente para a sua área de trabalho.  Esses conteúdos aparecem no registo de execução na experimentação, na área de trabalho. Consequentemente, o ficheiro do modelo também está disponível na área de trabalho.

Pode ver os ficheiros associados a essa execução.

```python
print(run.get_file_names())
```

Para poder (ou qualquer outro colaborador) consultar, examinar e implementar este modelo mais tarde, registe-o na área de trabalho.

```python
# register model 
model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
print(model.name, model.id, model.version, sep = '\t')
```

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Também pode eliminar apenas o cluster da Computação Gerida do Azure. No entanto, uma vez que o dimensionamento automático está ativado e o mínimo do cluster é 0, este recurso em particular não incorrerá em despesas de computação adicionais quando não é utilizado.


```python
# optionally, delete the Azure Managed Compute cluster
compute_target.delete()
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial do serviço do Azure Machine Learning, utilizou o Python para:

> [!div class="checklist"]
> * Configurar o ambiente de desenvolvimento
> * Aceder e examinar os dados
> * Utilizar a popular biblioteca de machine learning de scikit-learn para preparar uma regressão logística simples localmente
> * Preparar vários modelos num cluster remoto
> * Rever os detalhes da preparação e registar o melhor modelo

Está pronto para implementar este modelo registado com as instruções da segunda parte da série do tutorial:

> [!div class="nextstepaction"]
> [Tutorial 2 - Deploy models](tutorial-deploy-models-with-aml.md) (Tutorial 2 - Implementar modelos)
