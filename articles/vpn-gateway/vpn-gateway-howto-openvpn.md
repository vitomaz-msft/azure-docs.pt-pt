---
title: 'Como configurar OpenVPN no Gateway de VPN do Azure: PowerShell | Documentos da Microsoft'
description: Passos para configurar OpenVPN para o Gateway de VPN do Azure
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: cherylmc
ms.openlocfilehash: 958f4f46ec6ba407df7c739b7c62aa1489458485
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408281"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>Configurar OpenVPN para o Gateway de VPN do Azure ponto a site (pré-visualização)

Este artigo ajuda-o a configurar OpenVPN no Gateway de VPN do Azure. O artigo pressupõe que já tem um ambiente de ponto a site de trabalho. Se não o fizer, utilize as instruções no passo 1 para criar uma VPN ponto a site.

> [!IMPORTANT]
> Esta Pré-visualização Pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="register"></a>Registar esta funcionalidade

Clique nas **TryIt** nestes passos para registar esta funcionalidade facilmente utilizando o Azure Cloud Shell.

>[!NOTE]
>Se não registar esta funcionalidade, não poderá utilizá-lo.
>

Depois de clicar em **TryIt** para abrir o Azure Cloud Shell, copie e cole os seguintes comandos:

```azurepowershell-interactive
Register-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Assim que o recurso mostra como registrado, voltar a registar a subscrição ao espaço de nomes do Network.

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Criar uma VPN ponto a site

Se ainda não tiver um ambiente de ponto a site está a funcionar, siga as instruções para criar um. Ver [criar uma VPN ponto a site](vpn-gateway-howto-point-to-site-resource-manager-portal.md) para criar e configurar um gateway de VPN ponto a site com a autenticação de certificados nativa do Azure.

## <a name="cmdlets"></a>2. Instalar cmdlets do PowerShell

Instale a versão mais recente dos cmdlets do PowerShell do Resource Manager. Para obter mais informações sobre como instalar os cmdlets PowerShell, consulte [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview). Isto é importante porque as versões anteriores dos cmdlets não contêm os valores atuais de que precisa para este exercício.

## <a name="enable"></a>3. Ativar OpenVPN no gateway

Ative OpenVPN no seu gateway. Certifique-se de que o gateway já está configurado para ponto a site (IKEv2 ou SSTP) antes de executar os seguintes comandos:

```powershell
$gw = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Passos Seguintes

Para configurar clientes para OpenVPN, consulte [OpenVPN configurar clientes](vpn-gateway-howto-openvpn-clients.md).