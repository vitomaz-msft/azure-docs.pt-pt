---
title: Resolver problemas do Azure IoT Edge | Microsoft Docs
description: Resolver problemas comuns e conhecer as competências de resolução de problemas do Azure IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a553798a3ac15340805984a0e87312875f82c46c
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567660"
---
# <a name="common-issues-and-resolutions-for-azure-iot-edge"></a>Problemas comuns e resoluções do Azure IoT Edge

Se ocorrerem problemas com o Azure IoT Edge no seu ambiente, utilize este artigo como guia para a resolução de problemas. 

## <a name="standard-diagnostic-steps"></a>Passos de diagnóstico padrão 

Quando ocorrer um problema, saiba mais sobre o estado do seu dispositivo IoT Edge ao rever os registos de contentor e as mensagens que passam de e para o dispositivo. Utilize as ferramentas e comandos nesta secção para recolher informações. 

### <a name="check-the-status-of-the-iot-edge-security-manager-and-its-logs"></a>Verifique o estado do Gestor de segurança do IoT Edge e os respetivos registos:

No Linux:
- Para ver o estado do Gestor de segurança de borda de IoT:

   ```bash
   sudo systemctl status iotedge
   ```

- Para ver os registos do Gestor de segurança de borda de IoT:

    ```bash
    sudo journalctl -u iotedge -f
    ```

- Para ver mais detalhadas registos do Gestor de segurança de borda de IoT:

   - Edite as definições de daemon de iotedge:

      ```bash
      sudo systemctl edit iotedge.service
      ```
   
   - Atualize as seguintes linhas:
    
      ```
      [Service]
      Environment=IOTEDGE_LOG=edgelet=debug
      ```
    
   - Reinicie o Daemon de segurança de IoT Edge:
    
      ```bash
      sudo systemctl cat iotedge.service
      sudo systemctl daemon-reload
      sudo systemctl restart iotedge
      ```

No Windows:
- Para ver o estado do Gestor de segurança de borda de IoT:

   ```powershell
   Get-Service iotedge
   ```

- Para ver os registos do Gestor de segurança de borda de IoT:

   ```powershell
   # Displays logs from today, newest at the bottom.
 
   Get-WinEvent -ea SilentlyContinue `
   -FilterHashtable @{ProviderName= "iotedged";
     LogName = "application"; StartTime = [datetime]::Today} |
   select TimeCreated, Message |
   sort-object @{Expression="TimeCreated";Descending=$false} |
   format-table -autosize -wrap
   ```

### <a name="if-the-iot-edge-security-manager-is-not-running-verify-your-yaml-configuration-file"></a>Se o Gestor de segurança do IoT Edge não está em execução, verifique se o ficheiro de configuração yaml

> [!WARNING]
> Ficheiros YAML não podem conter separadores como identation. Utilize espaços de 2 em vez disso.

No Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

No Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

### <a name="check-container-logs-for-issues"></a>Verifique os registos de contentor para problemas

Assim que o Daemon de segurança do IoT Edge está em execução, consulte os registos dos contentores para detetar problemas. Comece a utilizar os contentores implementados e, em seguida, veja os contentores que compõem o runtime do IoT Edge: Agente do Edge e Hub do Edge. Os registos do Agente do Edge dispõem normalmente de informações sobre o ciclo de vida de cada contentor. Os registos do Hub do Edge dispõem de informações sobre mensagens e encaminhamento. 

   ```cmd
   iotedge logs <container name>
   ```

### <a name="view-the-messages-going-through-the-edge-hub"></a>Ver as mensagens que passam pelo hub do Edge

Pode ver as mensagens que passam pelo hub do Edge e recolher informações de registos verbosos dos contentores tempo de execução. Para ativar registos verbosos nestes contentores, defina `RuntimeLogLevel` no ficheiro de configuração de yaml. Para abrir o ficheiro:

No Linux:

   ```bash
   sudo nano /etc/iotedge/config.yaml
   ```

No Windows:

   ```cmd
   notepad C:\ProgramData\iotedge\config.yaml
   ```

Por predefinição, o `agent` elemento terá um aspeto semelhante ao seguinte exemplo:

   ```yaml
   agent:
     name: edgeAgent
     type: docker
     env: {}
     config:
       image: mcr.microsoft.com/azureiotedge-agent:1.0
       auth: {}
   ```

Substitua `env: {}` com:

> [!WARNING]
> Ficheiros YAML não podem conter separadores como identation. Utilize espaços de 2 em vez disso.

   ```yaml
   env:
     RuntimeLogLevel: debug
   ```

Guarde o ficheiro e reinicie o Gestor de segurança de IoT Edge.

Também pode verificar as mensagens que são enviadas entre o Hub IoT e os dispositivos do IoT Edge. Veja estas mensagens com a extensão [Toolkit IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) para o Visual Studio Code. Para obter mais informações, consulte [ferramenta útil ao desenvolver com o Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

### <a name="restart-containers"></a>Reiniciar contentores
Depois de investigar os registos e as mensagens para informações, pode tentar reiniciar contentores:

```
iotedge restart <container name>
```

Reinicie os contentores de runtime do IoT Edge:

```
iotedge restart edgeAgent && iotedge restart edgeHub
```

### <a name="restart-the-iot-edge-security-manager"></a>Reinicie o Gestor de segurança de IoT Edge

Se o problema ainda mantém, pode tentar reiniciar o Gestor de segurança de IoT Edge.

No Linux:

   ```cmd
   sudo systemctl restart iotedge
   ```

No Windows:

   ```powershell
   Stop-Service iotedge -NoWait
   sleep 5
   Start-Service iotedge
   ```

## <a name="edge-agent-stops-after-about-a-minute"></a>O Agente do Edge para após cerca de um minuto

O Agente do Edge é iniciado e executado com êxito durante cerca de um minuto e, em seguida, para. Os registos indicam que o agente do Edge tenta estabelecer ligação ao IoT Hub através de AMQP e, em seguida, tenta ligar através de AMQP sobre WebSocket. Quando ocorre uma falha, o Agente do Edge sai. 

Registos do Agente do Edge de exemplo:

```output
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### <a name="root-cause"></a>Causa raiz
Uma configuração de rede na rede do anfitrião está a impedir que o Agente do Edge alcance a rede. O agente primeiro tenta ligar através de AMQP (porta 5671). Se a ligação falhar, tenta WebSockets (porta 443).

