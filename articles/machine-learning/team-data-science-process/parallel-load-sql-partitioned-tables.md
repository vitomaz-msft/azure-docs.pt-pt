---
title: Compilar e otimizar tabelas para a importação de dados rápido paralela num SQL Server numa VM do Azure | Documentos da Microsoft
description: Importação de Dados em Massa e em Paralelo com Tabelas de Partição de SQL
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ff90fdb0-5bc7-49e8-aee7-678b54f901c8
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: c318411fe17fb60c1c0bf991a07b46a515252952
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683225"
---
# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Importação de Dados em Massa e em Paralelo com Tabelas de Partição de SQL

Este artigo descreve como criar tabelas particionadas para a importação em massa rápida paralela de dados para uma base de dados do SQL Server. Para grandes volumes de dados carregamento/transferência de um banco de dados do SQL, importar dados para a BD SQL e consultas subsequentes pode ser melhorado usando *vistas e tabelas Particionadas*. 

## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Criar uma nova base de dados e um conjunto de grupos de ficheiros
* [Criar uma nova base de dados](https://technet.microsoft.com/library/ms176061.aspx), se ainda não exista.
* Adicione grupos de ficheiros de base de dados na base de dados, que contém os arquivos físicos particionados. 
* Isso pode ser feito com [CREATE DATABASE](https://technet.microsoft.com/library/ms176061.aspx) se a nova ou [ALTER DATABASE](https://msdn.microsoft.com/library/bb522682.aspx) se a base de dados já existir.
* Adicione um ou mais arquivos (conforme necessário) para cada grupo de ficheiros de base de dados.
  
  > [!NOTE]
  > Especifique o grupo de ficheiros de destino, que contém dados para esta partição e os nomes de arquivo de banco de dados físico onde estão armazenados os dados do grupo de ficheiros.
  > 
  > 

O exemplo seguinte cria uma nova base de dados com três grupos de ficheiros que não seja o principal e grupos de registos, que contém um arquivo físico em cada um. Os ficheiros de base de dados são criados na pasta de dados do SQL Server predefinida, conforme configurado na instância do SQL Server. Para obter mais informações sobre localizações predefinidas de ficheiros, consulte [localizações de ficheiros para o padrão e com o nome de instâncias do SQL Server](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);

    EXECUTE ('
        CREATE DATABASE <database_name>
         ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
         FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
         LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')

## <a name="create-a-partitioned-table"></a>Criar uma tabela particionada
Para criar tabelas particionadas, de acordo com o esquema de dados, mapeado para os grupos de ficheiros de base de dados criados no passo anterior, primeiro tem de criar uma função de partição e o esquema. Quando os dados estão em massa importada para a tabela particionada (s), os registos são distribuídos entre os grupos de ficheiros, de acordo com um esquema de partição, conforme descrito abaixo.

### <a name="1-create-a-partition-function"></a>1. Criar uma função de partição
[Criar uma função de partição](https://msdn.microsoft.com/library/ms187802.aspx) esta função define o intervalo de valores/limites incluídos em cada tabela de partições individuais, por exemplo, para limitar as partições por mês (alguns\_datetime\_campo) no ano de 2013:
  
        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

### <a name="2-create-a-partition-scheme"></a>2. Criar um esquema de partição
[Criar um esquema de partição](https://msdn.microsoft.com/library/ms179854.aspx). Este esquema mapeia cada intervalo de partição na função de partição para um grupo de ficheiros físico, por exemplo:
  
        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )
  
  Para verificar os intervalos em vigor em cada partição, de acordo com a função/esquema, execute a seguinte consulta:
  
        SELECT psch.name as PartitionScheme,
            prng.value AS PartitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

### <a name="3-create-a-partition-table"></a>3. Criar uma tabela de partição
[Criar tabela particionada](https://msdn.microsoft.com/library/ms174979.aspx)(s) de acordo com seu esquema de dados e especificar o campo de esquema e restrição de partição utilizado para particionar a tabela, por exemplo:
  
        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Para obter mais informações, consulte [criar tabelas Particionadas e índices](https://msdn.microsoft.com/library/ms188730.aspx).

## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Em massa importar os dados para cada tabela de partições individuais

* Pode utilizar BCP, a inserção em massa ou outros métodos como [Assistente de migração do SQL Server](http://sqlazuremw.codeplex.com/). O exemplo fornecido usa o método BCP.
* [Alterar o banco de dados](https://msdn.microsoft.com/library/bb522682.aspx) para alterar o esquema de Registro em log de transação para BULK_LOGGED para minimizar a sobrecarga de iniciar sessão, por exemplo:
  
        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED
* Para acelerar o processo de carregamento de dados, inicie as operações de importação em massa em paralelo. Para obter dicas sobre acelerar em massa importação de grandes volumes de dados para bases de dados do SQL Server, consulte [carregar 1TB em menos de 1 hora](https://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

O seguinte script do PowerShell é um exemplo de paralelos com o BCP de carregamento de dados.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0

    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>

    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"

    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir

    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }

    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }

    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }

    Get-Job

    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Criar índices para otimizar as associações e o desempenho de consulta
* Se extrair dados para a Modelagem de várias tabelas, crie índices nas chaves de associação para melhorar o desempenho de associação.
* [Criar índices](https://technet.microsoft.com/library/ms188783.aspx) (em cluster ou não-clusterizados) direcionar o mesmo grupo de arquivos para cada partição, por exemplo:
  
        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  ou,
  
        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
  
  > [!NOTE]
  > Pode optar por criar os índices antes em massa importando os dados. Criação de índices antes de importar em massa pode atrasar o carregamento de dados.
  > 
  > 

## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Processo de análise avançada e tecnologia no exemplo de ação
Para obter um exemplo passo a passo-a-ponto utilizando o processo de ciência de dados de equipa com um conjunto de dados público, veja [processo de ciência de dados de equipa em ação: utilizar o SQL Server](sql-walkthrough.md).

