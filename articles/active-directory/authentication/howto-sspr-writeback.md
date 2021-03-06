---
title: Como configurar a repetição de escrita de palavra-passe para o Azure AD SSPR
description: Utilize o Azure AD e o Azure AD Connect para palavras-passe de repetição de escrita para um diretório no local
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: e8a09a9fc87bff692b5d5b4c54f87839f2591b63
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086952"
---
# <a name="how-to-configure-password-writeback"></a>Como: Configurar a repetição de escrita de palavra-passe

Recomendamos que utilize a funcionalidade de atualização automática do [do Azure AD Connect](../hybrid/how-to-connect-install-express.md) ao utilizar a repetição de escrita de palavra-passe.

Os passos seguintes partem do princípio de já ter configurado o Azure AD Connect no seu ambiente utilizando o [Express](../hybrid/how-to-connect-install-express.md) ou [personalizado](../hybrid/how-to-connect-install-custom.md) definições.

1. Para configurar e ativar a repetição de escrita de palavras-passe, inicie sessão no seu servidor do Azure AD Connect e inicie o assistente de configuração **Azure AD Connect**.
2. Na página de **Boas-vindas**, selecione **Configurar**.
3. Na página **Tarefas adicionais**, selecione **Personalizar opções de sincronização** e, em seguida, selecione **Seguinte**.
4. Na página **Ligar ao Azure AD**, introduza uma credencial de administrador global e, em seguida, selecione **Seguinte**.
5. Nas páginas de filtragem **Ligar diretórios** e **Domínio/UO**, selecione **Seguinte**.
6. Na página **Funcionalidades opcionais**, selecione a caixa junto a **Repetição de escrita de palavras-passe** e selecione **Seguinte**.
   ![Ativar a repetição de escrita de palavra-passe no Azure AD Connect][Writeback]
7. Na página **Pronto a configurar**, selecione **Configurar** e aguarde que o processo termine.
8. Quando vir a configuração a concluir, selecione **Sair**.

Para tarefas de resolução de problemas comuns relacionados com a repetição de escrita de palavra-passe, consulte a secção [resolver problemas de repetição de escrita de palavra-passe](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback) no nosso artigo de resolução de problemas.

> [!WARNING]
> Repetição de escrita de palavra-passe deixarão de funcionar para os clientes que estão a utilizar o Azure AD Connect versões 1.0.8641.0 e mais antigo quando o [o serviço de controlo de acesso do Azure (ACS) é descontinuado a 7 de Novembro de 2018](../develop/active-directory-acs-migration.md). O Azure AD Connect versões 1.0.8641.0 e mais antigos deixará de permitir repetição de escrita de palavra-passe neste momento porque dependem de ACS para obter essa funcionalidade.
>
> Para evitar uma interrupção do serviço, a atualização de uma versão anterior do Azure AD Connect para uma versão mais recente, consulte o artigo [do Azure AD Connect: atualizar de uma versão anterior para a versão mais recente](../hybrid/how-to-upgrade-previous-version.md)
>

## <a name="active-directory-permissions"></a>Permissões do Active Directory

A conta especificada no utilitário do Azure AD Connect tem os seguintes itens definiu se quiser ser no âmbito do SSPR:

* **Repor palavra-passe** 
* **Alterar palavra-passe** 
* **Permissões de escrita** no `lockoutTime`
* **Permissões de escrita** no `pwdLastSet`
* **Expandido direitos** em qualquer uma:
   * O objeto raiz de *cada domínio* nessa floresta
   * As unidades organizacionais (UOs) de utilizador que pretende no escopo para SSPR

Se não tiver a certeza sobre a conta a conta descrita refere-se, abra a IU de configuração do Azure Active Directory Connect e selecione o **ver configuração atual** opção. A conta que tem de adicionar permissão está listado em **diretórios sincronizados**.

Se definir estas permissões, a conta de serviço MA para cada floresta pode gerir as palavras-passe em nome das contas de utilizador nessa floresta. 

> [!IMPORTANT]
> Se não atribuir estas permissões, em seguida, mesmo que a repetição de escrita parece estar configurada corretamente, os utilizadores receberão erros quando tentarem gerir as respetivas palavras-passe no local na nuvem.
>

> [!NOTE]
> Pode demorar até uma hora ou mais até que estas permissões sejam replicadas para todos os objetos no seu diretório.
>

Para configurar as permissões adequadas para a repetição de escrita de palavra-passe ocorrer, conclua os seguintes passos:

1. Abra o Active Directory utilizadores e computadores com uma conta que possua as permissões de administração do domínio adequado.
2. Do **View** menu, certifique-se **funcionalidades avançadas** está ativada.
3. No painel esquerdo, clique no objeto que representa a raiz do domínio e selecione **propriedades** > **segurança** > **avançadas**.
4. Partir do **permissões** separador, selecione **Add**.
5. Escolha a conta que as permissões estão a ser aplicadas a (a partir do programa de configuração do Azure AD Connect).
6. Na **aplica-se ao** na lista pendente, selecione **objetos de utilizador de descendente**.
7. Sob **permissões**, selecione as caixas para as seguintes opções:
    * **Alterar palavra-passe**
    * **Repor palavra-passe**
8. Sob **propriedades**, selecione as caixas para as seguintes opções:
    * **Escrever lockoutTime**
    * **Escrever pwdLastSet**
9. Selecione **aplicar/OK** para aplicar as alterações e sair quaisquer caixas de diálogo de abertura.

## <a name="next-steps"></a>Passos Seguintes

[O que é a repetição de escrita de palavra-passe?](concept-sspr-writeback.md)

[Writeback]: ./media/howto-sspr-writeback/enablepasswordwriteback.png "Ativar a repetição de escrita de palavra-passe no Azure AD Connect"
