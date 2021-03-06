---
title: Tutorial do Centro de Segurança do Azure - Definir e avaliar políticas de segurança | Microsoft Docs
description: Tutorial do Centro de Segurança do Azure - Definir e avaliar políticas de segurança
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/30/2018
ms.author: rkarlin
ms.openlocfilehash: fcd3c2a95cea0a838fc16149a0a74fad95ea3300
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027066"
---
# <a name="tutorial-define-and-assess-security-policies"></a>Tutorial: definir e avaliar as políticas de segurança
O Centro de Segurança ajuda-o a manter a conformidade com os requisitos de segurança empresariais ou regulamentares mediante a utilização de políticas de segurança que definem a configuração pretendida para as suas cargas de trabalho. Depois de definir políticas para as suas subscrições do Azure e de as adaptar ao tipo de carga de trabalho ou à sensibilidade dos seus dados, o Centro de Segurança pode mostrar recomendações de segurança para os seus recursos de computação, de aplicações, de rede, de dados e armazenamento e de identidade e acesso. Neste tutorial, vai aprender a:

> [!div class="checklist"]
> * Configurar a política de segurança
> * Avaliar a segurança dos seus recursos

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para acompanhar as funcionalidades abrangidas neste tutorial, tem de estar no escalão de preço Standard do Centro de Segurança. Pode experimentar o Centro de Segurança Standard sem custos durante os primeiros 60 dias. O início rápido [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Incluir a sua subscrição do Azure no Centro de Segurança Standard) explica-lhe como atualizar para Standard.

## <a name="configure-security-policy"></a>Configurar a política de segurança
O Centro de Segurança cria automaticamente uma política de segurança predefinida para cada uma das suas subscrições do Azure. As políticas de segurança são compostas por recomendações que pode ativar ou desativar de acordo com os requisitos de segurança dessa subscrição. Para fazer alterações à política de segurança predefinida, tem de ser proprietário, contribuidor ou administrador de segurança da subscrição em causa.

