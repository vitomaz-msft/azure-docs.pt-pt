---
title: Opções para dados de transferência para o Azure com uma aplicação | Documentos da Microsoft
description: Saiba como escolher a aplicação da direita para transferir dados para o Azure
services: storsimple
author: alkohli
ms.service: storsimple
ms.topic: article
ms.date: 11/12/2018
ms.author: alkohli
ms.openlocfilehash: c28eaf22d05bfda5085f9e269bda85ca0d46a7d3
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51578132"
---
# <a name="compare-storsimple-with-azure-file-sync-and-data-box-edge-data-transfer-options"></a>Comparar o StorSimple com opções de transferência de dados do Azure File Sync e o limite de caixa de dados 
 
Este documento fornece uma descrição geral das opções para transferência de dados no local para o Azure, comparando: Edge de caixa de dados vs. Azure File Sync vs. Série StorSimple 8000.

- **[Dados caixa Edge](/azure/databox-online/data-box-edge-overview.md)**  – Edge de caixa de dados é um dispositivo de rede no local move os dados para dentro e fora do Azure e com a computação de extremidade habilitados para IA para processar previamente os dados durante o carregamento. Ele foi anunciado na Ignite 2018 e está em pré-visualização pública. Gateway de caixa de dados é uma versão virtual do dispositivo com as mesmas capacidades de transferência de dados.
- **[O Azure File Sync](/azure/storage/files/storage-sync-files-deployment-guide.md)**  – Azure File Sync pode ser usado para centralizar as partilhas de ficheiros da sua organização nos ficheiros do Azure, mantendo a flexibilidade, desempenho e compatibilidade de um servidor de ficheiros no local. O Azure File Sync transforma o Windows Server numa cache rápida da sua partilha de ficheiros do Azure. Disponibilidade geral do Azure File Sync foi anunciada anteriormente em 2018.
- **[StorSimple](/azure/storsimple/storsimple-overview.md)**  – StorSimple é um dispositivo de híbrida que ajuda as empresas a consolidar sua infra-estrutura de armazenamento para armazenamento primário, proteção de dados, arquivamento e recuperação de desastres numa única solução integrando rigidamente Armazenamento do Azure. O ciclo de vida do produto para o StorSimple pode ser encontrado [aqui](https://support.microsoft.com/lifecycle/search?alpha=Azure%20StorSimple%208000%20Series).

## <a name="comparison-summary"></a>Resumida de comparação

|                           |StorSimple 8000   |Azure File Sync   |Edge de caixa de dados (pré-visualização)           |
|---------------------------|----------------------------------------|-------------------------------|-----------------------------------------|
|Descrição geral         |Camadas de armazenamento híbrido e o arquivo|Armazenamento de servidor de ficheiros gerais com disposição em camadas e vários sites da sincronização da cloud.  |Solução de armazenamento para processar previamente os dados e enviar por meio de rede para o Azure.        |
|Cenários        |Servidor de ficheiros de destino de arquivo, cópia de segurança |Servidor de ficheiros, arquivamento (vários sites)   |Transferência de dados, dados pré-processamento incluindo ML inferência, IoT, arquivamento    |
|Computação Edge     |Não disponível |Não disponível |Suporta a execução de contentores com o Azure IoT Edge    |
|Fator de formulário      |Dispositivo físico   |Agente instalado no Windows Server |Dispositivo físico   |
|Hardware         |Dispositivo físico fornecido pela Microsoft como parte do serviço | Pelo cliente |Dispositivo físico fornecido pela Microsoft como parte do serviço  |
|Formato de dados      |Formato personalizado   |Ficheiros         |BLOBs ou ficheiros    |
|Suporte de protocolo |iSCSI          |SMB, NFS    | SMB ou NFS      |
|Preços          |[StorSimple](https://azure.microsoft.com/pricing/details/storsimple/) |[Sincronização de ficheiros do Azure](https://azure.microsoft.com/pricing/details/storage/files/)  |[Edge de caixa de dados](https://azure.microsoft.com/pricing/details/storage/databox/edge/)  |

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [do Azure Data Box Edge](/azure/databox-online/data-box-edge-overview) e [do Azure Data Box Gateway](/azure/databox-online/data-box-gateway-overview)
- Saiba mais sobre [sincronização de ficheiros do Azure](/azure/storage/files/storage-sync-files-deployment-guide)
