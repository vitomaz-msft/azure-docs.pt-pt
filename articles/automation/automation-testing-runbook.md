---
title: Testar um runbook na automatização do Azure
description: Antes de publicar um runbook na automatização do Azure, pode testar para garantir que funciona conforme esperado.  Este artigo descreve como testar um runbook e ver o resultado.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ebeaa8eb75373fc94f7e4e714e36d1167fd7f060
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testar um runbook na automatização do Azure
Quando testa um runbook, o [versão de rascunho](automation-creating-importing-runbook.md#publishing-a-runbook) é executado e todas as ações que executar são concluídas. Nenhum histórico da tarefa é criado, mas o [saída](automation-runbook-output-and-messages.md#output-stream) e [avisos e erros](automation-runbook-output-and-messages.md#message-streams) fluxos são apresentados no teste de saída do painel. As mensagens para o [fluxo verboso](automation-runbook-output-and-messages.md#message-streams) são apresentados do painel de resultados apenas se for o [$VerbosePreference variável](automation-runbook-output-and-messages.md#preference-variables) está definido para continuar.

Apesar da versão de rascunho está a ser executada, o runbook ainda executa normalmente o fluxo de trabalho e efetua quaisquer ações relativamente aos recursos no ambiente. Por este motivo, deverá testar apenas runbooks em recursos de não produção.

O procedimento para testar cada [tipo de runbook](automation-runbook-types.md) é a mesma e há não existe diferença nos testes entre o editor de texto e o editor gráfico no portal do Azure.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Para testar um runbook no portal do Azure
Pode trabalhar com qualquer [tipo de runbook](automation-runbook-types.md) no portal do Azure.

1. Abra a versão de rascunho do runbook no [editor de texto](automation-edit-textual-runbook.md) ou [editor gráfico](automation-graphical-authoring-intro.md).
2. Clique em de **teste** botão para abrir o painel de teste.
3. Se o runbook tiver parâmetros, estes serão apresentados no painel esquerdo, onde pode fornecer valores para ser utilizado para o teste.
4. Se pretender executar o teste num [Runbook Worker híbrido](automation-hybrid-runbook-worker.md), em seguida, altere **executar definições** para **Worker híbrido** e selecione o nome do grupo de destino.  Caso contrário, mantenha a predefinição **Azure** para executar o teste na nuvem.
5. Clique em de **iniciar** botão para iniciar o teste.
6. Se o runbook for [fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) ou [gráfico](automation-runbook-types.md#graphical-runbooks), em seguida, pode parar ou suspender-enquanto está a ser testado com os botões por baixo do painel de resultados. Quando suspende o runbook, conclui a atividade atual antes de ser suspenso. Depois do runbook está suspenso, pode pará-la ou reiniciá-lo.
7. Verifique os resultados do runbook no painel de resultados.

## <a name="next-steps"></a>Próximos Passos
* Para saber como criar ou importar um runbook, consulte [criar ou importar um runbook na automatização do Azure](automation-creating-importing-runbook.md)
* Para obter mais informações sobre a Criação de Gráficos, consulte [Criação de gráficos na Automatização do Azure](automation-graphical-authoring-intro.md)
* Para começar com runbooks do fluxo de trabalho do PowerShell, consulte o artigo [O meu primeiro runbook do fluxo de trabalho do PowerShell](automation-first-runbook-textual.md)
* Para obter mais informações sobre como configurar os runbooks para devolver mensagens de estado e de erros, incluindo recomendado práticas, consulte o artigo [Runbook resultados e mensagens na automatização do Azure](automation-runbook-output-and-messages.md)

