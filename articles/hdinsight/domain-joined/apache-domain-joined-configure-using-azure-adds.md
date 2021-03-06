---
title: Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure AD-DS
description: Saiba como definir e configurar um cluster do HDInsight Enterprise Security Package com o Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 10/9/2018
ms.openlocfilehash: 8b92191b60f8eb7e3c63f465926b434e205ef1b4
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684754"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Configurar um cluster do HDInsight com o Enterprise Security Package com o Azure Active Directory Domain Services

Os clusters de pacote de segurança da empresa (ESP) fornecem acesso de vários utilizadores em clusters do HDInsight do Azure. Clusters do HDInsight com o ESP estão ligados a um domínio para que os utilizadores de domínio possam utilizar as credenciais de domínio para autenticar com os clusters e executar tarefas de macrodados. 

Neste artigo, irá aprender a configurar um cluster do HDInsight com ESP, utilizando o Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>O ESP é a disponibilidade geral no HDI 3.6 para Spark, interativo e Apache Hadoop. O ESP para tipos de clusters de HBase e o Kafka está em pré-visualização.

## <a name="enable-azure-ad-ds"></a>Ativar o Azure AD DS

> [!NOTE]
> Apenas os administradores de inquilinos tem os privilégios para ativar o Azure AD-DS. Se o armazenamento de cluster é o Azure Data Lake Store (ADLS) geração 1 ou geração 2, desativar o multi-factor Authentication (MFA) apenas para utilizadores que necessitam de aceder ao cluster. Se o armazenamento de cluster é o armazenamento de Blobs do Azure (WASB), não desative a MFA.

