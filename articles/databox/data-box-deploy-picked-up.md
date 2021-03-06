---
title: Enviar o Microsoft Azure Data Box de volta | Microsoft Docs
description: Aprenda a enviar o Azure Data Box para a Microsoft
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/30/2018
ms.author: alkohli
ms.openlocfilehash: 42ed9091ff7ab8059ba253f62726b30899d6e697
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51036060"
---
# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutorial: devolver o Azure Data Box e verificar os dados carregados para o Azure

Este tutorial descreve como devolver o Azure Data Box e certificar-se de que os dados foram carregados para o Azure.

Neste tutorial, irá obter informações sobre tópicos, como:

> [!div class="checklist"]
> * Enviar o Data Box para a Microsoft
> * Verificar o carregamento de dados para o Azure
> * Eliminação de dados do Data Box

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que concluiu o [Tutorial: Copiar dados para o Azure Data Box e verificar](data-box-deploy-copy-data.md).

## <a name="ship-data-box-back"></a>Enviar o Data Box de volta

1. Certifique-se de que o dispositivo está desligado e os cabos são removidos. Coloque em segurança o cabo de alimentação enviado com o dispositivo atrás do mesmo.
2. Se o dispositivo for enviado nos EUA, confirme que a etiqueta de envio é apresentada no ecrã de tinta eletrónica e agende uma recolha com a sua operadora. Se a etiqueta estiver danificada ou perder-se, ou não estiver apresentada no ecrã de tinta eletrónica, transfira a nova etiqueta de envio do portal do Azure e coloque-a no dispositivo. Aceda a **Descrição Geral > Transferir etiqueta de envio**. 

    Se o dispositivo for enviado na Europa, o ecrã de tinta eletrónica não mostrará a etiqueta de envio. Em vez disso, a etiqueta de envio de devolução estará incluída na bolsa transparente sob a etiqueta de envio de encaminhamento. Remova a etiqueta de envio antiga e confirme que a etiqueta de envio está claramente visível.
    
3. Se estiver a devolver o dispositivo nos EUA, contacte a UPS para agendar uma recolha. Se estiver a devolver o dispositivo na Europa com a DHL, solicite uma recolha à DHL acedendo ao respetivo site e especificando o número de carta de porte aéreo. Aceda ao site da DHL Express do seu país e selecione **Book a Courier Collection (Agendar uma Recolha por Estafeta) > eReturn Shipment (Envio eReturn)**. 

    Especifique o número de carta de porte aéreo e clique em **Schedule Pickup (Agendar Recolha)** para marcar uma recolha.

4. Após a sua transportadora recolher e analisar o Data Box, o estado de encomenda no portal será atualizado para **Picked up (Recolhido)**. É também mostrado um ID de controlo.

## <a name="verify-data-upload-to-azure"></a>Verificar o carregamento de dados para o Azure

Quando a Microsoft receber e analisar o dispositivo, o estado da encomenda será atualizado para **Received (Recebido)**. O dispositivo, em seguida, passa pela verificação física para verificar os danos ou sinais de adulteração. 

Depois de a verificação estar concluída, o Data Box é ligado à rede no datacenter do Azure. A cópia de dados é iniciada automaticamente. Consoante o tamanho dos dados, a operação de cópia poderá demorar algumas horas a dias para concluir. Pode monitorizar o progresso da tarefa de cópia no portal.

Quando a cópia for concluída, o estado da encomenda será atualizado para **Completed (Concluído)**.

Certifique-se de que os dados estão na(s) conta(s) de armazenamento antes de eliminá-los da origem. Quando copia os dados para o Data Box, consoante o tipo, os dados são carregados para um dos seguintes caminhos na sua conta de Armazenamento do Azure.

- Para blobs de blocos e blobs de páginas: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
- Para Ficheiros do Azure: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

Em alternativa, pode aceder à sua conta de armazenamento do Azure no portal do Azure e navegar a partir daí.

## <a name="erasure-of-data-from-data-box"></a>Eliminação de dados do Data Box
 
Após o carregamento para o Azure, o Data Box elimina os dados dos discos de acordo com as [diretrizes SP 800-88 Revision 1 da NIST](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi). 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a conhecer tópicos do Azure Data Box, como:

> [!div class="checklist"]
> * Enviar o Data Box para a Microsoft
> * Verificar o carregamento de dados para o Azure
> * Eliminação de dados do Data Box

Avance para o artigo seguinte para saber como gerir o Data Box através da IU da Web local.

> [!div class="nextstepaction"]
> [Utilizar a IU da Web local para administrar um Azure Data Box](./data-box-local-web-ui-admin.md)


