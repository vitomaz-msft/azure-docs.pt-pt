---
title: Criar uma aplicação Web Java e MySQL no Azure
description: Saiba como ter uma aplicação Java que liga ao serviço de base de dados MySQL do Azure a funcionar no serviço de aplicações do Azure.
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: ec942d97e7671c0477d8d723afacb06b73565c1c
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414562"
---
# <a name="tutorial-build-a-java-and-mysql-web-app-in-azure"></a>Tutorial: Criar uma aplicação Web Java e MySQL no Azure

> [!NOTE]
> Este artigo implementa uma aplicação no Serviço de Aplicações no Windows. Para implementar no Serviço de Aplicações no _Linux_, veja [Implementar uma aplicação Spring Boot em contentores no Azure](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin).
>

Este tutorial mostra-lhe como criar uma aplicação Web Java e ligá-la a uma base de dados MySQL. Quando tiver terminado, terá uma aplicação [Spring Boot](https://projects.spring.io/spring-boot/) que armazena os dados na [Base de Dados do Azure para MySQL](../mysql/overview.md) em execução nas [Aplicações Web do Serviço de Aplicações do Azure](app-service-web-overview.md).

![Aplicação Java em execução no serviço de aplicações do Azure](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma base de dados MySQL no Azure
> * Ligar uma aplicação de exemplo à base de dados
> * Implementar a aplicação no Azure
> * Atualizar e reimplementar a aplicação
> * Transmitir os registos de diagnóstico em fluxo a partir do Azure
> * Monitorizar a aplicação no Portal do Azure

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

1. [Transferir e instalar o Git](https://git-scm.com/)
1. [Transferir e instalar o Java JDK](https://aka.ms/azure-jdks)
1. [Transferir, instalar e iniciar o MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Preparar o MySQL local 

Neste passo, vai criar uma base de dados no servidor MySQL local para utilizar em testes da aplicação localmente no seu computador.

### <a name="connect-to-mysql-server"></a>Ligar ao servidor MySQL

Numa janela de terminal, ligue ao seu servidor MySQL local. Pode utilizar esta janela de terminal para executar todos os comandos deste tutorial.

```bash
mysql -u root -p
```

Se lhe for pedida uma palavra-passe, introduza a palavra-passe da conta `root`. Se não se lembrar da sua palavra-passe da conta de raiz, veja [MySQL: Como Repor a Palavra-passe de Raiz](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Se o comando for executado com êxito, significa que o servidor MySQL já está em execução. Se não, siga os [passos de pós-instalação do MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) para verificar se o servidor MySQL local é iniciado.

### <a name="create-a-database"></a>Criar uma base de dados 

Na linha de comandos `mysql`, crie uma base de dados e uma tabela para os itens de tarefas.

```sql
CREATE DATABASE tododb;
```

Escreva `quit` para sair da ligação ao servidor.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Criar e executar aplicação de exemplo 

Neste passo, clone a aplicação Spring Boot de exemplo, configure-a para utilizar a base de dados MySQL local e execute-a no seu computador. 

### <a name="clone-the-sample"></a>Clonar o exemplo

Na janela do terminal, navegue para um diretório de trabalho e clone o repositório de exemplo. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Configurar a aplicação para utilizar a base de dados MySQL

Atualize o `spring.datasource.password` e o valor em *spring-boot-mysql-todo/src/main/resources/application.properties* com a mesma palavra-passe de raiz utilizada para abrir a linha de comandos MySQL:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Criar e executar o exemplo

Criar e executar o exemplo com o wrapper de Maven incluído no repositório:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Abra o browser em `http://localhost:8080` para ver o exemplo em ação. À medida que adiciona tarefas à lista, utilize os seguintes comandos do SQL na linha de comandos MySQL para ver os dados armazenados no MySQL.

```SQL
use testdb;
select * from todo_item;
```

Pare a aplicação, ao clicar em `Ctrl`+`C` no terminal. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Criar uma base de dados MySQL do Azure

Neste passo, cria uma instância da [Base de Dados do Azure para MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) com a [CLI do Azure](/cli/azure/install-azure-cli). Configure a aplicação de exemplo para utilizar esta base de dados mais tarde no tutorial.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um [grupo de recursos](../azure-resource-manager/resource-group-overview.md) com o comando [`az group create`](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contentor lógico onde recursos relacionados, como aplicações Web, bases de dados e contas de armazenamento são implementados e geridos. 

O exemplo seguinte cria um grupo de recursos na região Europa do Norte:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Para ver os possíveis valores que pode utilizar para `--location`, utilize o comando [`az appservice list-locations`](/cli/azure/appservice#list-locations).

### <a name="create-a-mysql-server"></a>Criar um servidor MySQL

No Cloud Shell, crie um servidor na Base de Dados do Azure para MySQL com o comando [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az-mysql-server-create).

No seguinte comando, substitua um nome de servidor exclusivo para o marcador de posição *\<mysql_server_name>*, um nome de utilizador para o marcador de posição *\<admin_user>* e uma palavra-passe para o marcador de posição *\<admin_password>*. O nome do servidor é utilizado como parte do ponto final do PostgreSQL (`https://<mysql_server_name>.mysql.database.azure.com`), por isso, o nome tem de ser exclusivo em todos os servidores no Azure.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql_server_name>--location "West Europe" --admin-user <admin_user> --admin-password <server_admin_password> --sku-name GP_Gen4_2
```

> [!NOTE]
> Uma vez que existem várias credenciais a considerar neste tutorial, para evitar confusões, `--admin-user` e `--admin-password` estão definidos para valores fictícios. Num ambiente de produção, siga as melhores práticas de segurança ao escolher um bom nome de utilizador e palavra-passe para o servidor MySQL no Azure.
>
>

Quando o servidor MySQL tiver sido criado, a CLI do Azure mostra informações semelhantes às do exemplo abaixo:

```json
{
  "location": "westeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "additionalProperties": {},
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  ...   +  
  -  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Configurar a firewall do servidor

No Cloud Shell, crie uma regra de firewall para o servidor MySQL permitir ligações ao cliente com o comando [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create). Quando os IPs inicial e final estão definidos como 0.0.0.0, a firewall apenas é aberta para outros recursos do Azure. 

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP] 
> Pode ser ainda mais restritivo na sua regra de firewall ao [utilizar apenas os endereços IP de saída que a aplicação utiliza](app-service-ip-addresses.md#find-outbound-ips).
>

## <a name="configure-the-azure-mysql-database"></a>Configurar a base de dados MySQL do Azure

Na janela de terminal local, ligue ao servidor MySQL no Azure. Utilize o valor que especificou anteriormente em _&lt;mysql_server_name>_. Quando lhe for pedida uma palavra-passe, utilize a que especificou quando criou a base de dados no Azure.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Criar uma base de dados 

Na linha de comandos `mysql`, crie uma base de dados e uma tabela para os itens de tarefas.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Criar um utilizador com permissões

Crie um utilizador de base de dados e conceda-lhe todos os privilégios da base de dados `tododb`. Substitua os marcadores de posição `<Javaapp_user>` e `<Javaapp_password>` pelo nome exclusivo da sua aplicação.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Escreva `quit` para sair da ligação ao servidor.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Implementar o exemplo no Serviço de Aplicações do Azure

Crie um plano do Serviço de Aplicações do Azure com o escalão de preço **FREE**, através do comando da CLI [`az appservice plan create`](/cli/azure/appservice/plan#az-appservice-plan-create). O plano do serviço de aplicações define os recursos físicos utilizados para alojar as suas aplicações. Todas as aplicações atribuídas a um plano do serviço de Aplicações partilham estes recursos, o que lhe permite economizar quando alojar várias aplicações. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Quando o plano estiver pronto, a CLI do Azure mostra um resultado semelhante ao seguinte exemplo:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Criar uma aplicação Web do Azure

No Cloud Shell, utilize o comando [`az webapp create`](/cli/azure/webapp#az-webapp-create) da CLI para criar uma definição de aplicação Web no plano do Serviço de Aplicações `myAppServicePlan`. A definição da aplicação Web fornece um URL com o qual pode aceder à sua aplicação e configura várias opções para implementar o código no Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Substitua o marcador de posição `<app_name>` pelo nome exclusivo da sua aplicação. Este nome exclusivo faz parte do nome de domínio predefinido da aplicação Web, pelo que o nome tem de ser exclusivo relativamente a todas as aplicações no Azure. Pode mapear qualquer entrada de nome de domínio personalizada para a aplicação Web antes de a expor aos utilizadores.

Quando a definição de aplicação Web estiver pronta, a CLI do Azure mostra informações semelhantes ao seguinte exemplo: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Configurar o Java 

No Cloud Shell, configure a configuração do tempo de execução de Java que a sua aplicação precisa, com o comando [`az webapp config set`](/cli/azure/webapp/config#az-webapp-config-set).

O comando seguinte configura a aplicação Web para ser executada num [Java 8 JDK](https://aka.ms/azure-jdks) recente e [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Configurar a aplicação para utilizar a base de dados SQL do Azure

Antes de executar a aplicação de exemplo, configure as definições da aplicação na aplicação Web para utilizar a base de dados MySQL do Azure que criou no Azure. Estas propriedades são expostas à aplicação Web como variáveis de ambiente e substituem os valores definidos em application.properties dentro da aplicação Web compactada. 

No Cloud Shell, configure as definições da aplicação com [`az webapp config appsettings`](/cli/azure/webapp/config/appsettings) na CLI:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Obter as credenciais de implementação de FTP 
Existem várias formas de implementar no serviço de aplicações do Azure, incluindo através de FTP, Git local, GitHub, Azure DevOps e BitBucket. Neste exemplo, será utilizado o FTP para implementar o ficheiro .WAR criado anteriormente no seu computador local no Serviço de Aplicações do Azure.

Para determinar quais as credenciais a transferir num comando de ftp para a Aplicação Web, utilize o comando [`az webapp deployment list-publishing-profiles`](/cli/azure/webapp/deployment#az-webapp-deployment-list-publishing-profiles) no Cloud Shell: 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>Carregar a aplicação através de FTP

Utilize a sua ferramenta FTP favorita para implementar o ficheiro .WAR na pasta */site/wwwroot/webapps* no endereço do servidor obtido do campo `URL` no comando anterior. Remova o diretório de aplicação predefinido (ROOT) existente e substitua o ROOT.war existente pelo ficheiro .WAR incorporado anteriormente no tutorial.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Testar a aplicação Web

Navegue para `http://<app_name>.azurewebsites.net/` e adicione algumas tarefas à lista. 

![Aplicação Java em execução no serviço de aplicações do Azure](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Parabéns!** Está a executar uma aplicação Java condicionada por dados no Serviço de Aplicações do Azure.

## <a name="update-the-app-and-redeploy"></a>Atualizar a aplicação e reimplementar

Atualize a aplicação para incluir uma coluna adicional na lista de tarefas para o dia no qual o item foi criado. O Spring Boot processa a atualização do esquema da base de dados por si, à medida que o modelo muda, sem alterar os registos de base de dados existentes.

1. No sistema local, abra *src/main/java/com/example/fabrikam/TodoItem.java* e adicione as seguintes importações à classe:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Adicione uma propriedade `String` `timeCreated` a *src/main/java/com/example/fabrikam/TodoItem.java*, inicializando-a com um carimbo de data/hora na criação do objeto. Adicione getters/setters à nova propriedade `timeCreated` enquanto estiver a editar este ficheiro.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Atualize *src/main/java/com/example/fabrikam/TodoDemoController.java* com uma linha no método `updateTodo` para definir o carimbo de data/hora:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Adicione suporte para o novo campo no modelo `Thymeleaf`. Atualize *src/main/resources/templates/index.html* com um novo cabeçalho de tabela para o carimbo de data/hora e um novo campo para apresentar o valor do carimbo de data/hora em cada linha de dados de tabela.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Recriar a aplicação:

    ```bash
    mvnw clean package 
    ```

6. Como anteriormente, transfira por FTP o ficheiro .WAR atualizado, removendo o diretório *site/wwwroot/webapps/ROOT* existente e *ROOT.war* e atualizando, em seguida, o ficheiro .WAR atualizado como ROOT.war. 

Quando atualizar a aplicação, uma coluna **Hora de Criação** estará agora visível. Quando adiciona uma nova tarefa, a aplicação irá preencher o carimbo de data/hora automaticamente. As suas tarefas existentes permanecem inalteradas e a funcionar com a aplicação, apesar de o modelo de dados subjacente ter sido alterado. 

![Aplicação Java atualizada com uma nova coluna](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Transmitir registos de diagnóstico em fluxo 

Enquanto executa a aplicação Java no Serviço de Aplicações do Azure, pode fazer com que os registos de consola sejam direcionados diretamente para o seu terminal. Dessa forma, pode obter as mesmas mensagens de diagnóstico para ajudar a depurar erros de aplicações.

Para iniciar a transmissão em fluxo do registo, utilize o comando [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-tail) no Cloud Shell.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Gerir a aplicação Web do Azure

Aceda ao [portal do Azure](https://portal.azure.com) para ver a aplicação Web que criou.

No menu à esquerda, clique em **Serviço de Aplicações** e clique no nome da sua aplicação Web do Azure.

![Navegação no portal para a aplicação Web do Azure](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Por predefinição, a página da sua aplicação Web mostra a página **Descrição Geral**. Esta página proporciona-lhe uma vista do desempenho da aplicação. Aqui, também pode realizar tarefas de gestão, como parar, iniciar, reiniciar e eliminar. Os separadores no lado esquerdo da página mostram as várias páginas de configuração que pode abrir.

![Página Serviço de Aplicações no portal do Azure](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Estes separadores na página mostram as muitas funcionalidades excelentes que pode adicionar à sua aplicação Web. A lista seguinte dá-lhe apenas algumas das possibilidades:
* Mapear um nome DNS personalizado
* Vincular um certificado SSL personalizado
* Configurar a implementação contínua
* Aumentar e reduzir verticalmente
* Adicionar a autenticação do utilizador

## <a name="clean-up-resources"></a>Limpar recursos

Se não precisar destes recursos para outro tutorial (veja os [Passos seguintes](#next)), pode eliminá-los ao executar o seguinte comando no Cloud Shell: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## Next steps

> [!div class="checklist"]
> * Create a MySQL database in Azure
> * Connect a sample Java app to the MySQL
> * Deploy the app to Azure
> * Update and redeploy the app
> * Stream diagnostic logs from Azure
> * Manage the app in the Azure portal

Advance to the next tutorial to learn how to map a custom DNS name to the app.

> [!div class="nextstepaction"] 
> [Map an existing custom DNS name to Azure Web Apps](app-service-web-tutorial-custom-domain.md)
