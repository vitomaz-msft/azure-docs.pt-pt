---
title: Proteção avançada contra ameaças - base de dados do Azure para MySQL | Documentos da Microsoft
description: Proteção avançada contra ameaças Deteta atividades anómalas da base de dados, indicando potenciais ameaças de segurança para a base de dados.
services: mysql
author: bolzmj
manager: kfile
ms.service: mysql
ms.topic: article
ms.date: 09/20/2018
ms.author: mbolz
ms.openlocfilehash: e123acd09e4cbfdbaf0531db72598dbd478fa1c9
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069391"
---
# <a name="azure-database-for-mysql-advanced-threat-protection"></a>Base de dados do Azure para MySQL proteção avançada contra ameaças

A Proteção Avançada Contra Ameaças para Base de Dados do Azure para MySQL deteta atividades anómalas que indicam tentativas potencialmente perigosas e invulgares para aceder ou explorar as suas bases de dados.

Proteção avançada contra ameaças faz parte da oferta de segurança de dados avançada, que é um pacote unificado para os recursos de segurança avançada. Proteção avançada contra ameaças podem ser acessada e gerenciada através da [portal do Azure](https://portal.azure.com) e está atualmente em pré-visualização.

> [!NOTE]
> A funcionalidade de proteção avançada contra ameaças está **não** disponíveis nas regiões de cloud soberana e seguir o Azure government: US Gov Texas, EUA Gov Arizona, US Gov Iowa, US, Gov Virginia, US DoD leste, US DoD Central, Alemanha Central, Alemanha Norte, leste da China, leste da China 2. Visite [produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/) para disponibilidade geral do produto.
> 

## <a name="what-is-advanced-threat-protection"></a>O que é a proteção avançada contra ameaças?

Proteção avançada contra ameaças para a base de dados do Azure para MySQL fornece uma nova camada de segurança, o que permite aos clientes detetar e responder a potenciais ameaças à medida que ocorrem ao fornecer alertas de segurança relativamente a atividades anómalas. Os utilizadores recebem um alerta após a atividades suspeitas da base de dados e potenciais vulnerabilidades, bem como os padrões de acesso e consultas de base de dados anómalas. Proteção avançada contra ameaças para a base de dados do Azure para MySQL integra alertas no [Centro de segurança do Azure](https://azure.microsoft.com/services/security-center/), que inclui detalhes de atividade suspeita e recomenda ação sobre como investigar e mitigar a ameaça. Proteção avançada contra ameaças para a base de dados do Azure para MySQL torna simples para resolução de potenciais ameaças à base de dados sem a necessidade de ser um especialista em segurança ou gerir sistemas de monitorização de segurança avançada. 

![Conceito de proteção avançada contra ameaças](media/concepts-data-access-and-security-threat-protection/advanced-threat-protection-concept.png)

## <a name="advanced-threat-protection-alerts"></a>Alertas de proteção contra ameaças avançadas 
Proteção avançada contra ameaças para a base de dados do Azure para MySQL Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de bases de dados e ele pode acionar os seguintes alertas:
- **Acesso a partir de uma localização invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor MySQL, onde alguém iniciou sessão para a base de dados do Azure para o servidor MySQL a partir de uma localização geográfica invulgar. Em alguns casos, o alerta deteta uma ação legítima (uma nova manutenção de programador ou aplicação). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso a partir do Centro de dados do Azure invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor MySQL, onde alguém iniciou sessão para o servidor de um centro de dados do Azure invulgar que foi visto neste servidor durante o período recente. Em alguns casos, o alerta Deteta uma ação legítima (sua nova aplicação no Azure, Power BI, base de dados para o Editor de consultas do MySQL). Noutros casos, o alerta deteta uma ação maliciosa de um recurso/serviço do Azure (ex-funcionário, atacante externo).
- **Acesso a partir de principal invulgar**: este alerta é acionado quando ocorre uma alteração no padrão de acesso à base de dados do Azure para o servidor MySQL, onde alguém iniciou sessão no servidor com um principal invulgar (base de dados de utilizador MySQL). Em alguns casos, o alerta deteta uma ação legítima (nova aplicação, manutenção de programador). Noutros casos, o alerta deteta uma ação maliciosa (ex-funcionário, atacante externo).
- **Acesso de uma localização potencialmente prejudicial**: este alerta é acionado quando uma aplicação potencialmente prejudicial é utilizada para aceder à base de dados. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque através de ferramentas de ataque comuns.
- **Base de dados do Azure de força bruta para credenciais do MySQL**: este alerta é acionado quando existe um número anormalmente elevado de inícios de sessão falhados com credenciais diferentes. Em alguns casos, o alerta deteta testes de penetração em ação. Noutros casos, o alerta deteta um ataque de força bruta.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
* Para obter mais informações sobre os preços, consulte o [base de dados do Azure para a página de preços do MySQL](https://azure.microsoft.com/pricing/details/mysql/) 
* Configurar [base de dados do Azure para MySQL Advanced Threat Protection](howto-database-threat-protection-portal.md) no portal do Azure  
