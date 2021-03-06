---
title: Distribuir dados globalmente com o Azure Cosmos DB | Documentos da Microsoft
description: Mais informações sobre recuperação de dados, ativação pós-falha, com vários mestres e georreplicação de escala planetária com bases de dados global do Azure Cosmos DB, um serviço de base de dados com múltiplos modelos distribuída globalmente.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: b9a5658c91a44289442f48993118e996bf2691c2
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52164052"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Distribuição de dados global com o Azure Cosmos DB

Aplicativos de hoje exigem para ser elevada capacidade de resposta e sempre online. Para alcançar a baixa latência e elevada disponibilidade, instâncias desses aplicativos precisam ser implantados nos centros de dados que estão próximas dos seus usuários. Esses aplicativos são normalmente implementados em vários datacenters e são chamados e distribuídos globalmente. Aplicações distribuídas globalmente tem uma base de dados distribuído globalmente que transparente pode replicar os dados em qualquer parte do mundo para permitir que os aplicativos operar numa cópia dos dados que está próximo de seus usuários. O Azure Cosmos DB é um serviço de base de dados globalmente distribuída que foi concebido para fornecer a baixa latência, escalabilidade elástica de débito, semântica bem definida para consistência dos dados e elevada disponibilidade. Em resumo, se as necessidades da sua aplicação a garantia de tempo de resposta rápidos em qualquer lugar do mundo, se ele requer que seja sempre online e tem de escalabilidade ilimitada e elástica de débito e armazenamento, deve considerar a criação de aplicativos com o Azure Cosmos DB.

Azure Cosmos DB é um serviço do Azure fundamental e está disponível em todos os [regiões do Azure](https://azure.microsoft.com/global-infrastructure/regions/) por predefinição. A Microsoft centros de dados do Azure em 54 + regiões em todo o mundo e continua a expandir a presença de regional para satisfazer as necessidades de cada vez mais dos clientes. Quando cria uma conta do Cosmos do Azure, decidir quais regiões devem ser implementada no. A Microsoft opera o Azure Cosmos DB 24 x 7, do serviço para que se possa focar nas suas aplicações.

Pode configurar seus bancos de dados para ser globalmente distribuídas e disponível em qualquer uma das regiões do Azure. Para reduzir a latência, deve colocar os dados mais próximos para onde estão os utilizadores. Escolher as regiões necessárias depende o alcance global da sua aplicação e onde estão localizados os seus utilizadores. O Azure Cosmos DB replica de forma transparente os dados na sua conta para todas as regiões à sua conta para todas as regiões à sua conta. Ele fornece uma única imagem do sistema dos seus distribuídos globalmente de forma a base de dados do Cosmos do Azure e contentores que seu aplicativo pode ler e escrever localmente. Com o Azure Cosmos DB, pode adicionar ou remover regiões associadas à sua conta em qualquer altura. Seu aplicativo não precisa de ser colocada em pausa ou implantados novamente para adicionar ou remover uma região. Continua a ser de elevada disponibilidade sempre devido as capacidades multi-homing, que fornece o serviço.

## <a name="key-benefits-of-global-distribution"></a>Principais benefícios da distribuição global

**Criar aplicações globais de ativo-ativo**: com a capacidade de vários mestre, cada região é uma região de escrita (além de ser legível). Também de vários mestres recursos garantias - escrita elástico ilimitado de escalabilidade, o 99,999% ler e escrever disponibilidade tudo em todo o mundo e garantidas leituras/escritas são servidos em menos de 10 milissegundos em 99 por cento dos.  

Com o Azure Cosmos DB para as APIs multi-homing, a sua aplicação está atento a região mais próxima e pode enviar pedidos para essa região. Região mais próxima é identificada sem quaisquer alterações de configuração. Como adicionar e remover regiões da sua conta do Azure Cosmos DB, seu aplicativo não precisa de ser novamente implementada e continua a ser de elevada disponibilidade.

**Crie aplicações altamente responsivos**: seu aplicativo pode ser facilmente desenvolvido para executar quase em tempo real leituras e escritas, com latências de milissegundo de dígito contra todas as regiões que escolheu para a base de dados.  O Azure Cosmos DB processa internamente a replicação de dados entre regiões, de modo a que o nível de consistência escolhido para a conta do Cosmos do Azure é garantido.

Muitos aplicativos irão se beneficiar as melhorias de desempenho que vêm com a capacidade de executar gravações (locais) de várias regiões. Alguns aplicativos que exigem uma consistência forte preferem todas as gravações de funil de uma única região. Para suportar estas aplicações, o Azure Cosmos DB suporta a região única e configurações de várias regiões.

**Crie aplicações altamente disponíveis**: executar uma base de dados em várias regiões aumenta a disponibilidade da base de dados. Se uma região não estiver disponível, outras regiões manipula automaticamente os pedidos de aplicações. O Azure Cosmos DB oferece 99,999% de leitura e escrita de disponibilidade para bases de dados de várias regiões.

**Continuidade do negócio durante falhas regionais**: Azure Cosmos DB suporta [a ativação pós-falha automática](how-to-manage-database-account.md#automatic-failover) durante uma falha regional. Além disso, durante uma falha regional, o Azure Cosmos DB continua a manter a latência, disponibilidade, consistência e SLAs de débito. Para ajudar a garantir que todo o seu aplicativo está altamente disponível, o Azure Cosmos DB oferece API de ativação pós-falha manual para simular uma falha regional. Ao utilizar esta API, pode efetuar as explorações de continuidade comercial normal.

**Global de leitura e escrita escalabilidade**: com capacidade de vários mestre, pode dimensionar leitura e escrita débito elástica em todo o mundo. Capacidade de vários mestre garante que o débito que configura o seu aplicativo num banco de dados do Azure Cosmos DB ou um contentor é entregue em todas as regiões e protegido por [apoiado financeiramente SLAs](https://aka.ms/acdbsla).

**Vários modelos de consistência bem definidos**: protocolo de replicação do Azure Cosmos DB foi criado para oferecer cinco modelos de consistência bem definidos, práticos e intuitivos. Cada modelo de consistência tem um compromisso entre a consistência e desempenho. Estes modelos de consistência permitem-lhe criar aplicações distribuídas globalmente com facilidade.

## <a id="Next Steps"></a>Passos seguintes

Leia mais sobre a distribuição global nos seguintes artigos:

* [Distribuição global - sob definições avançadas](global-dist-under-the-hood.md)
* [Como configurar clientes para multi-homing](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [Como adicionar ou remover regiões da sua conta do Cosmos do Azure](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Como criar uma política de resolução de conflito personalizado para contas da API de SQL](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)