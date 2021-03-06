---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 858d098f18604d1360af8509eb8a0cec6fa7d0c7
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49354590"
---
## <a name="prepare-your-web-app"></a>Preparar a sua aplicação Web

Para vincular um certificado SSL personalizado (um certificado de terceiros ou certificado do Serviço de Aplicações) à sua aplicação Web, o [plano do Serviço de Aplicações](https://azure.microsoft.com/pricing/details/app-service/) tem de estar no escalão **Básico**, **Standard**,**Premium** ou **Isolado**. Neste passo, vai confirmar que a aplicação Web está no escalão de preço suportado.

### <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Abra o [Portal do Azure](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Navegue até à sua aplicação Web

No menu à esquerda, clique em **Serviços de Aplicações** e clique no nome da sua aplicação Web.

![Selecionar a aplicação Web](./media/app-service-ssl-prepare-app/select-app.png)

Chegou à página de gestão da sua aplicação Web.  

### <a name="check-the-pricing-tier"></a>Verificar o escalão de preço

No painel de navegação esquerdo da página da aplicação Web, desloque-se para a secção **Definições** e selecione **Aumentar verticalmente (plano do Serviço de Aplicações)**.

![Menu de aumento vertical](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Confirme que a aplicação Web não está no escalão **F1** ou **D1**. O escalão atual da aplicação é realçado com uma caixa azul-escura.

![Verificar o escalão de preço](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

O SSL personalizado não é suportado nos escalões **F1** ou **D1**. Se precisar de aumentar verticalmente, siga os passos na secção seguinte. Caso contrário, feche a página **Aumentar verticalmente** e avance para [Carregar e vincular o certificado SSL](#upload).

### <a name="scale-up-your-app-service-plan"></a>Aumentar verticalmente o seu plano do Serviço de Aplicações

Selecione qualquer uma das camadas não gratuitas (**B1**, **B2**, **B3** ou qualquer camada na categoria **Produção**). Para obter opções adicionais, clique em **Ver opções adicionais**.

Clique em **Aplicar**.

![Escolher um escalão de preço](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Quando vir a notificação seguinte, significa que a operação de dimensionamento está completa.

![Notificação para “aumentar verticalmente”](./media/app-service-ssl-prepare-app/scale-notification.png)

