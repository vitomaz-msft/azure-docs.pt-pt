---
title: Ligar o Excel para Apache Hadoop com o controlador ODBC do Hive - Azure HDInsight
description: Saiba como configurar e utilizar o controlador Microsoft Hive ODBC para o Excel para consultar dados em clusters do HDInsight a partir do Microsoft Excel.
keywords: hadoop do excel, o hive do excel, do hive odbc
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 09642aba1cd0daa05e56e418330f21361d9263a2
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632514"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Ligar o Excel para Apache Hadoop no HDInsight do Azure com o controlador Microsoft Hive ODBC

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Solução de macrodados da Microsoft integra-se os componentes do Microsoft Business Intelligence (BI) com clusters do Apache Hadoop que foram implementados pelo Azure HDInsight. Um exemplo desta integração é a capacidade de ligar o Excel para o armazém de dados do Hive de um cluster de Hadoop no HDInsight com o controlador Microsoft Hive Open Database Connectivity (ODBC).

Também é possível ligar os dados associados um cluster do HDInsight e outras origens de dados, incluindo outros clusters do Hadoop (não HDInsight), do Excel usando o suplemento do Microsoft Power Query para Excel. Para obter informações sobre como instalar e utilizar o Power Query, consulte [ligar o Excel ao HDInsight com o Power Query][hdinsight-power-query].



**Pré-requisitos**:

Antes de começar este artigo, tem de ter os seguintes itens:

* **Um cluster do HDInsight**. Para criar um, veja [introdução ao Azure HDInsight](apache-hadoop-linux-tutorial-get-started.md).
* **Uma estação de trabalho** com o Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 Standalone ou Office 2010 Professional Plus.

## <a name="install-microsoft-hive-odbc-driver"></a>Instalar o controlador Microsoft Hive ODBC
Baixe e instale o controlador Microsoft Hive ODBC do [Centro de transferências][hive-odbc-driver-download].

Esse driver pode ser instalado em versões de 32 bits ou 64 bits do Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 e Windows Server 2012. O driver permite a ligação ao Azure HDInsight. Deve instalar a versão que corresponde à versão do aplicativo em que usar o controlador ODBC. Para este tutorial, o driver é utilizado a partir do Office Excel.

## <a name="create-apache-hive-odbc-data-source"></a>Criar origem de dados de ODBC do Hive do Apache
Os passos seguintes mostram como criar uma origem de dados de ODBC do Hive.

1. Do Windows 8 ou Windows 10, prima a tecla do Windows para abrir o ecrã de início e, em seguida, escreva **origens de dados**.
2. Clique em **configurar origens de dados ODBC (32-bit)** ou **configurar origens de dados ODBC (64-bit)** dependendo da versão do Office. Se estiver a utilizar o Windows 7, escolha **origens de dados ODBC (32 bits)** ou **origens de dados ODBC (64 bits)** partir **ferramentas administrativas**. Deverá ver o **administrador de fonte de dados de ODBC** caixa de diálogo.
   
    ![Administrador de fonte de dados OBDC](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png "configurar um DSN com o administrador de fonte de dados de ODBC")

