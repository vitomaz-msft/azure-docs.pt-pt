---
title: Descrição geral de etiquetas do FQDN para o Firewall do Azure
description: Saiba mais sobre as etiquetas FQDN na Firewall do Azure
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/1/2018
ms.author: victorh
ms.openlocfilehash: 897ea3856516b5429ffb770164f863d71e7ae0dd
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419015"
---
# <a name="fqdn-tags-overview"></a>Descrição geral de etiquetas FQDN

Uma etiqueta do FQDN representa um grupo de nomes de domínio completamente qualificado (FQDN) associado aos serviços do Microsoft bem conhecidos. Pode utilizar uma etiqueta do FQDN nas regras de aplicação para permitir o tráfego de rede de saída necessárias através da firewall.

Por exemplo, para permitir o tráfego de rede do Windows Update através da firewall manualmente, terá de criar várias regras de aplicações pela documentação da Microsoft. Utilização de etiquetas do FQDN, pode criar uma regra de aplicação, inclua o **atualizações do Windows** Etiquetar e agora tráfego de rede para pontos de extremidade podem fluir através da firewall do Microsoft Windows Update.

Não é possível criar suas próprias etiquetas do FQDN, nem pode especificar que FQDNs são incluídos nas mesmas. A Microsoft gere os FQDNs abrangidos a etiqueta do FQDN e atualiza a etiqueta como alteração de FQDNs. 

<!--- screenshot of application rule with a FQDN tag.-->

A tabela seguinte mostra as marcas FQDN atuais, que pode utilizar. A Microsoft mantém a essas marcas e pode esperar etiquetas adicionais a ser adicionados periodicamente.

|Etiqueta do FQDN  |Descrição  |
|---------|---------|
|Windows Update     |Permitir acesso de saída para o Microsoft Update, conforme descrito em [como configurar uma Firewall para atualizações de Software](https://technet.microsoft.com/library/bb693717.aspx).|
|Diagnóstico do Windows|Permitir acesso de saída a todos [pontos finais de diagnóstico do Windows](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints).|
|Serviço de Proteção Ativa Microsoft (MAPS)|Permitir acesso de saída [MAPS](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/).|
|Ambiente de serviço de aplicações (ASE)|Permite o acesso de saída para o tráfego de plataforma do ASE. Esta etiqueta não abrange específicas do cliente armazenamento SQL pontos de extremidade e criados pelo ASE. Estas devem ser ativadas através de [pontos finais de serviço](../virtual-network/tutorial-restrict-network-access-to-resources.md) ou adicionadas manualmente.<br><br>Para obter mais informações sobre como integrar o Firewall do Azure com o ASE, veja [o bloqueio de um ambiente de serviço de aplicações](../app-service/environment/firewall-integration.md#configuring-azure-firewall-with-your-ase).|
|Azure Backup|Permite o acesso de saída para os serviços de cópia de segurança do Azure.

> [!NOTE]
> Ao selecionar a etiqueta do FQDN numa regra de aplicação, o campo de protocolo: porta deve ser definido como **https**.

## <a name="next-steps"></a>Passos Seguintes

Para saber como implementar uma Firewall do Azure, veja [Tutorial: implementar e configurar a Firewall do Azure no portal do Azure](tutorial-firewall-deploy-portal.md).