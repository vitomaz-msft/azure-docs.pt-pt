---
title: Resolver problemas de domínio e problemas de certificado SSL em aplicações web do Azure | Documentos da Microsoft
description: Resolver problemas de domínio e problemas de certificado SSL em aplicações web do Azure
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: e10c6580c29784f8d56a31e267c47ec08894113b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418369"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Resolver problemas de domínio e problemas de certificado SSL em aplicações web do Azure

Este artigo lista problemas comuns que poderá encontrar ao configurar um domínio ou o certificado SSL para as suas aplicações web do Azure. Ele também descreve possíveis causas e soluções para esses problemas.

Se precisar de mais ajuda a qualquer momento neste artigo, pode contactar os especialistas do Azure no [os fóruns do MSDN e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.

## <a name="certificate-problems"></a>Problemas de certificado

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>Não é possível adicionar um enlace de certificado SSL para uma aplicação web 

#### <a name="symptom"></a>Sintoma

Quando adiciona um enlace SSL, recebe a seguinte mensagem de erro:

"Falha ao adicionar o enlace SSL. Não é possível definir certificado para o VIP existente porque outro VIP já utiliza esse certificado."

#### <a name="cause"></a>Causa

Este problema pode ocorrer se tiver vários enlaces de SSL baseado em IP para o mesmo endereço IP em várias aplicações web. Por exemplo, a aplicação web A tem um SSL baseado em IP com um certificado antigo. Aplicação Web B tem um SSL baseado em IP com um novo certificado para o mesmo endereço IP. Quando atualizar o enlace de SSL de aplicação web com o novo certificado, ele falha com este erro porque o mesmo endereço IP que está a ser utilizado para outra aplicação. 

#### <a name="solution"></a>Solução 

Para corrigir este problema, utilize um dos seguintes métodos:

- Elimine o enlace SSL baseado em IP na aplicação web que utiliza o certificado antigo. 
- Crie um novo enlace SSL baseado em IP que utiliza o novo certificado.

### <a name="you-cant-delete-a-certificate"></a>Não é possível eliminar um certificado 

#### <a name="symptom"></a>Sintoma

Ao tentar eliminar um certificado, receberá a seguinte mensagem de erro:

"Não é possível eliminar o certificado porque está atualmente a ser utilizado num enlace SSL. O enlace SSL tem de ser removido antes de poder eliminar o certificado."

#### <a name="cause"></a>Causa

Esse problema pode ocorrer se outra aplicação web utiliza o certificado.

#### <a name="solution"></a>Solução

Remova o enlace de SSL para esse certificado a partir das aplicações web. Em seguida, tente eliminar o certificado. Se ainda não é possível eliminar o certificado, limpar a cache do navegador de internet e volte a abrir o portal do Azure numa nova janela do browser. Em seguida, tente eliminar o certificado.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Não é possível comprar um certificado de serviço de aplicações 

#### <a name="symptom"></a>Sintoma
Não pode comprar uma [certificado do serviço de aplicações do Azure](./web-sites-purchase-ssl-web-site.md) do portal do Azure.

#### <a name="cause-and-solution"></a>Causa e solução
Este problema pode ocorrer em qualquer um dos seguintes motivos:

- O plano do serviço de aplicações é gratuito ou partilhado. Estes escalões de preço não suportam SSL. 

    **Solução**: atualizar o plano do serviço de aplicações para a aplicação web para o Standard.

- A subscrição não tem um cartão de crédito válido.

    **Solução**: adicionar um cartão de crédito válido para a sua subscrição. 

- A oferta de subscrição não suporta a adquirir um certificado de serviço de aplicações, tais como Microsoft Student.  

    **Solução**: atualizar a sua subscrição. 

- A subscrição atingiu o limite de compras que são permitidas numa subscrição.

    **Solução**: certificados de serviço de aplicações têm um limite de 10 compras de certificado para os tipos de subscrição pay as you go e EA. Para outros tipos de subscrição, o limite é 3. Para aumentar o limite, contacte [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- O certificado de serviço de aplicações foi marcado como fraudes. Recebeu a seguinte mensagem de erro: "o certificado foi sinalizado como possível fraude. O pedido está atualmente a ser analisadas. Se o certificado se tornar utilizável dentro de 24 horas, contacte o suporte do Azure."

    **Solução**: se o certificado está marcado como fraude e não é resolvido após 24 horas, siga estes passos:

    1. Inicie sessão no [portal do Azure](https://portal.azure.com).
    2. Aceda a **certificados do serviço de aplicações**e selecione o certificado.
    3. Selecione **configuração do certificado** > **passo 2: verificar** > **verificação de domínio**. Este passo envia uma notificação de e-mail para o fornecedor de certificados do Azure para resolver o problema.

## <a name="domain-problems"></a>Problemas de domínio

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Tiver adquirido um certificado SSL para o domínio errado

#### <a name="symptom"></a>Sintoma

Adquirido um certificado de serviço de aplicações para o domínio errado. Não é possível atualizar o certificado a utilizar o domínio correto.

#### <a name="solution"></a>Solução

Eliminar esse certificado e, em seguida, comprar um novo certificado.

Se o certificado atual que utiliza o domínio errado está no estado "Emitido", também pagará para esse certificado. Certificados de serviço de aplicações não são reembolsável, mas pode contactar [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para ver se existem outras opções. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Um certificado de serviço de aplicações foi renovado, mas a aplicação web mostra o antigo certificado 

#### <a name="symptom"></a>Sintoma

O certificado de serviço de aplicações foi renovado, mas a aplicação web que utiliza o certificado de serviço de aplicações ainda está a utilizar o certificado antigo. Além disso, é recebido um aviso de que o protocolo HTTPS é necessário.

#### <a name="cause"></a>Causa 
A funcionalidade de aplicações Web do serviço de aplicações do Azure é executada uma tarefa em segundo plano a cada oito horas e sincroniza o recurso de certificado, se existirem quaisquer alterações. Quando roda ou atualizar um certificado, às vezes, o aplicativo está ainda a obter o certificado antigo e não o certificado recentemente atualizado. O motivo é que a tarefa para sincronizar o recurso de certificado não tiver sido executada ainda. 
 
#### <a name="solution"></a>Solução

Pode forçar uma sincronização do certificado:

1. Inicie sessão no [portal do Azure](https://portal.azure.com). Selecione **certificados do serviço de aplicações**e, em seguida, selecione o certificado.
2. Selecione **recodificar e sincronizar**e, em seguida, selecione **sincronização**. A sincronização demora algum tempo a concluir. 
3. Quando a sincronização estiver concluída, verá a seguinte notificação: "Atualizado com êxito todos os recursos com o último certificado."

### <a name="domain-verification-is-not-working"></a>Verificação de domínio não está a funcionar 

#### <a name="symptom"></a>Sintoma 
O certificado de serviço de aplicações requer verificação de domínio antes do certificado está pronto a utilizar. Quando seleciona **Verifique se**, o processo de falha.

#### <a name="solution"></a>Solução
Verifique manualmente o seu domínio ao adicionar um registo TXT:
 
1.  Vá para o fornecedor de serviço de nomes de domínio (DNS) que aloja o seu nome de domínio.
2.  Adicione um registo TXT para o seu domínio que utiliza o valor do token de domínio que é apresentado no portal do Azure. 

Aguarde alguns minutos para a propagação de DNS executar e, em seguida, selecione o **atualizar** botão para acionar a verificação. 

Como alternativa, pode utilizar o método de página Web HTML para verificar manualmente o seu domínio. Este método permite que a autoridade de certificação confirmar a propriedade do domínio do domínio que o certificado é emitido.

1.  Crie um ficheiro HTML com o nome {token de verificação de domínio}.-HTML. O conteúdo deste ficheiro deve ser o valor do token de verificação de domínio.
3.  Carregar este ficheiro na raiz do servidor web que está a alojar o seu domínio.
4.  Selecione **atualizar** para verificar o estado do certificado. Poderá demorar alguns minutos para efeitos de verificação concluir.

Por exemplo, se estiver comprando um certificado standard para azure.com com o 1234abcd de token de verificação de domínio, feita uma solicitação da web para http://azure.com/1234abcd.html deverá devolver 1234abcd. 

> [!IMPORTANT]
> Um pedido de certificado tem prazo de 15 dias para concluir a operação de verificação de domínio. Após 15 dias, a autoridade de certificação nega o certificado e não lhe são cobradas para o certificado. Nesta situação, elimine este certificado e tente novamente.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Não é possível comprar um domínio

#### <a name="symptom"></a>Sintoma
Não é possível comprar um domínio do domínio das aplicações Web ou serviço de aplicações no portal do Azure.

#### <a name="cause-and-solution"></a>Causa e solução

Este problema ocorre para um dos seguintes motivos:

- Não existe nenhum cartão de crédito da subscrição do Azure ou o cartão de crédito é inválido.

    **Solução**: adicionar um cartão de crédito válido para a sua subscrição.

- Não é o proprietário da subscrição, pelo que não tem permissão para comprar um domínio.

    **Solução**: [atribuir a função de proprietário](../role-based-access-control/role-assignments-portal.md) à sua conta. Ou contacte o administrador da subscrição para obter permissão para comprar um domínio.
- Atingiu o limite para a compra de domínios na sua subscrição. O limite atual é 20.

    **Solução**: para pedir um aumento do limite, entre em contato com [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- O tipo de subscrição do Azure não suporta a compra de um domínio do serviço de aplicações.

    **Solução**: atualizar a sua subscrição do Azure para outro tipo de subscrição, como uma subscrição pay as you go.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>Não é possível adicionar um nome de anfitrião para uma aplicação web 

#### <a name="symptom"></a>Sintoma

Quando adiciona um nome de anfitrião, o processo de falha validar e verificar o domínio.

#### <a name="cause"></a>Causa 

Este problema ocorre para um dos seguintes motivos:

- Não tem permissão para adicionar um nome de anfitrião.

    **Solução**: peça ao administrador de subscrição para lhe dar permissão para adicionar um nome de anfitrião.
- Não foi possível verificar a propriedade do domínio.

    **Solução**: Certifique-se de que seu CNAME ou um registo está configurado corretamente. Para mapear um domínio personalizado para a aplicação web, crie um registo CNAME ou um registo. Se pretender utilizar um domínio de raiz, tem de utilizar registos a e TXT:

    |Tipo de registo|Anfitrião|Aponte para|
    |------|------|-----|
    |A|@|Endereço IP para uma aplicação web|
    |TXT|@|< nome da aplicação >. azurewebsites|
    |CNAME|www|< nome da aplicação >. azurewebsites|

### <a name="dns-cant-be-resolved"></a>Não é possível resolver o DNS

#### <a name="symptom"></a>Sintoma

Recebeu a mensagem de erro seguinte:

"O registo DNS não foi possível localizar."

#### <a name="cause"></a>Causa
Este problema ocorre para um dos seguintes motivos:

- O tempo para o período do live (TTL) não expirou. Verifique a configuração de DNS para o seu domínio determinar o valor de TTL e, em seguida, aguarde que o período de expirar.
- A configuração de DNS está incorreta.

#### <a name="solution"></a>Solução
- Aguarde até 48 horas para que este problema seja resolvido automaticamente.
- Se for possível alterar a definição do valor de TTL na sua configuração de DNS, altere o valor para 5 minutos para ver se isso resolve o problema.
- Uso [WhatsmyDNS.net](https://www.whatsmydns.net/) para verificar que o seu domínio aponta para o endereço IP da aplicação web. Se não configure o registo para o endereço IP correto da aplicação web.

### <a name="you-need-to-restore-a-deleted-domain"></a>Tem de restaurar um domínio eliminado 

#### <a name="symptom"></a>Sintoma
O domínio já não é visível no portal do Azure.

#### <a name="cause"></a>Causa 
O proprietário da subscrição pode ter sido excluído acidentalmente o domínio.

#### <a name="solution"></a>Solução
Se o seu domínio foi eliminado há menos de sete dias, o domínio ainda não iniciou o processo de eliminação. Neste caso, pode adquirir o mesmo domínio novamente no portal do Azure na mesma subscrição. (Certifique-se de que escreva o nome de domínio exata na caixa de pesquisa.) Não será cobrado novamente para este domínio. Se o domínio foi eliminado há mais de sete dias, contacte [suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter ajuda para restaurar o domínio.

### <a name="a-custom-domain-returns-a-404-error"></a>Um domínio personalizado devolve um erro 404 

#### <a name="symptom"></a>Sintoma

Quando navegar para o site com o nome de domínio personalizado, recebe a seguinte mensagem de erro:

"Erro 404-não foram encontradas aplicações Web."


#### <a name="cause-and-solution"></a>Causa e solução

**Causa 1** 

O domínio personalizado que configurou está em falta um registo CNAME ou um. 

**Solução para causa 1**

- Se tiver adicionado um registo, certifique-se de que também é adicionado um registo TXT. Para obter mais informações, consulte [criar um registo](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Se não tiver de utilizar o domínio de raiz para a sua aplicação web, recomendamos que utilize um registo CNAME, em vez de um registo.
- Não utilize um registo CNAME e um registo para o mesmo domínio. Isso pode causar um conflito e impedir que o domínio a ser resolvidos. 

**Causa 2** 

Browser da internet pode ainda ser colocação em cache o endereço IP antigo para o seu domínio. 

**Solução para causa 2**

Limpe o navegador. Para dispositivos Windows, pode executar o comando `ipconfig /flushdns`. Uso [WhatsmyDNS.net](https://www.whatsmydns.net/) para verificar que o seu domínio aponta para o endereço IP da aplicação web. 

### <a name="you-cant-add-a-subdomain"></a>Não é possível adicionar um subdomínio 

#### <a name="symptom"></a>Sintoma

Não é possível adicionar um novo nome de anfitrião para uma aplicação web para atribuir um subdomínio.

#### <a name="solution"></a>Solução

- Contacte o administrador da subscrição para se certificar de que tem permissões para adicionar um nome de anfitrião para a aplicação web.
- Se precisar de mais de subdomínios, recomendamos que altere o alojamento do domínio ao DNS do Azure. Ao utilizar o DNS do Azure, pode adicionar nomes de anfitrião 500 à sua aplicação web. Para obter mais informações, consulte [adicione um subdomínio](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















