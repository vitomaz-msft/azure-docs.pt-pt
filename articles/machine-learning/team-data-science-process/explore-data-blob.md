---
title: Explorar os dados no armazenamento de Blobs do Azure com o pandas | Documentos da Microsoft
description: Como explorar os dados armazenados no contentor de Blobs do Azure com o pandas.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: b617ae1a1df0ed220d03e0124aa9dafd9fc2388c
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51346002"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Explorar os dados no armazenamento de Blobs do Azure com o pandas

Este artigo aborda como explorar os dados armazenados no contentor de Blobs do Azure com [pandas](http://pandas.pydata.org/) pacote do Python.

Esta tarefa é um passo na [Team Data Science Process](overview.md).

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que tem:

* Criar uma conta de armazenamento do Azure. Se precisar de instruções, consulte [criar uma conta de armazenamento do Azure](../../storage/common/storage-quickstart-create-account.md)
* Armazenados os dados numa conta de armazenamento de Blobs do Azure. Se precisar de instruções, consulte [mover dados para e do armazenamento do Azure](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Carregar os dados para um pandas DataFrame
Para explorar e manipular um conjunto de dados, transferi-lo a tem primeiro de origem do blob para um arquivo local, que, em seguida, pode ser carregado num pandas DataFrame. Aqui estão as etapas a seguir para este procedimento:

1. Transferência dos dados do Azure de Blobs com o seguinte exemplo de código de Python com o serviço de Blobs. Substitua os valores específicos a variável no código a seguir: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. Ler os dados para um pandas DataFrame do ficheiro transferido.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Agora, está pronto para explorar os dados e gerar recursos neste conjunto de dados.

## <a name="blob-dataexploration"></a>Exemplos de exploração de dados com o pandas
Aqui estão alguns exemplos de formas para explorar dados com o pandas:

1. Inspecione o **número de linhas e colunas** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Inspecionar** poucos primeiros ou últimos **linhas** no conjunto de dados seguinte:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Verifique os **tipo de dados** cada coluna foi importada como usando o seguinte código de exemplo
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Verifique os **estatísticas básicas** para as colunas nos dados definidas da seguinte forma
   
        dataframe_blobdata.describe()
5. Veja o número de entradas para cada valor de coluna da seguinte forma
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Contar valores em falta** em comparação com o número real de entradas em cada coluna com o seguinte código de exemplo
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Se tiver **valores em falta** para uma coluna de específica nos dados, pode soltá-los da seguinte forma:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape
   
   Outra maneira de substituir valores em falta é com a função de modo:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Criar uma **histograma** desenhar com um número variável de discretizações para desenhar a distribuição de uma variável    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Examinar **correlações** entre as variáveis usando um gráfico de dispersão ou usando a função incorporada de correlação
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

