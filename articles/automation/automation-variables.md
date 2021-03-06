---
title: Recursos de variável na automatização do Azure
description: Recursos de variável são valores que estão disponíveis para todos os runbooks e configurações de DSC na automatização do Azure.  Este artigo explica os detalhes de variáveis e como trabalhar com os mesmos no texto e gráficos de criação.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ea6aae349bfbec0d1b6538010df42e7a0fb22d8e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="variable-assets-in-azure-automation"></a>Recursos de variável na automatização do Azure

Recursos de variável são valores que estão disponíveis para todos os runbooks e configurações de DSC na sua conta de automatização. Podem ser criados, modificados e obtidas a partir do portal do Azure, o Windows PowerShell e a partir do runbook ou configuração de DSC. As variáveis da automatização são úteis para os seguintes cenários:

- Partilhe um valor entre vários runbooks ou configurações de DSC.

- Partilhe um valor entre várias tarefas do mesmo runbook ou de configuração de DSC.

- Gerir um valor a partir do portal ou da linha de comandos do Windows PowerShell utilizada pelos runbooks ou configurações de DSC, tais como um conjunto comum de itens de configuração como lista específica de nomes VM, um grupo de recurso específico, um nome de domínio do AD, etc.  

As variáveis da automatização são mantidas, de modo a que continuam a estar disponíveis mesmo se a configuração de DSC ou o runbook falha. Isto também permite que um valor seja definido por um runbook e, em seguida, utilizado por outro ou é utilizado pelo mesmo runbook ou a configuração de DSC da próxima vez que for executada.

Quando uma variável é criada, pode especificar que é armazenado encriptado. Quando uma variável é encriptada, é armazenado em segurança na automatização do Azure e o valor não é possível obter a partir de [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable) cmdlet que é incluído como parte do módulo Azure PowerShell. É a única forma que um valor encriptado pode ser obtido a partir de **Get-AutomationVariable** atividade num runbook ou configuração de DSC.

>[!NOTE]
>Proteger recursos na automatização do Azure incluem as credenciais, certificados, ligações e as variáveis encriptadas. Estes elementos são encriptados e armazenados na automatização do Azure com uma chave exclusiva que é gerada para cada conta de automatização. Esta chave é armazenada no Cofre de chaves. Antes de o armazenamento de um recurso seguro, a chave é carregada a partir do Cofre de chaves e, em seguida, utilizada para encriptar o elemento.

## <a name="variable-types"></a>Tipos de variável

