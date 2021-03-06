---
title: Serviços do Azure que o suporte de identidades geridas para recursos do Azure
description: Lista de serviços que oferecem suporte a identidades geridas para recursos do Azure e a autenticação do Azure AD
services: active-directory
author: daveba
ms.author: daveba
ms.date: 10/23/2018
ms.topic: conceptual
ms.service: active-directory
ms.component: msi
manager: mtillman
ms.openlocfilehash: 25da856d8bd72f4e74718cf32f62bbb0a3eb5b88
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984694"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>Serviços que suportam identidades geridas para recursos do Azure

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Utilizar uma identidade gerida, pode autenticar a qualquer serviço que suporta a autenticação do Azure AD sem ter credenciais em seu código. Estamos no processo de integração de identidades geridas para recursos do Azure e a autenticação do Azure AD no Azure. Verificar back, muitas vezes, a existência de atualizações.

> [!NOTE]
> Identidades geridas para recursos do Azure é o novo nome para o serviço anteriormente conhecido como Identidade de Serviço Gerida (MSI).

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>Serviços do Azure que suportam identidades geridas para os recursos do Azure

Os seguintes serviços do Azure suportam identidades geridas para recursos do Azure:

| Serviço | Estado do sistema atribuído | Estado do utilizador atribuído| Configurar | Obter um token |
| ------- | ------ | ---- | --------- | ----------- |
| Máquinas Virtuais do Azure | Disponível | Pré-visualização | [Portal do Azure](qs-configure-portal-windows-vm.md)<br>[PowerShell](qs-configure-powershell-windows-vm.md)<br>[CLI do Azure](qs-configure-cli-windows-vm.md)<br>[Modelos Azure Resource Manager](qs-configure-template-windows-vm.md)<br>[REST](qs-configure-rest-vm.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell) |
| Conjuntos de Dimensionamento de Máquinas Virtuais | Disponível | Pré-visualização | [Portal do Azure](qs-configure-portal-windows-vmss.md)<br>[PowerShell](qs-configure-powershell-windows-vmss.md)<br>[CLI do Azure](qs-configure-cli-windows-vmss.md)<br>[Modelos Azure Resource Manager](qs-configure-template-windows-vmss.md)<br>[REST](qs-configure-rest-vmss.md) | [REST](how-to-use-vm-token.md#get-a-token-using-http)<br>[.NET](how-to-use-vm-token.md#get-a-token-using-c)<br>[Bash/Curl](how-to-use-vm-token.md#get-a-token-using-curl)<br>[Go](how-to-use-vm-token.md#get-a-token-using-go)<br>[PowerShell](how-to-use-vm-token.md#get-a-token-using-azure-powershell)
| Serviço de Aplicações do Azure | Windows: disponível <br> Linux: Disponível em breve | Não disponível | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell)  |
| Funções do Azure | Disponível | Não disponível | [Portal do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-portal)<br>[CLI do Azure](/azure/app-service/app-service-managed-service-identity#using-the-azure-cli)<br>[Azure PowerShell](/azure/app-service/app-service-managed-service-identity#using-azure-powershell)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#using-an-azure-resource-manager-template) | [REST](/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)<br>[.NET](/azure/app-service/app-service-managed-service-identity#asal)<br>[JavaScript](/azure/app-service/app-service-managed-service-identity#token-js)<br>[PowerShell](/azure/app-service/app-service-managed-service-identity#token-powershell) |
| Azure Logic Apps | Disponível | Não disponível | [Portal do Azure](/azure/logic-apps/create-managed-service-identity#azure-portal)<br>[Modelo do Azure Resource Manager](/azure/app-service/app-service-managed-service-identity#deployment-template) |  |
| Azure Data Factory V2 | Disponível | Não disponível | [Portal do Azure](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity)<br>[PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-powershell)<br>[REST](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-rest-api)<br>[SDK](~/articles/data-factory/data-factory-service-identity.md#generate-service-identity-using-sdk) |
| API Management do Azure | Disponível | Não disponível | [Modelo do Azure Resource Manager](/azure/api-management/api-management-howto-use-managed-service-identity) |
| Azure Container Instances | Linux: pré-visualização<br>Windows: Não está disponível | Linux: pré-visualização<br>Windows: Não está disponível | [CLI do Azure](~/articles/container-instances/container-instances-managed-identity.md)<br>[Modelo do Azure Resource Manager](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)<br>[YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file) |  |


## <a name="azure-services-that-support-azure-ad-authentication"></a>Suporte do Azure AD que a autenticação de serviços do Azure

Os seguintes serviços de suportam de autenticação do Azure AD e foram testados com serviços de cliente que utilizam identidades geridas para recursos do Azure.

| Serviço | ID do Recurso | Estado | Date | Atribuir acesso |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | `https://management.azure.com/` | Disponível | Setembro de 2017 | [Portal do Azure](howto-assign-access-portal.md) <br>[PowerShell](howto-assign-access-powershell.md) <br>[CLI do Azure](howto-assign-access-CLI.md) |
| Azure Key Vault | `https://vault.azure.net` | Disponível | Setembro de 2017 | |
| Azure Data Lake | `https://datalake.azure.net/` | Disponível | Setembro de 2017 | |
| SQL do Azure | `https://database.windows.net/` | Disponível | Outubro de 2017 | |
| Azure Event Hubs | `https://eventhubs.azure.net` | Pré-visualização | Dezembro de 2017 | |
| Service Bus do Azure | `https://servicebus.azure.net` | Pré-visualização | Dezembro de 2017 | |
| Storage do Azure | `https://storage.azure.com/` | Pré-visualização | Maio de 2018 | |