3. A partir do DNS do utilizador, clique em **Add** para abrir o **criar nova origem de dados** assistente.
4. Selecione **Microsoft Hive ODBC Driver**e, em seguida, clique em **concluir**. Deverá ver o **Microsoft Hive ODBC Driver de configuração de DNS** caixa de diálogo.
5. Escreva ou selecione os seguintes valores:
   
   | Propriedade | Descrição |
   | --- | --- |
   |  Nome da Origem de Dados |Atribua um nome para a sua origem de dados |
   |  Anfitrião |Introduza &lt;HDInsightClusterName>.azurehdinsight.net. Por exemplo, myHDICluster.azurehdinsight.net |
   |  Porta |Utilize <strong>443</strong>. (Esta porta foi alterada de 563 para 443.) |
   |  Base de Dados |Use <strong>Predefinição</strong>. |
   |  Mecanismo |Selecione <strong>Serviço do Azure HDInsight</strong> |
   |  Nome de Utilizador |Introduza o nome de utilizador do utilizador do HDInsight cluster HTTP. O nome de utilizador predefinido é <strong>admin</strong>. |
   |  Palavra-passe |Introduza a palavra-passe do utilizador de cluster do HDInsight. |
   
    </table>
   
    Existem alguns parâmetros importantes ter em consideração quando clica em **opções avançadas**:
   
   | Parâmetro | Descrição |
   | --- | --- |
   |  Utilizar a consulta nativa |Quando estiver selecionada, o controlador ODBC não tentará converter TSQL em HiveQL. Deve usá-lo apenas se for 100%-se de que está a enviar instruções de HiveQL puras. Ao ligar ao SQL Server ou base de dados do Azure SQL, deve deixar desmarcada. |
   |  Linhas obtidas por bloco |Quando a obter um grande número de registos, a otimização este parâmetro pode ser necessário para garantir que os desempenhos ideal. |
   |  Comprimento de coluna de cadeia de caracteres padrão, o comprimento de coluna binária, escala da coluna Decimal |O tipo de dados comprimentos e precisions pode afetar a forma como os dados são retornados. Estes provocarem informações incorretas a serem retornados devido a perda de precisão e/ou o truncamento. |

    ![Opções Avançadas](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png "opções de configuração do Advanced DSN")

1. Clique em **testar** para testar a origem de dados. Quando a origem de dados está configurada corretamente, ela mostra *testes foi CONCLUÍDO com êxito!*.
2. Clique em **OK** para fechar a caixa de diálogo de teste. A nova origem de dados deverá ser listada no **administrador de fonte de dados de ODBC**.
3. Clique em **OK** para sair do assistente.

## <a name="import-data-into-excel-from-hdinsight"></a>Importe dados para o Excel a partir do HDInsight
Os passos seguintes descrevem a forma de importar dados de uma tabela do Hive para um livro do Excel com a origem de dados ODBC que criou na secção anterior.

1. Abra um livro novo ou existente no Excel.
2. Do **dados** separador, clique em **obter dados**, clique em **de outras origens**e, em seguida, clique em **do ODBC** para iniciar o **dados Assistente de ligação**.
   
    ![Assistente de ligação de dados abertos](./media/apache-hadoop-connect-excel-hive-odbc-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png "Assistente de ligação de dados abertos")
4. Selecione o nome da origem de dados que criou na última secção e, em seguida, clique em **OK**.
5. Introduza o nome de utilizador do Hadoop (o nome predefinido é admin) e a palavra-passe e, em seguida, clique em **Connect**.
6. No navegador, expanda **HIVE**, expanda **predefinição**, clique em **hivesampletable**e, em seguida, clique em **carga**. Demora alguns segundos antes de os dados serem importados para o Excel.

    ![Navegador de ODBC do Hive do HDInsight](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight.hive.odbc.navigator.png "Assistente de ligação de dados abertos")


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar o controlador Microsoft Hive ODBC para recuperar dados do serviço HDInsight para o Excel. Da mesma forma, pode recuperar dados do serviço HDInsight na base de dados SQL. Também é possível carregar dados para um serviço do HDInsight. Para saber mais, consulte:

* [Visualizar dados de Hive com o Microsoft Power BI no Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Visualizar dados de consulta interativa do Hive com o Power BI no Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Utilizar o Zeppelin para executar consultas do Hive no Azure HDInsight ](./../hdinsight-connect-hive-zeppelin.md).
* [Ligar o Excel ao Hadoop com o Power Query](apache-hadoop-connect-excel-power-query.md).
* [Ligar ao Azure HDInsight e executar consultas do Hive com o Data Lake Tools para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Utilize a ferramenta do Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carregar dados para o HDInsight](./../hdinsight-upload-data.md).

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]:hdinsight-use-hive.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-power-query]: ../hdinsight-connect-excel-power-query.md
[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698


