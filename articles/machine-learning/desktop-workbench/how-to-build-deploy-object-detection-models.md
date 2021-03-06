---
title: Criar e implementar um modelo de deteção de objeto com o pacote do Azure Machine Learning para imagem digitalizada.
description: Saiba como criar, formar, testar e implementar um modelo de deteção do objeto do computador visão com o pacote do Azure Machine Learning para imagem digitalizada.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 06/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 1451cdc5efcb62cff5c5d3725d5b15eb44be1755
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613669"
---
# <a name="build-and-deploy-object-detection-models-with-azure-machine-learning"></a>Criar e implementar modelos de deteção de objeto com o Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Neste artigo, saiba como utilizar **pacote do Azure Machine Learning para imagem digitalizada** para preparar, testar e implementar um [mais rápido do R-CNN](https://arxiv.org/abs/1506.01497) modelo de deteção de objeto. 

Um grande número de problemas do domínio de visão do computador pode ser resolvido com a deteção de objetos. Esses problemas incluem a criação de modelos que encontrar um número variável de objetos numa imagem. 

Ao criar e implementar este modelo com este pacote, que execute os seguintes passos:
1.  Criação de conjunto de dados
2.  Definição de modelo de rede Neurais profundas (DNN)
3.  Preparação de Modelos
4.  Avaliação e visualização
5.  Implementação do serviço Web
6.  Serviço Web de teste de carga

Neste exemplo, o TensorFlow for utilizado como a estrutura de aprendizagem profunda, treinamento é executado localmente num computador com a tecnologia de GPU, como o [VM de ciência de dados de aprendizagem profunda](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), e a implementação utiliza a CLI do Azure ML Operacionalização.

