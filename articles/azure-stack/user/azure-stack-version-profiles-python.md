---
title: Através de perfis de versão de API com Python no Azure Stack | Documentos da Microsoft
description: Saiba mais sobre como utilizar perfis de versão de API com Python no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
<!-- dev: viananth -->
ms.openlocfilehash: 657eefcd5b7793f2f181d074084a272e65943681
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246451"
---
# <a name="use-api-version-profiles-with-python-in-azure-stack"></a>Utilizar perfis de versão de API com Python no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

## <a name="python-and-api-version-profiles"></a>Perfis de versão do Python e a API

O Python SDK suporta perfis de versão de API para plataformas de cloud diferente, como o Azure Stack e o global Azure. Pode utilizar perfis de API na criação de soluções para uma cloud híbrida. O Python SDK suporta os seguintes perfis de API:

1. **latest**  
    O perfil destina-se as versões de API mais recentes para todos os fornecedores de serviços na plataforma Azure.
2.  **perfil-de-03-09-2017**  
    **perfil-de-03-09-2017**  
    O perfil destina-se as versões de API dos fornecedores de recursos suportados pelo Azure Stack.

    Para obter mais informações sobre os perfis de API e o Azure Stack, veja [perfis de versão de API de gerir no Azure Stack](azure-stack-version-profiles.md).

## <a name="install-azure-python-sdk"></a>Instalar o SDK de Python para o Azure

