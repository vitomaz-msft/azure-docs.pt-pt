---
title: Começar com o SDK de dispositivos de voz
titleSuffix: Azure Cognitive Services
description: Pré-requisitos e instruções para começar a trabalhar com o SDK de dispositivos de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: erhopf
ms.openlocfilehash: e035e1bdedefc8e327b0179006b45f3bad4c41ee
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470205"
---
# <a name="get-started-with-the-speech-devices-sdk"></a>Começar com o SDK de dispositivos de voz

Este artigo descreve como configurar o seu desenvolvimento de PC e o kit de desenvolvimento de dispositivo de voz para o desenvolvimento de dispositivos com capacidade de voz com o SDK de dispositivos de voz. Em seguida, criar e implementar uma aplicação de exemplo para o dispositivo. 

O código-fonte da aplicação de exemplo está incluído com o SDK de dispositivos de voz. Também vale [está disponível no GitHub](https://github.com/Azure-Samples/Cognitive-Services-Speech-Devices-SDK).

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a desenvolver com o SDK de dispositivos de voz, recolha as informações e o software que necessário:

* Obter um [kit de desenvolvimento de ROOBO](http://ddk.roobo.com/). Kits estão disponíveis com as configurações de matriz de microfone linear ou circular. Escolha a configuração correta para as suas necessidades.

    |Configuração do kit de desenvolvimento|Localização de orador|
    |-----------------------------|------------|
    |Circular|Qualquer direção do dispositivo|
    |Linear|À frente do dispositivo|

* Obter a versão mais recente do SDK de dispositivos do voz, que inclui a partir de uma aplicação de exemplo do Android, o [site de download do SDK de dispositivos de voz](https://shares.datatransfer.microsoft.com/). Extraia o ficheiro. zip para uma pasta local, como C:\SDSDK.

* Instale [Android Studio](https://developer.android.com/studio/) e [Vysor](http://vysor.io/download/) no seu PC.

* Obter um [chave de subscrição do serviço de voz](get-started.md). Pode obter uma avaliação gratuita de 30 dias ou obter uma chave do seu dashboard do Azure.

* Se pretender utilizar o reconhecimento da intenção do serviço de voz, subscrever o [serviço de compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) (LUIS) e [obter uma chave de subscrição](https://docs.microsoft.com/azure/cognitive-services/luis/azureibizasubscription). 

    Pode [criar um modelo do LUIS simples](https://docs.microsoft.com/azure/cognitive-services/luis/) ou utilize o exemplo de modelo do LUIS, LUIS example.json. O exemplo de modelo do LUIS está disponível a partir da [site de download do SDK de dispositivos de voz](https://shares.datatransfer.microsoft.com/). Para carregar o ficheiro JSON de seu modelo para o [portal de LUIS](https://www.luis.ai/home), selecione **importar a nova aplicação**e, em seguida, selecione o ficheiro JSON.

## <a name="set-up-the-development-kit"></a>Configurar o kit de desenvolvimento

1. Ligar o kit de desenvolvimento para um PC ou ligar o adaptador usando um cabo USB mini. Quando o kit está ligado, um indicador de power verde são realçadas em quadro superior.

1. Ligue o kit de desenvolvimento para um computador com uma segunda mini cabo USB.

    ![ligar o kit de desenvolvimento](media/speech-devices-sdk/qsg-1.png)

1. Orientar o seu kit de desenvolvimento por qualquer configuração do circular ou linear.

    |Configuração do kit de desenvolvimento|Orientação|
    |-----------------------------|------------|
    |Circular|Upright, com microfones voltada para o limite|
    |Linear|No seu lado, com microfones voltada para (mostrado na imagem seguinte)|

    ![orientação do kit de programação linear](media/speech-devices-sdk/qsg-2.png)

1. Instale os certificados e o arquivo de tabela do word (palavra-chave) de reativação e definir as permissões do dispositivo de som. Escreva os seguintes comandos numa janela de linha de comando:

   ```
   adb push C:\SDSDK\Android-Sample-Release\scripts\roobo_setup.sh /data/ 
   adb shell
   cd /data/ 
   chmod 777 roobo_setup.sh
   ./roobo_setup.sh
   exit
   ```

    > [!NOTE]
    > Estes comandos utilizem a ponte de depuração do Android, `adb.exe`, que faz parte da instalação do Android Studio. Esta ferramenta está localizada no C:\Users\[nome de utilizador] \AppData\Local\Android\Sdk\platform ferramentas. Pode adicionar esse diretório ao seu caminho para que seja mais conveniente para invocar `adb`. Caso contrário, tem de especificar o caminho completo para a instalação do adb.exe em todos os comandos que invoquem `adb`.

    > [!TIP]
    > Mudo microfone e palestrante Certifique-se de que está a trabalhar com microfones o kit de desenvolvimento do seu PC. Dessa forma, não acidentalmente aciona o dispositivo com o áudio do PC.
    
1.  Inicie Vysor no seu computador.

    ![Vysor](media/speech-devices-sdk/qsg-3.png)

1.  O dispositivo deve estar listado na **escolha um dispositivo**. Selecione o **vista** botão junto ao dispositivo. 
 
1.  Ligar a sua rede sem fio ao selecionar o ícone de pasta e, em seguida, selecione **configurações** > **WLAN**.

    ![Vysor WLAN](media/speech-devices-sdk/qsg-4.png)
 
    > [!NOTE]
    > Se a sua empresa tem políticas sobre como ligar dispositivos ao seu sistema de Wi-Fi, terá de obter o endereço MAC e contacte o seu departamento de TI sobre como ligá-la à. o Wi-Fi sua empresa. 
    >
    > Para localizar o endereço MAC do kit de desenvolvimento, selecione o ícone de pasta do ficheiro no ambiente de trabalho do kit de desenvolvimento.
    >
    >  ![Pasta do ficheiro Vysor](media/speech-devices-sdk/qsg-10.png)
    >
    > Selecione **definições**. Procure "endereço mac" e, em seguida, selecione **endereço Mac** > **Advanced WLAN**. Anote o endereço MAC, que é apresentado junto à parte inferior da caixa de diálogo. 
    >
    > ![Endereço MAC de Vysor](media/speech-devices-sdk/qsg-11.png)
    >
    > Algumas empresas podem ter um limite de tempo em quanto tempo um dispositivo pode estar ligado ao seu sistema de Wi-Fi. Poderá ter de expandir o registo do kit de desenvolvimento com o seu sistema de Wi-Fi após um número de dias específico.
    > 
    > Se quiser anexar um palestrante para o kit de desenvolvimento, pode ligá-la na linha de áudio horizontalmente. Deve escolher um palestrante de boa qualidade, 3.5 mm.
    >
    > ![Vysor áudio](media/speech-devices-sdk/qsg-14.png)
 
## <a name="run-a-sample-application"></a>Executar uma aplicação de exemplo

Para executar os testes ROOBO e validar a configuração do kit de desenvolvimento, compilar e instalar a aplicação de exemplo:

1.  Inicie o Android Studio.

1.  Selecione **Open an existing Android Studio project** (Abrir um projeto existente do Android Studio).

    ![Android Studio - aberto um projeto existente](media/speech-devices-sdk/qsg-5.png)
 
1.  Vá para C:\SDSDK\Android-Sample-Release\example. Selecione **OK** para abrir o projeto de exemplo.
 
1.  Adicione a chave de subscrição de voz ao código-fonte. Se quiser experimentar o reconhecimento da intenção, adicione também seu [serviço de compreensão de idiomas](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) ID da chave de subscrição e a aplicação. 

    As chaves e informações sobre a aplicação entrar as seguintes linhas no arquivo de origem mainactivity. Java:

    ```java
    // Subscription
    private static final String SpeechSubscriptionKey = "[your speech key]";
    private static final String SpeechRegion = "westus";
    private static final String LuisSubscriptionKey = "[your LUIS key]";
    private static final String LuisRegion = "westus2.api.cognitive.microsoft.com";
    private static final String LuisAppId = "[your LUIS app ID]"
    ```

1. A palavra de reativação de predefinição (palavra-chave) é o "Computador". Também pode tentar uma das outras fornecidas reativação palavras, como "Máquina" ou "Assistente". Os ficheiros de recursos para essas palavras de reativação alternativo são no SDK de dispositivos de voz, na pasta de palavra-chave. Por exemplo, C:\SDSDK\Android-Sample-Release\keyword\Computer contém os arquivos usados para a palavra de reativação "Computador".

    Também pode [criar uma palavra de reativação personalizado](speech-devices-sdk-create-kws.md).

    Para instalar o word de reativação que pretende utilizar:
 
    * Crie uma pasta de palavra-chave na pasta de dados no dispositivo, executando os seguintes comandos numa janela de linha de comando:

        ```
        adb shell
        cd /data
        mkdir keyword
        exit
        ```

    * Copie os ficheiros kws.table kws_k.fst e words_kw.txt \data\keyword pasta do dispositivo. Execute os seguintes comandos numa janela de linha de comando. Se tiver criado uma [word personalizado reativação](speech-devices-sdk-create-kws.md), o ficheiro de kws.table gerado a partir da web está no mesmo diretório que os ficheiros kws.table kws_k.fst e words_kw.txt. Para uma palavra de reativação personalizado, utilize o `adb push C:\SDSDK\Android-Sample-Release\keyword\[wake_word_name]\kws.table /data/keyword` comando para enviar o ficheiro de kws.table para o kit de desenvolvimento:

        ```
        adb push C:\SDSDK\Android-Sample-Release\keyword\kws.table /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\kws_k.fst /data/keyword
        adb push C:\SDSDK\Android-Sample-Release\keyword\Computer\words_kw.txt /data/keyword
        ```
    
    * Estes ficheiros no aplicativo de exemplo de referência. Encontre as seguintes linhas no mainactivity. Java. Certifique-se de que a palavra-chave especificada é o que está a utilizar e que o caminho aponta para o `kws.table` ficheiro que enviou para o dispositivo.
        
        ```java
        private static final String Keyword = "Computer";
        private static final String KeywordModel = "/data/keyword/kws.table";
        ```

        > [!NOTE]
        > No seu próprio código, pode utilizar o ficheiro de kws.table para criar uma instância do modelo de palavra-chave e iniciar o reconhecimento de:
        >
        > ```java
        > KeywordRecognitionModel km = KeywordRecognitionModel.fromFile(KeywordModel);
        > final Task<?> task = reco.startKeywordRecognitionAsync(km);
        > ```

1.  Atualize as seguintes linhas, que contêm as definições de geometria de matriz de microfone:

    ```java
    private static final String DeviceGeometry = "Circular6+1";
    private static final String SelectedGeometry = "Circular6+1";
    ```
    A tabela seguinte descreve os valores disponíveis:
    
    |Variável|Significado|Valores disponíveis|
    |--------|-------|----------------|
    |`DeviceGeometry`|Configuração de mic físico|Para um kit de desenvolvimento circular: `Circular6+1` |
    |||Para um kit de programação linear: `Linear4`|
    |`SelectedGeometry`|Configuração do software do mic|Para um kit de desenvolvimento circular que utiliza todos os microfones: `Circular6+1`|
    |||Para um kit de desenvolvimento circular que utiliza quatro microfones: `Circular3+1`|
    |||Para um kit de programação linear que utiliza todos os microfones: `Linear4`|
    |||Para um kit de programação linear que utiliza dois microfones: `Linear2`|


1.  Para criar a aplicação, diante da **executar** menu, selecione **executar "aplicação"**. O **selecionar destino da implementação** é apresentada a caixa de diálogo. 

1. Selecione o seu dispositivo e, em seguida, selecione **OK** para implementar a aplicação no dispositivo.

    ![Selecione a caixa de diálogo de destino da implementação](media/speech-devices-sdk/qsg-7.png)
 
1.  O aplicativo de exemplo do SDK de dispositivos de fala é iniciado e exibe as seguintes opções:

    ![Exemplo de aplicativo de exemplo de SDK de dispositivos de voz e opções](media/speech-devices-sdk/qsg-8.png)

1. Experimentação!

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="certificate-failures"></a>Falhas de certificado

Se obtiver falhas de certificado ao utilizar o serviço de voz, certifique-se de que o dispositivo tem a correta data e hora:

1. Aceda a **definições**. Sob **System**, selecione **data e hora**.

    ![Em definições, selecione a data e hora](media/speech-devices-sdk/qsg-12.png)

1. Manter o **automática data e hora** opção selecionada. Sob **fuso de horário selecione**, selecione o seu fuso horário atual. 

    ![Selecione as opções de data e fuso horário](media/speech-devices-sdk/qsg-13.png)

    Quando vir que o tempo o kit de desenvolvimento corresponde a hora no seu PC, o kit de desenvolvimento está ligado à internet. 
    
    Para obter mais informações de desenvolvimento, consulte a [guia de desenvolvimento ROOBO](http://dwn.roo.bo/server_upload/ddk/ROOBO%20Dev%20Kit-User%20Guide.pdf).

### <a name="audio"></a>Áudio

ROOBO fornece uma ferramenta que captura todos os áudio a memória flash. Ele pode ajudar a resolver problemas de áudio. Uma versão da ferramenta é fornecida para cada configuração de kit de desenvolvimento. Sobre o [ROOBO site](http://ddk.roobo.com/), selecione o seu dispositivo e, em seguida, selecione a **ROOBO ferramentas** link na parte inferior da página.