Consulte a [documentação de referência do pacote](https://aka.ms/aml-packages/vision) para obter a referência detalhada para cada módulo e uma classe.

## <a name="prerequisites"></a>Pré-requisitos

1. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

1. As seguintes contas e aplicação tem de ser configurados e instalados:
   - Uma conta de Experimentação do Azure Machine Learning 
   - Uma conta de gestão de modelos do Azure Machine Learning
   - O Azure Machine Learning Workbench instalado

   Se esses três são ainda não foi criados ou instalados, siga os [instalação manual de início rápido do Azure Machine Learning e Bancada de trabalho](../desktop-workbench/quickstart-installation.md) artigo. 

1. Tem de estar instalado o pacote de Aprendizado de máquina do Azure para imagem digitalizada. Saiba como [instalar este pacote aqui](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Dados de exemplo e o bloco de notas

### <a name="get-the-jupyter-notebook"></a>Obter o bloco de notas do Jupyter

Transferir o bloco de notas para executar o exemplo descrito aqui por conta própria.

> [!div class="nextstepaction"]
> [Obter o bloco de notas do Jupyter](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Carregar os dados de exemplo

Para esta demonstração, é fornecido um conjunto de dados de itens de acessar dentro refrigeradores, consiste em 30 imagens e 8 classes (eggBox, joghurt, molho de tomates, mushroom, monteiro, laranja, proteção e água). Para cada imagem jpg, existe um arquivo de xml anotação com o mesmo nome. 

A figura seguinte mostra a estrutura de pastas recomendada. 

![estrutura de pastas](media/how-to-build-deploy-object-detection-models/data_directory.JPG)

## <a name="image-annotation"></a>Anotação de imagem

Anotado objeto localizações são necessários para preparar e avaliar um detector de objeto. [LabelImg](https://tzutalin.github.io/labelImg) é uma ferramenta de anotação de código-fonte aberto que pode ser utilizada para anotar imagens. LabelImg escreve um arquivo xml por imagem no formato de Pascal VOC, que pode ser lidos por este pacote. 


```python
import warnings
warnings.filterwarnings("ignore")
import os, time
from cvtk.core import Context, ObjectDetectionDataset, TFFasterRCNN
from cvtk.evaluation import DetectionEvaluation
from cvtk.evaluation.evaluation_utils import graph_error_counts
from cvtk.utils import detection_utils

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

from matplotlib import pyplot as plt
# Display the images
%matplotlib inline
```

## <a name="create-a-dataset"></a>Criar um conjunto de dados

Crie um conjunto de dados CVTK que consiste num conjunto de imagens, com seus respectivos anotações de caixa delimitadora. Neste exemplo, as imagens de refrigerator (refrigerador) que são fornecidos no "... pasta/sample_data/foods/treinamento"são utilizados. São suportadas apenas as imagens JPEG.


```python
image_folder = "detection/sample_data/foods/train"
data_train = ObjectDetectionDataset.create_from_dir(dataset_name='training_dataset', data_dir=image_folder,
                                                    annotations_dir="Annotations", image_subdirectory='JPEGImages')

# Show some statistics of the training image, and also give one example of the ground truth rectangle annotations
data_train.print_info()
_ = data_train.images[2].visualize_bounding_boxes(image_size = (10,10))
```

    F1 2018-05-25 23:12:21,727 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-05-25 23:12:21,733 INFO azureml.vision:dataset creating dataset for scenario=detection 
    Dataset name: training_dataset
    Total classes: 8, total images: 25
    Label-wise object counts:
        Label eggBox: 20 objects
        Label joghurt: 20 objects
        Label ketchup: 20 objects
        Label mushroom: 20 objects
        Label mustard: 20 objects
        Label orange: 20 objects
        Label squash: 40 objects
        Label water: 20 objects
    Bounding box width and height distribution:
        Bounding box widths  0/5/25/50/75/95/100-percentile: 54/61/79/117/133/165/311 pixels
        Bounding box heights 0/5/25/50/75/95/100-percentile: 48/58/75/124/142/170/212 pixels
    


![PNG](media/how-to-build-deploy-object-detection-models/output_6_1.JPG)


## <a name="define-a-model"></a>Definir um modelo

Neste exemplo, o modelo de forma mais rápida de R-CNN é usado. Vários parâmetros podem ser fornecidos ao definir esse modelo. O significado desses parâmetros, bem como os parâmetros utilizados para formação (consulte a secção seguinte) pode ser encontrado nos documentos de API de qualquer um dos CVTK ou no [Web site de deteção de objeto do Tensorflow](https://github.com/tensorflow/models/tree/master/research/object_detection). Podem encontrar mais informações sobre o modelo mais rápido do R-CNN na [esta ligação](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Faster-R-CNN#technical-details). Este modelo baseia-se no rápido R-CNN e mais informações sobre ela podem ser encontradas [aqui](https://docs.microsoft.com/cognitive-toolkit/Object-Detection-using-Fast-R-CNN#algorithm-details).


```python
score_threshold = 0.0       # Threshold on the detection score, use to discard lower-confidence detections.
max_total_detections = 300  # Maximum number of detections. A high value slows down training but might increase accuracy.
my_detector = TFFasterRCNN(labels=data_train.labels, 
                           score_threshold=score_threshold, 
                           max_total_detections=max_total_detections)
```

## <a name="train-the-model"></a>Dar formação sobre o modelo

O modelo de formação de sistema COCO mais rápido do R-CNN com ResNet50 é utilizado como o ponto de partida para treinamento. 

Para preparar o detetor de, o número de passos de treinamento no código está definido para 350, para que o treinamento é executado mais rapidamente (~ 5 minutos com GPU). Na prática, defini-lo para, pelo menos, 10 vezes o número de imagens no conjunto de treinamento.

Neste exemplo, o número de passos de treinamento detetor é definido como 350 para treinamento rápido. No entanto, na prática, uma boa regra prática é definir os passos para 10 ou mais vezes o número de imagens no conjunto de treinamento.

Dois parâmetros de chave para treinamento são:
- Número de passos para preparar o modelo, representado pelo argumento num_seps. Cada passo prepara o modelo com um minibatch de tamanho de lote um.
- Aprendizagem rate(s), que pode ser definida por initial_learning_rate

```python
print("tensorboard --logdir={}".format(my_detector.train_dir))

# to get good results, use a larger value for num_steps, e.g., 5000.
num_steps = 350
learning_rate = 0.001 # learning rate

start_train = time.time()
my_detector.train(dataset=data_train, num_steps=num_steps, 
                  initial_learning_rate=learning_rate)
end_train = time.time()
print(end_train-start_train)
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\train
    F1 2018-05-25 23:12:22,764 INFO azureml.vision:Fit starting in experiment  1125722225 
    F1 2018-05-25 23:12:22,767 INFO azureml.vision:model starting trainging for scenario=detection 
    Using existing checkpoint file that's saved at 'C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\models\detection\faster_rcnn_resnet50_coco_2018_01_28\model.ckpt.index'.
    TFRecords creation started.
    F1 2018-05-25 23:12:22,773 INFO On image 0 of 25
    TFRecords creation completed.
    Training started.
    Training progressing: step 0 ...
    Training progressing: step 100 ...
    Training progressing: step 200 ...
    Training progressing: step 300 ...
    F1 2018-05-25 23:18:02,730 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:18:10,722 INFO 2953 ops in the final graph.
    F1 2018-05-25 23:18:24,244 INFO azureml.vision:Fit finished in experiment  1125722225 
    Training completed.
    361.604615688324
    

TensorBoard pode ser utilizado para visualizar o curso de treinamento. Eventos de TensorBoard estão localizados na pasta especificada pelo atributo da train_dir o objeto de modelo. Para ver TensorBoard, siga estes passos:
1. Copie a impressão que começa com "tensorboard – logdir' para uma linha de comando e executá-lo. 
2. Copie a URL retornada da linha de comando para um navegador da web para ver o TensorBoard. 

O TensorBoard deverá ser semelhante à seguinte captura de ecrã. Demora alguns minutos para a pasta de treinamento deve ser preenchida. Portanto, se for TensorBoard não mostra a corretamente a primeira hora tente repetir os passos 1 a 2.  

![tensorboard](media/how-to-build-deploy-object-detection-models/tensorboard.JPG)

## <a name="evaluate-the-model"></a>Avaliar o modelo

O método "avaliar" é utilizado para avaliar o modelo. Essa função requer um objeto de ObjectDetectionDataset como entrada. O conjunto de dados de avaliação pode ser criado usando a mesma função que utilizou para o conjunto de dados de treinamento. A métrica suportada é médio de precisão, conforme definido para o [PASCAL VOC desafio](http://host.robots.ox.ac.uk/pascal/VOC/pubs/everingham10.pdf).  


```python
image_folder = "detection/sample_data/foods/test"
data_val = ObjectDetectionDataset.create_from_dir(dataset_name='val_dataset', data_dir=image_folder)
eval_result = my_detector.evaluate(dataset=data_val)
```

    F1 2018-05-25 23:18:24,253 INFO azureml.vision:dataset creating dataset for scenario=detection 
    F1 2018-05-25 23:18:24,286 INFO On image 0 of 5
    F1 2018-05-25 23:18:29,300 INFO Starting evaluation at 2018-05-26-03:18:29
    F1 2018-05-25 23:18:32,403 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:33,158 INFO Detection visualizations written to summary with tag image-0.
    F1 2018-05-25 23:18:33,518 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:34,342 INFO Detection visualizations written to summary with tag image-1.
    F1 2018-05-25 23:18:34,714 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:35,470 INFO Detection visualizations written to summary with tag image-2.
    F1 2018-05-25 23:18:35,835 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:36,654 INFO Detection visualizations written to summary with tag image-3.
    F1 2018-05-25 23:18:37,010 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:37,798 INFO Detection visualizations written to summary with tag image-4.
    F1 2018-05-25 23:18:37,804 INFO Running eval batches done.
    F1 2018-05-25 23:18:37,805 INFO # success: 5
    F1 2018-05-25 23:18:37,806 INFO # skipped: 0
    F1 2018-05-25 23:18:38,119 INFO Writing metrics to tf summary.
    F1 2018-05-25 23:18:38,121 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/eggBox: 1.000000
    F1 2018-05-25 23:18:38,205 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/joghurt: 0.942857
    F1 2018-05-25 23:18:38,206 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/ketchup: 1.000000
    F1 2018-05-25 23:18:38,207 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mushroom: 1.000000
    F1 2018-05-25 23:18:38,208 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mustard: 1.000000
    F1 2018-05-25 23:18:38,209 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/orange: 1.000000
    F1 2018-05-25 23:18:38,210 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/squash: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/water: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/Precision/mAP@0.5IOU: 0.992857
    F1 2018-05-25 23:18:38,253 INFO Metrics written to tf summary.
    F1 2018-05-25 23:18:38,254 INFO Finished evaluation!
    

Os resultados da avaliação podem ser impressos num formato limpo.


```python
# print out the performance metric values
for label_obj in data_train.labels:
    label = label_obj.name
    key = 'PASCAL/PerformanceByCategory/AP@0.5IOU/' + label
    print('{0: <15}: {1: <3}'.format(label, round(eval_result[key], 2)))
print('{0: <15}: {1: <3}'.format("overall:", round(eval_result['PASCAL/Precision/mAP@0.5IOU'], 2))) 
```

    joghurt        : 0.94
    squash         : 1.0
    mushroom       : 1.0
    eggBox         : 1.0
    ketchup        : 1.0
    mustard        : 1.0
    water          : 1.0
    orange         : 1.0
    overall:       : 0.99
    

Da mesma forma, pode computar a precisão do modelo no conjunto de treinamento. Fazer isto ajuda a certificar-se de treinamento de convergência para uma boa solução. A precisão no conjunto de treinamento, depois de treinamento bem-sucedido, muitas vezes, é quase 100%.

Também podem ser visualizados os resultados da avaliação TensorBoard, incluindo imagens com caixas de delimitadora previstas e valores de mapa. Copie o printout do código a seguir numa janela de linha de comandos para iniciar o cliente de TensorBoard. Aqui, é utilizado um valor de porta 8008 para evitar conflitos com o valor predefinido de 6006, o que estava a utilizar para visualizar o estado de treinamento.


```python
print("tensorboard --logdir={} --port=8008".format(my_detector.eval_dir))
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\eval --port=8008
    

## <a name="score-an-image"></a>Uma imagem de pontuação

Quando estiver satisfeito com o desempenho do modelo preparado, a função de 'pontuação' do objeto de modelo pode ser utilizada para classificar as novas imagens. As pontuações devolvidas podem ser visualizadas com a função "visualizar". 


```python
image_path = data_val.images[1].storage_path
detections_dict = my_detector.score(image_path)
path_save = "./scored_images/scored_image_preloaded.jpg"
ax = detection_utils.visualize(image_path, detections_dict, image_size=(8, 12))
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

![PNG](media/how-to-build-deploy-object-detection-models/output_20_0.JPG)

##  <a name="save-the-model"></a>Guarde o modelo

O modelo treinado pode ser salvos em disco e carregado novamente para a memória, conforme mostrado nos exemplos de código a seguir.


```python
save_model_path = "./frozen_model/faster_rcnn.model"
my_detector.save(save_model_path)
```

    F1 2018-05-25 23:18:55,166 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:19:03,867 INFO 2953 ops in the final graph.
    

## <a name="load-the-saved-model-for-scoring"></a>Carregar o modelo guardado para classificação

Para utilizar o modelo guardado, carregar o modelo na memória com a função de "load". Apenas terá de carregar uma vez. 

```python
my_detector_loaded = TFFasterRCNN.load(save_model_path)
```

Depois do modelo é carregado, ele pode ser utilizado para classificar uma imagem ou uma lista de imagens. Para uma única imagem, um dicionário é devolvido com chaves como 'detection_boxes', 'detection_scores' e 'num_detections'. Se a entrada é uma lista de imagens, uma lista de dicionário é devolvida, com um dicionário correspondente a uma imagem. 


```python
detections_dict = my_detector_loaded.score(image_path)
```

Os objetos detetados com pontuações acima 0,5, incluindo etiquetas, pontuações e coordenadas pode ser impressos.


```python
look_up = dict((v,k) for k,v in my_detector.class_map.items())
n_obj = 0
for i in range(detections_dict['num_detections']):
    if detections_dict['detection_scores'][i] > 0.5:
        n_obj += 1
        print("Object {}: label={:11}, score={:.2f}, location=(top: {:.2f}, left: {:.2f}, bottom: {:.2f}, right: {:.2f})".format(
            i, look_up[detections_dict['detection_classes'][i]], 
            detections_dict['detection_scores'][i], 
            detections_dict['detection_boxes'][i][0],
            detections_dict['detection_boxes'][i][1], 
            detections_dict['detection_boxes'][i][2],
            detections_dict['detection_boxes'][i][3]))    
        
print("\nFound {} objects in image {}.".format(n_obj, image_path))           
```

    Object 0: label=squash     , score=0.99, location=(top: 0.74, left: 0.30, bottom: 0.84, right: 0.42)
    Object 1: label=squash     , score=0.98, location=(top: 0.27, left: 0.21, bottom: 0.37, right: 0.33)
    Object 2: label=orange     , score=0.98, location=(top: 0.31, left: 0.39, bottom: 0.37, right: 0.48)
    Object 3: label=joghurt    , score=0.98, location=(top: 0.57, left: 0.29, bottom: 0.67, right: 0.39)
    Object 4: label=eggBox     , score=0.97, location=(top: 0.41, left: 0.53, bottom: 0.49, right: 0.69)
    Object 5: label=water      , score=0.95, location=(top: 0.23, left: 0.51, bottom: 0.37, right: 0.57)
    Object 6: label=mustard    , score=0.88, location=(top: 0.61, left: 0.47, bottom: 0.66, right: 0.57)
    Object 7: label=ketchup    , score=0.80, location=(top: 0.62, left: 0.62, bottom: 0.68, right: 0.72)
    
    Found 8 objects in image ../sample_data/foods/test\JPEGImages\10.jpg.
    

Visualize as pontuações como antes.


```python
path_save = "./scored_images/scored_image_frozen_graph.jpg"
ax = detection_utils.visualize(image_path, detections_dict, path_save=path_save, image_size=(8, 12))
# ax.get_figure() # use this code extract the returned image
```

![PNG](media/how-to-build-deploy-object-detection-models/output_30_0.JPG)

## <a name="operationalization-deploy-and-consume"></a>Operacionalização: implementar e consumir

Operacionalização é o processo de publicação de modelos e código como serviços da web e o consumo de um destes serviços para produzir resultados de negócios. 

Assim que é preparado o modelo, pode implementar esse modelo como um serviço web para a utilização de consumo [da CLI do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Os modelos podem ser implementados em seu computador local ou o cluster do Azure Container Service (ACS). Usando o ACS, pode dimensionar manualmente o seu serviço web ou utilizar a funcionalidade de dimensionamento automático.

**Inicie sessão com a CLI do Azure**

Utilizar um [Azure](https://azure.microsoft.com/) conta com uma subscrição válida, inicie sessão com o seguinte comando da CLI:
<br>`az login`

+ Para mudar para outra subscrição do Azure, utilize o comando:
<br>`az account set --subscription [your subscription name]`

+ Para ver a conta de gestão de modelo atual, utilize o comando:
  <br>`az ml account modelmanagement show`

**Criar e definir o seu ambiente de implementação de cluster**

Apenas terá de definir o seu ambiente de implantação uma vez. Se ainda não tiver uma, configurar o ambiente de implantação agora usando [estas instruções](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Para ver o seu ambiente de implantação do Active Directory, utilize o seguinte comando da CLI:
<br>`az ml env show`
   
Comando da CLI do Azure de exemplo para criar e definir o ambiente de implantação

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Gerir implementações e serviços da web

As seguintes APIs pode ser utilizadas para implementar modelos como serviços da web, gerir os serviços da web e gerir implementações.

|Tarefa|API|
|----|----|
|Criar o objeto de implementação|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|Implementar o serviço web|`deploy_obj.deploy()`|
|Imagem de pontuação|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Eliminar serviço web|`deploy_obj.delete()`|
|Criar a imagem do docker sem o serviço web|`deploy_obj.build_docker_image()`|
|Lista de implementação existente|`AMLDeployment.list_deployment()`|
|Eliminar se o serviço existe com o nome da implementação|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**Documentação da API:** consulte a [documentação de referência do pacote](https://aka.ms/aml-packages/vision) para obter a referência detalhada para cada módulo e uma classe.

**Referência da CLI:** para mais avançada operações relacionadas com a implantação, consulte a [referência da CLI de gestão de modelos](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Gestão de implementação no portal do Azure**: pode controlar e gerir as implementações no [portal do Azure](https://ms.portal.azure.com/). No portal do Azure, encontre a sua página de conta de gestão de modelos do Machine Learning usa seu nome. Em seguida, vá para a página de conta de gestão de modelos > Gestão de modelos > serviços.

```python
# ##### OPTIONAL - Interactive CLI setup helper ###### 
# # Interactive CLI setup helper, including model management account and deployment environment.
# # If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
# # UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT

# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Create deployment object
# It will use the current deployment environment (you can check it with CLI command "az ml env show").
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=my_detector, replicas=1)

# Alternatively, you can provide azure machine learning deployment cluster name (environment name) and resource group name
# to deploy your model. It will use the provided cluster to deploy. To do that, please uncomment the following lines to create 
# the deployment object.

# azureml_rscgroup = "<resource group>"
# cluster_name = "<cluster name>"
# deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=my_detector,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# create the webservice
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>Consumir o serviço web

Uma vez criado o webservice, pode classificar as imagens com o serviço Web implementado. Tem várias opções:

   - Diretamente pode classificar o serviço Web com o objeto de implementação com: deploy_obj.score_image(image_path_or_url) 
   - Em alternativa, pode utilizar o url de ponto final de serviço e a chave de serviço (nenhum para a implementação local) com: AMLDeployment.score_existing_service_with_image (image_path_or_url service_endpoint_url, service_key = None)
   - Forma os pedidos de http diretamente para o ponto de extremidade do serviço Web de pontuação (para utilizadores avançados).

### <a name="score-with-existing-deployment-object"></a>Com o objeto de implementação existente de pontuação
```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = data_train.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])

```


```python
# Time image scoring
import timeit

num_images = 3
for img_index, img_obj in enumerate(data_train.images[:num_images]):
    print("Calling API for image {} of {}: {}...".format(img_index, num_images, img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Classificar pelo url do ponto final de serviço e a chave de serviço
```
    AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
```


```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "http://xxx" # please replace with your own service url
service_key = "xxx" # please replace with your own service key

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])
```

### <a name="score-endpoint-with-http-request-directly"></a>Ponto final de pontuação com diretamente a solicitação de http
Segue-se alguns códigos de exemplo para formar o pedido de http diretamente em Python. Pode fazê-lo em outras linguagens de programação.


```python
def score_image_with_http(image, service_endpoint_url, service_key=None, parameters={}):
    """Score local image with http request

    Args:
        image (str): Image file path
        service_endpoint_url(str): web service endpoint url
        service_key(str): Service key. None for local deployment.
        parameters (dict): Additional request parameters in dictionary. Default is {}.


    Returns:
        str: serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    import json

    if service_key is None:
        headers = {'Content-Type': 'application/json'}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key)}
    payload = []
    encoded = None
    
    # Read image
    with open(image,'rb') as f:
        image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        
    # Convert your image to base64 string
    # image_in_base64 : "b'{base64}'"
    encoded = base64.b64encode(image_buffer.getvalue())
    image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
    payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
        json.loads(result[0])
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result[0]

```

### <a name="parse-serialized-result-from-webservice"></a>Analisar o resultado serializado de webservice
O resultado do serviço web é na cadeia de caracteres do json que pode ser analisada.


```python
image_path_or_url = image_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])
```


```python
# Parse result from json string
import numpy as np
parsed_result = TFFasterRCNN.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
```


```python
ax = detection_utils.visualize(image_path, parsed_result)
path_save = "./scored_images/scored_image_web.jpg"
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o pacote do Azure Machine Learning para imagem digitalizada nestes artigos:

+ Leitura a [descrição geral do pacote](https://aka.ms/aml-packages/vision).

+ Explore os [documentação de referência](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) para este pacote.

+ Saiba mais sobre [outros pacotes de Python para o Azure Machine Learning](reference-python-package-overview.md).
