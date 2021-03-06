---
title: Dados de utilização do Azure Cloud Shell para LUIS
titleSuffix: Azure Cognitive Services
description: Saiba como obter as informações de utilização no Azure Cloud Shell LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2017
ms.author: diberry
ms.openlocfilehash: 42bd8ef95ac1b55e76fe1cf0702b79de91832741
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870184"
---
# <a name="usage-data-for-luis-service-from-azure-cloud-shell"></a>Dados de utilização para o serviço de LUIS do Azure Cloud Shell
O portal do Azure permite-lhe utilizar cmdlets do PowerShell para trabalhar com recursos de LUIS. 

Estes cmdlets permitem-lhe [crie](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) uma assinatura do LUIS, obtenha informações sobre a subscrição, incluindo [utilização](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), e [remover](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) a subscrição. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Conta de armazenamento na cloud shell e autenticação
Para poder utilizar o PowerShell no portal do Azure [cloud shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell), tem de ter uma conta de armazenamento do Azure. Se não tiver uma [conta de armazenamento](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage), será solicitado a criar uma. A conta de armazenamento permite-lhe guardar os scripts do PowerShell no cloud shell.  

Também tem de autenticar para o Azure no cloud shell para aceder a quaisquer recursos. 

Depois de ter uma conta de armazenamento e são autenticados, pode executar cmdlets do PowerShell.

## <a name="open-cloud-shell"></a>Abrir o Cloud Shell
Quando utiliza o Azure portal cloud shell, são sempre a versão mais recente do PowerShell. 

Utilize o **Launch Cloud Shell** botão para abrir o Cloud Shell ou abra um browser com [ https://shell.azure.com ](https://shell.azure.com). 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" /></a>

## <a name="luis-endpoint-usage-information"></a>Informações de utilização do ponto final de LUIS

O cmdlet PowerShell do 6.x, `Get-AzureRmCognitiveServicesAccountUsage`, fornece informações de utilização para os serviços cognitivos da Microsoft incluindo LUIS. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) requer o grupo de recursos e o nome de recurso do serviço. 

A sintaxe de comando é:

```
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

No exemplo a seguir, é o nome do grupo de recursos `luis-westus-rg` e é o nome de subscrição do serviço de LUIS `luis-westus-1`. Ambos os esses nomes são escolhidos quando o serviço de LUIS é criado. 

O cmdlet retorna informações de utilização de 16 de 10 000 resultados de ponto final utilizados num período de 30 dias com o período que termina em 7 de Junho:

```
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Guarde o comando como um ficheiro de PowerShell, *. ps1, na conta de armazenamento do Azure associada com o cloud shell e execute em qualquer altura. 

![Execute o script a partir do armazenamento](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Assim que o script é guardado na unidade do cloud, pode executar o script do PowerShell do Azure cloudshell da aplicação de telefone. 

![Execute o script a partir do armazenamento numa aplicação para telemóvel](./media/luis-how-to-manage-from-powershell/phone-app.png)