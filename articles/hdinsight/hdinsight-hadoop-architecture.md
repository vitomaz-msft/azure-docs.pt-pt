---
title: Arquitetura do Apache Hadoop - Azure HDInsight
description: Descreve o processamento e armazenamento do Apache Hadoop em clusters do HDInsight.
services: hdinsight
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.openlocfilehash: 066734c88890d5f1a6e42c5350db47d1a398b60a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277584"
---
# <a name="apache-hadoop-architecture-in-hdinsight"></a>Arquitetura do Apache Hadoop no HDInsight

Apache Hadoop inclui dois componentes principais: o Hadoop Distributed File System (HDFS) que fornece armazenamento e ainda outro Resource Negotiator (YARN) que fornece processamento. Com as capacidades de processamento e armazenamento, um cluster torna-se com capacidade para executar programas MapReduce para executar o processamento de dados pretendido.

> [!NOTE]
> Normalmente, não é implementada uma HDFS dentro do cluster do HDInsight para fornecer armazenamento. Em vez disso, uma camada de interface compatível com HDFS é utilizada por componentes do Hadoop. A capacidade de armazenamento real é fornecida pelo armazenamento do Azure ou do Azure Data Lake Store. Para o Hadoop, tarefas de MapReduce em execução no cluster de HDInsight executar como se um HDFS estavam presentes e por isso, não necessitam de alterações para dar suporte a suas necessidades de armazenamento. Hadoop no HDInsight, armazenamento seja externo, mas o processamento de YARN continua sendo um componente fundamental. Para obter mais informações, consulte [introdução ao Azure HDInsight](hadoop/apache-hadoop-introduction.md).

Este artigo apresenta YARN e como ele coordena a execução de aplicativos no HDInsight.

## <a name="apache-yarn-basics"></a>Noções básicas do Apache YARN 

YARN rege e orquestra o processamento de dados no Hadoop. YARN tem dois serviços principais que são executadas como processos em nós do cluster: 

* ResourceManager 
* NodeManager

O ResourceManager concede aos recursos de computação de cluster para as aplicações, como tarefas de MapReduce. O ResourceManager concede estes recursos, como contentores, onde cada contentor é composta por uma alocação de núcleos de CPU e memória RAM. Se combinado de todos os recursos disponíveis num cluster e, em seguida, distribuídos os núcleos e memória em blocos, cada bloco de recursos é um contentor. Cada nó do cluster tem uma capacidade para um determinado número de contentores, portanto o cluster tem um limite fixo no número de contentores disponíveis. A alocação de recursos num contentor é configurável. 

Quando um aplicativo de MapReduce é executado num cluster, o ResourceManager fornece o aplicativo os contentores para executar. O ResourceManager controla o estado de execução de aplicativos, capacidade de cluster disponível e controla as aplicações à medida que concluir e seus recursos de versão. 

O ResourceManager também é executado um processo de servidor web que fornece uma interface do usuário de web para monitorizar o estado das aplicações.

Quando um utilizador submete um aplicativo de MapReduce para executar no cluster, o aplicativo foi submetido para o ResourceManager. Por sua vez, ResourceManager aloca um contentor em nós de NodeManager disponíveis. Os nós de NodeManager são onde o aplicativo for executado. O primeiro contentor alocado executa um aplicativo de especial chamado o ApplicationMaster. Este ApplicationMaster é responsável por a adquirir recursos, na forma de contentores subsequentes, necessários para executar uma aplicação submetida. O ApplicationMaster examina os estágios do aplicativo, como o estágio de mapa e reduzir a fase e fatores na quantidade de dados deve ser processado. Em seguida, pede a ApplicationMaster (*negocia*) os recursos a partir do ResourceManager em nome do aplicativo. O ResourceManager concede por sua vez para recursos NodeManagers no cluster para o Applicationmastê-la para utilizar na execução do aplicativo. 

NodeManagers executados as tarefas que compõem o aplicativo, em seguida, relatar seu estado e progresso para o ApplicationMaster. O ApplicationMaster relatórios, por sua vez, o estado da aplicação para o ResourceManager. O ResourceManager retorna os resultados para o cliente.

## <a name="yarn-on-hdinsight"></a>YARN no HDInsight

Todos os tipos de cluster do HDInsight implementar YARN. O ResourceManager é implementado para elevada disponibilidade com uma instância primária e secundária, que é executado em nós principais, primeiros e segundo, dentro do cluster, respetivamente. Apenas a uma instância do ResourceManager está ativa por vez. As instâncias de NodeManager são executadas em todos os nós de trabalho disponíveis no cluster.

![YARN no HDInsight](./media/hdinsight-hadoop-architecture/yarn-on-hdinsight.png)

## <a name="next-steps"></a>Passos Seguintes

* [Utilizar o MapReduce no Hadoop no HDInsight](hadoop/hdinsight-use-mapreduce.md)
* [Introdução ao Azure HDInsight](hadoop/apache-hadoop-introduction.md)
