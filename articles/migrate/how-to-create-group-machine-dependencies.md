---
title: Agrupar máquinas através de dependências de máquina com o Azure Migrate | Documentos da Microsoft
description: Descreve como criar uma avaliação com as dependências das máquinas com o serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 09/21/2018
ms.author: raynew
ms.openlocfilehash: 9ec69b474f72115f2104060b6f55af1992c31517
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51819309"
---
# <a name="group-machines-using-machine-dependency-mapping"></a>Agrupar máquinas através de mapeamento de dependências de máquina

Este artigo descreve como criar um grupo de máquinas para [do Azure Migrate](migrate-overview.md) avaliação ao visualizar as dependências de máquinas. Geralmente usa esse método quando pretender avaliar a grupos de VMs com níveis mais altos de confiança por verificar as dependências das máquinas, antes de executar uma avaliação. Visualização de dependência pode ajudar a planear efetivamente a migração para o Azure. Ele ajuda a garantir que nada seja deixado e não ocorrerem falhas de surpresa quando estiver a migrar para o Azure. Pode descobrir todos os sistemas interdependentes que precisam migrar em conjunto e identificar se um sistema em execução ainda está a servir os utilizadores ou é uma candidata para desativar em vez de migração.


## <a name="prepare-for-dependency-visualization"></a>Preparar para a visualização de dependências
O Azure Migrate tira partido da solução mapa de serviço do Log Analytics para ativar a visualização de dependências de máquinas.

### <a name="associate-a-log-analytics-workspace"></a>Associar uma área de trabalho do Log Analytics
Para aproveitar a visualização de dependências, precisa associar área de trabalho do Log Analytics, nova ou existente, com um projeto do Azure Migrate. Só pode criar ou anexar uma área de trabalho na mesma subscrição em que é criado o projeto de migração.

- Para anexar uma área de trabalho do Log Analytics a um projeto, no **descrição geral**, aceda a **Essentials** secção do projeto clique **requer configuração**

    ![Associar área de trabalho do Log Analytics](./media/concepts-dependency-visualization/associate-workspace.png)