1. No menu principal do Centro de Segurança, selecione **Política de segurança**.
2. Selecione a subscrição que pretende utilizar.

  ![Política de segurança](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

3. Em **Computação e aplicações**, **Rede** e **Dados**, defina cada configuração de segurança que quer monitorizar como **Ativado**. O Centro de Segurança avalia continuamente a configuração do seu ambiente e, se existirem vulnerabilidades, gerará uma recomendação de segurança. Selecione **Desativar** se a configuração de segurança não for recomendada ou não for relevante. Por exemplo, num ambiente de dev/test, poderá não precisar do mesmo nível de segurança do que num ambiente de produção. Depois de selecionar as políticas que são aplicáveis ao seu ambiente, clique em **Guardar**.

  ![Configuração de segurança](./media/tutorial-security-policy/tutorial-security-policy-fig6.png)  

Aguarde até que o Centro de Segurança processe estas políticas e gere recomendações. Algumas configurações, como as atualizações ao sistema e as configurações de SO, podem demorar até 12 horas, ao passo que as configurações de grupos de segurança de rede e de encriptação podem ser avaliadas quase de imediato. Quando vir as recomendações no dashboard do Centro de Segurança, pode avançar para o passo seguinte.

## <a name="assess-security-of-resources"></a>Avaliar a segurança dos recursos
1. De acordo com as políticas de segurança que tiverem sido ativadas, o Centro de Segurança irá disponibilizar um conjunto de recomendações de segurança, conforme necessário. Deve começar por rever as recomendações relativas à máquina virtual e aos computadores. No dashboard do Centro de Segurança, selecione **Descrição Geral** e **Computação e aplicações**.

  ![Computação](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Priorize as recomendações que aparecem a vermelho (prioridade elevada) e reveja cada recomendação. Algumas destas recomendações têm soluções que podem ser implementadas diretamente a partir do Centro de Segurança, como, por exemplo, os [problemas de proteção de pontos finais](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection). Outras recomendações têm apenas diretrizes para aplicar a solução, tais como a recomendação de encriptação de disco em falta.

2. Assim que abordar todas as recomendações de computação relevantes, deve avançar para a carga de trabalho seguinte, o funcionamento em rede. No dashboard do Centro de Segurança, clique em **Descrição Geral** e clique em **Funcionamento em Rede**.

  ![Redes](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  A página de recomendações de rede tem uma lista de problemas de segurança de configuração de rede, de pontos finais destinados à Internet e de topologia de rede. Tal como com a **Computação e aplicações**, algumas recomendações de rede disponibilizarão soluções integradas e outras não.

3. Assim que abordar todas as recomendações de rede relevantes, deve avançar para a carga de trabalho seguinte, o armazenamento e dados. No dashboard do Centro de Segurança, clique em **Descrição geral** e em **Dados e armazenamento**.

  ![Recursos de dados](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  A página **Recursos de Dados** contém recomendações relativas à ativação da auditoria para os servidores e as bases de dados SQL do Azure, à ativação da encriptação para as bases de dados SQL e à ativação da encriptação da sua conta de armazenamento do Azure. Se não tiver estas cargas de trabalho, não verá nenhuma recomendação. Tal como com a **Computação e aplicações**, algumas recomendações de dados e armazenamento disponibilizarão soluções integradas e outras não.

4. Assim que abordar todas as recomendações de dados e armazenamento relevantes, deve avançar para a carga de trabalho seguinte, identidade e acesso. No dashboard do Centro de Segurança, clique em **Descrição Geral** e em **Identidade e acesso**.

  ![Identidade e acesso](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  A página **Identidade e acesso** contém recomendações como:

   - Ativar o MFA para contas com privilégios na sua subscrição
   - Remover contas externas com permissões de escrita da sua subscrição
   - Remover as contas externas com privilégios da sua subscrição

## <a name="clean-up-resources"></a>Limpar recursos
Outros inícios rápidos e tutoriais desta coleção têm por base este início rápido. Se pretender continuar a trabalhar com inícios rápidos e tutoriais posteriores, continue a executar o escalão Standard e mantenha o aprovisionamento automático ativado. Se não pretender continuar ou quiser voltar para o Escalão gratuito:

1. Regresse ao menu principal do Centro de Segurança e selecione **Política de Segurança**.
2. Selecione a subscrição ou a política para a qual pretende voltar como Gratuita. **Política de segurança** abre-se.
3. Em **COMPONENTES DA POLÍTICA**, selecione **Escalão de preço**.
4. Selecione **Gratuito** para alterar a subscrição, do Escalão standard para o Escalão gratuito.
5. Selecione **Guardar**.

Se pretender desativar aprovisionamento automático:

1. Regresse ao menu principal do Centro de Segurança e selecione **Política de segurança**.
2. Selecione a subscrição para a qual pretende desativar o aprovisionamento automático.
3. Em **Política de segurança – Recolha de Dados**, selecione **Desativar** em **Inclusão** para desativar o aprovisionamento automático.
4. Selecione **Guardar**.

>[!NOTE]
> Desativar o aprovisionamento automático não remove o Microsoft Monitoring Agent das VMs do Azure onde o agente tiver sido aprovisionado. Desativar o aprovisionamento automático limita a monitorização da segurança dos seus recursos.
>

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, ficou a saber mais sobre a definição básica de políticas e a avaliação de segurança da sua carga de trabalho com o Centro de Segurança, tais como:

> [!div class="checklist"]
> * Configuração de política de segurança para garantir a conformidade com os requisitos de segurança da sua empresa ou com os requisitos regulamentares
> * Avaliação de segurança da computação, das redes, de SQL e armazenamento e de recursos de aplicações

Avance para o próximo tutorial para saber como utilizar o Centro de Segurança para proteger os seus recursos.

> [!div class="nextstepaction"]
> [Proteger os seus recursos](tutorial-protect-resources.md)
