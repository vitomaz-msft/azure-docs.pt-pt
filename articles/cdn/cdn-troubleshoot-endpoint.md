---
title: Resolução de problemas de pontos finais da CDN do Azure que devolvem um código de 404 Estado | Microsoft Docs
description: Resolver problemas de 404 códigos de resposta com pontos finais da CDN do Azure.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: b588a1eb-ab69-4fc7-ae4d-157c3e46f4a8
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 1cffef5bbda475032ee7ff07188ab0d9d52846ea
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766109"
---
# <a name="troubleshooting-azure-cdn-endpoints-that-return-a-404-status-code"></a>Pontos finais da CDN do Azure que devolvem um código de 404 estado de resolução de problemas
Este artigo permite-lhe resolver problemas com pontos finais Azure rede de entrega conteúdos (CDN) que devolvam os códigos de estado de resposta HTTP 404.

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [do MSDN Azure e os fóruns Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode também ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.

## <a name="symptom"></a>Sintoma
Criou um perfil da CDN e um ponto final, mas o conteúdo não parece estar disponíveis na CDN. Os utilizadores que tentam aceder ao conteúdo através do URL de CDN recebem um código de estado de HTTP 404. 

## <a name="cause"></a>Causa
Existem várias causas possíveis, incluindo:

* Origem do ficheiro não é visível para a CDN.
* O ponto final está configurado incorretamente, fazendo com que a CDN parecer no local incorreto.
* O anfitrião está a rejeitar o cabeçalho de anfitrião da CDN.
* O ponto final não registou a tempo para propagar em toda a CDN.

## <a name="troubleshooting-steps"></a>Passos de resolução de problemas
> [!IMPORTANT]
> Depois de criar um ponto final de CDN, não imediatamente estará disponível para utilização, que demora algum tempo para que o registo propagar pela CDN:
> - Para **CDN do Azure Standard da Microsoft** perfis, propagação normalmente conclusão na dez minutos. 
> - Para **CDN do Azure Standard da Akamai** perfis, propagação normalmente concluída num minuto. 
> - Para **CDN do Azure Standard da Verizon** e **CDN do Azure Premium da Verizon** perfis, propagação normalmente for concluída dentro de 90 minutos. 
> 
> Se concluir os passos neste documento e ainda está a obter 404 respostas, considere a aguardar a algumas horas para verificar novamente antes de abrir um pedido de suporte.
> 
> 

### <a name="check-the-origin-file"></a>Verifique o ficheiro de origem
Em primeiro lugar, certifique-se de que o ficheiro à cache está disponível no servidor de origem e está acessível publicamente na internet. A forma mais rápida para o fazer consiste em abrir um browser numa sessão privada ou incognito e procure diretamente o ficheiro. Escreva ou cole o URL na caixa de endereço e certifique-se de que o que resulta no ficheiro de que espera. Por exemplo, suponha que tem um ficheiro de uma conta de armazenamento do Azure, acessível em https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt. Se com êxito pode carregar o conteúdo deste ficheiro, transmite o teste.

![Êxito!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [!WARNING]
> Embora a forma mais rápida e mais fácil para verificar o ficheiro é publicamente disponível, algumas configurações de rede na sua organização podem fazer com que pareça que é um ficheiro publicamente disponível faz, de facto, apenas visível para os utilizadores da rede (mesmo que está alojado no Azure). Para assegurar que isto não é o caso, teste o ficheiro com um browser externo, como um dispositivo móvel que não está ligado à rede da sua organização ou uma máquina virtual no Azure.
> 
> 

### <a name="check-the-origin-settings"></a>Verifique as definições de origem
Depois de verificar que o ficheiro é publicamente disponível na internet, verifique as definições de origem. No [Portal do Azure](https://portal.azure.com), navegue para o perfil de CDN e selecione o ponto final que está a resolução de problemas. De resultante **Endpoint** página, selecione a origem.  

![Página de ponto final com origem realçada](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

O **origem** é apresentada a página. 

![Página de origem](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Tipo de origem e nome de anfitrião
Certifique-se de que os valores de **tipo de origem** e **nome de anfitrião de origem** estão corretos. Neste exemplo, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, a parte de nome de anfitrião do URL é *cdndocdemo.blob.core.windows.net*, que está correto. Uma vez que as origens de armazenamento do Azure, a aplicação Web e o serviço em nuvem utilizam um valor na lista pendente para o **nome de anfitrião de origem** spellings campo, incorretos não são um problema. No entanto, se utilizar uma origem personalizada, certifique-se de que o seu nome do anfitrião está escrito corretamente.

#### <a name="http-and-https-ports"></a>Portas HTTP e HTTPS
Verifique o **HTTP** e **portas HTTPS**. Na maioria dos casos, 80 e 443 estão corretas e que vai necessitar sem alterações.  No entanto, se o servidor de origem está à escuta numa porta diferente, que terá de ser representado aqui. Se não tiver a certeza, ver o URL para o ficheiro de origem. As especificações de HTTP e HTTPS utilizam as portas 80 e 443 como as predefinições. No URL de exemplo, https:\//cdndocdemo.blob.core.windows.net/publicblob/lorem.txt, uma porta não for especificada, pelo que a predefinição de 443 pressupõe-se e as definições estão corretas.  

No entanto, suponha que o URL para o ficheiro de origem que testada anteriormente é http:\//www.contoso.com:8080/file.txt. Tenha em atenção o *: 8080* parte no fim do segmento do nome do anfitrião. Que número dá instruções ao browser para utilizar a porta 8080 para ligar ao servidor de web em www.contoso.com, por conseguinte, terá de introduzir *8080* no **porta HTTP** campo. É importante ter em atenção que estas definições de porta afetam apenas as portas que o ponto final utiliza para obter informações da origem.

> [!NOTE]
> **Azure CDN Standard da Akamai** pontos finais não permitem o intervalo de portas TCP completo para origens.  Para obter uma lista das portas de origem que não são permitidas, consulte [Portas de Origem Permitidas do Azure CDN da Akamai](https://msdn.microsoft.com/library/mt757337.aspx).  
> 
> 

### <a name="check-the-endpoint-settings"></a>Verifique as definições de ponto final
No **Endpoint** página, selecione o **configurar** botão.

![Página de ponto final com o botão Configurar realçado](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Ponto final de CDN **configurar** é apresentada a página.

![Configurar página](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protocolos
Para **protocolos**, certifique-se de que o protocolo a ser utilizado pelos clientes está selecionado. Uma vez que o mesmo protocolo utilizado pelo cliente é utilizada para aceder à origem, é importante que tenha as portas de origem configuradas corretamente na secção anterior. Ponto final de CDN escuta apenas em HTTP e HTTPS portas predefinidas (80 e 443), independentemente das portas de origem.

Vamos voltar para o nosso exemplo hipotético com http:\//www.contoso.com:8080/file.txt.  Como irá lembrar, Contoso especificado *8080* como as respetivas HTTP de porta, mas vamos também partem do princípio de que o especificado *44300* como a respetiva porta HTTPS.  Se um ponto final com o nome criadas *contoso*, seria o respetivo nome de anfitrião de ponto final CDN *contoso.azureedge.net*.  Um pedido de http:\//contoso.azureedge.net/file.txt é um pedido HTTP, pelo que o ponto final teria de utilizar HTTP na porta 8080 para obtê-lo da origem.  Um pedido seguro através de HTTPS, https: \/ /contoso.azureedge.net/file.txt, fará com que o ponto final para utilizar HTTPS numa porta 44300 ao obter o ficheiro da origem.

#### <a name="origin-host-header"></a>Cabeçalho de anfitrião de origem
O **cabeçalho de anfitrião de origem** é o valor de cabeçalho de anfitrião enviado para a origem com cada pedido.  Na maioria dos casos, isto deve ser o mesmo que o **nome de anfitrião de origem** foi verificado anteriormente.  Um valor incorreto neste campo, geralmente, não irá fazer com que devolvem 404 Estados, mas provavelmente, causar outros Estados 4xx, consoante o que espera que a origem.

#### <a name="origin-path"></a>Caminho de origem
Por último, deve verificar a nossa **caminho de origem**.  Por predefinição, está em branco.  Só deve utilizar este campo se pretender restringir o âmbito dos recursos alojados de origem que pretende disponibilizar na CDN.  

No ponto final exemplo, queremos todos os recursos na conta de armazenamento esteja disponível, pelo que **caminho de origem** foi deixada em branco.  Isto significa que um pedido para https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt resulta numa ligação do ponto final para cdndocdemo.core.windows.net pedidos */publicblob/lorem.txt*.  Da mesma forma, um pedido para https:\//cdndocdemo.azureedge.net/donotcache/status.png resulta no pedido de ponto final */donotcache/status.png* da origem.

Mas e se não quiser utilizar a CDN para cada caminho na sua origem?  Imaginemos que pretendia apenas para expor o *publicblob* caminho.  Se introduzir o podemos */publicblob* no **caminho de origem** campo, o que fará com que o ponto final para inserir */publicblob* antes de todos os pedidos efetuados para a origem.  Isto significa que o pedido para https:\//cdndocdemo.azureedge.net/publicblob/lorem.txt agora irá demorar, na verdade, a parte do pedido de URL, */publicblob/lorem.txt*e de acréscimo */publicblob* para o início. Isto resulta num pedido para */publicblob/publicblob/lorem.txt* da origem.  Se o caminho não resolve para um ficheiro real, a origem irá devolver um estado 404.  O URL correto para obter lorem.txt neste exemplo seria, na verdade, https:\//cdndocdemo.azureedge.net/lorem.txt.  Tenha em atenção que iremos não incluem o */publicblob* caminho de todo, porque a parte do pedido do URL é */lorem.txt* e adiciona o ponto final */publicblob*, resultando num */publicblob/lorem.txt* a ser pedido transmitido para a origem.

