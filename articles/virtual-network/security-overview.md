---
title: Descrição geral dos grupos de segurança do Azure | Microsoft Docs
description: Saiba mais sobre os grupos de segurança de aplicações e rede. Os grupos de segurança ajudam-no a filtrar o tráfego de rede entre recursos do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: jdial
ms.openlocfilehash: 6501444aacd36c794c8a169a8d1b16898efad018
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977255"
---
# <a name="security-groups"></a>Grupos de segurança
<a name="network-security-groups"></a>

Pode filtrar o tráfego de rede de e para recursos do Azure numa [rede virtual](virtual-networks-overview.md) do Azure com um grupo de segurança de rede. Os grupos de segurança de rede contêm [regras de segurança](#security-rules) que permitem ou negam o tráfego de entrada ou de saída de e para vários tipos de recursos do Azure. Para saber que recursos do Azure podem ser implementados numa rede virtual e associar grupos de segurança de rede aos mesmos, veja [Virtual network integration for Azure services](virtual-network-for-azure-services.md) (Integração da rede virtual para serviços do Azure). Para cada regra, pode especificar a origem e o destino, a porta e o protocolo.

Este artigo explica os conceitos dos grupos de segurança de rede para ajudá-lo a utilizá-los eficientemente. Se nunca tiver criado um grupo de segurança de rede, pode seguir um [tutorial](tutorial-filter-network-traffic.md) rápido para ganhar experiência na criação de um. Se estiver familiarizado com os grupos de segurança de rede e tiver de geri-los, veja [Manage a network security group](manage-network-security-group.md) (Gerir um grupo de segurança de rede). Se estiver com problemas de comunicação e precisar de resolver problemas nos grupos de segurança de rede, veja [Diagnose a virtual machine network traffic filter problem](diagnose-network-traffic-filter-problem.md) (Diagnosticar problemas de filtro de tráfego de rede de uma máquina virtual). Pode ativar [registos de fluxos de grupos de segurança de rede](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para [analisar o tráfego de rede](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de e para recursos que têm um grupo de segurança de rede associado.

## <a name="security-rules"></a>Regras de segurança

Os grupos de segurança de rede contêm zero ou tantas regras conforme pretender, dentro dos [limites](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) das subscrições do Azure. Cada regra especifica as propriedades seguintes:

|Propriedade  |Explicação  |
|---------|---------|
|Nome|Um nome exclusivo no grupo de segurança de rede.|
|Prioridade | Um número entre 100 e 4096. As regras são processadas por ordem de prioridade, sendo os números mais baixos processados antes dos mais elevados, uma vez que têm uma prioridade superior. Quando o tráfego corresponder a uma regra, o processamento para. Como resultado, qualquer regra que exista com prioridades inferiores (números mais elevados) e que tenham os mesmos atributos das regras com prioridades superiores não é processada.|
|Origem ou destino| Qualquer endereço IP ou um endereço IP individual, um bloco CIDR (Classless Inter-domain Routing) (por exemplo, 10.0.0.0/24), [etiqueta de serviço](#service-tags) ou [grupo de segurança de aplicações](#application-security-groups). Se especificar um endereço para um recurso do Azure, indique o endereço IP privado atribuído ao mesmo. Os grupos de segurança de rede são processados depois de o Azure traduzir um endereço IP público num privado para tráfego de entrada e antes de traduzir um endereço IP privado num público para tráfego de saída. Saiba mais sobre os [endereços IP](virtual-network-ip-addresses-overview-arm.md) do Azure. Especificar um intervalo, uma etiqueta de serviço ou um grupo de segurança de aplicações permite-lhe criar menos regras de segurança. A capacidade de especificar vários endereços IP individuais e intervalos de endereços IP (não pode indicar etiquetas de serviço ou grupos de aplicações) numa regra é denominada [regras de segurança aumentadas](#augmented-security-rules). As regras de segurança aumentadas só podem ser criadas em grupos de segurança de rede gerados através do modelo de implementação do Resource Manager. Não pode especificar vários endereços IP nem intervalos de endereços IP em grupos de segurança de rede criados com o modelo de implementação clássica. Saiba mais sobre os [modelos de implementação do Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|Protocolo     | TCP, UDP ou Qualquer, o que inclui TCP, UDP e ICMP. Não pode especificar ICMP individualmente, pelo que, se precisar deste, utilize Qualquer. |
|Direção| Indica se a regra se aplica a tráfego de entrada ou de saída.|
|Intervalo de portas     |Pode especificar uma porta individual ou um intervalo de portas. Por exemplo, pode indicar 80 ou 10000-10005. Especificar intervalos permite-lhe criar menos regras de segurança. As regras de segurança aumentadas só podem ser criadas em grupos de segurança de rede gerados através do modelo de implementação do Resource Manager. Não pode especificar várias portas nem intervalos de portas na mesma regra de segurança em grupos de segurança de rede criados com o modelo de implementação clássica.   |
|Ação     | Permitir ou negar        |

As regras de segurança dos grupos de segurança de rede são avaliadas por prioridade, utilizando as informações de cinco cadeias de identificação (origem, porta de origem, destino, porta de destino e protocolo) para permitir ou negar o tráfego. É criado um registo de fluxo para as ligações existentes. A comunicação é permitida ou negada com base no estado da ligação do registo do fluxo. O registo do fluxo permite que um grupo de segurança de rede tenha monitoração de estado. Se especificar uma regra de segurança de saída para qualquer endereço através da porta 80, por exemplo, não é necessário especificar uma regra de segurança de entrada para a resposta ao tráfego de saída. Só tem de especificar uma regra de segurança de entrada se a comunicação for iniciada externamente. O oposto também se aplica. Se o tráfego de entrada for permitido numa porta, não é necessário especificar uma regra de segurança de saída para responder ao tráfego através da porta.
As ligações existentes não podem ser interrompidas quando remover uma regra de segurança que ativou o fluxo. Os fluxos de tráfego são interrompidos quando as ligações são paradas e não há qualquer tráfego a fluir em qualquer direção, pelo menos, durante alguns minutos.

Há limites ao número de regras de segurança que pode criar num grupo de segurança de rede. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Regras de segurança aumentadas

As regras de segurança aumentadas simplificam a definição de segurança das redes virtuais, permitindo-lhe definir políticas de segurança de rede maiores e mais complexas com menos regras. Pode combinar várias portas e vários endereços IP explícitos e intervalos numa regra de segurança individual e facilmente compreendida. Utilize as regras aumentadas nos campos de origem, destino e porta das regras. Para simplificar a manutenção da definição de segurança de rede, combine regras de segurança aumentadas com [etiquetas de serviço](#service-tags) ou [grupos de segurança de aplicações](#application-security-groups). Há limites ao número de endereços, intervalos e portas que pode especificar numa regra. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="service-tags"></a>Etiquetas de serviço

 As etiquetas de serviço representam um grupo de prefixos de endereços IP, que ajudam a minimizar a complexidade da criação de regras de segurança. Não pode criar as suas próprias etiquetas de serviço nem especificar que endereços IP estão incluídos nas mesmas. A Microsoft gere os prefixos de endereços que as etiquetas abrangem e atualiza-as automaticamente à medida que os endereços são alterados. Ao criar regras de segurança, pode utilizar etiquetas de serviço em vez de endereços IP específicos. 
 
 Pode transferir e integrar com uma firewall no local a lista de etiquetas de serviço com os detalhes de prefixo sobre as seguintes publicações semanais das clouds [Pública](https://www.microsoft.com/download/details.aspx?id=56519), da [Administração Pública dos EUA](https://www.microsoft.com/download/details.aspx?id=57063), da [China ](https://www.microsoft.com/download/details.aspx?id=57062) e da [Alemanha](https://www.microsoft.com/download/details.aspx?id=57064) do Azure.

 Estão disponíveis as etiquetas de serviço seguintes para utilização em definições de regras de segurança. Os nomes variam ligeiramente entre os [modelos de implementação do Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** para a clássica): esta etiqueta inclui o espaço de endereços das redes virtuais (todos os intervalos de CIDR definidos para as redes virtuais), todos os espaços de endereços no local ligados e as redes em [modo de peering](virtual-network-peering-overview.md) ou as redes virtuais ligadas a um [gateway de rede virtual](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** para clássico): esta etiqueta denota o balanceador de carga da infraestrutura do Azure. A etiqueta traduz o [Endereço IP virtual do anfitrião](security-overview.md##azure-platform-considerations) (168.63.129.16), onde as sondas de estado de funcionamento do Azure têm origem. Se não estiver a utilizar o balanceador de carga do Azure, pode substituir esta regra.
* **Internet** (Gestor de Recursos) (**INTERNET** para clássica): esta etiqueta denota o espaço de endereços IP que está fora da rede virtual e acessível pela Internet pública. O intervalo de endereços inclui o [espaço de endereço IP público pertencente ao Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureCloud** (apenas Resource Manager): esta etiqueta denota o espaço de endereços IP do Azure, incluindo todos os [endereços IP públicos do datacenter](https://www.microsoft.com/download/details.aspx?id=41653). Se especificar *AzureCloud* no valor, o tráfego é permitido ou negado para os endereços IP públicos do Azure. Se quiser apenas permitir o acesso ao AzureCloud numa determinada [região](https://azure.microsoft.com/regions), poderá especificar a região. Por exemplo, se pretender permitir o acesso ao AzureCloud do Azure apenas na região EUA Leste, poderá especificar *AzureCloud.EastUS* como a etiqueta de serviço. 
* **AzureTrafficManager** (apenas Resource Manager): esta etiqueta denota o espaço de endereços IP da pesquisa do Gestor de Tráfego do Microsoft Azure. Pode encontrar mais informações sobre os endereços IP da pesquisa do Gestor de Tráfego nas [FAQ do Gestor de Tráfego do Microsoft Azure](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). 
* **Armazenamento** (apenas Resource Manager): esta etiqueta denota o espaço de endereços IP do serviço Armazenamento do Azure. Se especificar *Storage* no valor, o tráfego é permitido ou negado para o armazenamento. Se quiser apenas permitir o acesso ao armazenamento numa determinada [região](https://azure.microsoft.com/regions), pode especificá-la. Por exemplo, se pretender permitir o acesso ao Armazenamento do Azure apenas na região E.U.A. Leste, pode especificar *Storage.EastUS* como a etiqueta de serviço. A etiqueta representa o serviço, mas não instâncias específicas do mesmos. Por exemplo, representa o serviço Armazenamento do Azure, mas não uma conta do Armazenamento do Azure específica. Todos os prefixos de endereço representados por esta etiqueta também são representados pela etiqueta **Internet**. 
* **Sql** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços dos serviços Base de Dados SQL do Azure e Azure SQL Data Warehouse. Se especificar *Sql* no valor, o tráfego é permitido ou negado para o Sql. Se quiser apenas permitir o acesso ao Sql numa determinada [região](https://azure.microsoft.com/regions), pode especificá-la. Por exemplo, se pretender permitir o acesso à Base de Dados SQL do Azure apenas na região E.U.A. Leste, pode especificar *Sql.EastUS* como a etiqueta de serviço. A etiqueta representa o serviço, mas não instâncias específicas do mesmos. Por exemplo, representa o serviço da Base de Dados SQL do Azure, mas não uma base de dados ou um servidor SQL. Todos os prefixos de endereço representados por esta etiqueta também são representados pela etiqueta **Internet**. 
* **AzureCosmosDB** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço de Base de Dados do Azure Cosmos. Se especificar *AzureCosmosDB* no valor, o tráfego é permitido ou negado para o AzureCosmosDB. Se só pretender permitir o acesso ao AzureCosmosDB numa [região](https://azure.microsoft.com/regions) específica, pode especificar a região no seguinte formato AzureCosmosDB.[ nome da região]. 
* **AzureKeyVault** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço KeyVault do Azure. Se especificar *AzureKeyVault* no valor, o tráfego é permitido ou negado para o AzureKeyVault. Se só pretender permitir o acesso ao AzureKeyVault numa [região](https://azure.microsoft.com/regions) específica, pode especificar a região no seguinte formato AzureKeyVault.[ nome da região]. 
* **EventHub** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço EventHub do Azure. Se especificar *EventHub* no valor, o tráfego será permitido ou negado para o EventHub. Se só pretender permitir o acesso ao EventHub numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato EventHub.[nome da região]. 
* **ServiceBus** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço ServiceBus do Azure. Se especificar *ServiceBus* no valor, o tráfego será permitido ou negado para o ServiceBus. Se só pretender permitir o acesso ao ServiceBus numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato ServiceBus.[nome da região]. 
* **MicrosoftContainerRegistry** (apenas Resource Manager): esta etiqueta denota os prefixos de endereço do serviço Container Registry da Microsoft. Se especificar *MicrosoftContainerRegistry* no valor, o tráfego será permitido ou negado para o MicrosoftContainerRegistry. Se só pretender permitir o acesso ao MicrosoftContainerRegistry numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato MicrosoftContainerRegistry.[nome da região]. 
* **AzureContainerRegistry** (apenas Resource Manager): esta etiqueta denota os prefixos de endereço do serviço Azure Container Registry. Se especificar *AzureContainerRegistry* no valor, o tráfego será permitido ou negado para o AzureContainerRegistry. Se só pretender permitir o acesso ao AzureContainerRegistry numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato AzureContainerRegistry.[nome da região]. 
* **AppService** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço AppService do Azure. Se especificar *AppService* no valor, o tráfego será permitido ou negado para o AppService. Se só pretender permitir o acesso ao AppService numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato AppService.[region nome da região]. 
* **AppServiceManagement** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço AppService Management do Azure. Se especificar *AppServiceManagement* no valor, o tráfego será permitido ou negado para o AppServiceManagement. Se só pretender permitir o acesso ao AppServiceManagement numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato AppServiceManagement.[nome da região]. 
* **ApiManagement** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço de Gestão de API do Azure. Se especificar *ApiManagement* no valor, o tráfego será permitido ou negado para ApiManagement. Se só pretender permitir o acesso ao ApiManagement numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato ApiManagement.[nome da região]. 
* **AzureConnectors** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço de Conectores do Azure. Se especificar *AzureConnectors* no valor, o tráfego será permitido ou negado para o AzureConnectors. Se só pretender permitir o acesso ao AzureConnectors numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato AzureConnectors.[nome da região]. 
* **GatewayManager** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço do Gestor de Gateway do Azure. Se especificar *GatewayManager* no valor, o tráfego será permitido ou negado para o GatewayManager. Se só pretender permitir o acesso ao GatewayManager numa [região](https://azure.microsoft.com/regions) específica, poderá especificar a região no seguinte formato GatewayManager.[nome da região]. 
* **AzureDataLake** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço Azure Data Lake. Se especificar *AzureDataLake* no valor, o tráfego será permitido ou negado para o AzureDataLake. 
* **AzureActiveDirectory** (apenas Resource Manager): esta etiqueta denota os prefixos de endereços do serviço AzureActiveDirectory. Se especificar *AzureActiveDirectory* no valor, o tráfego será permitido ou negado para o AzureActiveDirectory.  

> [!NOTE]
> As etiquetas de serviço dos serviços do azure indicam os prefixos de endereço da cloud específica que está a ser utilizada. As etiquetas de serviço regional não são suportadas nas clouds nacionais, apenas o são no formato global. Por exemplo, *Armazenamento* e *Sql*.

> [!NOTE]
> Se implementar um [ponto final de serviço de rede virtual](virtual-network-service-endpoints-overview.md) relativo a um serviço, como o Armazenamento do Azure ou a Base de Dados SQL do Azure, o Azure adiciona uma [rota](virtual-networks-udr-overview.md#optional-default-routes) para uma sub-rede da rede virtual do serviço. Os prefixos de endereço da rota são os mesmos prefixos de endereço, ou intervalos CIDR, da etiqueta de serviço correspondente.

## <a name="default-security-rules"></a>Regras de segurança predefinidas

O Azure cria as seguintes regras predefinidas em cada grupo de segurança de rede que criar:

### <a name="inbound"></a>Entrada

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Prioridade|Origem|Portas de origem|Destino|Portas de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Todos|Permitir|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Prioridade|Origem|Portas de origem|Destino|Portas de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Todos|Permitir|

#### <a name="denyallinbound"></a>DenyAllInbound

|Prioridade|Origem|Portas de origem|Destino|Portas de destino|Protocolo|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Todos|Negar|

### <a name="outbound"></a>Saída

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Todos | Permitir |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Todos | Permitir |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Todos | Negar |

Nas colunas **Source** (Origem) e **Destination** (Destino), *VirtualNetwork*, *AzureLoadBalancer* e *Internet* são [etiquetas de serviço](#service-tags) e não endereços IP. Na coluna Protocol (Protocolo), **All** (Todos) abrange TCP, UDP e ICMP. Ao criar uma regra, pode especificar TCP, UDP ou Todos, mas não pode especificar ICMP individualmente. Por conseguinte, se a sua regra exigir ICMP, selecione *Todos* para o protocolo. *0.0.0.0/0* nas colunas **Source** e **Destination** representa todos os endereços.
 
Não pode remover as regras predefinidas, mas pode criar regras com prioridades superiores para substituí-las.

## <a name="application-security-groups"></a>Grupos de segurança de aplicações

Os grupos de segurança de aplicações permitem-lhe configurar a segurança de rede como uma extensão natural da estrutura de uma aplicação, possibilitando o agrupamento de máquinas virtuais e a definição de políticas de segurança de rede com base nesses grupos. Pode reutilizar a política de segurança em escala sem a manutenção manual de endereços IP explícitos. A plataforma lida com a complexidade dos endereços IP explícitos e dos múltiplos conjuntos de regras, permitindo-lhe focar-se na lógica de negócio. Para compreender melhor os grupos de segurança de rede, considere o exemplo seguinte:

![Grupos de segurança de aplicações](./media/security-groups/application-security-groups.png)

Na imagem anterior, *NIC1* e *NIC2* são membros do grupo de segurança de rede *AsgWeb*. *NIC3* é membro do grupo de segurança de rede *AsgLogic*. *NIC4* é membro do grupo de segurança de rede *AsgDb*. Embora cada interface de rede neste exemplo seja membro de apenas um grupo de segurança de rede, as interfaces de rede podem ser membros de vários grupos, até atingirem os [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Não está associado nenhum grupo de segurança de rede às interfaces de rede. *NSG1* está associado a ambas as sub-redes e contém as seguintes regras:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Esta regra é necessária para permitir o tráfego da Internet para os servidores Web. Uma vez que o tráfego de entrada a partir da Internet é negado pela regra de segurança [DenyAllInbound](#denyallinbound) predefinida, não é necessária qualquer regra adicional para os grupos de segurança de aplicações *AsgLogic* ou *AsgDb*.

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | Permitir |

### <a name="deny-database-all"></a>Deny-Database-All

Uma vez que a regra de segurança predefinida [AllowVNetInBound](#allowvnetinbound) permite todas as comunicações entre recursos na mesma rede virtual, esta regra é necessária para negar o tráfego de todos os recursos.

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Todos | Negar |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Esta regra permite o tráfego do grupo de segurança de aplicações *AsgLogic* para o grupo de segurança de aplicações *AsgDb*. A prioridade desta regra é mais alta do que a da regra *Deny-Database-All*. Como resultado, esta regra é processada antes da regra *Deny-Database-All*, de modo a que o tráfego do grupo de segurança de aplicações *AsgLogic* seja permitido, ao passo que o restante tráfego é bloqueado.

|Prioridade|Origem|Portas de origem| Destino | Portas de destino | Protocolo | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Permitir |

As regras que especifiquem grupos de segurança de aplicações como a origem ou o destino só são aplicadas às interfaces de rede que são membros do grupo de segurança de aplicações. Se a interface de rede não for membro de um grupo de segurança de aplicações, a regra não é aplicada à interface de rede, mesmo que o grupo de segurança de rede esteja associado à sub-rede.

Os grupos de segurança de aplicação têm as seguintes restrições:

-   Há limites ao número de grupos de segurança de rede que podem ser incluídos numa subscrição, bem como outros limites relacionados com os grupos de segurança de aplicações. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Pode especificar um grupo de segurança de aplicações como origem e destino numa regra de segurança. Não pode especificar vários grupos de segurança de aplicações na origem ou no destino.
- Todas as interfaces de rede atribuídas a um grupo de segurança de aplicações têm de existir na mesma rede virtual em que se encontra a primeira interface de rede atribuída ao grupo de segurança da aplicação. Por exemplo, se a primeira interface de rede atribuída a um grupo de segurança de aplicações denominado *AsgWeb* estiver na rede virtual denominada *VNet1*, todas interfaces de rede subsequentes atribuídas a*ASGWeb* têm de existir em *VNet1*. Não é possível adicionar interfaces de rede de redes virtuais diferentes ao mesmo grupo de segurança de aplicação.
- Se especificar um grupo de segurança de aplicação como a origem e o destino numa regra de segurança, as interfaces de rede em ambos os grupos de segurança de aplicações têm de existir na mesma rede virtual. Por exemplo, se *AsgLogic* contivesse interfaces de rede de *VNet1* e *AsgDb* contivesse interfaces de rede de *VNet2*, não poderia atribuir *AsgLogic* como a origem e *AsgDb* como o destino numa regra. Todas as interfaces de rede para os grupos de segurança de origem e de destino têm de estar na mesma rede virtual.

> [!TIP]
> Para minimizar o número de regras de segurança de que precisa, bem como a necessidade de as alterar, planei os grupos de segurança de aplicações de que precisa e crie regras com etiquetas de serviço ou grupos de segurança de aplicações em vez de endereços IP individuais ou intervalos de endereços IP, sempre que possível.

## <a name="how-traffic-is-evaluated"></a>Como o tráfego é avaliado

Pode implementar recursos de vários serviços do Azure numa rede virtual do Azure. Para obter uma lista completa, veja [Services that can be deployed into a virtual network](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network) (Serviços que podem ser implementados numa rede virtual). Pode associar nenhum ou um grupo de segurança de rede à [sub-rede](virtual-network-manage-subnet.md#change-subnet-settings) e à [interface de rede](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) de cada rede virtual numa máquina virtual. O mesmo grupo de segurança de rede pode ser associado a tantas sub-redes e interfaces de rede que escolher.

A imagem seguinte ilustra diferentes cenários de implementação dos grupos de segurança de rede para permitir o tráfego de rede de e para a Internet a partir da porta TCP 80:

![NSG-processing](./media/security-groups/nsg-interaction.png)

Veja a imagem anterior, juntamente com o texto seguinte, para compreender de que forma é que o Azure processa as regras de entrada e de saída para os grupos de segurança de rede:

### <a name="inbound-traffic"></a>Tráfego de entrada

Relativamente ao tráfego de entrada, o Azure processa primeiro as regras num grupo de segurança de rede associado a uma sub-rede, se existir alguma, e, depois, as regras num grupo de segurança de rede associado à interface de rede, se existir alguma.

- **VM1**: as regras de segurança em *NSG1* são processadas, uma vez que está associado a *Subnet1* e *VM1* está em *Subnet1*. A menos que tenha criado uma regra que permita a porta de entrada 80, o tráfego é negado pela regra de segurança predefinida [DenyAllInbound](#denyallinbound) e nunca é avaliado por *NSG2*, pois *NSG2* está associado à interface de rede. Se *NSG1* tiver uma regra de segurança que permite a porta 80, o tráfego é processado por *NSG2*. Para permitir a porta 80 na máquina virtual, tanto *NSG1*, como *NSG2*, têm de ter uma regra que permite a porta 80 a partir da Internet.
- **VM2**: as regras em *NSG1* são processadas, porque *VM2* também está em *Subnet1*. Uma vez que *VM2* não tem um grupo de segurança de rede associado à respetiva interface de rede, recebe todo o tráfego permitido através *NSG1* ou é-lhe negado todo o tráfego negado por *NSG1*. O tráfego é permitido ou negado para todos os recursos na mesma sub-rede se um grupo de segurança de rede estiver associado a uma sub-rede.
- **VM3**: uma vez que não existe nenhum grupo de segurança de rede associado a *Subnet2*, o tráfego é permitido na sub-rede e processado por *NSG2*, pois *NSG2* está associado à interface de rede anexada a *VM3*.
- **VM4**: o tráfego é permitido para *VM4,* porque não existe nenhum grupo de segurança de rede associado a *Subnet3* ou à interface de rede na máquina virtual. Todo o tráfego de rede é permitido através de uma sub-rede e de uma interface de rede se não houver nenhum grupo de segurança de rede associado às mesmas.

### <a name="outbound-traffic"></a>Tráfego de saída

Relativamente ao tráfego de saída, o Azure processa primeiro as regras num grupo de segurança de rede associado a uma interface de rede, se existir alguma, e, depois, as regras num grupo de segurança de rede associado à sub-rede, se existir alguma.

- **VM1**: as regras de segurança em *NSG2* são processadas. A menos que crie uma regra de segurança que negue a porta 80 de saída para a Internet, o tráfego é permitido pela regra de segurança predefinida [AllowInternetOutbound](#allowinternetoutbound) em *NSG1* e *NSG2*. Se *NSG2* tiver uma regra de segurança que negue a porta 80, o tráfego é negado e nunca avaliado por *NSG1*. Para negar a porta 80 a partir da máquina virtual, um ou ambos os grupos de segurança de rede têm de ter uma regra que negue a porta 80 para a Internet.
- **VM2**: todo o tráfego é enviado da interface de rede para a sub-rede, uma vez que a interface de rede anexada a *VM2* não tem um grupo de segurança de rede associado a si. As regras em *NSG1* são processadas.
- **VM3**: se *NSG2* tiver uma regra de segurança que negue a porta 80, o tráfego é negado. Se *NSG2* tiver uma regra de segurança que permita a porta 80, é permitido tráfego de saída para a Internet na mesma, uma vez que não existe nenhum grupo de segurança de rede associado a *Subnet2*.
- **VM4**: todo o tráfego de rede é permitido a partir de *VM4,* porque não está associado nenhum grupo de segurança de rede à interface de rede anexada à máquina virtual ou a *Subnet3*.

Pode ver as [regras de segurança em vigor](virtual-network-network-interface.md#view-effective-security-rules) numa interface de rede para ver facilmente as regras agregadas que estão aplicadas à mesma. Também pode utilizar a capacidade [Verificação de fluxo de IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) do Observador de Rede do Azure para saber se a comunicação é permitida de ou para uma interface de rede. O fluxo de IP indica se a comunicação é permitida ou negada e que regra de segurança de rede permite ou nega o tráfego.

> [!NOTE]
> Grupos de segurança de rede são associados a sub-redes ou máquinas virtuais e serviços implementados no modelo de implementação clássica, e não a interfaces de rede no modelo de implementação do Resource Manager da cloud. Para saber mais sobre os modelos de implementação do Azure, veja [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Compreender os modelos de implementação do Azure).

> [!TIP]
> A menos que tenha um motivo específico, recomendamos que associe um grupo de segurança de rede a uma sub-rede ou a uma interface de rede, mas não a ambas. Uma vez que as regras num grupo de segurança de rede associado a uma sub-rede podem entrar em conflito com as regras num grupo associado a uma interface de rede, poderão ocorrer problemas de comunicação inesperados que exijam resolução.

## <a name="azure-platform-considerations"></a>Considerações sobre a plataforma do Azure

- **IP Virtual do nó de anfitrião:** os serviços de infraestrutura básica, tais como DHCP, DNS e monitorização do estado de funcionamento, são fornecidos através dos endereços IP do anfitrião virtualizado 168.63.129.16 e 169.254.169.254. Estes endereços IP públicos pertencem à Microsoft e são os únicos endereços IP virtualizados utilizados em todas as regiões para este fim. O mapa de endereços para o endereço IP físico do computador servidor (nó de anfitrião) que está a alojar a máquina virtual. O nó de anfitrião funciona como o reencaminhamento DHCP, o resolvedor recursivo de DNS e a origem de sonda para a sonda de estado de funcionamento do balanceador de carga e a sonda de estado de funcionamento da máquina. A comunicação para estes endereços IP não é um ataque. Se bloquear o tráfego de ou para estes endereços IP, uma máquina virtual poderá não funcionar corretamente.
- **Licenciamento (Serviço de Gestão de Chaves):** as imagens do Windows em execução nas máquinas virtuais têm de ser licenciadas. Para garantir o licenciamento, é enviado um pedido para os servidores de anfitrião do Key Management Service que processam estas consultas. O pedido é feito através da porta 1688 de saída. Para implementações que utilizam a configuração de [rota predefinida 0.0.0.0/0](virtual-networks-udr-overview.md#default-route), esta regra de plataforma será desativada.
- **Máquinas virtuais em conjuntos com balanceamento de carga**: a porta de destino e o intervalo de endereços aplicados são provenientes do computador de origem, não do balanceador de carga. A porta de destino e o intervalo de endereços destinam-se ao computador de destino, não ao balanceador de carga.
- **Instâncias de serviço do Azure**: as instâncias de vários serviços do Azure, como o HDInsight, os Ambientes de Serviço de Aplicações e os Conjuntos de Dimensionamento de Máquinas Virtuais, são implementados em sub-redes da rede virtual. Para obter uma lista completa dos serviços que pode implementar em redes virtuais, veja [Rede virtual para os serviços do Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Certifique-se de que se familiariza com os requisitos de portas de cada serviço antes de aplicar um grupo de segurança de rede à sub-rede na qual o recurso está implementado. Se negar portas de que os serviços precisam, estes não funcionarão corretamente.
- **Enviar e-mails de saída**: a Microsoft recomenda que utilize serviços de reencaminhamento de SMTP autenticados (normalmente ligados através da porta TCP 587, mas, muitas vezes, também através de outras portas) para enviar e-mails a partir de Máquinas Virtuais do Azure. Os serviços de reencaminhamento de SMTP especializam-se na reputação do remetente, para minimizar a possibilidade de os fornecedores de e-mail de terceiros rejeitarem mensagens. Esses serviços de reencaminhamento de SMTP incluem, sem estarem limitados, o Exchange Online Protection e o SendGrid. A utilização dos serviços de reencaminhamento de SMTP não está de forma alguma restringida no Azure, independentemente do seu tipo de subscrição. 

  Se tiver criado a sua subscrição do Azure antes de 15 de novembro de 2017, para além de poder utilizar os serviços de reencaminhamento de SMTP, também pode enviar e-mails diretamente através da porta TCP 25. Se tiver criado a sua subscrição após 15 de novembro de 2017, poderá não conseguir enviar e-mails diretamente através da porta 25. O comportamento da comunicação de saída através da porta 25 depende do seu tipo de subscrição, da seguinte forma:

     - **Contrato Enterprise**: a comunicação de saída através da porta 25 é permitida. Pode enviar e-mails de saída diretamente a partir de máquinas virtuais para fornecedores de e-mail externos, sem restrições impostas pela plataforma do Azure. 
     - **Pay as you go:** a comunicação de saída através da porta 25 está bloqueada a partir de todos os recursos. Se tiver de enviar e-mails a partir de uma máquina virtual diretamente para fornecedores de e-mail externos (sem utilizar um reencaminhamento de SMTP autenticado), pode fazer um pedido para remover a restrição. Os pedidos são analisados e aprovados à discrição da Microsoft, apenas sendo concedidos após as verificações contra fraudes. Para fazer um pedido, abra um processo de suporte com o tipo de problema *Técnico*, *Conectividade da Rede Virtual*, *Não é possível enviar e-mails (SMTP/Porta 25)*. No processo de suporte, inclua detalhes sobre o motivo pelo qual a subscrição tem de enviar e-mails diretamente para fornecedores de e-mail, em vez de utilizar um reencaminhamento de SMTP autenticado. Se a sua subscrição for isenta, apenas as máquinas virtuais criadas após a data de isenção conseguem realizar comunicações de saída através da porta 25.
     - **MSDN, Azure Pass, Azure no Open, Education, BizSpark e Avaliação gratuita**: a comunicação de saída através da porta 25 está bloqueada a partir de todos os recursos. Não pode fazer pedidos para remover a restrição, porque os pedidos não são concedidos. Se tiver de enviar e-mails a partir da sua máquina virtual, tem de utilizar um serviço de reencaminhamento de SMTP.
     - **Fornecedor de serviços cloud**: Os clientes que consomem recursos do Azure através de um fornecedor de serviços cloud podem criar um pedido de suporte no respetivo fornecedor de serviços cloud e pedir que o fornecedor crie um pedido de desbloqueio em seu nome, se não for possível utilizar uma transmissão SMTP segura.

  Se o Azure lhe permitir enviar e-mails através da porta 25, a Microsoft não pode garantir que os fornecedores de e-mail aceitarão e-mails enviados a partir da sua máquina virtual. Se um fornecedor específico rejeitar e-mails da sua máquina virtual, trabalhe diretamente com o fornecedor para resolver quaisquer problemas relacionados com a entrega de mensagens ou com a filtragem de spam, ou tem de utilizar um serviço de reencaminhamento de SMTP autenticado.

## <a name="next-steps"></a>Passos Seguintes

* Leia o artigo [Create a network security group](tutorial-filter-network-traffic.md) (Criar um grupo de segurança de rede).
