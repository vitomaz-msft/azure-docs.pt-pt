---
title: Criar alertas de métricas para os registos no Azure Monitor
description: Tutorial sobre como criar alertas de métricas de quase em tempo real nos dados de análise de registo populares.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: a8baa67296c721fdda4cb0728e1b68f2e67e217c
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824239"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Criar alertas de métricas para os registos no Azure Monitor  

## <a name="overview"></a>Descrição geral
O Azure suporta Monitor [tipo de alerta de métrica](monitoring-near-real-time-metric-alerts.md) que tem vantagens sobre o [alertas clássicos](alert-metric-classic.md). As métricas estão disponíveis para [grande lista de serviços do Azure](monitoring-supported-metrics.md). Este artigo explica a utilização de um subconjunto (ou seja,) para o recurso - `Microsoft.OperationalInsights/workspaces`. 

Pode utilizar alertas de métricas em populares registos do Log Analytics extraídos como métricas como parte das métricas de Logs, incluindo recursos no Azure ou no local. As soluções do Log Analytics suportadas estão listadas abaixo:
- [Contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md) para máquinas Windows e Linux
- [Registos de heartbeat de agente de estado de funcionamento](../azure-monitor/insights/solution-agenthealth.md)
- [Gestão de atualizações](../automation/automation-update-management.md) registos
- [Dados de eventos](../log-analytics/log-analytics-data-sources-windows-events.md) registos
 
Há muitos benefícios de utilização **alertas de métrica para os registos** através de consulta com base [alertas de registo](alert-log.md) no Azure; algumas delas estão listadas abaixo:
- Alertas de métricas oferecem quase realtime capacidade de monitorização e alertas de métrica para dados de bifurcações de registos da origem de registo para garantir que o mesmo
- Alertas de métricas têm monitorização de estado - notificar apenas uma vez quando o alerta é acionado e uma vez quando o alerta é resolvido; em vez de alertas de registo, que são sem monitoração de estado e manter trabalharem em cada intervalo, se for cumprida a condição de alerta
- Alertas de métricas para o registo de fornecem várias dimensões, permitindo que a filtragem a valores específicos, como computadores, o tipo de SO, etc. mais simples; sem a necessidade de tentou dar seus consulta do analytics

> [!NOTE]
> Métrica específica e/ou de dimensão só será apresentada se os dados para o mesmo existem num período de escolhido. Estas métricas estão disponíveis para clientes com áreas de trabalho do Log Analytics do Azure.

## <a name="metrics-and-dimensions-supported-for-logs"></a>Métricas e suportadas para os registos de dimensões
 alertas de métricas suportam alertas de métricas que usam dimensões. Pode usar as dimensões para filtrar a métrica para o nível certo. A lista completa de métricas suportadas para os registos da [áreas de trabalho do Log Analytics](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces) está listado; em soluções suportadas.

> [!NOTE]
> Para ver métricas suportadas para a ser extraídos da área de trabalho do Log Analytics através de [Monitor do Azure - métricas](monitoring-metric-charts.md); uma métrica de alerta de registo tem de ser criado para a métrica disse. As dimensões escolhidas no alerta de métrica para os registos - apenas serão apresentados para exploração através do Azure Monitor - métricas.

# <a name="creating-metric-alert-for-log-analytics"></a>Criar alerta de métrica para o Log Analytics
Dados métricos de logs populares é enviada por pipe antes de ela é processada no Log Analytics, no Azure Monitor - métricas. Isso permite que os utilizadores a alavancar as capacidades da plataforma métrica, bem como alerta de métrica - incluindo ter alertas com frequência de apenas 1 minuto. Abaixo encontram-se o meio de um alerta de métrica para os registos de composição.

