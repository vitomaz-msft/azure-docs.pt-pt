---
title: Pesquisa cognitiva para extração de dados, processamento de linguagem natural AI no Azure Search | Documentos da Microsoft
description: Extração de conteúdo, (NLP) de processamento de linguagem natural e processamento de imagens para criar conteúdo pesquisável no Azure Search, indexação com capacidades cognitivas e algoritmos de IA
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 08/07/2018
ms.author: heidist
ms.openlocfilehash: 68d546fc4c853f1a19230b8aee7e86519aaa7e4c
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729057"
---
# <a name="what-is-cognitive-search"></a>O que é a pesquisa cognitiva?

Pesquisa cognitiva cria pesquisáveis informações fora de conteúdo não pesquisável anexando algoritmos de ia para um pipeline de indexação. Integração de IA é através de *capacidades cognitivas*, aprimorando documentos de origem caminho para um índice de pesquisa. 

**Processamento de linguagem natural** habilidades incluem [reconhecimento de entidades](cognitive-search-skill-named-entity-recognition.md), deteção de idioma [extração de expressões da chave](cognitive-search-skill-keyphrases.md), manipulação de texto e a deteção de sentimentos. Com essas habilidades, torna-se estruturados textos não estruturados, mapeado para os campos filtráveis e pesquisáveis num índice.

**Processamento de imagens** inclui [OCR](cognitive-search-skill-ocr.md) e a identificação de [recursos visuais](cognitive-search-skill-image-analysis.md), tais como deteção facial, interpretação de imagens, a imagem de reconhecimento (pessoas famosas e pontos de referência) ou atributos como cores ou orientação da imagem. Pode criar representações de texto do conteúdo da imagem, pesquisável com todos os recursos de consulta do Azure Search.

![Diagrama de pipeline de pesquisa cognitiva](./media/cognitive-search-intro/cogsearch-architecture.png "descrição geral do pipeline de pesquisa cognitiva")

As capacidades cognitivas no Azure Search baseiam-se os mesmos algoritmos de IA utilizados em APIs serviços cognitivos: [API de reconhecimento de entidades com o nome](cognitive-search-skill-named-entity-recognition.md), [API de extração de frase chave](cognitive-search-skill-keyphrases.md), e [OCR API](cognitive-search-skill-ocr.md) são apenas alguns. 

Linguagem natural e processamento de imagens é aplicada durante a fase de ingestão de dados, com resultados se tornar parte de composição de um documento num índice pesquisável no Azure Search. Os dados são originados como um conjunto de dados do Azure e, em seguida, enviados por meio de um pipeline de indexação com o que ocorrer [habilidades internas](cognitive-search-predefined-skills.md) que precisa. A arquitetura é extensível, de modo a se as habilidades internas não forem suficientes, pode criar e anexar [competências personalizadas](cognitive-search-create-custom-skill-example.md) para integrar o processamento personalizado. Exemplos podem ser um classificador de módulo ou o documento de entidade personalizada destinado um domínio específico, como Finanças, publicações científicas ou medicina.

> [!NOTE]
> A Pesquisa Cognitiva está em pré-visualização pública. Conjunto de capacidades execução e a extração de imagem e a normalização atualmente são oferecidos gratuitamente. Posteriormente, os preços para estas capacidades serão anunciado. 

## <a name="components-of-cognitive-search"></a>Componentes de pesquisa cognitiva

Pesquisa cognitiva é uma funcionalidade de pré-visualização do [Azure Search](search-what-is-azure-search.md), disponível em todas as camadas numa Centro-Sul e Europa Ocidental. 

O pipeline de pesquisa cognitiva baseia [Azure Search *indexadores* ](search-indexer-overview.md) que pesquisam origens de dados e fornecer o processamento de índice de ponto-a-ponto. Habilidades estão agora ligadas ao indexadores, interceptando e enriquecer os documentos de acordo com o conjunto de capacidades que definir. Depois de indexada, pode aceder a conteúdo através de pedidos de pesquisa através de todos os [consultar os tipos suportados pelo Azure Search](search-query-overview.md).  Se estiver familiarizado com indexadores, esta secção descreve os passos.

### <a name="source-data-and-document-cracking-phase"></a>Origem de dados e a fase de abertura do documento

