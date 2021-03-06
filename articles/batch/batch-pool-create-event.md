---
title: Evento de criação de conjunto do Batch do Azure | Documentos da Microsoft
description: Evento de criação de referência para o conjunto do Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: f8c0adf96d027f58a35dbe570f1b19c311cd84b9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246519"
---
# <a name="pool-create-event"></a>Evento de criação de conjunto

 Este evento é emitido quando um conjunto tiver sido criado. O conteúdo do registo irá expor informações gerais sobre o conjunto. Tenha em atenção que, se o tamanho de destino do pool for maior que 0 nós de computação, um evento de início de redimensionamento de conjunto irá seguir imediatamente após este evento.

 O exemplo seguinte mostra o corpo de um agrupamento de eventos para um conjunto criado usando a propriedade CloudServiceConfiguration de criar.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Elemento|Tipo|Notas|
|-------------|----------|-----------|
|ID|Cadeia|O id do conjunto.|
|displayName|Cadeia|O nome a apresentar do conjunto.|
|vmSize|Cadeia|O tamanho das máquinas virtuais no conjunto. Todas as máquinas virtuais num conjunto têm o mesmo tamanho. <br/><br/> Para informações sobre os tamanhos disponíveis das máquinas virtuais para serviços em nuvem pools (conjuntos criados com cloudServiceConfiguration), consulte [tamanhos para os serviços Cloud](https://azure.microsoft.com/documentation/articles/cloud-services-sizes-specs/). O batch suporta todos os tamanhos de VM dos serviços Cloud, exceto `ExtraSmall`.<br/><br/> Para obter informações sobre a VM disponível tamanhos de conjuntos utilizando imagens do mercado de máquinas virtuais (conjuntos criados com virtualMachineConfiguration) veja [tamanhos de máquinas virtuais](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-sizes/) (Linux) ou [tamanhos para Virtual Máquinas](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) (Windows). O Batch suporta todos os tamanhos de VM do Azure, exceto `STANDARD_A0` e os do armazenamento premium (séries `STANDARD_GS`, `STANDARD_DS` e `STANDARD_DSV2`).|
|[cloudServiceConfiguration](#bk_csconf)|Tipo complexo|A configuração do serviço cloud para o conjunto.|
|[virtualMachineConfiguration](#bk_vmconf)|Tipo complexo|A configuração de máquina virtual para o conjunto.|
|[networkConfiguration](#bk_netconf)|Tipo complexo|A configuração de rede para o conjunto.|
|resizeTimeout|Hora|O tempo limite da alocação de nós de computação para o conjunto especificado para a última operação de redimensionamento no conjunto.  (O tamanho inicial quando é criado o conjunto é contabilizado como um redimensionamento.)|
|targetDedicated|Int32|O número de nós de computação que são pedidos para o conjunto.|
|enableAutoScale|Bool|Especifica se o tamanho do conjunto se ajusta automaticamente ao longo do tempo.|
|enableInterNodeCommunication|Bool|Especifica se o conjunto está configurado para comunicação direta entre nós.|
|isAutoPool|Bool|Speficies se o conjunto foi criado por meio de mecanismo de AutoPool de uma tarefa.|
|maxTasksPerNode|Int32|O número máximo de tarefas que pode ser executado simultaneamente num único nó de computação no conjunto.|
|vmFillType|Cadeia|Define como o serviço Batch distribui as tarefes entre nós de computação no conjunto. Valores válidos encontram-se distribuídas ou pacote.|

###  <a name="bk_csconf"></a> cloudServiceConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|osFamily|Cadeia|A família de SO convidado do Azure para ser instalado nas máquinas virtuais no conjunto.<br /><br /> Os valores possíveis são:<br /><br /> **2** – 2 de família de SO, equivalente ao Windows Server 2008 R2 SP1.<br /><br /> **3** – 3 de família de SO, equivalente para o Windows Server 2012.<br /><br /> **4** – 4 de família de SO, equivalente para o Windows Server 2012 R2.<br /><br /> Para obter mais informações, consulte [versões do SO convidado do Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|
|targetOSVersion|Cadeia|A versão de SO convidado do Azure para ser instalado nas máquinas virtuais no conjunto.<br /><br /> O valor predefinido é **\*** que especifica a versão mais recente do sistema operativo para a família especificada.<br /><br /> Para outros valores permitidos, consulte [versões do SO convidado do Azure](https://azure.microsoft.com/documentation/articles/cloud-services-guestos-update-matrix/#releases).|

###  <a name="bk_vmconf"></a> virtualMachineConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|[imageReference](#bk_imgref)|Tipo complexo|Especifica informações sobre a plataforma ou imagem do Marketplace para utilizar.|
|nodeAgentSKUId|Cadeia|O SKU do agente de nó do Batch aprovisionado no nó de computação.|
|[windowsConfiguration](#bk_winconf)|Tipo complexo|Especifica as definições do sistema operativo Windows na máquina virtual. Esta propriedade não pode ser especificado se o imageReference está a referenciar uma imagem de SO Linux.|

###  <a name="bk_imgref"></a> imageReference

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|publicador|Cadeia|O publicador da imagem.|
|oferta|Cadeia|A oferta da imagem.|
|sku|Cadeia|O SKU da imagem.|
|versão|Cadeia|A versão da imagem.|

###  <a name="bk_winconf"></a> windowsConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|----------|-----------|
|enableAutomaticUpdates|Booleano|Indica se a máquina virtual está ativada para as atualizações automáticas. Se esta propriedade não for especificada, o valor predefinido é verdadeiro.|

###  <a name="bk_netconf"></a> networkConfiguration

|Nome do elemento|Tipo|Notas|
|------------------|--------------|----------|
|subnetId|Cadeia|Especifica o identificador de recurso da sub-rede na qual nós de computação do conjunto são criados.|
