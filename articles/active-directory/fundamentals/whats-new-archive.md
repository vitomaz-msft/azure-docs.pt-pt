---
title: Arquivada para o que há de novo? no Azure Active Directory | Documentos da Microsoft
description: A que há de novo notas de versão na descrição geral secção deste conjunto de conteúdos contém seis meses da atividade. Depois de 6 meses, os itens são removidos do artigo principal e colocar neste artigo de arquivo.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 11/14/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: 22e0f95b1afec39574309a8deed8a020145c38ec
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708811"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>Arquivada para o que há de novo? no Azure Active Directory

O principal [o que há de novo notas de versão](whats-new.md) artigo contém de 6 meses mais recente de informações, embora este artigo inclui todas as informações mais antigas.

A que há de novo notas de versão lhe fornece informações sobre:

- Versões mais recentes
- Problemas conhecidos
- Correções de erros
- Funcionalidades preteridas
- Planos para que as alterações

---
 
## <a name="april-2018"></a>Abril de 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>O Azure AD B2C Token de acesso estão GA

**Tipo:** novo recurso  
**Categoria de serviço:** B2C - gestão de identidades de consumidor  
**Capacidade de produto:** B2B/B2C 

Agora pode aceder a APIs da Web protegida pelo Azure AD B2C com tokens de acesso. A funcionalidade está a mudar de pré-visualização pública para GA. Foi melhorada a experiência de interface do Usuário para configurar aplicações do Azure AD B2C e web APIs e outras pequenas melhorias foram feitas.
 
