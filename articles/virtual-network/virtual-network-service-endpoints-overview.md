---
title: Pontos finais do serviço de rede virtual do Azure | Microsoft Docs
description: Saiba como ativar o acesso direto aos recursos do Azure a partir de uma rede virtual através de pontos finais de serviço.
services: virtual-network
documentationcenter: na
author: sumeetmittal
manager: narayan
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: sumeet.mittal
ms.custom: ''
ms.openlocfilehash: 77fad7b0035a9ba21d71e6c493a4f1a5bd9a2111
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49395212"
---
# <a name="virtual-network-service-endpoints"></a>Pontos Finais de Serviço de Rede Virtual

Os pontos finais de serviço da Rede Virtual (VNet) expandem o seu espaço de endereços privados de rede virtual e da identidade da sua VNet para os serviços do Azure através de uma ligação direta. Os pontos finais permitem-lhe obter os seus recursos críticos de serviço do Azure apenas para as suas redes virtuais. O tráfego da sua VNet para o serviço do Azure permanece sempre na rede backbone do Microsoft Azure.

Esta funcionalidade está disponível para os seguintes serviços e regiões do Azure:

**Disponível em Geral**

- **[Armazenamento do Azure](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)**: Disponibilidade geral em todas as regiões do Azure.
- **[Bases de dados SQL do Azure](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponibilidade geral em todas as regiões do Azure.
- **[Base de Dados do Azure para servidor PostgreSQL](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponível em geral em regiões do Azure onde o serviço de base de dados está disponível.
- **[Base de Dados do Azure para servidor MySQL](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponível em geral em regiões do Azure onde o serviço de base de dados está disponível.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponibilidade Geral em todas as regiões da cloud pública do Azure.
- **[Azure Key Vault](https://blogs.technet.microsoft.com/kv/2018/06/25/announcing-virtual-network-service-endpoints-for-key-vault-preview/)**: disponibilidade geral em todas as regiões da cloud pública do Azure.

**Pré-visualização**

- **[Azure SQL Data Warehouse](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponível em pré-visualização em todas as regiões da cloud pública do Azure.
- **[Azure Service Bus](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponível em pré-visualização.
- **[Hubs de Eventos do Azure](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: Disponíveis em pré-visualização.
- **[Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)**: disponível em pré-visualização.

Para obter as notificações mais atualizadas, veja a página [Atualizações da Rede Virtual do Azure](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="key-benefits"></a>Principais vantagens

Os pontos finais de serviço oferecem as seguintes vantagens:

- **Segurança melhorada para os seus recursos de serviço do Azure**: o espaço de endereços privados da VNet pode estar sobreposto e, por isso, não pode ser utilizado para identificar de forma exclusiva o tráfego com origem na sua VNet. Os pontos finais de serviço permitem proteger os recursos de serviço do Azure para a sua rede virtual, ao expandir a identidade da VNet para o serviço. Depois de ativar os pontos finais de serviço na rede virtual, pode proteger os recursos de serviço do Azure para a rede virtual ao adicionar uma regra de rede virtual aos recursos. Beneficia assim de segurança melhorada ao remover totalmente o acesso de Internet público aos recursos e ao permitir o tráfego apenas a partir da sua rede virtual.
- **Encaminhamento ideal para o tráfego do serviço do Azure a partir da rede virtual**: atualmente, quaisquer rotas na rede virtual que imponham o tráfego de Internet através de aplicações no local e/ou virtuais, operação designada como imposição de túnel, também forçam o tráfego de serviço do Azure a seguir a mesma rota que o tráfego de Internet. Os pontos finais de serviço fornecem encaminhamento ótimo para o tráfego do Azure. 

  Os pontos finais assumem sempre o tráfego de serviço diretamente a partir da rede virtual para o serviço na rede backbone do Microsoft Azure. Manter o tráfego na rede backbone do Azure permite-lhe continuar a auditar e a monitorizar o tráfego de Internet de saída das suas redes virtuais através da imposição de túnel e sem afetar o tráfego de serviço. Saiba mais sobre as [rotas definidas pelo utilizador e a imposição de túnel](virtual-networks-udr-overview.md).
- **Fácil de configurar com menos overhead de gestão**: já não necessita de endereços IP reservados e públicos nas redes virtuais para proteger os recursos do Azure através da firewall do IP. Não são necessários dispositivos NAT ou de gateway para configurar os pontos finais de serviço. Basta um clique numa sub-rede para configurar os pontos finais de serviço. Não há qualquer overhead adicional para a manutenção dos pontos finais.

## <a name="limitations"></a>Limitações

- A funcionalidade só está disponível para redes virtuais implementadas através do modelo de implementação Azure Resource Manager.
- Os pontos finais estão ativados nas sub-redes configuradas em redes virtuais do Azure. Os pontos finais não podem ser utilizados para tráfego dos serviços no local para os serviços do Azure. Para obter mais informações, veja [Proteger o acesso do serviço do Azure no local](#securing-azure-services-to-virtual-networks)
- No Azure SQL, um ponto final de serviço aplica-se apenas ao tráfego de serviço do Azure numa região da rede virtual. No Armazenamento do Azure, para suportar tráfego RA-GRS e GRS, os pontos finais também se expandem para incluir regiões emparelhadas nas quais a rede virtual está implementada. Saiba mais sobre as [regiões emparelhadas do Azure](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).

## <a name="securing-azure-services-to-virtual-networks"></a>Proteger serviços do Azure para redes virtuais

- Um ponto final de serviço de rede virtual transmite a identidade da sua rede virtual ao serviço do Azure. Depois de ativar os pontos finais de serviço na rede virtual, pode proteger os recursos de serviço do Azure para a rede virtual ao adicionar uma regra de rede virtual aos recursos.
- Atualmente, o tráfego de serviço do Azure a partir de uma rede virtual utiliza endereços IP públicos como endereços IP de origem. Com pontos finais de serviço, o tráfego de serviço comuta-se para utilizar endereços privados da rede virtual como endereços IP de origem quando acede ao serviço do Azure a partir de uma rede virtual. Esta comutação permite-lhe aceder aos serviços sem necessitar de endereços IP reservados públicos utilizados nas firewalls IP.

>[!NOTE]
> Com pontos finais de serviço, os endereços IP de origem das máquinas virtuais na sub-rede para tráfego de serviço mudam da utilização de endereços IPv4 públicos para a utilizam de endereços IPv4 privados. As regras de firewall de serviço do Azure existentes com os endereços IP públicos do Azure deixarão de funcionar com esta troca. Certifique-se de que as regras de firewall do serviço do Azure permitem esta troca antes de configurar os pontos finais de serviço. Também poderá experienciar uma interrupção temporária do tráfego de serviço desta sub-rede ao configurar pontos finais de serviço. 
 
- __Obter acesso a serviços do Azure no local__:

  Por predefinição, os recursos de serviço do Azure obtidos para redes virtuais não são acessíveis a partir de redes no local. Se pretender permitir o tráfego no local, tem também de permitir endereços IP (habitualmente NAT) públicos no local ou ExpressRoute. Estes endereços IP podem ser adicionados através da configuração da firewall do IP para os recursos de serviço do Azure.

  ExpressRoute: se estiver a utilizar o [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) do local, para o peering público ou peering da Microsoft, terá de identificar os endereços IP NAT que são utilizados. Para peering público, cada circuito ExpressRoute, por predefinição, utiliza dois endereços IP NAT que são aplicados ao tráfego de serviço do Azure quando o tráfego entra no backbone de rede do Microsoft Azure. Para peering da Microsoft, o(s) endereço(s) IP NAT que são utilizados são fornecidos pelo cliente ou são fornecidos pelo fornecedor de serviços. Para permitir o acesso aos recursos de serviço, tem de permitir estes endereços IP públicos na definição da firewall do IP dos recursos. Para localizar os endereços IP do circuito ExpressRoute de peering público, [abra um pedido de suporte no ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) através do portal do Azure. Saiba mais sobre [NAT para peering público e da Microsoft do ExpressRoute.](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)

![Proteger serviços do Azure para redes virtuais](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Configuração

- Os pontos finais de serviço são configurados numa sub-rede de uma rede virtual. Os pontos finais funcionam com qualquer tipo de instância de computação em execução nessa sub-rede.
- Pode configurar vários pontos finais de serviço para todos os serviços do Azure suportados (por exemplo, Armazenamento ou Base de Dados SQL do Microsoft Azure) numa sub-rede.
- Na Base de Dados SQL do Azure, as redes virtuais têm de estar na mesma região que o recurso de serviço do Azure. Se utilizar contas do Armazenamento do Microsoft Azure GRS e RA-GRS, a conta principal deve existir na mesma região que a rede virtual. Em todos os outros serviços, os recursos de serviço do Azure podem ser protegidos para redes virtuais em qualquer região. 
- A rede virtual onde o ponto final está configurado pode existir na mesma subscrição ou numa subscrição diferente que a do recurso de serviço do Azure. Para obter mais informações sobre as permissões necessárias para configurar pontos finais e para proteger serviços do Azure, veja [Aprovisionamento](#Provisioning).
- Para os serviços suportados, pode proteger recursos novos ou existentes para redes virtuais através de pontos finais de serviço.

### <a name="considerations"></a>Considerações

- Depois de ativar um ponto final de serviço, os endereços IP de origem das máquinas virtuais na sub-rede mudam da utilização de endereços IPv4 públicos para a utilização do respetivo endereço IPv4 privado quando comunicam com o serviço a partir dessa sub-rede. Todas as ligações TCP abertas existentes para o serviço serão fechadas durante esta mudança. Certifique-se de que não existem tarefas críticas em execução quando ativar ou desativar um ponto final de serviço para um serviço para uma sub-rede. Verifique também se as aplicações conseguem ligar-se automaticamente aos serviços do Azure após esta mudança do endereço IP.

  A mudança do endereço IP só afeta o tráfego de serviço da rede virtual. Não afeta qualquer outro tráfego enviado ou recebido a partir de endereços IPv4 públicos atribuídos às máquinas virtuais. Nos serviços do Azure, se tiver regras de firewall existente que utilizem IPs públicos do Azure, estas regras deixam de funcionar com a mudança para os endereços privados da rede virtual.
- Com pontos finais de serviço, as entradas de DNS dos serviços do Azure permanecem inalteradas e continuam a resolver endereços IP atribuídos ao serviço do Azure.

- Grupos de segurança de rede (NSGs) com pontos finais de serviço:
  - Por predefinição, os NSGs permitem tráfego de saída da Internet e, por isso, também permitem o tráfego da sua VNet para os serviços do Azure. A operação permanece inalterada com os pontos finais de serviço. 
  - Se pretende negar todo o tráfego de saída da Internet e permitir apenas o tráfego para serviços específicos do Azure, pode fazê-lo com as [etiquetas de serviço](security-overview.md#service-tags) nos seus NSGs. Pode especificar os serviços do Azure suportados como destino nas regras do NSG e a manutenção dos endereços IP indicando que cada etiqueta é fornecida pelo Azure. Para obter mais informações, consulte [Etiquetas de Serviço do Azure para NSGs.](security-overview.md#service-tags) 

### <a name="scenarios"></a>Cenários

- **Redes virtuais múltiplas, ligadas ou ponto a ponto**: para proteger serviços do Azure em várias sub-redes numa rede virtual ou em várias redes virtuais, pode ativar pontos finais de serviço em cada uma destas sub-redes de forma independente e proteger os recursos de serviço do Azure em todas essas sub-redes.
- **Filtrar tráfego de saída de uma rede virtual para os serviços do Azure**: se pretender inspecionar ou filtrar o tráfego destinado a um serviço do Azure a partir da rede virtual, pode implementar uma aplicação de rede virtual nessa rede virtual. Em seguida, pode aplicar pontos finais de serviço à sub-rede onde está implementada a aplicação de rede virtual e proteger o recurso de serviço do Azure apenas nesta sub-rede. Este cenário poderá ser útil se pretender restringir o acesso aos serviços do Azure a partir da rede virtual apenas a recursos específicos do Azure através da filtragem da aplicação de rede virtual. Para obter mais informações, veja [saída com aplicações de rede virtual](/azure/architecture/reference-architectures/dmz/nva-ha#egress-with-layer-7-nvas.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- **Proteger recursos do Azure nos serviços implementados diretamente em redes virtuais**: vários serviços do Azure podem ser implementados diretamente em sub-redes específicas numa rede virtual. Pode proteger recursos de serviço do Azure para sub-redes de [serviços geridos](virtual-network-for-azure-services.md) ao configurar um ponto final de serviço na sub-rede do serviço gerido.
- **Tráfego de disco de uma máquina virtual do Azure**: o tráfego de Disco da Máquina Virtual (incluindo montar e desmontar, diskIO), para discos geridos/não geridos, não é afetado pelas alterações do encaminhamento de pontos finais de serviço para o Armazenamento do Azure. Pode limitar o acesso REST aos blobs de página para selecionar redes, através de pontos finais de serviço e [regras de rede de Armazenamento do Azure](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

### <a name="logging-and-troubleshooting"></a>Registo e resolução de problemas

Depois configurar os pontos finais de serviço para um serviço específico, valide se a rota do ponto final de serviço está em vigor através do seguinte: 
 
- Validação do endereço IP de origem de qualquer pedido de serviço no diagnóstico do serviço. Todos os novos pedidos com pontos finais de serviço mostram o endereço IP de origem para o pedido como o endereço IP privado da rede virtual atribuído ao cliente que efetua o pedido a partir da sua rede virtual. Sem o ponto final, o endereço é um endereço IP público do Azure.
- Visualização das rotas efetivas em qualquer interface de rede numa sub-rede. A rota para o serviço:
  - Mostra uma rota predefinida mais específica para endereçar intervalos de prefixo de cada serviço
  - Tem um nextHopType de *VirtualNetworkServiceEndpoint*
  - Indica que está em vigor uma ligação mais direta ao serviço, em comparação com quaisquer rotas de imposição de túnel

>[!NOTE]
> As rotas do ponto final de serviço substituem quaisquer rotas BGP ou UDR para a correspondência de prefixo de endereço de um serviço do Azure. Saiba mais sobre como [resolver problemas com rotas efetivas](diagnose-network-routing-problem.md)

## <a name="provisioning"></a>Aprovisionamento

Os pontos finais de serviço podem ser configurados de forma independente em redes virtuais por um utilizador com acesso de escrita numa rede virtual. Para proteger os recursos de serviço do Azure para uma VNet, o utilizador tem de ter permissão *Microsoft.Network/JoinServicetoaSubnet* para as sub-redes que estão a ser adicionadas. Esta permissão está incluída por predefinição nas funções incorporadas de administrador de serviço e podem ser modificadas mediante a criação de funções personalizadas.

Saiba mais sobre [funções incorporadas](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e a atribuição de permissões específicas a [funções personalizadas](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

As redes virtuais e os recursos de serviço do Azure podem pertencer às mesmas subscrições ou a subscrições diferentes. Se os recursos de serviço da rede virtual e do Azure pertencerem a subscrições diferentes, os recursos devem existir no mesmo inquilino do Active Directory (AD). 

## <a name="pricing-and-limits"></a>Preços e limites

Não há encargos adicionais para a utilização de pontos finais de serviço. O modelo de preços atual dos serviços do Azure (Armazenamento do Azure, Base de Dados SQL do Azure, etc.) aplica-se tal como está definido atualmente.

Não existe limite para o número total de pontos finais de serviço numa rede virtual.

Para um recurso de serviço do Azure (como uma conta do Armazenamento do Microsoft Azure), os serviços podem impor limites ao número de sub-redes utilizadas para a proteção do recurso. Consulte a documentação de vários serviços em [Passos seguintes](#next-steps) para obter mais detalhes.

## <a name="next-steps"></a>Passos seguintes

- Saiba como [configurar pontos finais de serviço de rede virtual](tutorial-restrict-network-access-to-resources.md)
- Saiba como [proteger uma conta do Armazenamento do Microsoft Azure para uma rede virtual](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Saiba como [proteger uma conta da Base de Dados SQL do Microsoft Azure para uma rede virtual](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- Saiba mais sobre a [integração do serviço do Azure em redes virtuais](virtual-network-for-azure-services.md)
-  Início rápido: [modelo do Azure Resource Manager](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) para configurar o ponto final de serviço numa sub-rede da VNet e na conta de Armazenamento do Azure segura dessa sub-rede.