- Quando cria uma nova área de trabalho, tem de especificar um nome para a área de trabalho. A área de trabalho, em seguida, é criada na mesma subscrição que o projeto de migração e, numa região na mesma [geografia do Azure](https://azure.microsoft.com/global-infrastructure/geographies/) como o projeto de migração.
- O **utilizar existente** opção apresenta uma lista apenas essas áreas de trabalho que são criadas em regiões onde o mapa de serviço está disponível. Se tiver uma área de trabalho numa região em que o mapa de serviço não está disponível, não serão apresentado na lista pendente.

> [!NOTE]
> Não é possível alterar a área de trabalho associada a um projeto de migração.

### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes da VM
Depois de configurar uma área de trabalho, terá de transferir e instalar agentes em cada máquina no local que pretende avaliar. Além disso, se tiver máquinas sem conectividade internet, terá de transferir e instalar [gateway do Log Analytics](../log-analytics/log-analytics-oms-gateway.md) nos mesmos.

1. Na **descrição geral**, clique em **gerir** > **máquinas**e selecione a máquina necessária.
2. Na **dependências** coluna, clique em **instalar agentes**.
3. Sobre o **dependências** página, transferir e instalar o Microsoft Monitoring Agent (MMA) e o agente de dependência em cada VM que pretende avaliar.
4. Copie o ID e a chave da área de trabalho. Vai precisar de ambos quando instalar o MMA na máquina no local.

> [!NOTE]
> Para automatizar a instalação de agentes pode utilizar qualquer ferramenta de implementação, como o System Center Configuration Manager ou utilizar a nossa ferramenta de parceiros [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration), que tem uma solução de implantação do agente para o Azure Migrate.

### <a name="install-the-mma"></a>Instalar o MMA

Para instalar o agente num computador Windows:

1. Faça duplo clique no agente transferido.
2. Na página **Bem-vindo**, clique em **Seguinte**. Na página **Termos de Licenciamento**, clique em **Concordo** para aceitar a licença.
3. Na **pasta de destino**, manter ou modificar a pasta de instalação predefinida > **próxima**.
4. Na **opções de configuração do agente**, selecione **Azure Log Analytics** > **seguinte**.
5. Clique em **adicionar** para adicionar uma nova área de trabalho do Log Analytics. Cole o ID de área de trabalho e a chave que copiou do portal. Clique em **Seguinte**.

[Saiba mais](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) sobre a lista de suporte de sistemas operativos do Windows por MMA.

Para instalar o agente num computador Linux:

1. Transfira o pacote adequado (x86 ou x64) para o seu computador Linux utilizar o scp/sftp.
2. Instalar o pacote utilizando o argumento-- instalação.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

[Saiba mais](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) sobre a lista de suporte de sistemas operativos Linux por MMA.

### <a name="install-the-dependency-agent"></a>Instalar o agente de Dependência
1. Para instalar o agente de dependência num computador Windows, clique duas vezes o ficheiro de configuração e siga o assistente.
2. Para instalar o agente de dependência numa máquina Linux, instale como raiz com o seguinte comando:

    ```sh InstallDependencyAgent-Linux64.bin```

Saiba mais sobre o suporte de agente de dependência para o [Windows](../azure-monitor/insights/service-map-configure.md#supported-windows-operating-systems) e [Linux](../azure-monitor/insights/service-map-configure.md#supported-linux-operating-systems) sistemas operativos.

[Saiba mais](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) sobre como pode utilizar scripts para instalar o agente de dependência.

## <a name="create-a-group"></a>Criar um grupo

1. Depois de instalar os agentes, vá para o portal e clique em **Manage** > **máquinas**.
2. Pesquisa para a máquina em que instalou os agentes.
3. O **dependências** coluna para a máquina deve agora mostrar como **ver dependências**. Clique na coluna para ver as dependências da máquina.
4. O mapa de dependência para a máquina mostra os seguintes detalhes:
    - Entrada (clientes) e as ligações de saída (servidores) TCP de/para a máquina
        - As máquinas dependentes que não têm o agente MMA e de dependência instalado são agrupadas por números de porta
        - As máquinas dependentes que têm o MMA e instalado o agente de dependência são apresentadas como caixas separadas
    - Processos em execução no interior da máquina, pode expandir cada caixa de máquina para ver os processos
    - Propriedades como nome de domínio completamente qualificado, sistema operativo, etc. do endereço MAC de cada máquina, pode clicar em cada caixa de máquina para ver estes detalhes

 ![Ver as dependências das máquinas](./media/how-to-create-group-machine-dependencies/machine-dependencies.png)

4. Pode ver as dependências para diferentes horas de duração clicando-se na duração de tempo na etiqueta de intervalo de tempo. Por predefinição, o intervalo é uma hora. Pode modificar o intervalo de tempo ou especificar o início e datas de término e duração.
5. Depois de identificar as máquinas dependentes que pretende agrupar, utilize Ctrl + clique para selecionar várias máquinas no mapa e clique em **agrupar máquinas**.
6. Especifique um nome de grupo. Certifique-se de que as máquinas dependentes são detetadas pelo Azure Migrate.

    > [!NOTE]
    > Se uma máquina dependente não for detetada pelo Azure Migrate, não é possível adicioná-lo ao grupo. Para adicionar essas máquinas ao grupo, terá de executar o processo de deteção novamente com o âmbito correto no vCenter Server e certifique-se de que a máquina é detetada pelo Azure Migrate.  

7. Se quiser criar uma avaliação para este grupo, selecione a caixa de verificação para criar uma nova avaliação para o grupo.
8. Clique em **OK** para guardar o grupo.

Assim que o grupo for criado, recomenda-se para instalar agentes em todas as máquinas do grupo e refinar o grupo ao visualizar a dependência de grupo inteiro.

## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) sobre as perguntas frequentes sobre a visualização de dependência.
- [Saiba como](how-to-create-group-dependencies.md) para refinar o grupo ao visualizar as dependências de grupo.
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
