---
title: Azure notas de versão do fornecedor 1.1.30.0 de recursos do SQL de pilha | Documentos da Microsoft
description: Saiba mais sobre o que há na atualização mais recente do Azure Stack SQL recursos fornecedor, incluindo os problemas conhecidos e onde para baixá-lo.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: d2dda25c63a6e4a73205b9e4a830a211d025b3ed
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688170"
---
# <a name="sql-resource-provider-11300-release-notes"></a>Notas de versão do fornecedor 1.1.30.0 de recursos do SQL

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Estas notas de versão descrevem as melhorias e problemas conhecidos no SQL 1.1.30.0 de versão do fornecedor de recursos.

## <a name="build-reference"></a>Criar a referência
Transferir o fornecedor de recursos do SQL, binário e, em seguida, execute o Self-extractor para extrair o conteúdo para um diretório temporário. O fornecedor de recursos tem um mínimo correspondente do Azure Stack criar. A versão de lançamento do Azure Stack mínima necessária para instalar esta versão do fornecedor de recursos do SQL está listada abaixo:

> |Versão mínima do Azure Stack|Versão do fornecedor de recursos SQL|
> |-----|-----|
> |Versão 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> Aplicar a atualização com suporte do Azure Stack mínimo ao seu sistema integrado do Azure Stack ou implementar a mais recente do Azure Stack Development Kit (ASDK) antes de implementar a versão mais recente do fornecedor de recursos do SQL.

## <a name="new-features-and-fixes"></a>Novas funcionalidades e correções
Esta versão do fornecedor de recursos de SQL do Azure Stack inclui as seguintes melhorias e correções:

- **Telemetria ativada para implementações de fornecedor de recursos do SQL**. Coleção de telemetria tenha sido ativada para implementações de fornecedor de recursos do SQL. Telemetria recolhida inclui a implementação de fornecedor de recursos, iniciar e parar vezes, sair do Estado, mensagens de saída e detalhes do erro (se aplicável).

- **Atualização de encriptação de TLS 1.2**. Ativado apenas 1.2 suporte TLS para a comunicação de fornecedor de recursos com componentes internos do Azure Stack. 

### <a name="fixes"></a>Correções

- **Compatibilidade de Azure Stack do PowerShell de fornecedor de recursos SQL**. O fornecedor de recursos do SQL foi atualizado para trabalho com o perfil de PowerShell híbrido do Azure Stack 2018-03-01- e para proporcionar compatibilidade com o AzureRM 1.3.0 e posterior.

- **Painel de palavra-passe de alteração de início de sessão SQL**. Foi corrigido um problema em que a palavra-passe não pode ser alterada no painel de palavra-passe de alteração. Notificações de alteração de links removidos da palavra-passe.

- **Atualização do painel de definições de servidor de alojamento de SQL**. Foi corrigido um problema em que o painel de definições incorretamente foi intitulado como "Palavra-passe".

## <a name="known-issues"></a>Problemas conhecidos 

- **SKUs de SQL pode demorar até uma hora para ser visível no portal**. Pode demorar até uma hora para recentemente SKUs criados para ser visível para utilização durante a criação de novas bases de dados do SQL. 

    **Solução**: nenhuma.

- **Reutilizado inícios de sessão SQL**. Início de sessão com o mesmo nome de utilizador como um início de sessão existente na mesma subscrição tentar criar um novo SQL irá resultar em reutilizar o mesmo início de sessão e a palavra-passe existente. 

    **Solução**: utilizar diferentes nomes de utilizador durante a criação de novos inícios de sessão na mesma subscrição ou criar inícios de sessão com o mesmo nome de utilizador em subscrições diferentes.

- **Inícios de sessão SQL partilhados inconsistências nos dados**. Se um início de sessão SQL é partilhado por várias bases de dados do SQL na mesma subscrição, alterar a palavra-passe de início de sessão irá provocar inconsistências nos dados.

    **Solução**: Utilize sempre os inícios de sessão diferentes para bases de dados diferentes na mesma subscrição.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Problemas conhecidos para os administradores de nuvem operacionais do Azure Stack
Consulte a documentação sobre o [notas de versão do Azure Stack](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais sobre o fornecedor de recursos do SQL](azure-stack-sql-resource-provider.md).

[Preparar para implementar o fornecedor de recursos do SQL](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[Atualize o fornecedor de recursos do SQL de uma versão anterior](azure-stack-sql-resource-provider-update.md). 