## <a name="prerequisites-for-metric-alert-for-logs"></a>Pré-requisitos para o alerta de métrica para os registos
Antes de métrica para os registos recolhidos sobre funciona de dados do Log Analytics, o seguinte tem de ser definido ativas e disponíveis:
1. **Active Directory área de trabalho do Log Analytics**: uma área de trabalho do Log Analytics válida e ativa tem de estar presente. Para obter mais informações, consulte [criar uma área de trabalho do Log Analytics no portal do Azure](../log-analytics/log-analytics-quick-create-workspace.md).
2. **Agente for configurado para a área de trabalho do Log Analytics**: agente tem de ser configuradas para as VMs do Azure (e/ou) VMs no local enviar dados para a área de trabalho de análise do registo utilizada no passo anterior. Para obter mais informações, consulte [descrição geral do agente do Log Analytics -](../azure-monitor/platform/agents-overview.md).
3. **Soluções de análise de registo suportada está instalada**: solução de análise de registo deve ser configurados e envio dados na área de trabalho do Log Analytics - suportado soluções são [contadores de desempenho para Windows e Linux](../log-analytics/log-analytics-data-sources-performance-counters.md), [Registos de heartbeat de agente de estado de funcionamento](../azure-monitor/insights/solution-agenthealth.md), [a gestão, de atualizações e [dados de eventos](../log-analytics/log-analytics-data-sources-windows-events.md).
4. **Configurado para enviar registos de soluções de análise de registo**: solução de análise de registo deve ter dados/registos necessários correspondente para [métricas suportadas para áreas de trabalho do Log Analytics](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces) ativada. Por exemplo, para *% de memória disponível* contador do mesmo tem de ser configurado na [contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md) solução primeiro.

## <a name="configuring-metric-alert-for-logs"></a>Configurar o alerta de métrica para os registos
 alertas de métricas podem ser criadas e geridos através do portal do Azure, modelos do Resource Manager, REST API, PowerShell e CLI do Azure. Como alertas de métrica para os registos, é uma variante de alertas de métricas - depois de terminar os pré-requisitos, o alerta de métrica para os registos pode ser criada para a área de trabalho do Log Analytics especificada. Todas as características e funcionalidades do [ alertas de métricas](monitoring-near-real-time-metric-alerts.md) serão aplicáveis a alertas de métricas para os registos, também, incluindo o esquema do payload, limites de quota aplicável e preços de faturação.

Para detalhes passo a passo e exemplos - veja [criar e gerir alertas de métricas](https://aka.ms/createmetricalert). Especificamente, para os alertas de métrica para os registos - siga as instruções para a gestão de alertas de métricas e certifique-se o seguinte:
- Direcionar para o alerta de métrica é um válido *área de trabalho do Log Analytics*
- Sinal escolhido para alerta de métrica para selecionada *área de trabalho do Log Analytics* é do tipo **métrica**
- Filtrar por condições específicas ou recurso usando filtros de dimensão as métricas para os registos estão multidimensionais
- Quando configurar *lógica de sinal*, um único alerta pode ser criado para abranger vários valores de dimensão (como o computador)
- Se **não** com o portal do Azure para criar o alerta de métrica para selecionado *área de trabalho do Log Analytics*; em seguida, o utilizador tem de primeiro criar uma regra de explícita para a conversão de dados de registo numa métrica utilizando [o azure Monitor - regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules
).

> [!NOTE]
> Ao criar o alerta de métrica para a área de trabalho do Log Analytics através do portal do Azure - correspondente a regra para a conversão de dados de registo em métrica via [do Azure Monitor - regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) é criado automaticamente em segundo plano,  *sem a necessidade de qualquer intervenção do utilizador ou a ação*. Para o alerta de métrica para a criação de registos através de meios que não o portal do Azure, consulte [modelo de recurso para os alertas de métrica para os registos](#resource-template-for-metric-alerts-for-logs) secção no meio de exemplo de criação de um log de ScheduledQueryRule com base a regra de conversão de métrica antes de alerta de métrica criação - else haverá não existem dados para o alerta de métrica no logs criados.

## <a name="resource-template-for-metric-alerts-for-logs"></a>Modelo de recurso para alertas de métricas para os registos
Conforme declarado anteriormente, o processo para a criação de alertas de métricas de registos é dois conceitos:
1. Criar uma regra de extração de métricas dos registos suportados com a API de scheduledQueryRule
2. Criar um alerta de métrica para a métrica extraído do registo (no passo 1) e área de trabalho do Log Analytics como um recurso de destino

Para alcançar o mesmo, é possível usar o exemplo de modelo de Gestor de recursos do Azure abaixo - em que a criação de alerta de métrica depende a criação com êxito da regra para extrair as métricas de registos através de scheduledQueryRule.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],   
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"                
                    }
                ]
            }
        }
    ]
}

```
Digamos que o JSON acima é guardado como metricfromLogsAlert.json -, em seguida, ele pode ser integrado a um ficheiro JSON de parâmetro para a criação do modelo de recursos com base. Um ficheiro de JSON de parâmetros de exemplo está listado abaixo:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan" 
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }    
}
```
Supondo que o ficheiro de parâmetros acima é guardado como metricfromLogsAlert.parameters.json; em seguida, é possível criar o alerta de métrica para os registos de utilização [modelo de recursos para a criação no portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md). 

Em alternativa, também é usar o comando do Powershell do Azure abaixo:
```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlert.json TemplateParameterFile metricfromLogsAlert.parameters.json
```

Ou utilize implementar o modelo de recursos com a CLI do Azure:
```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlert.json --parameters @metricfromLogsAlert.parameters.json
```

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o [ alertas de métricas](https://aka.ms/createmetricalert).
* Saiba mais sobre [alertas de registo no Azure](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).
