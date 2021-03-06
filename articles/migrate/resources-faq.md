---
title: Azure Migrate - perguntas mais frequentes (FAQ) | Documentos da Microsoft
description: Endereços de perguntas mais frequentes sobre o Azure Migrate
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: snehaa
ms.openlocfilehash: 2f04fe103d010a64a77b7d80730cf80007c3c126
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51256380"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Azure Migrate - perguntas mais frequentes (FAQ)

Este artigo contém perguntas mais frequentes sobre o Azure Migrate. Se tiver quaisquer consultas adicionais depois de ler este artigo, publique o [fórum do Azure Migrate](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Geral

### <a name="does-azure-migrate-support-assessment-of-only-vmware-workloads"></a>O Azure Migrate suporta a avaliação de apenas as cargas de trabalho VMware?

Sim, do Azure Migrate só suporta atualmente a avaliação das cargas de trabalho do VMware. Suporte para Hyper-V e servidores físicos será ativado no futuro.

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>Azure Migrate precisa vCenter Server para detetar um ambiente do VMware?

Sim, o Azure Migrate requer o vCenter Server para detetar um ambiente de VMware. Não suporta a deteção de anfitriões ESXi que não são geridos por um vCenter Server.

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Como é o Azure Migrate diferente do Azure Site Recovery?

O Azure Migrate é um serviço de avaliação que ajuda a detetar as suas cargas de trabalho no local e planeie a migração para o Azure. [O Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), juntamente com a ser uma solução de recuperação após desastre, ajuda-o a migrar cargas de trabalho no local para VMs de IaaS no Azure.

### <a name="whats-the-difference-between-using-azure-migrate-for-assessments-and-the-map-toolkit"></a>O que é a diferença entre a utilização do Azure Migrate para avaliações e o Map Toolkit?

[O Azure Migrate](migrate-overview.md) fornece avaliação de migração especificamente para ajudar na preparação da migração e avaliação das cargas de trabalho no local para o Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/en-us/download/details.aspx?id=7826) tem outras funcionalidades. Por exemplo, a migração de planeamento para versões mais recentes do Windows server sistemas operativos cliente e, software de utilização controlo etc. Para esses cenários, continue a usar o MAP Toolkit.


### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Como é o Azure Migrate diferente do Azure Site Recovery Deployment Planner?

O Azure Migrate é uma ferramenta de planejamento de migração e do Azure Site Recovery Deployment Planner é uma recuperação após desastre (DR), ferramenta de planejamento.

**Migração do VMware para Azure**: Se pretender migrar as cargas de trabalho no local para o Azure, utilizar o Azure Migrate para planeamento da migração. O Azure Migrate avalia cargas de trabalho no local e fornece orientações, informações e mecanismos para ajudá-lo a migrar para o Azure. Assim que estiver pronto com o seu plano de migração, pode utilizar serviços como o Azure Site Recovery e o serviço de migração de base de dados do Azure para migrar as máquinas para o Azure.

**Migração do Hyper-V para o Azure**: Azure Migrate atualmente suporta apenas a avaliação das máquinas de virtuais de VMware para migração para o Azure. Suporte para Hyper-V está nas previsões para o Azure Migrate. Até lá, pode usar o Site Recovery Deployment Planner. Assim que o suporte de Hyper-V está ativado no Azure Migrate, pode utilizar o Azure Migrate para planejamento de migração de cargas de trabalho do Hyper-V.

**Recuperação após desastre do VMware/Hyper-V para o Azure**: se pretende fazer a recuperação após desastre (DR) no Azure com o Azure Site Recovery (recuperação de sites), utilize o Site Recovery Deployment Planner para planejamento de DR. Site Recovery Deployment Planner faz uma avaliação profunda ASR específicos do seu ambiente no local. Fornece recomendações que são necessários pelo Site Recovery para operações bem-sucedidas de DR, como replicação, ativação pós-falha das suas máquinas virtuais.  

### <a name="which-azure-regions-are-supported-by-azure-migrate"></a>Que regiões do Azure são suportados pelo Azure Migrate?

