---
title: Estágio de compreensão de negócios do ciclo de vida do processo de ciência de dados de equipa - Azure | Documentos da Microsoft
description: As metas, tarefas e resultados finais para a fase de compreensão de negócios dos seus projetos de ciência de dados
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 00dc359fc171afe470b1dfe8f38d0951085ec580
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228305"
---
# <a name="business-understanding"></a>Noções sobre empresas

Este artigo descreve os objetivos, tarefas e resultados associados a fase de compreensão de negócios do Team Data Science Process (TDSP). Este processo fornece um ciclo de vida recomendado que pode utilizar para estruturar os seus projetos de ciência de dados. O ciclo de vida destaca as fases principais projetos normalmente executadas, muitas vezes iterativamente:

   1. **Compreensão empresarial**
   2. **Aquisição de dados e compreensão**
   3. **Modelagem**
   4. **Implementação**
   5. **Aceitação do cliente**

Esta é uma representação visual de ciclo de vida do TDSP: 

![Ciclo de vida do TDSP](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Objetivos
* Especifique as variáveis-chave que está a servir como destinos de modelo e cujas métricas relacionadas são utilizadas determinam o sucesso do projeto.
* Identifica as origens de dados relevantes que tem acesso à empresa, ou tem de obter.

## <a name="how-to-do-it"></a>Como fazê-lo
Existem duas tarefas principais abordadas neste estágio: 

   * **Definir os objetivos**: trabalhar com seu cliente e outros intervenientes para compreender e a identificar os problemas empresariais. Formule perguntas que definem os objetivos de negócio que as técnicas de ciência de dados podem ter como destino.
   * **Identificar origens de dados**: localizar os dados relevantes que o ajuda a responder às perguntas que definem os objetivos do projeto.

### <a name="define-objectives"></a>Definir os objetivos
1. Um objetivo central deste passo é identificar as variáveis de negócios essenciais que a análise tem de prever. Fazemos referência a essas variáveis como o *modelar destinos*, e podemos usar as métricas associadas a eles para determinar o sucesso do projeto. Dois exemplos de tais destinos são as previsões de vendas ou a probabilidade de um pedido que está sendo fraudulentos.

2. Defina as metas do projeto ao perguntar e refinar perguntas "sharp", que são relevantes, específicas e inequívoca. Ciência de dados é um processo que utiliza nomes e números para responder a essas perguntas. Para obter mais informações sobre perguntas sharp, consulte a [como fazer ciência de dados](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blog. Geralmente usa ciência de dados ou de aprendizagem automática responder a cinco tipos de perguntas:
 
   * Quantidade ou quantos? (regressão)
   * Qual categoria? (classificação)
   * Que grupo? (clustering)
   * É estranho? (deteção de anomalias)
   * Qual é a opção deve ser levada em consideração? (recomendação)

   Determine qual dessas perguntas, está pedindo e como respondendo atinge seus objetivos empresariais.

3. Defina a equipe do projeto, especificando as funções e responsabilidades de seus membros. Desenvolva um plano de alto nível etapa que itera à medida que descobre mais informações. 

4. Defina as métricas de sucesso. Por exemplo, pode querer obter uma previsão de abandono de clientes. Precisa de uma taxa de precisão de "x" por cento no final deste projeto de três meses. Com esses dados, pode oferecer promoções de cliente para reduzir o abandono. As métricas têm de ser **INTELIGENTE**: 

   * **S**pecíficas 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**ligados a ime 

### <a name="identify-data-sources"></a>Identificar origens de dados
Identifica origens de dados que contêm conhecidos exemplos de respostas para suas perguntas sharp. Procure os seguintes dados:

* Dados que são relevantes para a pergunta. Tem medidas de destino e de recursos que estão relacionadas com o destino?
* Dados que é uma medida precisa do seu destino do modelo e os recursos de interesse.

Por exemplo, pode descobrir que os sistemas existentes necessário recolher e registar os tipos adicionais de dados para resolver o problema e atingir as metas do projeto. Nesta situação, pode querer procurar origens de dados externas ou atualizar seus sistemas a recolher novos dados.

## <a name="artifacts"></a>Artefactos
Aqui estão os resultados finais neste estágio:

   * [Documento de responsabilidade](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): um modelo padrão é fornecido na definição de estrutura de projeto de TDSP. O documento de responsabilidade é um documento dinâmico. Atualizar o modelo em todo o projeto como fizer deteções novas e quanto a empresa requisitos se alteram. A chave é a iteração após neste documento, adicionando mais detalhes, conforme avança no processo de deteção. Manter o cliente e outras partes interessadas envolvido em fazer as alterações e comuniquem claramente os motivos para que as alterações aos mesmos.  
   * [Origens de dados](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Data_Report/Data%20Defintion.md#raw-data-sources): A **origens de dados não processados** seção o **definições de dados** relatório que se encontra no projeto de TDSP **relatório de dados** pasta contém os dados origens. Esta secção especifica as localizações originais e de destino para os dados não processados. Nos estágios posteriores, preencha os detalhes adicionais, como os scripts para mover os dados ao seu ambiente de análise.  
   * [Os dicionários de dados](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Data_Dictionaries): este documento fornece descrições dos dados que são fornecidos pelo cliente. Estas descrições incluem informações sobre o esquema (tipos de dados e informações sobre as regras de validação, se aplicável) e os diagramas de entidade-relação, se disponível.

## <a name="next-steps"></a>Passos Seguintes

Seguem-se ligações para cada etapa do ciclo de vida do TDSP:

   1. [Compreensão empresarial](lifecycle-business-understanding.md)
   2. [Aquisição de dados e compreensão](lifecycle-data.md)
   3. [Modelagem](lifecycle-modeling.md)
   4. [Implementação](lifecycle-deployment.md)
   5. [Aceitação do cliente](lifecycle-acceptance.md)

Fornecemos orientações passo a passo de completa-a-ponto, que demonstram todas as etapas do processo para cenários específicos. O [instruções passo a passo do exemplo](walkthroughs.md) artigo fornece uma lista dos cenários com links e descrições em miniatura. A instruções passo a passo mostram como combinar a cloud, ferramentas no local e serviços num fluxo de trabalho ou um pipeline para criar uma aplicação inteligente. 

Para obter exemplos de como executar os passos no TDSPs que utilizam o Azure Machine Learning Studio, consulte [utilizar o TDSP com o Azure Machine Learning](https://aka.ms/datascienceprocess).
