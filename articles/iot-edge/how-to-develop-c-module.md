---
title: Depurar os módulos de C para o Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para desenvolver, criar e depurar um módulo de C para o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/13/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d40b82b5beac2da78038e303cb50402d6fa0be7a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566029"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depurar os módulos de C para o Azure IoT Edge

Pode transformar sua lógica de negócio em módulos do Azure IoT Edge. Este artigo mostra-lhe como utilizar o Visual Studio Code (código de VS) como a principal ferramenta para desenvolver e depurar os módulos de C.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que utilize um computador ou máquina virtual a executar o Windows ou Linux da máquina de desenvolvimento. E simular o seu dispositivo IoT Edge no computador de desenvolvimento com o daemon de segurança de IoT Edge.

> [!NOTE]
> Este artigo depuração demonstra como anexar um processo num contêiner de módulo e depurá-lo com o VS Code. Apenas pode depurar módulos de C em Linux amd64 contentores. Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging).

Uma vez que este artigo usa código do Visual Studio como a ferramenta de desenvolvimento principal, instale o VS Code. Em seguida, adicione as extensões necessárias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão C/C++](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) para o Visual Studio Code.
* [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de Docker para criar a imagem do módulo e um registo de contentores para armazenar a imagem de módulo:
* [Docker Community Edition](https://docs.docker.com/install/) no computador de desenvolvimento. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Pode utilizar um registo do Docker local para o protótipo e fins de testes em vez de um registo de cloud. 

Para testar seu módulo num dispositivo, precisa de um hub IoT Active Directory com, pelo menos, um dispositivo do IoT Edge. Para utilizar o seu computador como um dispositivo IoT Edge, siga os passos no guia de introdução para [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Siga estes passos para criar um módulo do IoT Edge com base no SDK de C do IoT do Azure com o Visual Studio Code e a extensão Azure IoT Edge. Em primeiro lugar é criar uma solução e, em seguida, gerar primeiro módulo nessa solução. Cada solução pode conter mais de um módulo. 

1. No Visual Studio Code, selecione **View** > **Terminal integrado**.

2. Selecione **View** > **paleta de comandos**. 

3. Na paleta de comandos, introduza e execute o comando **do Azure IoT Edge: nova solução do IoT Edge**.

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

4. Procure a pasta onde pretende criar a nova solução. Escolher **selecionar pasta**. 

5. Introduza um nome para a sua solução. 

6. Selecione **C módulo** como o modelo para o primeiro módulo na solução.

7. Introduza um nome para seu módulo. Escolha um nome que é exclusivo no seu registo de contentor. 

8. Forneça o nome do repositório de imagens do módulo. Código de VS autopopulates o módulo nome com **localhost:5000**. Substitua-o com as suas próprias informações de registo. Se usar um registo do Docker local para fins de teste, em seguida, **localhost** tudo bem. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **\<nome do registo\>. azurecr.io**. Substitua apenas a parte localhost da cadeia de carateres, não elimine o nome do módulo. 

   ![Fornecer repositório de imagens do Docker](./media/how-to-develop-c-module/repository.png)

Código VS usa as informações fornecidas, cria uma solução de IoT Edge e, em seguida, carrega-o numa nova janela.

   ![Ver a solução de IoT Edge](./media/how-to-develop-c-module/view-solution.png)

Existem quatro itens dentro da solução: 
* R **.vscode** pasta contém as configurações de depuração.
* R **módulos** pasta contém subpastas para cada módulo. Neste momento, tem apenas um. Mas pode adicionar mais na paleta de comandos com o comando **do Azure IoT Edge: Adicionar módulo do IoT Edge**. 
* Uma **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo. 

   > [!NOTE]
   > Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente. 

* R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [Aprenda a usar manifestos de implantação para implementar módulos e rotas](module-composition.md). 

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O código do módulo de C predefinido que é fornecido com a solução está localizado em **módulos** > [nome da sua módulo] > **Main**. O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo de C com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Crie e implemente o seu módulo para depuração

Em cada pasta de módulo, há vários arquivos de Docker para tipos de contentor diferente. Utilizar qualquer um destes ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste. Atualmente, os módulos de C suportam a depuração apenas em contentores do Linux amd64.

1. No VS Code, navegue para o `deployment.template.json` ficheiro. Atualizar o seu URL de imagem do módulo adicionando **.debug** ao fim.

    ![Adicionar *** .debug para o seu nome de imagem](./media/how-to-develop-c-module/image-debug.png)

2. Substitua o createOptions do módulo de C na **deployment.template.json** com abaixo conteúdo e guarde este ficheiro: 
    
    ```json
    "createOptions": "{\"HostConfig\": {\"Privileged\": true}}"
    ```

2. Na paleta de comandos VS Code, introduza e execute o comando **do Azure IoT Edge: solução de compilação e de Push IoT Edge**.
3. Selecione o `deployment.template.json` ficheiro para a sua solução da paleta de comandos. 
4. No Azure IoT Hub Device Explorer, clique com botão direito um ID de dispositivo do IoT Edge. Em seguida, selecione **criar a implementação de único dispositivo**. 
5. Abra a solução **config** pasta. Em seguida, selecione o `deployment.json` ficheiro. Escolher **selecionar o manifesto de implantação do Edge**. 

Verá a implementação foi criada com êxito com um ID de implementação num terminal integrado do VS Code.

Verificar o estado do contentor no Explorador do Docker de código do VS ou ao executar o `docker ps` comando no terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>Iniciar a depuração de módulo de C no VS Code
Mantém depuração de código de VS informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isso `launch.json` arquivo foi gerado quando criou uma nova solução de IoT Edge. Ele atualiza cada vez que adiciona um novo módulo que oferece suporte à depuração. 

1. Navegue para a vista de depuração do VS Code. Selecione o ficheiro de configuração de depuração para seu módulo. O nome da opção de depuração deve ser semelhante a **ModuleName Remote Debug (C)**

   ![Configuração de depuração Select](./media/how-to-develop-c-module/debug-config.png)

2. Navegue para `main.c`. Adicione um ponto de interrupção nesse arquivo.

3. Selecione **iniciar depuração** ou selecione **F5**. Selecione o processo para anexar a.

4. Na vista de depuração de código do VS, verá as variáveis no painel esquerdo. 

O exemplo anterior mostra como depurar os módulos do IoT Edge do C em contentores. Ele adicionou portas expostas no seu createOptions de contentor do módulo. Depois de concluir a depuração de seus módulos de C, recomendamos que remova estas portas expostas de módulos do IoT Edge prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Depois de criar seu módulo, saiba como [implementar módulos do Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md).

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
