---
title: Executar tarefas do Apache Sqoop com .NET e o HDInsight - Azure
description: Saiba como utilizar o SDK de .NET do HDInsight para executar o Apache Sqoop import e export entre um cluster do Apache Hadoop e uma base de dados SQL do Azure.
keywords: tarefa de sqoop
ms.reviewer: jasonh
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 0b8d408482f1f6e2bcd25182208a46d28f7b4f7a
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633043"
---
# <a name="run-apache-sqoop-jobs-by-using-net-sdk-for-apache-hadoop-in-hdinsight"></a>Executar tarefas de Apache Sqoop com o .NET SDK para o Apache Hadoop no HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Saiba como utilizar o SDK de .NET do Azure HDInsight para executar tarefas do Apache Sqoop no HDInsight para importar e exportar entre um cluster do HDInsight e uma base de dados SQL do Azure ou a base de dados do SQL Server.

> [!NOTE]
> Apesar de poder utilizar os procedimentos neste artigo com a um cluster do HDInsight baseado em Windows ou Linux, eles funcionam apenas a partir de um cliente do Windows. Para escolher a outros métodos, utilize o Seletor de separador na parte superior deste artigo.
> 

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar este tutorial, tem de ter o seguinte item:

* Um cluster do Hadoop no HDInsight. Para obter mais informações, consulte [criar um cluster e uma base de dados do SQL](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="use-sqoop-on-hdinsight-clusters-with-the-net-sdk"></a>Utilize o Sqoop nos clusters do HDInsight com o SDK de .NET
O SDK de .NET do HDInsight fornece bibliotecas de cliente .NET, para que seja mais fácil trabalhar com clusters do HDInsight de .NET. Nesta secção, vai criar uma aplicação de consola c# para exportar o hivesampletable para a tabela de base de dados do Azure SQL que criou anteriormente no tutorial.

## <a name="submit-a-sqoop-job"></a>Submeter uma tarefa de Sqoop

1. Crie uma aplicação de consola c# no Visual Studio.

2. Na consola do Gestor de pacotes do Visual Studio, importe o pacote ao executar o seguinte comando do NuGet:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

3. Utilize o seguinte código no ficheiro Program.cs:
   
        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
   
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitSqoopJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
   
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
   
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
   
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
   
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }

4. Para executar o programa, selecione o **F5** chave. 

## <a name="limitations"></a>Limitações
HDInsight baseado em Linux apresenta as seguintes limitações:

* Exportação em massa: Sqoop o conector que é utilizado para exportar dados para o Microsoft SQL Server ou SQL Database do Azure não suporta atualmente inserções em massa.

* Criação de batches: ao utilizar o `-batch` mudar quando ele executa inserções, Sqoop realiza várias inserções em vez das operações de inserção de criação de batches.

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu a utilizar o Sqoop. Para saber mais, consulte:

* [Utilizar o Oozie com o HDInsight](../hdinsight-use-oozie.md): utilização Sqoop ação num fluxo de trabalho do Oozie.
* [Analisar dados de atraso de voo com o HDInsight](../hdinsight-analyze-flight-delay-data.md): utilizar o Hive para analisar voo atrasar a dados e, em seguida, utilize o Sqoop para exportar dados para uma base de dados SQL do Azure.
* [Carregar dados para o HDInsight](../hdinsight-upload-data.md): encontrar outros métodos para carregar dados para o armazenamento do HDInsight ou de Blobs do Azure.