O runtime do IoT Edge configura uma rede para comunicar em cada um dos módulos. No Linux, esta rede é uma rede de ponte. No Windows, utiliza NAT. Este problema é mais comum nos dispositivos Windows com contentores do Windows que utilizam a rede NAT. 

### <a name="resolution"></a>Resolução
Certifique-se de que existe uma rota para a Internet para os endereços IP atribuídos a esta rede de ponte/NAT. Por vezes, uma configuração de VPN no anfitrião sobrepõe-se à rede do IoT Edge. 

## <a name="edge-hub-fails-to-start"></a>O Hub do Edge falha ao iniciar

O Hub do Edge falha ao iniciar e apresenta a seguinte mensagem nos registos: 

```output
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### <a name="root-cause"></a>Causa raiz
Outro processo no computador anfitrião vinculou a porta 443. O Hub do Edge mapeia as portas 5671 e 443 para utilizar em cenários de gateway. Este mapeamento de portas falha se outro processo já tiver vinculado esta porta. 

### <a name="resolution"></a>Resolução
Encontre e pare o processo que está a utilizar a porta 443. Este processo é normalmente um servidor Web.

## <a name="edge-agent-cant-access-a-modules-image-403"></a>O Agente do Edge não consegue aceder a uma imagem do módulo (403)
A execução de um contentor falha e os registos do Agente mostram um erro 403. 

### <a name="root-cause"></a>Causa raiz
O Agente do Edge não tem permissões para aceder à imagem de um módulo. 

### <a name="resolution"></a>Resolução
Certifique-se de que as suas credenciais de registo estão especificados corretamente no seu manifesto de implantação

## <a name="iot-edge-security-daemon-fails-with-an-invalid-hostname"></a>O daemon de segurança de IoT Edge falha com um nome de anfitrião inválido

O comando `sudo journalctl -u iotedge` falhar e imprime a mensagem seguinte: 

```output
Error parsing user input data: invalid hostname. Hostname cannot be empty or greater than 64 characters
```

### <a name="root-cause"></a>Causa raiz
O runtime do IoT Edge só pode suportar os nomes de anfitrião que são mais curta do que 64 carateres. Máquinas físicas, normalmente, não têm nomes de anfitrião longo, mas o problema é mais comum numa máquina virtual. Os nomes de anfitrião gerados automaticamente para máquinas de virtuais do Windows alojadas no Azure, em particular, tendem a ser longos. 

### <a name="resolution"></a>Resolução
Quando vir este erro, pode resolvê-lo ao configurar o nome DNS da sua máquina virtual e, em seguida, definir o nome DNS como o nome de anfitrião no comando de configuração.

1. No portal do Azure, navegue para a página de descrição geral da sua máquina virtual. 
2. Selecione **configurar** sob o nome DNS. Se a sua máquina virtual já tem um nome DNS configurado, não precisa de configurar um novo. 

   ![Configurar o nome DNS](./media/troubleshoot/configure-dns.png)

3. Forneça um valor para **etiqueta de nome DNS** e selecione **guardar**.
4. Copie o novo nome DNS, o que deve estar no formato  **\<DNSnamelabel\>.\< uma vmlocation\>. cloudapp.azure.com**.
5. Dentro da máquina virtual, utilize o seguinte comando para configurar o runtime do IoT Edge com o nome de DNS:

   - No Linux:

      ```bash
      sudo nano /etc/iotedge/config.yaml
      ```

   - No Windows:

      ```cmd
      notepad C:\ProgramData\iotedge\config.yaml
      ```

## <a name="stability-issues-on-resource-constrained-devices"></a>Problemas de estabilidade no recurso restrita dispositivos 
Pode encontrar problemas de estabilidade nos dispositivos restritos, como o Raspberry Pi, especialmente quando utilizado como um gateway. Os sintomas incluem exceções de memória no módulo de hub do edge insuficiente, não é possível ligar dispositivos downstream ou o dispositivo deixa de enviar mensagens de telemetria após algumas horas.

### <a name="root-cause"></a>Causa raiz
O hub do edge, que faz parte do runtime do edge, é otimizado para desempenho, por padrão e tenta alocar grandes segmentos de memória. Esta otimização não é ideal para dispositivos periféricos restrita e pode causar problemas de estabilidade.

### <a name="resolution"></a>Resolução
Para o hub do edge, defina uma variável de ambiente **OptimizeForPerformance** ao **falso**. Existem duas formas de fazer isso:

Na interface de Usuário: 

No portal de *detalhes do dispositivo*->*definir módulos*->*configurar definições de Runtime do Edge avançadas*, criar uma variável de ambiente chamado *OptimizeForPerformance* que está definido como *false* para o *Hub do Edge*.

![optimizeforperformance](./media/troubleshoot/OptimizeForPerformanceFalse.png)

**OR**

No manifesto de implantação:

```json
  "edgeHub": {
    "type": "docker",
    "settings": {
      "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
      "createOptions": <snipped>
    },
    "env": {
      "OptimizeForPerformance": {
          "value": "false"
      }
    },
```
## <a name="cant-get-the-iot-edge-daemon-logs-on-windows"></a>Não é possível obter registos de daemon o IoT Edge no Windows
Se obtiver um EventLogException quando utilizar `Get-WinEvent` no Windows, verifique as suas entradas de registo.

### <a name="root-cause"></a>Causa raiz
O `Get-WinEvent` comando do PowerShell se baseia numa entrada de registo estar presente para localizar os registos por um específico `ProviderName`.

### <a name="resolution"></a>Resolução
Defina uma entrada de registo para o daemon de IoT Edge. Criar uma **iotedge.reg** com o seguinte conteúdo de ficheiros e importar para o registo do Windows ao fazer duplo clique ou utilizar o `reg import iotedge.reg` comando:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog\Application\iotedged]
"CustomSource"=dword:00000001
"EventMessageFile"="C:\\ProgramData\\iotedge\\iotedged.exe"
"TypesSupported"=dword:00000007
```

## <a name="iot-edge-module-fails-to-send-a-message-to-the-edgehub-with-404-error"></a>Falha do módulo do IoT Edge enviar uma mensagem para o edgeHub com o erro 404

Um personalizado ocorre uma falha do módulo de IoT Edge para enviar uma mensagem para o edgeHub com um 404 `Module not found` erro. O daemon de IoT Edge imprime a mensagem seguinte nos registos: 

```output
Error: Time:Thu Jun  4 19:44:58 2018 File:/usr/sdk/src/c/provisioning_client/adapters/hsm_client_http_edge.c Func:on_edge_hsm_http_recv Line:364 executing HTTP request fails, status=404, response_buffer={"message":"Module not found"}u, 04 ) 
```

### <a name="root-cause"></a>Causa raiz
O daemon de IoT Edge impõe a identificação de processo para ligar ao edgeHub por motivos de segurança de todos os módulos. Verifica que todas as mensagens sendo enviadas por um módulo é proveniente do ID de processo principal do módulo. Se uma mensagem está a ser enviada por um módulo de um ID de processo diferente do que inicialmente estabelecido, ele rejeitará a mensagem com uma mensagem de 404 erro.

### <a name="resolution"></a>Resolução
Certifique-se de que o mesmo ID de processo é sempre usado pelo módulo do IoT Edge personalizado para enviar mensagens para o edgeHub. Por exemplo, certifique-se de que `ENTRYPOINT` em vez de `CMD` comando no seu ficheiro do Docker, uma vez que `CMD` levará a um processo de ID para o módulo e outro ID de processo para o comando de bash, ao passo que a executar o programa principal `ENTRYPOINT` levará a um id de processo único.


## <a name="firewall-and-port-configuration-rules-for-iot-edge-deployment"></a>Regras de configuração de firewall e a porta para implementação do IoT Edge
O Azure IoT Edge permite a comunicação entre um servidor de borda no local e na cloud Azure através de protocolos suportados do IoT Hub, veja [escolher um protocolo de comunicação](../iot-hub/iot-hub-devguide-protocols.md). Para maior segurança, os canais de comunicação entre o Azure IoT Edge e IoT Hub do Azure estão sempre configurados para serem saída. Esta configuração se baseia a [padrão de comunicação Auxiliado de serviços](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/), que minimiza a superfície de ataque para uma entidade maliciosa explorar. Comunicação de entrada só é necessário para cenários específicos em que precisa enviar mensagens para o dispositivo Azure IoT Edge IoT Hub do Azure. Mensagens da cloud para dispositivo estão protegidas através de canais TLS seguras e podem ser ainda mais protegidas usando certificados X.509 e módulos de dispositivo TPM. O Gestor de segurança do Azure IoT Edge rege como essa comunicação pode ser estabelecida, consulte [Gestor de segurança do Microsoft Edge IoT](../iot-edge/iot-edge-security-manager.md).

Embora o IoT Edge fornece uma configuração avançada para proteger o runtime do Azure IoT Edge e implementar módulos, é ainda depende da configuração de máquina e da rede subjacente. Portanto, é imperativo assegurar a regras de rede e de firewall adequadas são configuradas para o Edge seguro para comunicação na nuvem. O seguinte pode ser usado como uma diretriz quando de regras de firewall de configuração para os servidores subjacentes onde está alojado o runtime do Azure IoT Edge:

|Protocolo|Porta|entrada|Saída|Orientação|
|--|--|--|--|--|
|MQTT|8883|BLOQUEADO (predefinição)|BLOQUEADO (predefinição)|<ul> <li>Configure o envio (saída) para ser aberto quando utiliza MQTT como o protocolo de comunicação.<li>1883 para MQTT não é suportado pelo IoT Edge. <li>Ligações de entrada (entrada) devem ser bloqueadas.</ul>|
|AMQP|5671|BLOQUEADO (predefinição)|ABERTO (predefinição)|<ul> <li>Protocolo de comunicação padrão para o IoT Edge. <li> Tem de ser configurado para ser aberto, se o Azure IoT Edge não está configurado para outros protocolos suportados ou AMQP é o protocolo de comunicação desejada.<li>5672 para AMQP não é suportado pelo IoT Edge.<li>Bloquear esta porta, quando o protocolo de suporte do Azure IoT Edge utiliza um Hub de IoT diferentes.<li>Ligações de entrada (entrada) devem ser bloqueadas.</ul></ul>|
|HTTPS|443|BLOQUEADO (predefinição)|ABERTO (predefinição)|<ul> <li>Configure o envio (saída) para ser aberto em 443 para o aprovisionamento do IoT Edge. Esta configuração é necessária quando usar scripts manuais ou do Azure IoT dispositivo aprovisionamento DPS (serviço). <li>Conexão de entrada (entrada) deve ser aberto apenas para cenários específicos: <ul> <li>  Se tiver um gateway transparente com dispositivos de folha que poderá enviar pedidos de método. Neste caso, porta 443 não precisa de estar abertas a redes externas para ligar ao IoTHub ou fornecer serviços de IoTHub através do Azure IoT Edge. Assim, a regra de entrada pode ser restrita para abrirem apenas de entrada (entrada) a partir da rede interna. <li> Para o cliente para cenários de dispositivo (C2D).</ul><li>80 para HTTP não é suportado pelo IoT Edge.<li>Se protocolos diferentes do HTTP (por exemplo, AMQP ou MQTT) não podem ser configurados na empresa; as mensagens podem ser enviadas por WebSockets. Porta 443 será utilizada para a comunicação WebSocket nesse caso.</ul>|


## <a name="next-steps"></a>Passos Seguintes
Encontrou um erro na plataforma do IoT Edge? [Submeter um problema](https://github.com/Azure/iotedge/issues) para que possamos continuar a melhorar. 

Se tiver mais perguntas, crie uma [pedido de suporte](https://portal.azure.com/#create/Microsoft.Support) para obter ajuda. 