No início do pipeline, que tem texto não estruturado ou conteúdo que não sejam de texto (como imagens e documentos digitalizados JPEG ficheiros). Dados tem de existir um serviço de armazenamento de dados do Azure que pode ser acedido por um indexador. Indexadores podem "violar" documentos de origem para extrair texto de origem de dados.

![Fase de abertura do documento](./media/cognitive-search-intro/document-cracking-phase-blowup.png "aberturas de documentos")

 Fontes suportadas incluem armazenamento de Blobs do Azure, armazenamento de tabelas do Azure, base de dados do Azure SQL e do Azure Cosmos DB. Conteúdo baseado em texto pode ser extraído dos seguintes tipos de ficheiro: PDFs, Word, PowerPoint, ficheiros CSV. Para a lista completa, consulte [formatos suportados](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Capacidades cognitivas e a fase de melhoria

Melhoria é através de *capacidades cognitivas* efetuar operações atómicas. Por exemplo, quando tiver conteúdo de texto de PDF, pode aplicar deteção de idioma de reconhecimento de entidades ou extração de expressões-chave para produzir novos campos no seu índice que não estão disponíveis nativamente na origem. Em conjunto, a recolha de suas habilidades utilizadas no seu pipeline é chamada um *conjunto de capacidades*.  

![Fase de enriquecimento](./media/cognitive-search-intro/enrichment-phase-blowup.png "fase de melhoria")

Um conjunto de capacidades baseia [predefinidos capacidades cognitivas](cognitive-search-predefined-skills.md) ou [competências personalizadas](cognitive-search-create-custom-skill-example.md) fornecer e ligue-se para o conjunto de capacidades. Um conjunto de capacidades pode ser muito pequeno ou altamente complexas e determina não só o tipo de processamento, mas também a ordem de operações. Os mapeamentos de campo definidos como parte de um indexador totalmente Especifica o pipeline de enriquecimento além disso, um conjunto de capacidades. Para obter mais informações sobre como colocar todas essas partes em conjunto, consulte [definir um conjunto de capacidades](cognitive-search-defining-skillset.md).

Internamente, o pipeline gera uma coleção de documentos plena. Pode decidir que partes dos documentos plena devem ser mapeados para indexáveis campos no seu índice de pesquisa. Por exemplo, se aplicado a extração de expressões-chave e as competências de reconhecimento de entidades, em seguida, esses novos campos seriam tornar-se parte do documento plena e podem ser mapeados para os campos no seu índice. Ver [anotações](cognitive-search-concept-annotations-syntax.md) para saber mais sobre formações de entrada/saída.

### <a name="search-index-and-query-based-access"></a>Índice de pesquisa e acesso com base na consulta

Quando o processamento é concluído, tem um corpo consistindo em plena documentos, texto integral no Azure Search. [Consultar o índice](search-query-overview.md) é como desenvolvedores e usuários acessar o conteúdo plena gerado pelo pipeline. 

![Índice com o ícone de pesquisa](./media/cognitive-search-intro/search-phase-blowup.png "índice com o ícone de pesquisa")

O índice é como qualquer outro pode criar para o Azure Search: pode complementar com analisadores personalizados, invocar consultas de pesquisa difusa, adicionar pesquisa filtrada ou experimentar perfis reformatar os resultados da pesquisa de classificação.

Os índices são gerados a partir de um esquema de índice que define os campos, atributos, e outras construções ligado a um índice específico, como perfis de classificação e mapas de sinónimos. Depois de um índice é definido e preenchido, pode indexar incrementalmente para recolher os documentos de origem novos e atualizados. Determinados modificações requerem uma reconstrução completa. Deve usar um pequeno conjunto de dados até que o design do esquema esteja estável. Para obter mais informações, veja [Como reconstruir um índice](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Funcionalidades e conceitos principais

| Conceito | Descrição| Ligações |
|---------|------------|-------|
| Conjunto de capacidades | Um nível superior com o nome que contém uma coleção de habilidades de recursos. Um conjunto de capacidades é o pipeline de melhoria. É invocado durante a indexação por um indexador. | [Definir um conjunto de capacidades](cognitive-search-defining-skillset.md) |
| Competências cognitivas | Uma transformação Atómica num pipeline de melhoria. Muitas vezes, é um componente que extrai ou infere a estrutura e, portanto, aumenta a compreensão dos dados de entrada. Quase sempre, a saída é baseado em texto e o processamento é o processamento de linguagem natural ou processamento de imagens que extrai ou gera texto a partir de entradas de imagem. Saída de uma habilidade pode ser mapeada para um campo num índice ou usada como entrada para uma melhoria de downstream. Uma habilidade é predefinida e fornecida pela Microsoft ou personalizado: criados e implementados por si. | [Competências predefinidas](cognitive-search-predefined-skills.md) |
| Extração de dados | Abrange uma ampla gama de processamento, mas relativas a pesquisa cognitiva, a habilidade de reconhecimento de entidades nomeadas mais frequentemente é utilizada para extrair dados (entidade) de uma origem de que não fornece essas informações nativamente. | [Habilidade de reconhecimento de entidades nomeadas](cognitive-search-skill-named-entity-recognition.md)| 
| Processamento de imagens | Infere o texto a partir de uma imagem, como a capacidade de reconhecer um ponto de referência, ou extrai o texto de uma imagem. Exemplos comuns incluem OCR para elevação de caracteres de um arquivo de documentos digitalizados (JPEG), ou no reconhecimento de um nome da rua numa fotografia que contém um sinal de rua. | [Habilidade de análise de imagem](cognitive-search-skill-image-analysis.md) ou [habilidade de OCR](cognitive-search-skill-ocr.md)
| Processamento de linguagem natural | Processamento de informações e obter informações sobre entradas de texto de texto. Deteção de idioma, análise de sentimentos e extração de expressões-chave são habilidades que se inserem o processamento de linguagem natural.  | [Chave a habilidade de extração de frase](cognitive-search-skill-keyphrases.md), [habilidade de deteção de idioma](cognitive-search-skill-language-detection.md), [habilidade de análise de sentimentos](cognitive-search-skill-sentiment.md) |
| Abertura do documento | O processo de extração ou criar conteúdo de texto a partir de origens de que não sejam de texto durante a indexação. Reconhecimento ótico de carateres (OCR) é um exemplo, mas geralmente se refere a funcionalidade de indexador principal como o indexador extrai o conteúdo de arquivos do aplicativo. A origem de dados, fornecendo a localização do ficheiro de origem e a definição de indexador fornecendo mapeamentos de campo, são ambos os principais fatores de abertura do documento. | Consulte [indexadores](search-indexer-overview.md) |
| Formatação | Consolidação de fragmentos de texto numa estrutura maior, ou por outro lado, dividir blocos maiores de texto num tamanho gerenciável para ainda mais processamento a jusante. | [Habilidade de Modelador](cognitive-search-skill-shaper.md), [habilidade de fusão de texto](cognitive-search-skill-textmerger.md), [habilidade de dividir texto](cognitive-search-skill-textsplit.md) |
| Documentos plena | Uma transitória estrutura interna, não acessível diretamente no código. Documentos plena são gerados durante o processamento, mas apenas finais saídas são mantidas num índice de pesquisa. Mapeamentos de campo determinam quais elementos de dados são adicionados ao índice. | Ver [aceder a documentos plena](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexador |  Um crawler que extrai dados pesquisáveis e metadados de uma origem de dados externa e preenche um índice com base nos mapeamentos campo a campo entre o índice e sua origem de dados de aberturas de documentos. Para possível de pesquisa cognitiva, o indexador invoca um conjunto de capacidades e contém os mapeamentos de campo associar a saída de melhoria para campos de destino no índice. A definição de indexador contém todas as instruções e referências para operações de pipeline e o pipeline é invocado quando executar o indexador. | [Indexadores](search-indexer-overview.md) |
| Origem de Dados  | Um objeto utilizado por um indexador para ligar a uma origem de dados externas dos tipos suportados no Azure. | Consulte [indexadores](search-indexer-overview.md) |
| Índice | Um corpo persistente no Azure Search, criados a partir de um esquema de índice que define a estrutura de campo e a utilização. | [Índices no Azure Search](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Por onde devo começar?

**Passo 1: Criar um serviço de pesquisa numa região, fornecendo as APIs** 

+ EUA Centro-Sul
+ Europa Ocidental

**Passo 2: Experiência prática para dominar o fluxo de trabalho**

+ [Início rápido (portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (pedidos HTTP)](cognitive-search-tutorial-blob.md)
+ [Competências personalizadas de exemplo (c#)](cognitive-search-create-custom-skill-example.md)

**Passo 3: Rever a API (REST apenas)**

Atualmente, são fornecidas apenas as APIs de REST. Utilize `api-version=2017-11-11-Preview` em todos os pedidos. Utilize as seguintes APIs para criar uma solução de pesquisa cognitiva. Apenas duas APIs são adicionadas ou expandidas para pesquisa cognitiva. Outras APIs de ter a mesma sintaxe que as versões em disponibilidade geral.

| API REST | Descrição |
|-----|-------------|
| [Criar origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Um recurso de identificação de uma origem de dados externa a fornecer os dados de origem utilizados para criar documentos plena.  |
| [Criar conjunto de capacidades (api-version = 2017-11-11-pré-visualização)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Um recurso de coordenar o uso de [predefinidos habilidades](cognitive-search-predefined-skills.md) e [personalizadas capacidades cognitivas](cognitive-search-custom-skill-interface.md) usado num pipeline de enriquecimento durante a indexação. |
| [Criar índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Um esquema de expressar um índice da Azure Search. Campos no índice mapeiam para campos numa origem de dados ou para campos fabricados durante a fase de melhoria (por exemplo, um campo para nomes de organização criado pelo reconhecimento de entidades). |
| [Criar indexador (api-version = 2017-11-11-pré-visualização)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)  | Um recurso definindo componentes utilizados durante a indexação: incluindo uma origem de dados, um conjunto de capacidades, associações de campo da origem e de estruturas de dados intermédios ao índice de destino e o índice em si. É o acionador para ingestão de dados e enriquecimento de executar o indexador. O resultado é um corpo de pesquisa com base no esquema de índice, preenchido com dados de origem, enriquecidos através de conjuntos de competências.  |

**Lista de verificação: Um fluxo de trabalho típico**

1. Subconjunto os dados de origem do Azure para uma amostra representativa. Indexação ocupa o tempo de então, comece com um conjunto de dados representativo e pequeno e, em seguida, criá-lo incrementalmente como sua solução evoluiu.

1. Criar uma [objeto de origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source) no Azure Search para fornecer uma cadeia de ligação para obtenção de dados.

1. Criar uma [conjunto de capacidades](https://docs.microsoft.com/rest/api/searchservice/create-skillset) com passos de melhoria.

1. Definir o [esquema de índice](https://docs.microsoft.com/rest/api/searchservice/create-index). O *campos* coleção inclui campos da origem de dados. Deve também um stub campos adicionais para conter os valores gerados para o conteúdo criado durante a melhoria.

1. Definir o [indexador](https://docs.microsoft.com/rest/api/searchservice/create-skillset) referenciar a origem de dados, o conjunto de capacidades e o índice.

1. Dentro do indexador, adicione *outputFieldMappings*. Esta secção mapeia a saída do conjunto de capacidades (no passo 3) para os campos de entradas no esquema do índice (no passo 4).

1. Envie *criar indexador* pedir que acabou de criar (um pedido POST com uma definição de indexador no corpo do pedido) para expressar o indexador do Azure Search. Este passo é como executar o indexador, invocado o pipeline.

1. Execute consultas para avaliar os resultados e modificar o código para conjuntos de habilidades de atualização, o esquema ou a configuração do indexador.

1. [Repor o indexador](search-howto-reindex.md) antes de recriar o pipeline.

Para obter mais informações sobre problemas ou perguntas específicas, consulte [sugestões de resolução de problemas](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Passos Seguintes

+ [Documentação de pesquisa cognitiva](cognitive-search-resources-documentation.md)
+ [Início rápido: Experimentar a pesquisa cognitiva numa instruções do portal](cognitive-search-quickstart-blob.md)
+ [Tutorial: Saiba a APIs de pesquisa cognitiva](cognitive-search-tutorial-blob.md)