O Azure Migrate suporta atualmente E.U.A. leste e e.u.a. Centro-Oeste como localizações de projeto de migração. Mesmo que só pode criar projetos de migração nos e.u.a. centro-oeste e este dos E.U.A., ainda pode avaliar as máquinas para [várias localizações dos destinos](https://docs.microsoft.com/azure/migrate/how-to-modify-assessment#edit-assessment-properties). A localização do projeto só é utilizada para armazenar os dados detetados.

### <a name="how-does-the-on-premises-site-connect-to-azure-migrate"></a>Como ligar o site no local para o Azure Migrate?

A ligação pode ser através da internet ou utilizar o ExpressRoute com peering público.

### <a name="can-i-harden-the-vm-set-up-with-the-ova-template"></a>Pode proteger a VM que configurar com o modelo de OVA?

Componentes adicionais (por exemplo, software antivírus) podem ser adicionados no modelo de OVA, desde que as regras de comunicação e de firewall necessárias para a aplicação do Azure Migrate trabalhar são deixadas como está.   

## <a name="discovery"></a>Deteção

### <a name="what-data-is-collected-by-azure-migrate"></a>Que dados são recolhidos pelo Azure Migrate?

O Azure Migrate suporta dois tipos de deteção, a deteção com base na aplicação e a deteção com base no agente.
A deteção com base na aplicação recolhe metadados sobre as VMs no local, a lista completa de metadados recolhidos pela aplicação da esteja listada abaixo:

**Dados de configuração da VM**
- Nome de exibição da VM (no vCenter)
- Caminho de inventário VM (anfitrião/cluster/pasta no vCenter)
- Endereço IP
- Endereço MAC
- Sistema operativo
- Número de núcleos, discos, NICs
- Tamanho da memória, tamanhos de disco

**Dados de desempenho da VM**
- Utilização da CPU
- Utilização de memória
- Para cada disco anexado à VM:
  - Débito de leitura de disco
  - Débito de escritas de disco
  - Operações por seg de leitura de disco
  - Operações por segundo de escritas de disco
- Para cada adaptador de rede ligado à VM:
  - Entrada de rede
  - Saída de rede

A deteção com base no agente é uma opção disponível além da deteção com base na aplicação e ajuda os clientes a [visualizarem as dependências](how-to-create-group-machine-dependencies.md) das VMs no local. Os agentes de dependência recolhem detalhes como FQDN, SO, endereço IP, endereço MAC, processos em execução dentro da VM e as ligações de TCP de entrada/saída da VM. A deteção baseada em agente é opcional e pode optar por não instalar os agentes se não pretender visualizar as dependências das VMs.

### <a name="would-there-be-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Deve haver nenhum impacto no desempenho no ambiente de anfitrião do ESXi analisado?

No caso do [uma abordagem de deteção de tempo](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods), para recolher os dados de desempenho, o nível de estatísticas no vCenter server tinha que ser definido como 3. Defini-la para este nível recolheria uma grande quantidade de dados, que seriam armazenados na base de dados do servidor vCenter de resolução de problemas. Portanto, poderão resultar em alguns problemas de desempenho no vCenter Server. Deve haver impacto mínimo no anfitrião ESXi.

Introduzimos de criação de perfis contínua de dados de desempenho (o que estão em pré-visualização). Com a criação de perfis contínua, já não existe a necessidade de alterar o nível de estatísticas do servidor para executar uma avaliação com base no desempenho do vCenter. A aplicação recoletora vai agora criar perfis para as máquinas no local para medir os dados de desempenho das máquinas virtuais. Isso ter um impacto de desempenho de quase zero nos anfitriões ESXi, bem como no vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Em que são os dados recolhidos armazenados e como há muito tempo?

Os dados coletados pela aplicação recoletora são armazenados na localização do Azure que especificar ao criar o projeto de migração. Os dados são armazenados em segurança numa subscrição Microsoft e são eliminados quando o utilizador elimina o projeto do Azure Migrate.

Para a visualização de dependência, se instalar agentes nas VMs, os dados recolhidos pelos agentes do dependência são armazenados nos EUA numa área de trabalho do Log Analytics criada na subscrição do utilizador. Estes dados são eliminados quando eliminar a área de trabalho do Log Analytics na sua subscrição. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>Os dados são encriptados em descanso e em trânsito?

Sim, os dados recolhidos são encriptados em inatividade tanto em trânsito. Os metadados recolhidos pela aplicação da são enviados de forma segura para o serviço Azure Migrate pela internet através de https. Os metadados recolhidos são armazenados na [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) e, na [armazenamento de Blobs do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) numa subscrição Microsoft e são encriptados em descanso.

Os dados recolhidos pelos agentes do dependência é também criptografado em trânsito (canal de https seguro) e são armazenados numa área de trabalho do Log Analytics na subscrição do utilizador. Também são encriptado em inatividade.

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Como o recoletor se comunique com o vCenter Server e o serviço Azure Migrate?

A aplicação recoletor liga-se ao vCenter Server (porta 443) utilizando as credenciais fornecidas pelo utilizador na aplicação. Ele consulta o vCenter Server com o VMware PowerCLI para recolher os metadados sobre as VMs geridas pelo vCenter Server. Recolhe os dados de configuração acerca das VMs (núcleos, memória, discos, etc. NIC), bem como o histórico de desempenho de cada VM para o último mês do vCenter Server. Os metadados recolhidos, em seguida, é enviado para o serviço Azure Migrate (através da internet através de https) para avaliação. [Saiba mais](concepts-collector.md)

### <a name="can-i-connect-the-same-collector-appliance-to-multiple-vcenter-servers"></a>Pode ligar a aplicação recoletora mesmo para vários servidores vCenter?

Sim, uma aplicação de recoletor único pode ser utilizada para detetar o vCenter vários servidores, mas não em simultâneo. Terá de executar as deteções um após o outro.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>O modelo de OVA utilizado pelo Site Recovery é integrado com o OVA utilizado pelo Azure Migrate?

Atualmente, não há nenhuma integração. A. Modelo de OVA no Site Recovery é utilizado para configurar um servidor de configuração do Site Recovery para a replicação de servidor de VM de VMware/físico. A. OVA utilizados pelo Azure Migrate é utilizado para detetar VMs de VMware, gerido por um vCenter server, para fins de avaliação da migração.

### <a name="i-changed-my-machine-size-can-i-rerun-the-assessment"></a>Alterei a minha tamanho da máquina. Pode executar novamente a avaliação?

Se alterar as definições numa VM que pretende avaliar, o acionador detetar novamente com a aplicação recoletora. A aplicação, utilize o **iniciar novamente a recolha** opção para fazer isso. Depois de terminar a coleção, selecione a opção **Recalcular** para a avaliação no portal, para obter os resultados da avaliação atualizada.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Como posso descobrir um ambiente de multi-inquilino no Azure Migrate?

Se tiver um ambiente que é partilhado em inquilinos e não pretender detetar as VMs de um inquilino na subscrição de outro inquilino, pode utilizar o campo de âmbito em que a aplicação recoletora para definir o âmbito de deteção. Se os inquilinos estão a partilhar anfitriões, criar uma credencial que tem acesso só de leitura para apenas as VMs que pertencem ao inquilino específico e, em seguida, utilize esta credencial a aplicação recoletora e especifique o âmbito como host para fazer a deteção. Em alternativa, também pode criar pastas no vCenter Server (Digamos Pasta1 para tenant1 e folder2 para tenant2), em anfitriões partilhado, mover as VMs para tenant1 em Pasta1 e para tenant2 para folder2 e, em seguida, definir âmbito as deteções no coletor de em conformidade ao especificar a pasta apropriada.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Número de máquinas virtuais possam ser detetado num projeto de migração única?

Pode descobrir 1500 máquinas de virtuais num projeto de migração única. Se tiver mais máquinas no seu ambiente no local, [Saiba mais](how-to-scale-assessment.md) sobre como é possível descobrir um ambiente de grandes dimensões no Azure Migrate.

## <a name="assessment"></a>Avaliação

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Suporte do Azure Migrate Enterprise Agreement (EA) com base custa estimativa?

O Azure Migrate atualmente não suporta a estimativa de custos para [oferta Enterprise Agreement](https://azure.microsoft.com/offers/enterprise-agreement-support/). A solução é especificar pay as you go, como a oferta e a especificação manual a percentagem de desconto (aplicável à subscrição) no campo "De desconto" de propriedades de avaliação.

  ![Desconto](./media/resources-faq/discount.png)
  

## <a name="dependency-visualization"></a>Visualização de dependência

### <a name="what-is-dependency-visualization"></a>O que é a visualização de dependências?

Visualização de dependência permite-lhe avaliar de grupos de VMs para a migração com mais confiança por verificação de dependências de máquina antes de executar uma avaliação. Visualização de dependências ajuda-o a garantir que nada seja deixado, evitar falhas inesperadas, ao migrar para o Azure. O Azure Migrate utiliza a solução mapa de serviço do Log Analytics para ativar a visualização de dependência.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>Preciso de pagar para utilizar a funcionalidade de visualização de dependências?

Não. Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>É necessário instalar nada para visualização de dependências?

Para utilizar a visualização de dependência, terá de transferir e instalar agentes em cada máquina no local que pretende avaliar. 

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) tem de ser instalado em cada máquina.
- O [agente de dependência](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) tem de ser instalado em cada máquina.
- Além disso, se tiver máquinas sem conectividade internet, terá de transferir e instalar o gateway de Log Analytics nos mesmos.

Não precisa destes agentes nas máquinas que pretende avaliar a menos que está usando a visualização de dependência.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Pode utilizar uma área de trabalho existente para visualização de dependências?

Sim, do Azure Migrate agora permite-lhe ligar uma área de trabalho existente para o projeto de migração e tirar partido dos mesmos para visualização de dependência. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Pode exportar o relatório de visualização de dependências?

Não, a visualização de dependência não pode ser exportada. No entanto, uma vez que o Azure Migrate utiliza o mapa de serviço para visualização de dependências, pode utilizar o [APIs de REST de mapa de serviço](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obter as dependências no formato json.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Como posso automatizar a instalação do Microsoft Monitoring Agent (MMA) e o agente de dependência?

[Aqui](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) é um script que pode utilizar para a instalação do agente de dependência. Para o MMA, [aqui](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab) é um script disponível no TechNet, que pode ser aproveitado.

Além de scripts, também pode aproveitar, como o System Center Configuration Manager (SCCM), as ferramentas de implantação [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) etc. para implantar os agentes.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Quais são os sistemas operativos suportados por MMA?

É a lista de sistemas de operativos do Windows suportados pelo MMA [aqui](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
É a lista de sistemas de operativos Linux suportados pelo MMA [aqui](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Quais são os sistemas operativos suportados pelo agente de dependência?

A lista de sistemas de operativos do Windows suportados pelo agente de dependência é [aqui](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems).
A lista dos sistemas de operativos Linux suportados pelo agente de dependência é [aqui](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Posso visualizar dependências no Azure Migrate para obter mais informações à duração de uma hora?
Não, a Azure Migrate permite que visualize as dependências de duração do até uma hora. O Azure Migrate permite-lhe voltar atrás para uma data específica na história para até o último mês, mas a duração máxima para o qual é possível visualizar as dependências é até uma hora. Por exemplo, pode usar a funcionalidade de duração de tempo do mapa de dependência, para ver as dependências de ontem, mas pode apenas visualizá-lo para uma janela de uma hora.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-10-vms"></a>Visualização de dependência é suportada para grupos com mais de 10 VMs?
Pode [visualize as dependências para grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que têm cópia de segurança para 10 VMs, se tiver um grupo com mais de 10 VMs, recomendamos que dividir o grupo em grupos mais pequenos e visualize as dependências.


## <a name="next-steps"></a>Passos Seguintes

- Leia o [descrição geral do Azure Migrate](migrate-overview.md)
- Saiba como pode [detetar e avaliar](tutorial-assessment-vmware.md) um ambiente de VMware
