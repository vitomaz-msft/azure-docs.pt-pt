---
title: Explore Java registos de rastreio no Application Insights do Azure | Documentos da Microsoft
description: Rastreios de pesquisa Log4J ou Logback no Application Insights
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: mbullwin
ms.openlocfilehash: 99f172245a0a42a95928a785c4bd70efcb19a694
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093916"
---
# <a name="explore-java-trace-logs-in-application-insights"></a>Explore Java registos de rastreio no Application Insights
Se estiver a utilizar Logback ou Log4J (versão 1.2 ou 2.0) para o rastreio, pode ter os registos de rastreio enviados automaticamente para o Application Insights, onde pode explorar e pesquisar nos mesmos.

## <a name="install-the-java-sdk"></a>Instalar o Java SDK

Siga as instruções para instalar [Application Insights SDK para Java][java], se ainda não fez que.

## <a name="add-logging-libraries-to-your-project"></a>Adicionar bibliotecas de Registro em log ao seu projeto
*Escolha a forma adequada para o seu projeto.*

#### <a name="if-youre-using-maven"></a>Se estiver a utilizar Maven...
Se o seu projeto já está configurado para utilizar Maven para a compilação, intercale um dos seguintes fragmentos de código o ficheiro pom. XML.

Em seguida, Atualize as dependências do projeto, para obter os binários transferidos.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.0*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.2*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[2.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Se estiver a utilizar Gradle...
Se o seu projeto já está configurado para utilizar Gradle para a compilação, uma das seguintes linhas ao adicionar o `dependencies` grupo no seu ficheiro de gradle:

Em seguida, Atualize as dependências do projeto, para obter os binários transferidos.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '2.0.+'
```

**Log4J v2.0**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '2.0.+'
```

**Log4J v1.2**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '2.0.+'
```

#### <a name="otherwise-"></a>Caso contrário...
Siga as orientações para instalar o SDK de Java do Application Insights manualmente, transferir jar (depois de ariving na página da Central Maven clique no link de "jar" na seção download) para appender apropriado e adicionar jar appender transferido para o projeto.

| Logger | Transferência | Biblioteca |
| --- | --- | --- |
| Logback |[Logback appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-logback%22) |applicationinsights-logging-logback |
| Log4J v2.0 |[Log4J v2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j2%22) |applicationinsights-logging-log4j2 |
| Log4j v1.2 |[Log4J versão 1.2 appender Jar](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22applicationinsights-logging-log4j1_2%22) |applicationinsights-logging-log4j1_2 |


## <a name="add-the-appender-to-your-logging-framework"></a>Adicionar o appender à sua arquitetura de registo
Para iniciar a obtenção de rastreios, intercale o fragmento de código para o ficheiro de configuração Log4J ou Logback relevante: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.0*

```XML

    <Configuration packages="com.microsoft.applicationinsights.log4j.v2">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.2*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Os appenders do Application Insights podem ser referenciadas por qualquer logger configurado e não necessariamente pelo agente a raiz (conforme mostrado nos exemplos de código acima).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Prozkoumat trasování no portal do Application Insights
Agora que configurou o seu projeto para enviar os rastreios para o Application Insights, pode ver e procurar estes rastreios no portal do Application Insights, o [pesquisa] [ diagnostic] painel.

Submetida de exceções por meio de agentes será apresentada no portal como telemetria de exceção.

![No portal do Application Insights, abrir a pesquisa](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Passos Seguintes
[Pesquisa de diagnóstico][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md