Quando criar uma variável com o portal do Azure, tem de especificar um tipo de dados na lista pendente para que o portal, pode apresentar o controlo apropriado para introduzir o valor da variável. A variável não está limitada a este tipo de dados, mas tem de definir a variável com o Windows PowerShell se pretender especificar um valor de um tipo diferente. Se especificar **não definido**, o valor da variável é definida com **$null**, e tem de definir o valor com o [conjunto AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable) cmdlet ou **Set-AutomationVariable** atividade. Não é possível criar ou alterar o valor para um tipo complexo variável no portal, mas pode fornecer um valor de qualquer tipo com o Windows PowerShell. Tipos complexos são devolvidos como um [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Pode armazenar vários valores para uma única variável através da criação de uma matriz ou uma tabela hash e guardá-lo para a variável.

Seguem-se uma lista de variáveis tipos disponíveis na automatização:

* Cadeia
* Número inteiro
* DateTime
* Booleano
* Null

## <a name="azurerm-powershell-cmdlets"></a>Cmdlets do AzureRM PowerShell
Para AzureRM, os cmdlets na tabela seguinte são utilizados para criar e gerir recursos de credencial de automatização com o Windows PowerShell. Estes são enviados como parte do [AzureRM.Automation módulo](/powershell/azure/overview) que está disponível para utilização nos runbooks de automatização e configurações de DSC.

| Cmdlets | Descrição |
|:---|:---|
|[Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable)|Obtém o valor de uma variável existente.|
|[New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable)|Cria uma nova variável e define o valor.|
|[Remove-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationVariable)|Remove uma variável existente.|
|[Set-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable)|Define o valor de uma variável existente.|

## <a name="activities"></a>Atividades
As atividades na tabela seguinte são utilizadas para aceder a credenciais num runbook e configurações de DSC.

| Atividades | Descrição |
|:---|:---|
|Get-AutomationVariable|Obtém o valor de uma variável existente.|
|Set-AutomationVariable|Define o valor de uma variável existente.|

> [!NOTE] 
> Deve evitar utilizar variáveis no parâmetro – Name de **Get-AutomationVariable** num runbook ou configuração de DSC, uma vez que isso pode dificultar a deteção de dependências entre runbooks ou configuração de DSC e as variáveis da automatização no momento da concepção.

As funções na tabela seguinte são utilizadas para aceder e obter variáveis num Python2 runbook. 

|Funções de Python2|Descrição|
|:---|:---|
|automationassets.get_automation_variable|Obtém o valor de uma variável existente. |
|automationassets.set_automation_variable|Define o valor de uma variável existente. |

> [!NOTE] 
> Tem de importar o módulo "automationassets" na parte superior do Python runbook para poder aceder as funções de recurso.

## <a name="creating-a-new-automation-variable"></a>Criar uma nova variável de automatização

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Para criar uma nova variável com o portal do Azure

1. Da sua conta de automatização, clique em de **ativos** mosaico e, em seguida, no **ativos** painel, selecione **variáveis**.
2. No **variáveis** mosaico, selecione **adicionar uma variável**.
3. Preencha as opções no **nova variável** painel e clique em **criar** guardar a variável nova.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Para criar uma nova variável com o Windows PowerShell

O [New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet cria uma nova variável e define o valor inicial. Pode obter o valor a utilizar [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable). Se o valor é um tipo simples, em seguida, é devolvido esse mesmo tipo. Se se tratar de um tipo complexo, em seguida, um **PSCustomObject** é devolvido.

Os comandos de exemplo seguintes mostram como criar uma variável de cadeia de tipo e, em seguida, devolver o valor.

    New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

Os comandos de exemplo seguintes mostram como criar uma variável com um tipo complexo e, em seguida, devolvem as respetivas propriedades. Neste caso, o objeto de uma máquina virtual, do **Get-AzureRmVm** é utilizado.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Utilizar uma variável num runbook ou configuração DSC

Utilize o **Set-AutomationVariable** atividade para definir o valor de uma variável de automatização de um runbook do PowerShell ou a configuração de DSC e **Get-AutomationVariable** para obtê-lo. Não deve utilizar o **conjunto AzureRMAutomationVariable** ou **Get-AzureRMAutomationVariable** cmdlets no runbook ou configuração de DSC uma vez que são menos eficientes do que as atividades de fluxo de trabalho. Também não é possível obter o valor das variáveis seguras com **Get-AzureRMAutomationVariable**. A única forma de criar uma nova variável de dentro de um runbook ou a configuração DSC consiste em utilizar o [New-AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet.


### <a name="textual-runbook-samples"></a>Exemplos de textual runbook

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Definir e obter um valor simple de uma variável

Os comandos de exemplo seguintes mostram como definir e obter uma variável num textual runbook. Neste exemplo, presume-se que as variáveis do tipo número inteiro denominado *NumberOfIterations* e *NumberOfRunnings* e uma variável de cadeia de tipo com o nome *SampleMessage* já foram criadas.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Definir e obter um objeto complexo numa variável

O código de exemplo seguinte mostra como atualizar uma variável com um valor complexo num textual runbook. Neste exemplo, uma máquina virtual do Azure é obtida com **Get-AzureVM** e guardado para uma variável de automatização existente.  Conforme explicado no [tipos variável](#variable-types), este é armazenado como uma PSCustomObject.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

No seguinte código, o valor é obtido a variável e utilizado para iniciar a máquina virtual.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Definir e obter uma coleção numa variável

O código de exemplo seguinte mostra como utilizar uma variável com uma coleção de valores complexos num textual runbook. Neste exemplo, são obtidas a várias máquinas virtuais do Azure com **Get-AzureVM** e guardado para uma variável de automatização existente. Conforme explicado no [tipos variável](#variable-types), este é armazenado como uma coleção de PSCustomObjects.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

No código seguinte, a coleção é obtida a variável e utilizada para iniciar a cada máquina virtual.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }
    
#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Definir e obter uma variável num Python2
O código de exemplo seguinte mostra como utilizar uma variável, definir uma variável e processar uma exceção para a variável num Python2 runbook inexistente.

    import automationassets
    from automationassets import AutomationAssetNotFound

    # get a variable
    value = automationassets.get_automation_variable("test-variable")
    print value

    # set a variable (value can be int/bool/string)
    automationassets.set_automation_variable("test-variable", True)
    automationassets.set_automation_variable("test-variable", 4)
    automationassets.set_automation_variable("test-variable", "test-string")

    # handle a non-existent variable exception
    try:
        value = automationassets.get_automation_variable("non-existing variable")
    except AutomationAssetNotFound:
        print "variable not found"


### <a name="graphical-runbook-samples"></a>Exemplos de runbook gráfico

Um runbook gráfico, adicione o **Get-AutomationVariable** ou **Set-AutomationVariable** ao clicar no variável no painel de biblioteca do editor gráfico e selecionar a atividade que pretende.

![Adicione a variável à tela](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Valores de definição numa variável
A imagem seguinte mostra as atividades de exemplo para atualizar uma variável com um valor de um runbook gráfico simple. Neste exemplo, uma única máquina virtual do Azure é obtida com **Get-AzureRmVM** e o nome do computador é guardado para uma variável de automatização existente com um tipo de cadeia. É irrelevante se o [ligação é um pipeline ou sequência](automation-graphical-authoring-intro.md#links-and-workflow) , uma vez que espera apenas um único objeto no resultado.

![Definir variável simple](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Próximos Passos

* Para saber mais sobre a ligação de atividades em conjunto na criação de gráficos, consulte [nas hiperligações na criação de gráficos](automation-graphical-authoring-intro.md#links-and-workflow)
* Para começar com runbooks Gráficos, consulte o artigo [O meu primeiro runbook gráfico](automation-first-runbook-graphical.md) 
