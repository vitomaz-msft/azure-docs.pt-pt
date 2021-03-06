---
title: Perguntas comuns para o modo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba mais sobre as perguntas mais freqüentes e respostas para o modo de malha do Azure Service Fabric.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 06/25/2018
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: f80f61cbfc1f7b719e73d7d29c6948bebe84aa6c
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278315"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Malha de recursos de infraestrutura do serviço perguntas mais frequentes
O Azure Service Fabric Mesh é um serviço totalmente gerido que permite aos programadores implementar aplicações de microsserviços sem gerir máquinas virtuais, armazenamento ou redes. Este artigo tem respostas para perguntas freqüentes.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Como posso comunicar um problema ou faça uma pergunta?

Faça perguntas, receba respostas dos engenheiros da Microsoft e reportar problemas na [repositório do GitHub service-fabric-malha-pré-visualização](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Quota e custo

**O que é o custo de participar na pré-visualização?**

Não existem custos para implementar aplicações ou contentores para a pré-visualização de malha atualmente. No entanto, Encorajamo-lo para eliminar os recursos a implementar e não deixá-los em execução, a menos que está testando intensamente o-los.

**Existe um limite de quota do número de núcleos e RAM?**

Sim, as quotas para cada subscrição estão definidas da seguinte forma:

- Número de aplicativos - 5 
- Núcleos por aplicação – 12 
- Total de RAM por aplicação – 48 GB 
- Pontos de extremidade de rede e entrada – 5  
- Volumes do Azure que pode anexar - 10 
- Número de réplicas do serviço – 3 
- O contentor maior, que pode implementar está limitado a 4 núcleos, 16GB de RAM.
- Pode alocar núcleos parciais para os contentores em incrementos de 0,5 núcleos até um máximo de 6 núcleos.

**O tempo que pode deixar os meu aplicativo implementado?**

Atualmente, limitada a vida útil de um aplicativo em dois dias. Trata-se para maximizar a utilização dos núcleos livres alocado para a pré-visualização. Como resultado, só são permitidas para execução de uma determinada continuamente para 48 horas, após o qual ele será encerrado pelo sistema. Se vir que isso aconteça, pode validar que o sistema encerrá-lo ao executar um `az mesh app show` comando na CLI do Azure e a verificar se ela retornar `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` 

Por exemplo: 

```cli
chackdan@Azure:~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Para continuar a implementar a mesma aplicação para a malha, deve eliminar o grupo de recursos associado à aplicação ou individualmente a remover a aplicação e todos os recursos de malha (incluindo a rede) relacionados. 

Para eliminar o grupo de recursos, utilize o `az group delete <nameOfResourceGroup>` comando. 

## <a name="supported-container-os-images"></a>Imagens de contentor suportados SO
As seguintes imagens de sistema operacional de contentor podem ser utilizadas quando a implementação de serviços.

- Windows - windowsservercore e nanoserver
    - Windows Server 2016
    - Versão 1709 do Windows Server
- Linux
    - Não existem limitações conhecidas

## <a name="features-gaps-and-known-issues"></a>Lacunas de funcionalidades e problemas conhecidos

**Depois de implantar meu aplicativo, o recurso de rede associado, pelo que não parece um endereço IP**

Existe um problema conhecido hoje em dia com o endereço IP aparecer após um atraso. Verificar o estado do recurso de rede dentro de alguns minutos para ver o endereço IP associado.

**Meu aplicativo está a conseguir aceder ao recurso de rede/volume correta**

No seu modelo de aplicação, terá de utilizar o ID de recurso completo para redes e volumes para poder aceder ao recurso associado. Eis o que isso é semelhante, o exemplo de início rápido:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

**Não vejo o atual modelo de aplicativo que suporta uma forma de encriptar moje tajné kódy**

Sim, o segredos de encriptação não é suportada na pré-visualização privada atual. 

**DNS não funciona da mesma forma no meu cluster de desenvolvimento do Service Fabric e na malha**

Existe um problema conhecido, onde poderá ter de fazer referência a serviços de forma diferente no seu cluster de desenvolvimento local e na malha do Azure. Utilize o seu cluster de desenvolvimento local {serviceName}. {applicationName}. Na malha de recursos de infraestrutura do serviço de Azure, utilize {servicename}. Malha do Azure não suporta atualmente a resolução de dns em todas as aplicações.

Para outros problemas conhecidos do DNS com a execução de um cluster de desenvolvimento do Service Fabric no Windows 10, veja aqui: [contentores Windows depurar](/azure/service-fabric/service-fabric-how-to-debug-windows-containers).

**Posso obter este erro quando utiliza o módulo CLI, ImportError: não é possível importar o nome "sdk_no_wait'**

Se estiver usando a versão mais antiga da CLI que 2.0.30, poderá receber este erro-

```
cannot import name 'sdk_no_wait'
Traceback (most recent call last):
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\cli.py", line 193, in invoke cmd_result = self.invocation.execute(args)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 241, in execute self.commands_loader.load_arguments(command)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 201, in load_arguments self.command_table[command].load_arguments() # this loads the arguments via reflection
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core\commands_init_.py", line 142, in load_arguments super(AzCliCommand, self).load_arguments()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\knack\commands.py", line 76, in load_arguments cmd_args = self.arguments_loader()
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 332, in default_arguments_loader op = handler or self.get_op_handler(operation)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\site-packages\azure\cli\core_init_.py", line 375, in get_op_handler op = import_module(mod_to_import)
File "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\lib\importlib_init_.py", line 126, in import_module return _bootstrap._gcd_import(name[level:], package, level)
File "", line 978, in _gcd_import
File "", line 961, in _find_and_load
File "", line 950, in _find_and_load_unlocked
File "", line 655, in _load_unlocked
File "", line 678, in exec_module
File "", line 205, in _call_with_frames_removed
File "C:\Users\annayak.azure\cliextensions\azure-cli-sbz\azext_sbz\custom.py", line 18, in 
from azure.cli.core.util import get_file_json, shell_safe_json_parse, sdk_no_wait
ImportError: cannot import name 'sdk_no_wait'.
```

**Recebo um erro de nome de distribuição de erro de correspondência ao instalar o pacote de extensão da CLI**

Isso não significa que a extensão não foi instalado. Ainda deve ser capaz de usar os comandos da CLI sem problemas.

**Quando eu aumentar horizontalmente, vejo que todos os meus contentores são afetados, incluindo meu aqueles em execução**

Este é um bug e deve ser corrigido na próxima atualização de tempo de execução.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral](service-fabric-mesh-overview.md).
