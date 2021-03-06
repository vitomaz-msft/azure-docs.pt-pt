---
title: Criar uma conta do Video Indexer no portal do Azure
titlesuffix: Azure Media Services
description: Este artigo mostra como criar uma conta do Video Indexer no portal do Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: 49e05047d58cc5b6bb5e0099c24a131a26dc8af1
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292364"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Criar uma conta do Video Indexer ligada ao Azure

Quando criar uma conta do Video Indexer, pode optar por uma conta de avaliação gratuita (através da qual obtém um determinado número de minutos de indexação gratuitos) ou uma opção paga (não fica limitado pela quota). Com a avaliação gratuita, o Video Indexer fornece até 600 minutos de indexação gratuita a utilizadores de sites e até 2400 minutos de indexação gratuita a utilizadores de APIs. Com uma opção paga, pode criar uma conta do Video Indexer que está ligada à sua subscrição do Azure e uma conta de Media Services do Azure. Irá pagar pelos minutos indexados, bem como pelas cobranças relacionadas com a Conta de Multimédia. 

Este artigo mostra como criar uma conta do Video Indexer que está ligada a uma subscrição do Azure e uma conta de Media Services do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

* Uma subscrição do Azure. 

    Se não tiver uma subscrição do Azure, inscreva-se [avaliação gratuita do Azure](https://azure.microsoft.com/free/).

* Um domínio do Azure Active Directory (AD). 

    Se não tiver um domínio do Azure AD, crie este domínio com a sua subscrição do Azure. Para obter mais informações, consulte [gerir nomes de domínio personalizados no Azure Active Directory](../../active-directory/users-groups-roles/domains-manage.md)

* Um utilizador e o membro no seu domínio do Azure AD. Usará esse membro quando ligar a sua conta do Video Indexer para o Azure.

    Este utilizador deve atender a esses critérios:

    * Ser um utilizador do Azure AD com uma ou conta profissional, não uma conta pessoal, como hotmail.com, live.com ou outlook.com.
        
        ![todos os utilizadores do AAD](./media/create-account/all-aad-users.png)

    *  Ser um membro na sua subscrição do Azure com uma função de proprietário ou funções de contribuinte e administrador de acesso do utilizador. Um utilizador pode ser adicionado duas vezes, com 2 funções. Uma vez com e outra com o utilizador administrador de acesso de contribuinte.

        ![Controlo de acesso](./media/create-account/access-control-iam.png)

* Registe o fornecedor de recursos de EventGrid no portal do Azure.

    Na [portal do Azure](https://portal.azure.com/), aceda à **subscrições** > [subscrição] > **ResourceProviders** > **Microsoft.EventGrid**. Se não estiver no estado "Registado", clique em **registar**. Demora alguns minutos para se registar. 

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Ligar ao Azure

1. Aceda ao site do [Video Indexer](https://www.videoindexer.ai/) e inicie sessão.

2. Clique nas **ligar ao Azure** botão:

    ![Ligar ao Azure](./media/create-account/connect-to-azure.png)

3. Quando for apresentada a lista de subscrições, selecione a subscrição que pretende utilizar. 

    ![ligar o indexador de vídeo para o Azure](./media/create-account/connect-vi-to-azure-subscription.png)

4. Selecione uma região do Azure a partir de localizações suportadas: E.U.A. oeste 2, Europa do Norte ou Ásia Oriental.
5. Sob **conta de Media Services do Azure**, escolha uma destas opções:

    * Para criar uma nova conta de Media Services, selecione **criar novo grupo de recursos**. Forneça um nome para o grupo de recursos.

        Azure irá criar a sua nova conta na sua subscrição, incluindo uma nova conta de armazenamento do Azure. A nova conta de Media Services tem uma configuração inicial do padrão com um ponto final de transmissão em fluxo e de 10 unidades reservadas de S3.
    * Para utilizar uma conta de Media Services existente, selecione **usar o recurso existente**. Na lista de contas, selecione a sua conta.

        Conta dos Media Services tem de ter a mesma região que a sua conta do Video Indexer. Para minimizar a duração de indexação e baixo débito, ajuste o tipo e número de unidades reservadas para **10 unidades reservadas de S3** na sua conta de Media Services.
    * Para configurar manualmente a ligação, clique nas **mudar para a configuração manual**. 
    
        Pode querer configurar manualmente a ligação, se por algum motivo conseguir concluir a opção automática, ou se a sua instalação e configuração é diferente dos casos comuns ou, se pretender ter visibilidade e controlo sobre as definições. 
        
        Na **ligar uma subscrição do Azure do Video Indexer**, forneça as seguintes informações.

        |Definição|Descrição|
        |---|---|
        |Região de conta de indexador vídeo|O nome da região de conta do Video Indexer. Para um melhor desempenho e reduzir os custos, recomenda-se elevada para especificar o nome da região onde se encontram os recursos de serviços de multimédia do Azure e a conta de armazenamento do Azure. |
        |Inquilino do Azure Active Directory (AAD)|O nome do inquilino do Azure AD, por exemplo "contoso.onmicrosoft.com". As informações de inquilino podem ser obtidas a partir do portal do Azure. Coloque o cursor sobre o nome de utilizador com sessão iniciada no canto superior direito.|
        |ID da subscrição|A subscrição do Azure sob a qual esta ligação deverá ser criada. O ID de subscrição pode ser obtido a partir do portal do Azure. Clique em **todos os serviços** no painel esquerdo e procure "subscrições". Selecione, **subscrições** e escolha o ID de pretendida na lista das suas subscrições.|
        |Nome do grupo de recursos de serviços de multimédia do Azure|O nome do grupo de recursos em que a conta de Media Services existe.|
        |Nome de recurso do serviço de suporte de dados|O nome do recurso dos serviços de multimédia do Azure.|
        |ID da aplicação|O ID de aplicação do Azure AD com permissões para a conta de Media Services especificada. Para obter mais informações, consulte [autenticação do principal de serviço utilização](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|
        |Chave da Aplicação|Para obter mais informações, consulte [autenticação do principal de serviço utilização](../../media-services/previous/media-services-portal-get-started-with-aad.md#service-principal-authentication).|

6. Quando tiver terminado, escolha **Connect**. Esta operação poderá demorar alguns minutos. 

    Depois que estiver ligado ao Azure, a sua nova conta do Video Indexer aparece na lista conta:

    ![nova conta](./media/create-account/new-account.png)

7. Navegue para a sua nova conta: 

    ![Conta de indexador de vídeo](./media/create-account/vi-account.png)

## <a name="considerations"></a>Considerações

As seguintes considerações relacionadas de serviços de multimédia do Azure aplicam-se:

* Se estiver ligado a uma nova conta de Media Services, verá um novo grupo de recursos, conta de Media Services e uma conta de armazenamento na sua subscrição do Azure.
* Se estiver ligado a uma nova conta de Media Services, o Video Indexer definirá o suporte de dados **unidades reservadas** para 10 unidades de S3:

    ![Unidades reservadas de serviços de multimédia](./media/create-account/ams-reserved-units.png)

* Se estiver ligado a uma conta de Media Services existente, o indexador de vídeo não altera o suporte de dados existente **unidades reservadas** configuração.

    Poderá ter de ajustar o tipo e número de suportes de dados **unidades reservadas**, de acordo com a carga em planeadas. Tenha em atenção que, se a carga for elevada e não tiver suficiente unidades ou velocidade, o processamento de vídeos podem resultar em falhas de tempo limite.

* Se estiver ligado a uma nova conta de Media Services, o indexador de vídeo inicia automaticamente a predefinição **ponto final de transmissão em fluxo** no mesmo:

    ![Ponto final de transmissão em fluxo dos serviços de multimédia](./media/create-account/ams-streaming-endpoint.png)

* Se ligado a uma conta de Media Services existente, o indexador de vídeo não altera a configuração de ponto final de transmissão em fluxo predefinido. Se não houver nenhuma execução **ponto final de transmissão em fluxo**, não será capaz de ver vídeos desta conta dos serviços de multimédia ou em Video Indexer.

## <a name="next-steps"></a>Passos Seguintes

Pode interagir programaticamente com a sua conta de avaliação e/ou com as suas contas do indexador de vídeo que estiver ligadas ao azure ao seguir as instruções em: [utilize as APIs](video-indexer-use-apis.md).

Deve usar o mesmo utilizador do Azure AD que utilizou quando ligar ao Azure.