Para obter mais informações, consulte [do Azure AD B2C: solicitar tokens de acesso](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Testar a configuração de início de sessão única para aplicações baseadas em SAML

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO

Quando configurar as aplicações baseadas em SAML SSO, pode testar a integração na página de configuração. Se ocorrer um erro durante o início de sessão, pode fornecer o erro na experiência de teste e do Azure AD fornece-lhe os passos de resolução para resolver o problema específico.

Para obter mais informações, consulte:

- [Configurar o início de sessão único em aplicações que não fazem parte da galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Como depurar baseado em SAML início de sessão único para aplicações no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Agora tem o Azure AD termos de utilização por utilizador de relatórios

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 
Os administradores podem agora selecionar um determinado termos de utilização e ver todos os utilizadores tenham consentimento ao que termos de utilização e o que data/hora ocorreu.

Para obter mais informações, consulte a [termos do Azure AD de utilização da funcionalidade](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>O Azure AD Connect Health: IP em risco para a proteção de bloqueio de extranet do AD FS 

**Tipo:** novo recurso  
**Categoria de serviço:** outros  
**Capacidade de produto:** monitoramento e relatório

Agora suporta a capacidade de detetar IP, endereços que excedem um limiar de inícios de sessão falhados U/P numa base por hora ou diário do Connect Health. Os recursos fornecidos por esta funcionalidade são:

- Relatório abrangente, que mostra o endereço IP e o número de inícios de sessão falhados gerados numa base por hora/dia com o limiar personalizável.
- Alertas baseados no e-mail que mostra quando um endereço IP específico tiver excedido o limiar de inícios de sessão falhados U/P numa base por hora/dia.
- Uma opção de download para fazer uma análise detalhada dos dados

Para obter mais informações, consulte [relatório de IP arriscado](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Configuração da aplicação fácil com o ficheiro de metadados ou URL

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO

Na página de aplicações empresariais, os administradores podem carregar um ficheiro de metadados SAML para configurar SAML com base em início de sessão para a aplicação da galeria do AAD e não à galeria.

Além disso, pode utilizar o URL de metadados de Federação de aplicação do Azure AD para configurar o SSO com a aplicação de destino.

Para obter mais informações, consulte [configurar o início de sessão único para aplicações que não estão na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>O Azure AD termos de utilização agora em disponibilidade geral

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 

AD termos de utilização do Azure foram movidas de pré-visualização pública para disponibilidade geral.

Para obter mais informações, consulte a [termos do Azure AD de utilização da funcionalidade](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Permitir ou bloquear convites para utilizadores B2B de organizações específicos

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C
 

Agora pode especificar quais as organizações parceiras que pretende partilhar e colaborar com em colaboração B2B do Azure AD. Para tal, pode optar por criar a lista de específicos de permitir ou negar a domínios. Quando um domínio for bloqueado com estas capacidades, os funcionários não podem mais enviar convites para as pessoas nesse domínio.

Isto ajuda a controlar o acesso aos seus recursos, ao ativar uma experiência positiva para os utilizadores aprovados.

Esta funcionalidade de colaboração B2B está disponível para todos os clientes do Azure Active Directory e pode ser utilizada em conjunto com os recursos do Azure AD Premium, como a proteção de identidades e acesso condicional para um controle mais granular de quando e como os utilizadores de negócios externos iniciam sessão no e obter acesso.

Para obter mais informações, consulte [permitir ou bloquear convites aos utilizadores B2B de organizações específicos](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros

Em Abril de 2018, adicionámos que 13 novos aplicativos com a Federação de suporte para nossa Galeria de aplicações:

O critério HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [segredo do servidor (no local)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [sinal dinâmica](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Agora](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [Monitor de desempenho de AppNeta](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx laboratórios](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [ Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), prateleiras, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Os utilizadores de concessão B2B no Azure AD acedem às suas aplicações no local (pré-visualização pública)

**Tipo:** novo recurso  
**Categoria de serviço:** B2B  
**Capacidade de produto:** B2B/B2C

Como uma organização que utiliza recursos de colaboração B2B do Azure Active Directory (Azure AD) para convidar utilizadores de organizações parceiras com o Azure AD, pode agora fornecer estes utilizadores B2B acesso a aplicações no local. Estas aplicações no local podem utilizar a autenticação baseada no SAML ou autenticação integrada do Windows (IWA) com a delegação restringida de Kerberos (KCD).

Para obter mais informações, consulte [B2B de concessão de utilizadores no Azure AD acedem às suas aplicações no local](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Obtenha tutoriais de integração do SSO do Azure Marketplace

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** outros  
**Capacidade de produto:** 3º integração de terceiros

Se um aplicativo que está listado na [do Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) suporta SAML com base em início de sessão único, ao clicar em **obter agora** fornece-lhe o tutorial de integração associado essa aplicação. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Desempenho mais rápido de aprovisionamento de utilizadores automático do Azure AD para aplicações SaaS

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3º integração de terceiros
 
Anteriormente, os clientes que utilizam os conectores para aplicações SaaS (por exemplo Salesforce, ServiceNow e caixa) de provisionamento de usuários do Azure Active Directory foi possível detetar um desempenho lento se seus inquilinos do Azure AD contidos mais de 100.000 usuários combinados e grupos e eles usavam atribuições de utilizador e grupo para determinar quais os utilizadores que devem ser aprovisionados.

No dia 2 de Abril de 2018, melhorias de desempenho significativos foram implementadas para o serviço de aprovisionamento do Azure AD que reduzem significativamente a quantidade de tempo necessário para efetuar sincronizações iniciais entre o Azure Active Directory e aplicações de SaaS de destino.

Como resultado, muitos clientes que tinha sincronizações iniciais para aplicações que demorou o número de dias ou nunca foi concluída, agora são concluir em questão de minutos ou horas.

Para obter mais informações, consulte [o que acontece durante o aprovisionamento?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Senhas de auto-atendimento, repor a partir do ecrã de bloqueio do Windows 10 para o Azure AD híbrido associado máquinas

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** reposição personalizada de palavra-passe  
**Capacidade de produto:** autenticação de utilizador
 
Atualizámos a funcionalidade SSPR do Windows 10 para incluir suporte para máquinas que estão associados ao Azure AD híbrido. Esta funcionalidade está disponível no Windows 10 RS4 permite que os utilizadores reponham as respetivas palavras-passe de ecrã de bloqueio de uma máquina com o Windows 10. Os utilizadores que forem ativados e registados na reposição de palavra-passe self-service podem utilizar esta funcionalidade.

Para obter mais informações, consulte [palavra-passe do Azure AD a partir do ecrã de início de sessão de reposição](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows).

---

## <a name="march-2018"></a>Março de 2018
 
### <a name="certificate-expire-notification"></a>Notificação de expiração do certificado

**Tipo:** fixo  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO
 
O Azure AD envia uma notificação quando um certificado de uma galeria ou externas à Galeria aplicações está prestes a expirar. 

Alguns utilizadores não tiverem recebido notificações para aplicações empresariais, configuradas para baseado em SAML início de sessão único. Este problema foi resolvido. O Azure AD envia a notificação para certificados irá expirar em 7, 30 e 60 dias. É possível ver este evento nos registos de auditoria. 

Para obter mais informações, consulte:

- [Gerir certificados para início de sessão único federado no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Relatórios de atividade de auditoria no portal do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Fornecedores de identidade de twitter e do GitHub no Azure AD B2C

**Tipo:** novo recurso  
**Categoria de serviço:** B2C - gestão de identidades de consumidor  
**Capacidade de produto:** B2B/B2C
 
Agora pode adicionar o Twitter ou o GitHub como fornecedor de identidade no Azure AD B2C. Twitter é mudar de pré-visualização pública para GA. GitHub está a ser lançado em pré-visualização pública.

Para obter mais informações, consulte [o que é a colaboração B2B do Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Restringir o acesso ao browser com o Intune Managed Browser para iOS e Android com acesso condicional com base na aplicação do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção
 
**Agora em pré-visualização pública!**

**Intune Managed Browser SSO:** seus funcionários podem utilizar o início de sessão único em clientes nativos (como o Microsoft Outlook) e o Intune Managed Browser para todas as aplicações do Azure AD-ligado.

**Intune Managed Browser condicional suporte de acesso:** agora pode exigir que os funcionários para usarem o Intune Managed browser através de políticas de acesso condicional baseado na aplicação.

Saiba mais sobre isso em nossa [mensagem de blogue](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Para obter mais informações, consulte:

- [Configurar o acesso condicional com base na aplicação](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Configurar políticas de browser gerido](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Cmdlets do Proxy de aplicações no módulo de disponibilidade geral do Powershell

**Tipo:** novo recurso  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** controlo de acesso
 
Suporte para os cmdlets do Proxy da aplicação está agora em disponibilidade geral do módulo do Powershell! É necessário para se manter atualizado em Powershell módulos - se tornam-se mais do que um ano atrás, alguns cmdlets podem deixar de funcionar. 

Para obter mais informações, consulte [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Clientes nativos do Office 365 são suportados pelo SSO totalmente integrado com um protocolo não interativa

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Utilizador com clientes nativos do Office 365 (versão 16.0.8730.xxxx e acima) obter uma silenciosa experiência de logon usando o SSO totalmente integrado. Este suporte é fornecido pela adição um protocolo de não-interativa (WS-Trust) para o Azure AD.

Para obter mais informações, consulte [como início de sessão num cliente nativo com o trabalho de SSO totalmente integrado?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Os utilizadores obtêm uma silenciosa experiência de logon, com o SSO totalmente integrado, se uma aplicação envia pedidos de início de sessão para pontos finais de inquilino do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Os utilizadores obtêm uma silenciosa experiência de logon, com o SSO totalmente integrado, se um aplicativo (por exemplo, `https://contoso.sharepoint.com`) envia pedidos de início de sessão para pontos finais de inquilino do Azure AD - ou seja, `https://login.microsoftonline.com/contoso.com/<..>` ou `https://login.microsoftonline.com/<tenant_ID>/<..>` - em vez de ponto de extremidade comum do Azure AD (`https://login.microsoftonline.com/common/<...>`).

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Tem de adicionar apenas um URL do Azure AD, em vez de dois URLs como anteriormente, para as definições de zona de Intranet dos utilizadores para implementar o SSO totalmente integrado

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Para distribuir o SSO totalmente integrado aos seus utilizadores, tem de adicionar definições de zona apenas um URL de AD do Azure para Intranet dos utilizadores ao utilizar a política de grupo no Active Directory: `https://autologon.microsoftazuread-sso.com`. Anteriormente, os clientes eram necessário adicionar dois URLs.

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada Single Sign-On](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros

Março de 2018, adicionámos que 15 novos aplicativos com a Federação de suporte para nossa Galeria de aplicações:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), assistente pelo FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Servidor empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial), [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM para recursos do Azure está disponível em geral

**Tipo:** novo recurso  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Se estiver a utilizar o Azure AD Privileged Identity Management para funções de diretório, agora, pode utilizar o acesso de limite de tempo e recursos de atribuição do PIM para funções de recursos do Azure, como as subscrições, grupos de recursos, máquinas virtuais e qualquer outro recurso suportado pelo Azure Resource Manager. Impor o multi-factor Authentication durante a ativação de funções Just-In-Time e agendar ativações em coordenação com o windows de alteração aprovados. Além disso, esta versão adiciona melhorias não está disponíveis durante a pré-visualização pública, incluindo uma interface do Usuário atualizada, fluxos de trabalho de aprovação e a capacidade de estender funções irá expirar em breve e renovar funções expiradas.

Para obter mais informações, consulte [PIM para recursos do Azure (pré-visualização)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Adicionar afirmações opcionais aos tokens das aplicações (pré-visualização pública)

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
A aplicação do Azure AD pode agora afirmações de pedidos personalizados ou opcionais em JWTs ou SAML tokens.  Estes são declarações sobre o utilizador ou o inquilino que não estão incluídas por predefinição no token, devido a restrições de tamanho ou a aplicabilidade.  Está atualmente em pré-visualização pública para aplicações do Azure AD nos pontos finais v1.0 e v2.0.  Consulte a documentação para obter informações sobre quais as afirmações podem ser adicionados e como editar o manifesto da aplicação para solicitá-los.  

Para obter mais informações, consulte [opcional de afirmações no Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>O Azure AD suporta PKCE para os fluxos do OAuth mais seguros

**Tipo:** novo recurso  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Documentos de AD do Azure foram atualizados observar o suporte para PKCE, que permite a comunicação mais segura durante o fluxo de concessão do código de autorização do OAuth 2.0.  Os pontos finais v1.0 e v2.0 são suportadas code_challenges S256 e texto sem formatação. 

Para obter mais informações, consulte [pedir um código de autorização](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Suporte para o aprovisionamento de todos os valores de atributo de utilizador disponíveis na API de Get_Workers Workday

**Tipo:** novo recurso  
**Categoria de serviço:** aprovisionamento de aplicações  
**Capacidade de produto:** 3º integração de terceiros
 
A pré-visualização pública de entrada aprovisionamento a partir do Workday para o Active Directory e o Azure AD agora suporta a capacidade de extrair e o aprovisionamento de todos os valores de atributos disponíveis na API de Get_Workers do Workday. Esta ação adiciona suporta para centenas de standard adicional e atributos personalizados além dos fornecidos com a versão inicial do dia de trabalho de entrada conector de aprovisionamento.

Para obter mais informações, consulte: [personalizar a lista de atributos de utilizador do Workday](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Alterar a associação ao grupo de dinâmico para estático e vice versa

**Tipo:** novo recurso  
**Categoria de serviço:** grupo de gestão  
**Capacidade de produto:** colaboração
 
É possível alterar a forma como a associação é gerida num grupo. Isto é útil quando pretender manter o mesmo nome do grupo e o ID no sistema, para que todas as referências existentes para o grupo ainda são válidas; criar um novo grupo exigiria a atualizar essas referências.
Atualizámos o Centro de administração do Azure AD para suportar esta funcionalidade. Agora, os clientes podem converter grupos existentes de associação de grupo dinâmica atribuída de associação e vice-versa. Os cmdlets do PowerShell existentes também ainda estão disponíveis.

Para obter mais informações, consulte [a alteração de associação dinâmica para estático e vice-versa](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Comportamento de fim de sessão melhorado com o SSO totalmente integrado

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador
 
Anteriormente, mesmo que os utilizadores com sessão iniciada explicitamente fora de uma aplicação protegida pelo Azure AD, eles seriam iniciar sessão automaticamente na através do SSO totalmente integrado se estiver a tentar aceder uma aplicação do Azure AD novamente dentro de sua rede empresarial a partir dos seus dispositivos associados a um domínio. Com esta alteração, a fim de sessão é suportada.  Isto permite aos utilizadores escolher o Azure idêntica ou diferente conta de AD para iniciar sessão novamente com, em vez de automaticamente a ser iniciado sessão com o SSO totalmente integrado.

Para obter mais informações, consulte [do Azure Active Directory totalmente integrada início de sessão único](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Versão do conector de Proxy de aplicação 1.5.402.0 lançado

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** identidade de segurança e proteção
 
Esta versão do conector será gradualmente implementada por meio de Novembro. Esta nova versão do conector inclui as seguintes alterações:

- O conector agora define cookies de nível de domínio em vez disso, o nível de subdomínio. Isso garante uma experiência SSO mais suave e evita a pedidos de autenticação redundante.
- Suporte para solicitações de codificação em partes
- Monitorização de estado de funcionamento do conector melhorado 
- Várias correções de erros e melhorias de estabilidade

Para obter mais informações, consulte [conectores de Proxy de aplicações do AD Azure compreender](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Fevereiro de 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Navegação melhorada para gerir utilizadores e grupos

**Tipo:** plano de alteração  
**Categoria de serviço:** gerenciamento de diretório  
**Capacidade de produto:** diretório

A experiência de navegação para gerir utilizadores e grupos foi otimizada. Pode agora navegar a partir da descrição geral do diretamente à lista de todos os utilizadores, com acesso mais fácil para a lista de utilizadores eliminados. Também pode navegar da descrição geral do diretório diretamente para a lista de todos os grupos, com acesso mais fácil para as definições de gestão de grupo. E também da página de descrição geral de diretório, pode pesquisar por um utilizador, grupo, aplicação empresarial ou registo da aplicação. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Relatórios de disponibilidade de inícios de sessão e auditoria no Microsoft Azure operado pela 21Vianet (Azure China 21Vianet)

**Tipo:** novo recurso  
**Categoria de serviço:** Azure Stack  
**Capacidade de produto:** monitoramento e relatório

Relatórios de registo de atividade do AD do Azure estão agora disponíveis no Microsoft Azure operado pela 21Vianet (Azure China 21Vianet) instâncias. Os seguintes registos estão incluídos:

- **Registos de atividades de inícios de sessão** -inclui todos os inícios de sessão registos associados com o seu inquilino.

- **Registos de auditoria de palavra-passe de autoatendimento** -inclui todos os registos de auditoria SSPR.

- **Registos de auditoria de gerenciamento de diretório** -inclui todos os registos de auditoria relacionado ao gerenciamento de diretório como utilizador gestão, gestão de aplicações e outros.

Com estes registos, pode obter informações sobre como estado do seu ambiente. Os dados fornecidos permite-lhe:

- Determine como as aplicações e serviços são utilizados pelos seus utilizadores.

- Resolva problemas de impedir os utilizadores de realizar seu trabalho.

Para obter mais informações sobre como utilizar estes relatórios, consulte [do Azure Active Directory reporting](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Utilize a função de "Leitor de relatório" (função de não administrador) para ver relatórios de atividade do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** monitoramento e relatório

À medida que faz parte de comentários de clientes para ativar as funções de não-administrador, para ter acesso a atividade do Azure AD, ativámos a capacidade para utilizadores que estão na função "Leitor de relatório" para acesso inícios de sessão e a atividade de auditoria no portal do Azure, bem como com nossa Graph APIs. 

Para obter mais informações, como utilizar estes relatórios, consulte [do Azure Active Directory reporting](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Ação employeeid disponível como atributo de utilizador e identificador de utilizador

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** SSO
 
Pode configurar **EmployeeID** como o identificador de utilizador e o atributo de utilizador para utilizadores membros e convidados B2B no SAML aplicativos baseados no início de sessão da aplicação empresarial da interface do Usuário.

Para obter mais informações, consulte [personalizar afirmações emitidas no token SAML para aplicações empresariais no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Gestão simplificada de aplicação com carateres universais no Proxy de aplicações do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** autenticação de utilizador
 
Para facilitar a implementação de aplicação e reduzir o overhead administrativo, suportamos agora a capacidade de publicar aplicações utilizando carateres universais. Para publicar uma aplicação de caráter universal, pode seguir o fluxo de publicação do aplicativo padrão, mas utilizar um caráter universal nos URLs internos e externos.

Para obter mais informações, consulte [aplicações de caráter universal no proxy de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Novos cmdlets para suportar a configuração do Proxy de aplicações

**Tipo:** novo recurso  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** plataforma

A versão mais recente do módulo de pré-visualização do AzureAD PowerShell contém novos cmdlets que permitem aos clientes configurar aplicações de Proxy de aplicação com o PowerShell.

Os novos cmdlets são: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Novos cmdlets para suportar a configuração de grupos

**Tipo:** novo recurso  
**Categoria de serviço:** do Proxy de aplicações  
**Capacidade de produto:** plataforma

A versão mais recente do módulo do AzureAD PowerShell contém cmdlets para gerir grupos no Azure AD. Estes cmdlets estavam disponíveis anteriormente no módulo do AzureADPreview e estão agora adicionados ao módulo do AzureAD

Os cmdlets de grupo que estão a lançamento agora para disponibilidade geral são: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Está disponível uma versão nova do Azure AD Connect

**Tipo:** novo recurso  
**Categoria de serviço:** sincronização do AD  
**Capacidade de produto:** plataforma
 
Azure AD Connect é a ferramenta preferencial para sincronizar dados entre o Azure AD e em origens de dados locais, incluindo o Windows Server Active Directory e LDAP.

>[!Important]
>Apresenta o desta versão de esquema e sincronização as alterações da regra. O serviço do Azure AD Connect sincronização aciona uma importação completa e a sincronização completa etapas após uma atualização. Para obter informações sobre como alterar este comportamento, consulte [como diferir a sincronização completa após a atualização](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Esta versão tem as seguintes atualizações e alterações:

**Problemas de fixos**

- Corrigi a janela de tempo em tarefas em segundo plano para a página de filtragem de partições quando mudar para a página seguinte.

- Foi corrigido um erro que provocou a violação de acesso durante a ação personalizada ConfigDB.

- Foi corrigido um erro ao recuperar a partir de tempo limite da ligação de sql.

- Foi corrigido um erro em que os certificados com carateres universais de SAN falharem a verificação dos pré-requisitos.

- Foi corrigido um erro que faz com que miiserver.exe falha durante a exportação de conector do AAD.

- Foi corrigido um erro em que uma tentativa de palavra-passe incorreta com sessão iniciada num controlador de domínio quando em execução causado o AAD connect Assistente para alterar a configuração

**Novos recursos e aperfeiçoamentos**
 
- Telemetria do Application - os administradores podem alternar essa classe de dados liga/desliga.

- Dados de estado de funcionamento do AD do Azure - os administradores tem de visitar o portal de estado de funcionamento para controlar as definições de estado de funcionamento. Assim que a política do serviço tiver sido alterada, os agentes serão ler e impor-lo.

- Adicionar ações de configuração de repetição de escrita do dispositivo e uma barra de progresso para inicialização da página.

- Melhorada diagnósticos gerais com o relatório em HTML e recolha de dados completo num texto ZIP / relatório em HTML.

- Fiabilidade melhorada auto Atualize e adicionado telemetria adicional para garantir que o estado de funcionamento do servidor pode ser determinado.

- Restringir as permissões disponíveis para contas com privilégios na conta de conector AD. Para instalações novas, o assistente restringe as permissões que contas privilegiadas existentes na conta MSOL depois de criar a conta MSOL. As alterações afetam instalações express e das instalações personalizadas com a criação de conta.

- Alterar o instalador para não exigir privilégio de SA na instalação de raiz do AADConnect.

- Novo utilitário para resolver problemas de sincronização para um objeto específico. Atualmente, o utilitário verifica a existência dos seguintes pontos:

    - Erro de correspondência de UserPrincipalName entre a conta de utilizador no inquilino do Azure AD e o objeto de utilizador sincronizado.
  
    - Se o objeto é filtrado em sincronização devido a filtragem de domínio
  
    - Se o objeto é filtrado em sincronização devido a unidade organizacional (UO), filtragem

- Novo utilitário para sincronizar o hash de palavra-passe atual armazenado no diretório do Active Directory no local para uma conta de utilizador específico. O utilitário não requer uma alteração de palavra-passe. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Aplicações, políticas de proteção de aplicações do Intune suporte adicionadas para utilizam com o acesso condicional com base na aplicação do Azure AD

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** identidade de segurança e proteção

Adicionámos mais aplicações que suportam o acesso condicional com base na aplicação. Agora, pode obter acesso ao Office 365 e outras aplicações de cloud do Azure AD-ligado a utilizar estas aplicações aprovadas do cliente.

As seguintes aplicações serão adicionadas ao final de Fevereiro:

- Microsoft Power BI

- Microsoft Launcher

- Faturação da Microsoft

Para obter mais informações, consulte:

- [Requisito de aplicação aprovada do cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [O Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Termos de utilização atualizados para a experiência móvel 

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade

Quando os termos de utilização são apresentados, pode agora clicar **com problemas em visualizar? Clique aqui**. Ao clicar nesta ligação abre os termos de utilização de forma nativa no dispositivo. Independentemente do tamanho do tipo de letra no documento ou o tamanho de ecrã do dispositivo, pode ampliar e ler o documento conforme necessário. 

---
 
## <a name="january-2018"></a>Janeiro de 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Novas aplicações federadas disponíveis na Galeria de aplicações do Azure AD 

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros

Em Janeiro de 2018, as seguintes novas aplicações com suporte para federação foram adicionadas na Galeria de aplicações:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Software de gestão de privacidade](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk federado, e [fidelidade NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Inicie sessão com risco adicional detetado

**Tipo:** novo recurso  
**Categoria de serviço:** Identity Protection  
**Capacidade de produto:** identidade de segurança e proteção

A informação que obtém um evento de risco detetados está associada à sua subscrição do Azure AD. Com a edição do Azure AD Premium P2, obtenha as informações mais detalhadas sobre todas as detecções subjacentes.

Com a edição do Azure AD Premium P1, deteções de Trojans não são cobertas pela sua licença aparecem como o evento de risco de início de sessão com risco adicional detetado.

Para obter mais informações, consulte [Eventos de risco do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ocultar aplicações do Office 365 a partir de painéis de acesso do utilizador final

**Tipo:** novo recurso  
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** SSO

Pode agora gerenciar melhor como aplicações do Office 365 aparecem nos painéis de acesso dos seus utilizadores através de uma nova definição de utilizador. Esta opção é útil para reduzir o número de aplicações nos painéis de acesso de um utilizador, se preferir Mostrar apenas as aplicações do Office no portal do Office. A definição está localizada no **definições de utilizador** e é etiquetado, **os utilizadores apenas podem ver aplicações do Office 365 no portal do Office 365**.

Para obter mais informações, consulte [ocultar uma aplicação da experiência do utilizador no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Início de sessão totalmente integrado em aplicações ativadas para SSO de palavra-passe diretamente a partir do URL da aplicação 

**Tipo:** novo recurso  
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** SSO

A extensão do browser as minhas aplicações agora está disponível por meio de uma ferramenta conveniente que lhe dá o minhas aplicações início de sessão único na capacidade como um atalho no seu browser. Depois de instalar, usuário verá um ícone de bolachas no browser que proporciona-lhes acesso rápido às aplicações. Os utilizadores podem agora tirar partido do:

- A capacidade de iniciar sessão diretamente no SSO de palavra-passe com base em aplicações a partir da página de início de sessão da aplicação
- Iniciar qualquer aplicação a utilizar a funcionalidade de pesquisa rápida
- Atalhos para aplicações recentemente utilizadas partir da extensão
- A extensão está disponível para o Edge, Chrome e Firefox.
 
Para obter mais informações, consulte [segura de aplicações minha extensão de início de sessão](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Administração do Azure AD foi extinto experiência no Portal clássico do Azure

**Tipo:** preterido   
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** diretório

A partir de 8 de Janeiro de 2018, a administração do Azure AD foi extinto experiência no portal clássico do Azure. Isso ocorria em conjunto com a desativação do portal clássico do Azure em si. No futuro, deve utilizar o [Centro de administração do Azure AD](https://aad.portal.azure.com) todos os seus administração baseada no portal do Azure AD.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>O portal web do PhoneFactor foi extinto

**Tipo:** preterido  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** diretório
 
A partir de 8 de Janeiro de 2018, o portal web do PhoneFactor foi preterido. Este portal foi utilizado para a administração de servidor MFA, mas essas funções foram movidas para o portal do Azure em portal.azure.com. 

A configuração de MFA está localizada em: **do Azure Active Directory \> servidor MFA**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Preterir o relatórios do Azure AD

**Tipo:** preterido  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade  


Com a disponibilidade geral do novo console de administração do Active Directory do Azure e novas APIs agora disponíveis para relatórios de atividade e segurança, o relatório APIs em ponto final de "/ relatórios" foi descontinuado a partir do final do dia 31 de Dezembro de 2017.

**O que está disponível?**

Como parte da transição para a nova consola de administração, que fizemos 2 novas APIs disponíveis para recuperar os registos de atividade do Azure AD. O novo conjunto de APIs fornece mais avançado de filtragem e ordenação funcionalidade além de fornecermos o auditoria mais rica e atividades de início de sessão. Os dados anteriormente disponíveis através de relatórios de segurança agora podem ser acedidos através de eventos de risco do Identity Protection API no Microsoft Graph.

Para obter mais informações, consulte:

- [Introdução ao Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Introdução ao Azure Active Directory Identity Protection e Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Dezembro de 2017

### <a name="terms-of-use-in-the-access-panel"></a>Termos de utilização no painel de acesso

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 
Agora pode aceder ao painel de acesso e ver os termos de utilização que aceitou anteriormente.

Siga estes passos.

1. Vá para o [MyApps portal](https://myapps.microsoft.com)e iniciar sessão.

2. No canto superior direito, selecione o nome e, em seguida, selecione **perfil** da lista. 

3. No seu **perfil**, selecione **Rever termos de utilização**. 

4. Agora, pode rever os termos de utilização aceitaram. 

Para obter mais informações, consulte a [termos do Azure AD da funcionalidade de utilização (pré-visualização)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Nova experiência de início de sessão do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** autenticação de utilizador
 
O Azure AD e o sistema de identidade de conta do Microsoft interfaces do usuário foram reprojetadas para que tenham uma aspeto e funcionalidade consistentes. Além disso, a página de início de sessão do Azure AD recolhe o nome de utilizador em primeiro lugar, seguido da credencial numa segunda tela.

Para obter mais informações, consulte [a nova experiência de início de sessão do Azure AD está agora em pré-visualização pública](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Menos avisos de início de sessão: uma experiência de nova "manter sessão iniciada" do Azure AD para início de sessão

**Tipo:** novo recurso  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** autenticação de utilizador
 
O **manter sessão iniciada** caixa de verificação na página de início de sessão do Azure AD foi substituída por uma nova linha de comandos que aparece depois de autenticar com êxito. 

Se responder **Sim** para esta linha de comandos, o serviço dá-lhe um token de atualização persistente. Este comportamento é o mesmo que quando tiver selecionado o **manter sessão iniciada** caixa de verificação da experiência antiga. Para inquilinos federados, esta linha de comandos mostra depois de autenticar com êxito com o serviço federado.

Para obter mais informações, consulte [menos avisos de início de sessão: A nova experiência de "manter sessão iniciada" para o Azure AD está em pré-visualização](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Adicionar a configuração para exigir que os termos de utilização para ser expandido antes de os aceitarem

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade
 
Uma opção para administradores requer que os utilizadores expandam os termos de utilização antes de aceitar os termos.

Selecione **nos** ou **desativar** para exigir que os utilizadores expandam os termos de utilização. O **no** definição requer que os utilizadores vejam os termos de utilização antes de os aceitarem.

Para obter mais informações, consulte a [termos do Azure AD da funcionalidade de utilização (pré-visualização)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Ativação de âmbito para atribuições de função elegível

**Tipo:** novo recurso  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Pode usar o âmbito de ativação para ativar as atribuições de funções de recursos do Azure elegíveis com menos autonomia dos padrões de atribuição original. Um exemplo é se estiver atribuído como o proprietário de uma subscrição no seu inquilino. Com a ativação de âmbito, pode ativar a função de proprietário para até cinco recursos contidos dentro da subscrição (por exemplo, grupos de recursos e máquinas virtuais). A ativação de âmbito, poderá reduzir a possibilidade de alterações indesejadas aos recursos do Azure críticas em execução.

Para obter mais informações, consulte [o que é o Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Novas aplicações federadas na Galeria de aplicações do Azure AD

**Tipo:** novo recurso  
**Categoria de serviço:** aplicações empresariais  
**Capacidade de produto:** 3º integração de terceiros

Em Dezembro de 2017, adicionámos que suportam a essas novas aplicações com a Federação para nossa Galeria de aplicações:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), o Adobe Experience Manager, [loja Digital de EFI](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [imagem FUNCIONA](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [integração MobileIron do Azure AD](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO para Bamboo pela resolução GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO para Bitbucket pela resolução GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, integração do AD Zenegy Azure.

Para obter mais informações sobre as aplicações, consulte [integração de aplicações SaaS com o Azure Active Directory](https://aka.ms/appstutorial).

Para obter mais informações sobre a listagem a sua aplicação na Galeria de aplicações do Azure AD, consulte [inclua a sua aplicação na Galeria de aplicações do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Fluxos de trabalho de aprovação para funções de diretório do Azure AD

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Fluxo de trabalho de aprovação para funções de diretório do Azure AD está disponível em geral.

Com o fluxo de trabalho de aprovação, os administradores de função com privilégios podem exigir membros da função de elegíveis para pedir a ativação de função antes de poderem utilizar a função com privilégios. Vários utilizadores e grupos podem ser delegada aprovação responsabilidades. Os membros da função elegível recebem notificações quando a aprovação foi concluída e a respetiva função está ativa.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Autenticação pass-through: o Skype para suporte de negócio

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** autenticações (inícios de sessão)  
**Capacidade de produto:** autenticação de utilizador

Agora a autenticação pass-through suporta início de sessão de utilizador-ins ao Skype para aplicações de cliente de negócio que suportam a autenticação moderna, que inclui online e topologias híbridas. 

Para obter mais informações, consulte [Skype para topologias de negócio com autenticação moderna](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Atualizações ao Azure AD Privileged Identity Management do RBAC do Azure (pré-visualização)

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management
 
Com a atualização da pré-visualização pública do Azure AD Privileged Identity Management (PIM) para o controlo de acesso de controlo (RBAC), pode agora:

* Utilize a administração Just Enough.
* Exigir aprovação para ativar as funções de recursos.
* Agende uma ativação futura de uma função que necessite de aprovação para ambas do Azure AD e funções RBAC do Azure.
 
Para obter mais informações, consulte [Privileged Identity Management para recursos do Azure (pré-visualização)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Novembro de 2017
 
### <a name="access-control-service-retirement"></a>Extinção do serviço de controlo de acesso

**Tipo:** plano de alteração  
**Categoria de serviço:** serviço de controlo de acesso  
**Capacidade de produto:** serviço de controlo de acesso 

O Azure Active Directory controlo de acesso (também conhecido como o serviço de controlo de acesso) vão ser descontinuado no final de 2018. Obter mais informações, que inclui um detalhados e orientações para a migração de elevado nível irão ser fornecidas nas próximas semanas. Pode deixar comentários nesta página, com quaisquer perguntas sobre o serviço de controlo de acesso e um membro da Equipe será respondê-las.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Restringir o acesso ao browser para o Intune Managed Browser 

**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade

Pode restringir o acesso ao browser ao Office 365 e outras aplicações de cloud do Azure AD-ligado ao utilizar o Intune Managed Browser como uma aplicação aprovada. 

Agora pode configurar para o acesso condicional com base na aplicação, a seguinte condição:

**Aplicações de cliente:** Browser

**O que é o efeito da alteração?**

Hoje em dia, o acesso é bloqueado ao utilizar esta condição. Quando a pré-visualização está disponível, todos os acessos exige a utilização da aplicação de browser gerido. 

Procure esta capacidade e mais informações nos próximas blogs e notas de versão. 

Para obter mais informações, consulte [acesso condicional no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovadas para acesso condicional baseado em aplicações do Azure AD

**Tipo:** plano de alteração  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade

As seguintes aplicações estão na lista de [aplicações de cliente aprovadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Para obter mais informações, consulte:

- [Requisito de aplicação aprovada do cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [O Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Suporte de termos de utilização de vários idiomas

**Tipo:** novo recurso    
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade

Agora, os administradores podem criar novos termos de utilização que contêm vários documentos PDF. Pode Etiquetar estes documentos PDF com um idioma correspondente. Os utilizadores são apresentados o PDF com a linguagem correspondente com base nas respetivas preferências. Se não houver nenhuma correspondência, é mostrado o idioma predefinido.

---
 
### <a name="real-time-password-writeback-client-status"></a>Estado do cliente de repetição de escrita de palavra-passe em tempo real

**Tipo:** novo recurso  
**Categoria de serviço:** de reposição de palavra-passe self-service  
**Capacidade de produto:** autenticação de utilizador

Agora pode rever o estado do seu cliente de repetição de escrita de palavra-passe no local. Esta opção está disponível na **integração no local** secção a [reposição de palavra-passe](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) página. 

Se existirem problemas com a ligação para o cliente de repetição de escrita no local, verá uma mensagem de erro que fornece-lhe:

- Informações sobre por que não é possível ligar ao seu cliente de repetição de escrita no local.
- Uma ligação para a documentação que ajuda a resolver o problema. 

Para obter mais informações, consulte [integração no local](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>O Azure AD com base na aplicação acesso condicional 
 
**Tipo:** novo recurso  
**Categoria de serviço:** do Azure AD  
**Capacidade de produto:** proteção e segurança de identidade

Agora pode restringir o acesso ao Office 365 e outras aplicações de cloud do Azure AD-ligado ao [aplicações de cliente aprovadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) que suportam políticas de proteção de aplicações do Intune ao utilizar [acesso condicional com base na aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Políticas de proteção de aplicações do Intune são utilizadas para configurar e proteger os dados da empresa nesses aplicativos de cliente.

Ao combinar [com base na aplicação](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) com [com base no dispositivo](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications) políticas de acesso condicional, tem a flexibilidade para proteger dados pessoais e dispositivos da empresa.

As seguintes condições e os controles estão agora disponíveis para utilização com o acesso condicional com base na aplicação:

**Condição de plataforma suportada**

- iOS
- Android

**Condição de aplicações de cliente**

- Aplicações móveis e clientes de ambiente de trabalho

**Controlo de acesso**

- Requer aplicação aprovada do cliente

Para obter mais informações, consulte [acesso condicional com base na aplicação do Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Gerir dispositivos do Azure AD no portal do Azure

**Tipo:** novo recurso  
**Categoria de serviço:** gestão e de registo de dispositivos  
**Capacidade de produto:** proteção e segurança de identidade

Agora pode encontrar todos os seus dispositivos ligados ao Azure AD e as atividades relacionadas com o dispositivo num único local. Há uma nova experiência de administração para gerir todas as suas identidades do dispositivo e definições no portal do Azure. Nesta versão, pode:

- Ver todos os seus dispositivos que estão disponíveis para acesso condicional no Azure AD.
- Ver as propriedades, que incluem o seu híbrido do Azure dispositivos associados ao AD.
- Localizar as chaves de BitLocker para os seus dispositivos associados ao AD do Azure, gerir o seu dispositivo com Intune e muito mais.
- Gerir definições relacionadas com o dispositivo do Azure AD.

Para obter mais informações, consulte [gerir dispositivos através do portal do Azure](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Suporte para macOS como uma plataforma de dispositivo de acesso condicional do Azure AD 

**Tipo:** novo recurso    
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade 

Agora pode incluir (ou excluir) macOS como uma condição de plataforma de dispositivo na sua política de acesso condicional do Azure AD. Com a adição do macOS para as plataformas de dispositivos suportados, pode:

- **Inscrever e gerir dispositivos macOS com o Intune.** Assim como outras plataformas como iOS e Android, uma aplicação do portal da empresa está disponível para macOS fazer inscrições unificadas. Pode utilizar a nova aplicação do portal da empresa para macOS para inscrever um dispositivo com o Intune e registá-lo com o Azure AD.
- **Certifique-se de que os dispositivos macOS aderem às políticas de conformidade da sua organização definidas no Intune.** No Intune no portal do Azure, agora pode configurar políticas de conformidade para dispositivos macOS. 
- **Restringir o acesso a aplicações no Azure AD apenas a dispositivos macOS em conformidade.** Criação de política de acesso condicional tem macOS como uma opção de plataforma de dispositivo separado. Agora pode criar políticas de acesso condicional do macOS específicas para a aplicação de destino definido no Azure.

Para obter mais informações, consulte:

- [Criar uma política de conformidade para dispositivos macOS com o Intune](https://aka.ms/macoscompliancepolicy)
- [Acesso condicional no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Extensão de servidor de políticas de rede para o Azure multi-factor Authentication 

**Tipo:** novo recurso    
**Categoria de serviço:** multi-factor authentication  
**Capacidade de produto:** autenticação de utilizador

A extensão de servidor de políticas de rede para o Azure multi-factor Authentication adiciona capacidades de multi-factor Authentication com base na cloud à sua infraestrutura de autenticação ao utilizar seus servidores existentes. Com a extensão de servidor de políticas de rede, pode adicionar a chamada telefónica, mensagem de texto ou verificação de aplicação do telefone para o fluxo de autenticação existente. Não é preciso instalar, configurar e manter novos servidores. 

Esta extensão foi criada para as organizações que pretendem proteger as ligações de rede privada virtual sem ter de implementar o servidor do Azure multi-factor Authentication. O servidor de políticas de rede extensão age como um adaptador entre RADIUS e com base na cloud do Azure multi-factor Authentication para fornecer um segundo fator de autenticação para federada ou utilizadores sincronizados.

Para obter mais informações, consulte [integrar a sua infraestrutura existente do servidor de políticas de rede com o Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Restaure ou remover utilizadores eliminados permanentemente

**Tipo:** novo recurso    
**Categoria de serviço:** gestão de utilizadores  
**Capacidade de produto:** diretório 

No Centro de administração do Azure AD, pode agora:

- Restaure um utilizador eliminado. 
- Elimine permanentemente um utilizador.

**Para experimentá-lo:**

1. No Centro de administração do Azure AD, selecione [todos os utilizadores](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) no **gerir** secção. 

2. Partir do **mostrar** lista, selecione **utilizadores recentemente eliminados**. 

3. Selecione um ou mais utilizadores recentemente eliminados e, em seguida, restaurá-los ou eliminá-los permanentemente.
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Novas aplicações de cliente aprovadas para acesso condicional baseado em aplicações do Azure AD
 
**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade

As seguintes aplicações foram adicionadas à lista de [aplicações de cliente aprovadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 

Para obter mais informações, consulte:

- [Requisito de aplicação aprovada do cliente](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [O Azure AD com base na aplicação acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Utilizar "OR" entre os controles de uma política de acesso condicional 

**Tipo:** funcionalidade foi alterado    
**Categoria de serviço:** acesso condicional  
**Capacidade de produto:** proteção e segurança de identidade
 
Agora pode utilizar "ou" (exigir um dos controlos selecionados) para controles de acesso condicional. Pode utilizar esta funcionalidade para criar políticas com "ou" entre os controles de acesso. Por exemplo, pode utilizar esta funcionalidade para criar uma política que requer que um utilizador para iniciar sessão com a multi-factor Authentication "ou" para estar num dispositivo em conformidade.

Para obter mais informações, consulte [controlos de acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).
 
---

### <a name="aggregation-of-real-time-risk-events"></a>Agregação de eventos de risco em tempo real

**Tipo:** funcionalidade foi alterado    
**Categoria de serviço:** Identity protection  
**Capacidade de produto:** proteção e segurança de identidade

No Azure AD Identity Protection, todos os eventos de risco em tempo real provenientes do mesmo endereço IP num determinado dia agora são agregados para cada tipo de evento de risco. Esta alteração limita o volume de eventos de risco mostrado sem qualquer alteração na segurança do utilizador.

A deteção em tempo real subjacente funciona sempre que o utilizador inicia sessão. Se tiver uma política de segurança de risco de início de sessão para o multi-factor Authentication ou bloquear o acesso, ainda é acionado durante cada risco início de sessão.
 
---
 
## <a name="october-2017"></a>Outubro de 2017

### <a name="deprecate-azure-ad-reports"></a>Preterir o relatórios do Azure AD

**Tipo:** plano de alteração  
**Categoria de serviço:** relatórios  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade  

O portal do Azure fornece-lhe:

- Uma nova consola de administração do Azure AD.
- Novas APIs para relatórios de atividade e segurança.
 
Devido a estas novas capacidades, o relatório APIs sob o ponto de extremidade /reports foram extinguidas a 10 de Dezembro de 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Deteção de campo de início de sessão automático

**Tipo:** fixo   
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** início de sessão único  

O Azure AD suporta a deteção de campo de início de sessão automático para aplicações que compõem um campo de nome e palavra-passe de utilizador do HTML. Estes passos estão documentados em [How to capture automaticamente os campos de início de sessão para uma aplicação como](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Pode encontrar esta capacidade adicionando um *não galeria* aplicação no **aplicações empresariais** página no [portal do Azure](http://aad.portal.azure.com). Além disso, pode configurar o **início de sessão único** modo nesta aplicação nova para **baseado em palavra-passe de início de sessão único**, introduza um URL de web e, em seguida, guarde a página.
 
Devido a um problema de serviço, essa funcionalidade foi temporariamente desativada. O problema foi resolvido, e a deteção de campo de início de sessão automático estiver novamente disponível.

---

### <a name="new-multi-factor-authentication-features"></a>Novos recursos de multi-factor Authentication

**Tipo:** novo recurso  
**Categoria de serviço:** multi-factor authentication  
**Capacidade de produto:** proteção e segurança de identidade  

Autenticação multifator (MFA) é uma parte essencial de proteger a sua organização. Para tornar as credenciais mais adaptáveis e a experiência mais integrada, foram adicionadas as seguintes funcionalidades: 

- Resultados do desafio de multi-factor são diretamente integrados ao Azure AD início de sessão no relatório, que inclui o acesso programático aos resultados MFA.
- A configuração de MFA está mais profundamente integrada para a configuração do Azure AD experiência no portal do Azure.

Com esta pré-visualização pública, gestão de MFA e relatórios são uma parte integrada da experiência de configuração do núcleo do Azure AD. Agora pode gerir a funcionalidade de portal de gestão MFA na experiência do Azure AD.

Para obter mais informações, consulte [referência para MFA relatórios no portal do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 

---

### <a name="terms-of-use"></a>Termos de utilização

**Tipo:** novo recurso  
**Categoria de serviço:** termos de utilização  
**Capacidade de produto:** conformidade  

Pode utilizar os termos do Azure AD de utilização para apresentar informações tais como avisos de isenção relevantes para os requisitos legais ou de conformidade para os utilizadores.

Pode utilizar os termos do Azure AD de utilização nos seguintes cenários:

- Termos de utilização para todos os utilizadores na sua organização gerais
- Termos de utilização com base em atributos de um utilizador (por exemplo, médicos versus enfermeiras) ou doméstica versus a colaboradores internacionais, feito por grupos dinâmicos específicos
- Termos de utilização específicos para aceder a aplicações de elevado impacto comercial, como o Salesforce

Para obter mais informações, consulte [termos do Azure AD de utilização](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="enhancements-to-privileged-identity-management"></a>Aprimoramentos do Privileged Identity Management

**Tipo:** novo recurso  
**Categoria de serviço:** Privileged Identity Management  
**Capacidade de produto:** Privileged Identity Management  

Com o Azure AD Privileged Identity Management, pode gerir, controlar e monitorizar o acesso aos recursos do Azure (pré-visualização) na sua organização para:

- Subscrições
- Grupos de recursos
- Máquinas virtuais 

Todos os recursos no portal do Azure que utilizam a funcionalidade de RBAC do Azure podem tirar partido de toda a segurança e capacidades de gestão do ciclo de vida que o Azure AD Privileged Identity Management tem a oferecer.

Para obter mais informações, consulte [Privileged Identity Management para recursos do Azure](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---

### <a name="access-reviews"></a>Revisões de acesso

**Tipo:** novo recurso  
**Categoria de serviço:** as revisões de acesso  
**Capacidade de produto:** conformidade  

As organizações podem utilizar as revisões de acesso (pré-visualização) para gerir de forma eficiente as associações de grupo e o acesso a aplicações empresariais: 

- Pode voltar a certificar o acesso do utilizador convidado ao utilizar as revisões do respetivo acesso às aplicações e as associações a grupos. Os revisores podem decidir eficazmente se a permitir que os convidados acesso com base nas informações fornecidas pelas revisões de acesso contínuo.
- Pode voltar a certificar o acesso dos colaboradores às aplicações e as associações a grupos com revisões de acesso.

Pode recolher os controlos de revisão de acesso relativos a programas relevantes para a sua organização de modo a controlar as revisões de conformidade ou de aplicações sensíveis a riscos.

Para obter mais informações, consulte [revisões de acesso do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Ocultar aplicações de terceiros partir as minhas aplicações e iniciador de aplicações do Office 365

**Tipo:** novo recurso  
**Categoria de serviço:** as minhas aplicações  
**Capacidade de produto:** início de sessão único  

Agora pode melhor gerir as aplicações que aparecem nos portais dos seus utilizadores através de uma nova **ocultar aplicação** propriedade. Pode ocultar aplicações para ajudar a nos casos em que os mosaicos de aplicação apresentada para serviços de back-end ou mosaicos duplicados e iniciadores de aplicações dos utilizadores de confusão. O botão de alternar está no **propriedades** secção da aplicação de terceiros e tem o nome **visível ao utilizador?** Também pode ocultar uma aplicação através de programação através do PowerShell. 

Para obter mais informações, consulte [ocultar uma aplicação de terceiros da experiência do utilizador no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**O que está disponível?**

 Como parte da transição para a nova consola de administração, duas novas APIs para recuperação das atividades do Azure AD, os registos estão disponíveis. O novo conjunto de APIs fornece mais avançado de filtragem e ordenação funcionalidade além de fornecermos o auditoria mais rica e atividades de início de sessão. Os dados anteriormente disponíveis através de relatórios de segurança, agora podem ser acessados por meio da API de eventos de risco do Identity Protection no Microsoft Graph.


## <a name="september-2017"></a>Setembro de 2017

### <a name="hotfix-for-identity-manager"></a>Correção do Identity Manager

**Tipo:** funcionalidade foi alterado  
**Categoria de serviço:** Identity Manager  
**Capacidade de produto:** gerenciamento de ciclo de vida de identidade  

Um pacote de rollup de correção (compilação 4.4.1642.0) está disponível a partir de 25 de Setembro de 2017 do Identity Manager 2016 Service Pack 1. Este pacote de rollup:

- Resolve problemas e adiciona melhorias.
- É uma atualização cumulativa que substitui todas as atualizações do Identity Manager 2016 Service Pack 1 até a compilação 4.4.1459.0 do Identity Manager 2016. 
- Requer que tenha de criar 4.4.1302.0 do Identity Manager 2016. 

Para obter mais informações, consulte [pacote cumulativo de correções (compilação 4.4.1642.0) está disponível para Identity Manager 2016 Service Pack 1](https://support.microsoft.com/help/4021562). 

---