---
title: Base de dados SQL do Azure gerido no DNS do instância personalizado | Documentos da Microsoft
description: Este tópico descreve opções de configuração para um DNS personalizado com uma instância de gerida de base de dados do Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/23/2018
ms.openlocfilehash: f26ea763d48d03fe7e981b7abbbe64e573ec0b3a
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224278"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Configurar um DNS personalizado para a base de dados SQL do Azure a instância gerida

Uma instância de gerida de base de dados do Azure SQL deve ser implementada numa do Azure [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md). Existem alguns cenários (ou seja, servidores vinculados para as outras instâncias SQL no seu ambiente na nuvem ou híbrida), que requerem nomes de anfitrião privada para ser resolvido a partir da instância gerida. Neste caso, terá de configurar um DNS personalizado no Azure. Uma vez que a instância gerida utiliza o DNS do mesmo para seus funcionamentos, a configuração de DNS de rede virtual tem de ser compatível com a instância gerida. 

Para fazer uma configuração DNS personalizada é compatível com a instância gerida, terá de: 
- Configurar o servidor DNS personalizado para que seja capaz de resolver os nomes de domínio público 
- Colocar o endereço de IP de DNS de resolução de recursiva do Azure 168.63.129.16 no final da lista DNS de rede virtual 
 
## <a name="setting-up-custom-dns-servers-configuration"></a>Configurar a configuração de servidores DNS personalizada

1. No portal do Azure, encontre opções de DNS personalizados para a sua VNet.

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-option.png) 

2. Mude para personalizado e introduza o seu endereço IP do servidor DNS personalizado, bem como o endereço IP de resoluções do Azure recursiva 168.63.129.16. 

   ![opção de dns personalizado](./media/sql-database-managed-instance-custom-dns/custom-dns-server-ip-address.png) 

   > [!IMPORTANT]
   > Não definir o resolvedor recursivo do Azure na lista de DNS pode fazer com que a instância gerida entrar num Estado com falhas quando os servidores DNS personalizados não estão disponíveis por algum motivo. Recuperar a partir de que o estado pode exigir a criação de nova instância numa VNet com as políticas de rede em conformidade, criar dados de nível de instância e restaurar as bases de dados. Definir o resolvedor recursivo do Azure como a última entrada na lista de DNS garante que, mesmo quando todos os servidores DNS personalizados falharem, os nomes de públicos ainda podem ser resolvidos. Ver [configuração da VNet](sql-database-managed-instance-vnet-configuration.md).

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral, consulte [o que é uma instância gerida](sql-database-managed-instance.md)
- Para obter um tutorial que mostra como criar uma nova instância gerida, veja [criar uma instância gerida](sql-database-managed-instance-get-started.md).
- Para obter informações sobre como configurar uma VNet para uma instância gerida, consulte [configuração de VNet para instâncias geridas](sql-database-managed-instance-vnet-configuration.md)