1.  Instalar o Git a partir do [o site oficial do](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  Para obter instruções instalar o SDK de Python, veja [do Azure para programadores de Python](https://docs.microsoft.com/python/azure/python-sdk-azure-install?view=azure-python).
3.  Se não está disponível, criar uma subscrição e guarde o ID de subscrição a ser utilizado mais tarde. Para obter instruções criar uma subscrição, veja [criar subscrições de ofertas no Azure Stack](../azure-stack-subscribe-plan-provision-vm.md). 
4.  Criar um principal de serviço e guarde o seu ID e segredo. Para obter instruções criar um principal de serviço para o Azure Stack, veja [fornecer acesso de aplicações para o Azure Stack](../azure-stack-create-service-principals.md). 
5.  Certifique-se de que o seu principal de serviço tem a função de proprietário/Contribuidor na sua subscrição. Para obter instruções sobre como atribuir a função ao principal de serviço, consulte [fornecer acesso de aplicações para o Azure Stack](../azure-stack-create-service-principals.md).

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o SDK do Python do Azure com o Azure Stack, tem de indicar os valores seguintes e, em seguida, definir valores com variáveis de ambiente. Veja as instruções depois da tabela para o seu sistema de operativo sobre como definir as variáveis de ambiente. 

| Valor | Variáveis de ambiente | Descrição |
|---------------------------|-----------------------|-------------------------------------------------------------------------------------------------------------------------|
| ID do inquilino | AZURE_TENANT_ID | O valor do seu Azure Stack [ID de inquilino](../azure-stack-identity-overview.md). |
| ID de Cliente | AZURE_CLIENT_ID | O serviço de ID da aplicação principal guardado quando principal de serviço foi criado na secção anterior deste documento. |
| ID da subscrição | AZURE_SUBSCRIPTION_ID | O [ID de subscrição](../azure-stack-plan-offer-quota-overview.md#subscriptions) é como acessa ofertas no Azure Stack. |
| Segredo do Cliente | AZURE_CLIENT_SECRET | A aplicação do principal de serviço segredo guardado quando principal de serviço foi criado. |
| Ponto final do Gestor de recursos | ARM_ENDPOINT | Ver [o ponto de final do Gestor de recursos do Azure Stack](azure-stack-version-profiles-ruby.md#the-azure-stack-resource-manager-endpoint). |


## <a name="python-samples-for-azure-stack"></a>Amostras de Python para o Azure Stack 

Pode utilizar os seguintes exemplos de código para executar tarefas de gestão comuns para máquinas virtuais no seu Azure Stack.

Os exemplos de código mostram-lhe para:

- Crie máquinas virtuais:
    - Criar uma Máquina Virtual do Linux
    - Criar uma máquina virtual do Windows
- Atualize uma máquina virtual:
    - Expanda uma unidade
    - Etiquetar uma máquina virtual
    - Anexar discos de dados
    - Desanexar discos de dados
- Opere uma máquina virtual:
    - Iniciar uma máquina virtual
    - Parar uma máquina virtual
    - Reiniciar uma máquina virtual
- Máquinas de virtuais de lista
- Eliminar uma máquina virtual

Para rever o código para executar estas operações, consulte a **run_example()** função no script de Python **Hybrid/unmanaged-disks/example.py** no repositório GitHub [ virtual máquinas-python-gerenciar](https://github.com/viananth/virtual-machines-python-manage/tree/8643ed4bec62aae6fdb81518f68d835452872f88).

Cada operação é claramente rotulada com um comentário e uma função de impressa.
Os exemplos não são necessariamente pela ordem apresentada na lista acima.


## <a name="run-the-python-sample"></a>Executar o exemplo de Python

1.  Se ainda não o tiver, [instalar o Python](https://www.python.org/downloads/).

    Este exemplo (e o SDK) são compatível com o Python 2.7, 3.4, 3.5 e 3.6.

2.  Recomendação geral para o desenvolvimento do Python é usar um ambiente Virtual. 
    Para obter mais informações, consulte https://docs.python.org/3/tutorial/venv.html
    
    Instalar e inicializar o ambiente virtual com o módulo "venv" em Python 3 (tem de instalar [virtualenv](https://pypi.python.org/pypi/virtualenv) para o Python 2.7):

    ````bash
    python -m venv mytestenv # Might be "python3" or "py -3.6" depending on your Python installation
    cd mytestenv
    source bin/activate      # Linux shell (Bash, ZSH, etc.) only
    ./scripts/activate       # PowerShell only
    ./scripts/activate.bat   # Windows CMD only
    ````

3.  Clone o repositório.

    ````bash
    git clone https://github.com/Azure-Samples/virtual-machines-python-manage.git
    ````

4.  Instale as dependências através do pip.

    ````bash
    cd virtual-machines-python-manage\Hybrid
    pip install -r requirements.txt
    ````

5.  Criar uma [principal de serviço](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals) para trabalhar com o Azure Stack. Certifique-se de que tem seu principal de serviço [função de proprietário/Contribuidor](https://docs.microsoft.com/azure/azure-stack/azure-stack-create-service-principals#assign-role-to-service-principal) na sua subscrição.

6.  Defina as seguintes variáveis e exportar estas variáveis de ambiente para sua shell atual. 

    ```bash
    export AZURE_TENANT_ID={your tenant id}
    export AZURE_CLIENT_ID={your client id}
    export AZURE_CLIENT_SECRET={your client secret}
    export AZURE_SUBSCRIPTION_ID={your subscription id}
    export ARM_ENDPOINT={your AzureStack Resource Manager Endpoint}
    ```

7.  Para executar este exemplo, Ubuntu 16.04-LTS e WindowsServer 2012 R2 Datacenter imagens tem de estar presentes no mercado do Azure Stack. Estes podem ser [transferido a partir do Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-download-azure-marketplace-item) ou [adicionado ao repositório de imagens de plataforma](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-vm-image).

8. Execute o exemplo.

    ```
    python unmanaged-disks\example.py
    ```

## <a name="notes"></a>Notas

Pode ficar tentado a tentar obter disco de SO de uma VM utilizando `virtual_machine.storage_profile.os_disk`.
Em alguns casos, isso pode fazer o que quiser, mas lembre-se de que ele fornece um `OSDisk` objeto.
Para atualizar o tamanho do disco SO, como `example.py` não, não precisa uma `OSDisk` objeto, mas um `Disk` objeto.
`example.py` obtém o `Disk` objeto com o seguinte:

```python
os_disk_name = virtual_machine.storage_profile.os_disk.name
os_disk = compute_client.disks.get(GROUP_NAME, os_disk_name)
```

## <a name="next-steps"></a>Passos Seguintes

- [Centro de desenvolvimento de Python do Azure](https://azure.microsoft.com/develop/python/)
- [Documentação de máquinas virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/)
- [Percurso de aprendizagem para máquinas virtuais](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/)
- Se não tiver uma subscrição do Microsoft Azure, pode obter uma conta de avaliação gratuita [aqui](https://go.microsoft.com/fwlink/?LinkId=330212).
