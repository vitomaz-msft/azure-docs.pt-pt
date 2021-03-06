---
title: Microsoft Dynamics CRM e o Azure Application Insights | Documentos da Microsoft
description: Obter telemetria de utilizar o Application Insights do Microsoft Dynamics CRM Online. Passo a passo da configuração, a obtenção de dados, visualização e exportação.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.reviewer: mazhar
ms.author: mbullwin
ms.openlocfilehash: 5d61c3a3232645fc5f1c18696cf3232bf9b37aa2
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957751"
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Instruções: Ativar a telemetria para utilizar o Application Insights do Microsoft Dynamics CRM Online
Este artigo mostra-lhe como obter dados de telemetria dos [do Microsoft Dynamics CRM Online](https://www.dynamics.com/) usando [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Vamos examinar o processo completo de adicionar o script do Application Insights à sua aplicação, capturar dados e visualização de dados.

> [!NOTE]
> [Procurar a solução de exemplo](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Adicionar o Application Insights para a instância de CRM Online nova ou existente
Para monitorizar a sua aplicação, adicione um Application Insights SDK à sua aplicação. O SDK envia a telemetria para o [portal do Application Insights](https://portal.azure.com), onde pode utilizar as nossas ferramentas de diagnóstico e análise poderosas, ou exportar os dados para o armazenamento.

### <a name="create-an-application-insights-resource-in-azure"></a>Criar um recurso do Application Insights no Azure
1. Obtenha [uma conta no Microsoft Azure](http://azure.com/pricing). 
2. Inicie sessão para o [portal do Azure](https://portal.azure.com) e adicionar um novo recurso do Application Insights. Isso é onde os seus dados serão processados e apresentados.

    ![Clique em +, serviços de programação, Application Insights.](./media/app-insights-sample-mscrm/01.png)

    Escolha ASP.NET como o tipo de aplicação.
3. Siga as instruções para [obter o script do JavaScript SDK para a sua aplicação](app-insights-javascript.md#set-up-application-insights-for-your-web-page), copie o fragmento do JavaScript e certifique-se substituir a chave de instrumentação com o valor correto para o seu recurso do Application Insights.

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Criar um recurso da web JavaScript no Microsoft Dynamics CRM
1. Abra a sua instância do CRM Online e o início de sessão com privilégios de administrador.
2. Abra o Microsoft Dynamics CRM definições, personalizações, personalizar o sistema

    ![Definições do Microsoft Dynamics CRM](./media/app-insights-sample-mscrm/00001.png)

    ![Definições > personalizações](./media/app-insights-sample-mscrm/00002.png)

1. Crie um recurso de JavaScript.

    ![Caixa de diálogo do nova recurso da Web](./media/app-insights-sample-mscrm/07.png)

    Atribua um nome, selecione **Script (JScript)** e abrir o editor de texto.

    ![Abra o editor de texto](./media/app-insights-sample-mscrm/00004.png)
2. Copie o código de que configurou sua chave de instrumentação antes do Application Insights JavaScript SDK. Ao copiar, certifique-se ignorar etiquetas de script. Veja a captura de ecrã abaixo:

    ![Definir a sua chave de instrumentação](./media/app-insights-sample-mscrm/000005.png)

    O código inclui a chave de instrumentação que identifica o recurso do Application insights.
3. Guardar e publicar.

    ![Guardar e publicar](./media/app-insights-sample-mscrm/00006.png)

### <a name="instrument-forms"></a>Formulários de instrumento
1. No Microsoft CRM Online, abra o formulário de conta

    ![Formulário de conta](./media/app-insights-sample-mscrm/00007.png)
2. Abra o formulário de propriedades

    ![Propriedades do formulário](./media/app-insights-sample-mscrm/00008.png)
3. Adicione o recurso da web de JavaScript que criou

    ![Menu Adicionar](./media/app-insights-sample-mscrm/13.png)

4. Guardar e publicar as suas personalizações do formulário.

## <a name="metrics-captured"></a>Métricas capturadas
Configurou a captura de telemetria para o formulário. Sempre que é utilizado, os dados serão enviados para o recurso do Application Insights.

Seguem-se exemplos dos dados que verá.

#### <a name="application-health"></a>Estado de funcionamento da aplicação
![Tempo de carregamento de página de exemplo](./media/app-insights-sample-mscrm/15.png)

![Gráfico de vistas de página de exemplo](./media/app-insights-sample-mscrm/16.png)

Exceções do browser:

![Gráfico de exceções de browser](./media/app-insights-sample-mscrm/17.png)

Clique no gráfico para obter mais detalhes:

![Lista de exceções](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Utilização
![Os utilizadores, sessões e visualizações de página](./media/app-insights-sample-mscrm/19.png)

![Gráficos de sessão](./media/app-insights-sample-mscrm/20.png)

![Versões do navegador](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers
![Divisão do tempo de carregamento de página](./media/app-insights-sample-mscrm/22.png)

![Contagem de sessões por versão do browser](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocalização
![Número de sessões por país](./media/app-insights-sample-mscrm/24.png)

![Sessões e os utilizadores por país](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Solicitação de vista de página por dentro
![Resumo de vista de página](./media/app-insights-sample-mscrm/26.png)

![Ver eventos de página de pesquisa](./media/app-insights-sample-mscrm/27.png)

![Vistas de página semelhantes](./media/app-insights-sample-mscrm/28.png)

![Propriedades de visualização de página](./media/app-insights-sample-mscrm/29.png)

![Páginas por sessão](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Código de exemplo
[Procurar o código de exemplo](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Pode fazer análises ainda mais, se [exportar os dados para o Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Solução do exemplo Microsoft Dynamics CRM
[Eis a solução de exemplo implementada no Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Saiba mais
* [O que é o Application Insights?](app-insights-overview.md)
* [Application Insights para páginas da web](app-insights-javascript.md)
* [Mais exemplos e orientações passo a passo](app-insights-overview.md)
