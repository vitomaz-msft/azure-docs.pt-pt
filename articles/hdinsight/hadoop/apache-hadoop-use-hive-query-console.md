---
title: Utilizar o Apache Hive na consola de consulta no HDInsight - Azure
description: Saiba como utilizar a consola de consulta baseada na web para executar consultas do Apache Hive num cluster do HDInsight Hadoop a partir do seu browser.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/12/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: da270792c8987ff43c422c5b03eb8b789b8bda5e
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634596"
---
# <a name="run-apache-hive-queries-using-the-query-console"></a>Executar consultas do Apache Hive, utilizando a consola de consulta
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Neste artigo, aprenderá a utilizar a consola de consulta do HDInsight para executar consultas do Apache Hive num cluster do HDInsight Hadoop a partir do seu browser.

> [!IMPORTANT]
> A consola de consulta do HDInsight só está disponível em clusters do HDInsight baseado em Windows. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).
>
> Para o HDInsight 3.4 ou superior, consulte [executar consultas do Hive no modo de exibição do Ambari Hive](apache-hadoop-use-hive-ambari-view.md) para obter informações sobre como executar consultas do Hive a partir de um navegador da web.

## <a id="prereq"></a>Pré-requisitos
Para concluir os passos neste artigo, terá de fazer o seguinte.

* Um cluster de Hadoop do HDInsight baseado em Windows
* Um navegador da web modernos

## <a id="run"></a> Executar consultas do Hive utilizando a consola de consulta
1. Abra um browser e navegue para **https://CLUSTERNAME.azurehdinsight.net**, em que **CLUSTERNAME** é o nome do cluster do HDInsight. Se lhe for pedido, introduza o nome de utilizador e palavra-passe que utilizou quando criou o cluster.
2. As ligações na parte superior da página, selecione **Editor do Hive**. Esta ação apresenta um formulário que pode ser usado para inserir as declarações HiveQL que pretende executar no cluster do HDInsight.

    ![o editor do hive](./media/apache-hadoop-use-hive-query-console/queryconsole.png)

    Substitua o texto `Select * from hivesampletable` com as seguintes declarações HiveQL:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Essas instruções executam as seguintes ações:

   * **DROP TABLE**: elimina a tabela e o ficheiro de dados, se a tabela já existe.
   * **Criar tabela externa**: cria uma nova tabela de "externa" no Hive. Tabelas externas armazenam apenas a definição de tabela no Hive; os dados são deixados na localização original.

     > [!NOTE]
     > Tabelas externas devem ser utilizadas quando espera que os dados subjacentes sejam atualizados por uma origem externa (por exemplo, um processo de carregamento de dados automatizada) ou por outra operação de MapReduce, mas é sempre melhor consultas do Hive para utilizar os dados mais recentes.
     >
     > Remover uma tabela externa faz **não** eliminar os dados, apenas a definição da tabela.
     >
     >
   * **FORMATO de linha**: informa ao Hive como os dados estiverem formatados. Neste caso, os campos em cada registo são separados por um espaço.
   * **LOCALIZAÇÃO de TEXTFILE AS ARMAZENADOS**: informa ao ramo de registo onde os dados são armazenados (o diretório/dados de exemplo) e que são armazenados como texto
   * **SELECIONE**: selecione uma contagem de todas as linhas em que coluna **t4** conter o valor **[erro]**. Isto deverá devolver um valor de **3** porque existem três linhas que contêm este valor.
   * **INPUT__FILE__NAME como '%.log'** -informa ao Hive que podemos deverá devolver apenas dados de ficheiros terminados em. log. Isso restringe a pesquisa para o ficheiro Sample log que contém os dados e impede que ela retornar dados do outro exemplo, ficheiros de dados que não correspondem o esquema que definimos.
3. Clique em **submeter**. O **tarefa sessão** na parte inferior da página deve apresentar os detalhes da tarefa.
4. Quando o **Status** campo é alterado para **concluído**, selecione **ver detalhes** para a tarefa. Na página de detalhes, o **saída da tarefa** contém `[ERROR]    3`. Pode utilizar o **transferir** botão sob este campo para transferir um ficheiro que contém o resultado da tarefa.

## <a id="summary"></a>Resumo
Como pode ver, a consola de consulta fornece uma forma fácil de executar consultas do Hive num cluster do HDInsight, monitorizar o estado da tarefa e obter o resultado.

Para saber mais sobre como utilizar a consola de consulta do Hive para executar tarefas do Hive, selecione **introdução ao** na parte superior da consola de consulta, em seguida, utilize os exemplos que são fornecidos. Cada exemplo explica o processo de uso do Hive para analisar dados, incluindo explicações sobre as declarações HiveQL usadas nesse exemplo.

## <a id="nextsteps"></a>Passos seguintes
Para obter informações gerais sobre o Hive no HDInsight:

* [Utilizar o Hive com o Hadoop no HDInsight](hdinsight-use-hive.md)

Para obter informações sobre outras formas pode trabalhar com o Hadoop no HDInsight:

* [Utilizar o Pig com o Hadoop no HDInsight](hdinsight-use-pig.md)
* [Utilizar o MapReduce com o Hadoop no HDInsight](hdinsight-use-mapreduce.md)

Se estiver a utilizar com o Hive no Tez, consulte os seguintes documentos para informações de depuração:

* [Utilize a IU do Tez no HDInsight baseado em Windows](../hdinsight-debug-tez-ui.md)
* [Utilize a vista Ambari Tez no HDInsight baseado em Linux](../hdinsight-debug-ambari-tez-view.md)

[1]:apache-hadoop-visual-studio-tools-get-started.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
