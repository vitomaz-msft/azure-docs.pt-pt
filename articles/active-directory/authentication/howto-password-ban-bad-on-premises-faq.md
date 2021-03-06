---
title: Proteção de palavra-passe do Azure AD FAQ do local
description: Proteção de palavra-passe do Azure AD FAQ do local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 59c89c81f618876de48de66a38e1063eb658fba4
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50743297"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Pré-visualização: Azure AD palavra-passe proteção no local - perguntas mais frequentes

|     |
| --- |
| Proteção de palavra-passe do Azure AD é uma funcionalidade de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Perguntas gerais

**P: quando é que a proteção de palavra-passe do Azure AD será contactado disponibilidade geral (GA)?**

Não, Anunciamos uma data de disponibilidade geral.

**P: é no local, proteção de palavra-passe do Azure AD suportada nas clouds não pública?**

Não, a proteção de palavra-passe do Azure AD no local só é suportada na cloud pública.

**P: como posso aplicar os benefícios de proteção de palavra-passe do Azure AD para um subconjunto dos meus utilizadores no local?**

Não suportado. Depois de implementada e ativada, a proteção de palavra-passe do Azure AD não distinguir - todos os utilizadores recebem benefícios de segurança igual.

**P: é possível para instalar a proteção de palavra-passe do Azure AD lado a lado com outros produtos baseados em palavra-passe-filtro?**

Sim. Suporte para várias dlls de filtro de palavra-passe registado é uma funcionalidade de Windows core e não específico para a proteção de palavra-passe do Azure AD. Todas as dlls de filtro de palavras-passe registado tem de concordar antes de uma palavra-passe é aceite.

**P: por que motivo o DFSR é necessário para a replicação de sysvol?**

O FRS (a tecnologia antecessora da DFSR) tem vários problemas conhecidos e é totalmente suportado em versões mais recentes do Windows Server Active Directory. Zero testes de proteção de palavra-passe do Azure AD serão efetuada em domínios configurados do FRS.

Para obter mais informações, consulte os artigos seguintes:

[O caso para replicação de sysvol de migrar para o DFSR](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[O End é Nigh para DRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**P: por que motivo é necessário um reinício para instalar ou atualizar o software do agente DC?**

Este requisito é causado por núcleo de comportamento do Windows.

**P: existe alguma forma de configurar um agente de controlador de domínio para utilizar um servidor proxy específico?**

Não.

## <a name="next-steps"></a>Passos Seguintes

Se tiver uma questão de proteção de palavra-passe no local do Azure AD, não é respondida aqui, submeta um item de comentário abaixo - obrigado!

[Implementar proteção de palavras-passe do Azure AD](howto-password-ban-bad-on-premises-deploy.md)