Ativar AzureAD DS é um pré-requisito antes de poder criar um cluster do HDInsight com ESP. Para obter mais informações, consulte [ativar o Azure Active Directory Domain Services no portal do Azure](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Quando o Azure AD-DS está ativado, todos os utilizadores e objetos de iniciar a sincronização do Azure Active Directory (AAD) para o Azure AD-DS por predefinição. O comprimento da operação de sincronização depende do número de objetos no Azure AD. A sincronização pode demorar alguns dias para centenas de milhares de objetos. 

Os clientes podem optar por sincronizar apenas os grupos que precisam de acesso para os clusters do HDInsight. Esta opção de apenas determinados grupos de sincronização é chamada *âmbito de sincronização*. Ver [configurar um âmbito sincronização do Azure AD ao seu domínio gerido](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization) para obter instruções.

Quando ativar o LDAP seguro, coloque o nome de domínio no nome do requerente e o nome alternativo do requerente no certificado. Por exemplo, se o nome de domínio for *contoso100.onmicrosoft.com*, certifique-se de que nome exato existe no seu nome de requerente do certificado e o nome alternativo do requerente. Para obter mais informações, consulte [configurar LDAP seguro para um Azure AD-DS o domínio gerido](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md). Seguem-se um exemplo de criação de um certificado autoassinado e ter o nome de domínio (*contoso100.onmicrosoft.com*) no nome do requerente e DnsName (nome alternativo do requerente):

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
``` 

## <a name="check-azure-ad-ds-health-status"></a>Verificar estado de funcionamento do Azure AD-DS
Ver o estado de funcionamento do seu Azure Active Directory Domain Services, selecionando **estado de funcionamento** sob a **gerir** categoria. Certificar-se de que o estado do Azure AD-DS está verde (em execução) e a sincronização estiver concluída.

![Estado de funcionamento de serviços de domínio do Active Directory do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Criar e autorizar uma identidade gerida

R **atribuído ao utilizador a identidade gerida** é usado para simplificar e Proteja as operações de serviços de domínio. Quando atribui a função de contribuinte de serviços de domínio do HDInsight para a identidade gerida, ele pode ler, criar, modificar e eliminar operações de serviços de domínio. Determinadas operações de serviços de domínio como a criação de UOs e os princípios de serviço são necessários para o Enterprise Security Package do HDInsight. Identidades geridas podem ser criadas em qualquer subscrição. Para obter mais informações, consulte [geridos identidades para recursos do Azure](../../active-directory/managed-identities-azure-resources/overview.md).

Para configurar clusters do ESP, crie uma identidade gerida atribuído ao utilizador se ainda não tiver um. Ver [Create, lista, delete ou o atribuir uma função para uma identidade gerida atribuído ao utilizador com o portal do Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) para obter instruções. Em seguida, atribuir os **contribuinte de serviços de domínio do HDInsight** função para a identidade gerida no controle de acesso do Azure AD DS (privilégios de administrador do AAD-DS são necessários para fazer esta atribuição de função).

![Controlo de acesso de serviços de domínio do Active Directory do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Atribuir a **contribuinte de serviços de domínio do HDInsight** função garante que, esta identidade tem adequadas (em nome) acesso a executar operações de serviços de domínio, como criar UOs, a eliminação de UOs, etc. no domínio do AAD-DS.

Depois de a identidade gerida é criada e tendo em conta a função correta, o administrador do AAD-DS pode configurar que pode utilizar esta identidade gerida. Para configurar utilizadores para a identidade gerida, o administrador deve selecionar a identidade gerida no portal e clique em **controlo de acesso (IAM)** sob **descrição geral**. Em seguida, à direita, atribua a **operador de identidade gerida** função para os utilizadores ou grupos que pretendem criar clusters do HDInsight ESP. Por exemplo, o administrador do AAD-DS pode atribuir esta função para o grupo de "MarketingTeam" para a identidade de "sjmsi" gerida, conforme mostrado na imagem abaixo. Isto irá garantir que as pessoas corretas na organização têm acesso para utilizar esta identidade gerida com o objetivo de criação de clusters do ESP.

![Atribuição de função de operador de identidade de gerida do HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="networking-considerations"></a>Considerações de redes

> [!NOTE]
> Azure AD DS tem de ser implementada numa vNET do Azure Resource Manager (ARM) com base. Redes virtuais clássicas não são suportadas para o Azure AD-DS. Consulte a [ativar o Azure Active Directory Domain Services no portal do Azure](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-network) para obter mais detalhes.

Depois de ativar o Azure AD-DS, um servidor local do serviço de nomes de domínio (DNS) é executado em máquinas de virtuais AD (VMs). Configure o Azure AD DS rede Virtual (VNET) para utilizar estes servidores DNS personalizados. Para localizar os endereços IP corretos, selecione **propriedades** sob a **gerir** categoria e examinar os endereços IP listados abaixo **endereço IP na rede Virtual**.

![Localizar endereços IP para servidores DNS locais](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Alterar a configuração dos servidores DNS na VNET do Azure AD DS para utilizar estes IPs personalizado, selecionando **servidores DNS** sob a **definições** categoria. Em seguida, clique no botão de opção junto a **personalizada**, introduza o primeiro endereço IP na caixa de texto abaixo e clique em **guardar**. Adicione os endereços IP adicionais com os mesmos passos.

![A atualizar a configuração de DNS da VNET](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

É mais fácil colocar a instância do Azure AD-DS e o cluster do HDInsight na mesma rede virtual do Azure. Se planeia utilizar VNETs de diferentes, deve fazer um ponto essas redes virtuais para que o controlador de domínio seja visível para VMs do HDI. Para obter mais informações, consulte [peering de rede Virtual](../../virtual-network/virtual-network-peering-overview.md). 

Depois das VNETs em modo de peering, configure a VNET do HDInsight para utilizar um servidor DNS personalizado e de entrada de IPs privados do Azure AD-DS como os endereços de servidor DNS. Quando ambas as VNETs usa os mesmos servidores DNS, o seu nome de domínio personalizado será resolvido para o IP certo e estará acessível a partir do HDInsight. Por exemplo, se o nome de domínio for "contoso.com", em seguida, após este passo, "contoso.com" ping deve resolver para o direito de IP do Azure AD DS. 

![Configurar servidores DNS personalizados para o VNET Peering](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

**Para testar** se o seu sistema de rede está configurado corretamente, Junte-se a uma VM do windows para HDInsight VNET/subrede e enviar pings para o nome de domínio (ele deve resolver para um IP), em seguida, execute **ldp.exe** para aceder ao domínio do Azure AD-DS. Em seguida, **associar esta VM do windows ao domínio para confirmar** de todas as chamadas RPC necessárias ser bem-sucedida entre o cliente e servidor. Também pode utilizar **nslookup** para confirmar o acesso de rede à sua conta de armazenamento ou de qualquer DB externo, poderá utilizar (por exemplo, externo Hive metastore ou Ranger DB).
Deve certificar-se de que todos os [necessárias portas](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) está na lista de permissões na sub-rede de AAD-DS regras do grupo de segurança de rede, se AAD-DS está protegida por um NSG. Se a associação a domínios deste windows VM for bem-sucedida, pode avançar para o passo seguinte e criar clusters do ESP.

## <a name="create-a-hdinsight-cluster-with-esp"></a>Criar um cluster do HDInsight com ESP

Depois de configurar as etapas anteriores corretamente, a próxima etapa é criar o cluster do HDInsight com ESP ativado. Quando cria um cluster do HDInsight, pode ativar o Enterprise Security Package no **personalizado** separador. Se preferir usar um modelo Azure Resource Manager para a implementação, utilize a experiência do portal de uma vez e transfira o modelo preenchido previamente na última página "Resumo" para futura reutilização.

![Validação do domínio de pacote de segurança do Azure HDInsight Enterprise](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Depois de ativar o ESP, configurações incorretas comuns relacionados com o Azure AD-DS serão automaticamente detetadas e validadas. Depois de corrigir estes erros podem continuar com o passo seguinte: 

![Pacote de segurança do Azure HDInsight Enterprise falha na validação do domínio](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Quando cria um cluster do HDInsight com ESP, deve fornecer os seguintes parâmetros:

- **Utilizador de administrador de cluster**: Escolha um administrador para o cluster de sincronização do Azure AD-DS. Esta conta de domínio têm de já estar sincronizados e estão disponíveis no Azure AD-DS.

- **Grupos de acesso de cluster**: os grupos de segurança, cujos usuários que pretende sincronizar e ter acesso ao cluster devem estar disponíveis no Azure AD-DS. Por exemplo, HiveUsers grupo. Para obter mais informações, consulte [criar um grupo e adicionar membros no Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **URL de LDAPS**: um exemplo é ldaps://contoso.com:636.

Captura de ecrã seguinte mostra uma configuração com êxito no portal do Azure:

![Configuração do HDInsight ESP Active Directory Domain Services do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

A identidade gerida que criou no pode ser escolhida no menu pendente atribuído ao utilizador a identidade gerida quando criar um novo cluster.

![Configuração do HDInsight ESP Active Directory Domain Services do Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Passos Seguintes
* Para configurar políticas do Hive e executar consultas do Hive, consulte [configurar políticas do Hive para HDInsight clusters com ESP](apache-domain-joined-run-hive.md).
* Para utilizar o SSH para ligar a clusters do HDInsight com ESP, consulte [utilizar o SSH com Hadoop baseado em Linux no HDInsight do Linux, Unix ou OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
