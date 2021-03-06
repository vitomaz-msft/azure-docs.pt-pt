---
title: Descrição geral do serviço de migração da base de dados do Azure | Documentos da Microsoft
description: Descrição geral do serviço de migração de base de dados do Azure, que fornece migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: ''
ms.reviewer: douglasl
ms.service: database-migration
ms.workload: data-services
ms.topic: article
ms.date: 10/19/2018
ms.openlocfilehash: 053e571b6285cd405ea17f43fec1d3ea99732070
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235586"
---
# <a name="what-is-the-azure-database-migration-service"></a>O que é o serviço de migração de base de dados do Azure?
O serviço de migração de base de dados do Azure é um serviço completamente gerido criado para permitir migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo (migrações online).

## <a name="migrate-databases-to-azure-with-familiar-tools"></a>Migrar bases de dados para o Azure com ferramentas familiares
O serviço de migração de base de dados do Azure integra-se algumas das funcionalidades das nossas ferramentas e serviços existentes. Fornece aos clientes com uma solução abrangente e de elevada disponibilidade. O serviço utiliza a [Assistente de migração de dados](https://aka.ms/dma) para gerar relatórios de avaliação que fornecem recomendações para o orientar as alterações necessárias antes de efetuar uma migração. Cabe-lhe para efetuar quaisquer atualizações necessárias. Quando estiver pronto para iniciar o processo de migração, o serviço de migração de base de dados do Azure executa todas as etapas necessárias. Pode disparar e esquecer seus projetos de migração com tranquilidade, sabendo que o processo tira partido das melhores práticas, conforme determinado pela Microsoft.

> [!NOTE]
> Utilizar o serviço de migração de base de dados do Azure para efetuar uma migração online requer a criação de uma instância com base no críticas para a empresa (pré-visualização) escalão de preço.

## <a name="regional-availability"></a>Disponibilidade regional
O serviço de migração de base de dados do Azure está atualmente disponível nas seguintes regiões:

![Disponibilidade regional do serviço de migração de base de dados do Azure](media\overview\dms-regional-availability1.png)

Para obter informações mais atualizadas sobre a disponibilidade regional do serviço de migração de base de dados do Azure, no site da infraestrutura global do Azure, consulte [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/).

## <a name="next-steps"></a>Passos Seguintes
- [Criar uma instância do serviço de migração de base de dados do Azure com o portal do Azure](quickstart-create-data-migration-service-portal.md).
- [Migrar o SQL Server para a base de dados SQL do Azure](tutorial-sql-server-to-azure-sql.md).
- [Descrição geral da pré-requisitos para utilizar o Azure Database Migration Service](pre-reqs.md).
- [FAQ sobre como utilizar o Azure Database Migration Service](faq.md).
