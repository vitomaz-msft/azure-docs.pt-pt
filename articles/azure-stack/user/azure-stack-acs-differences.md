---
title: Diferenças de armazenamento do Azure stack e considerações | Documentos da Microsoft
description: Compreenda as diferenças entre o armazenamento do Azure stack e o armazenamento do Azure, juntamente com considerações de implementação do Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/05/2018
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.openlocfilehash: 14e32bdfcde6969b820c0950d59bd5cf946a51e6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802326"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>O armazenamento do Azure Stack: diferenças e considerações

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

O armazenamento do Azure Stack é o conjunto de serviços de cloud de armazenamento no Microsoft Azure Stack. Armazenamento de pilha do Azure fornece BLOBs, tabela, fila e funcionalidade de gestão de conta com uma semântica consistente para o Azure.

Este artigo resume as diferenças de armazenamento do Azure Stack conhecidas dos serviços de armazenamento do Azure. Ele também apresenta uma lista de aspetos a considerar ao implementar o Azure Stack. Para saber mais sobre das principais diferenças entre global do Azure e o Azure Stack, veja a [considerações da chave](azure-stack-considerations.md) artigo.

## <a name="cheat-sheet-storage-differences"></a>Referência rápida: diferenças de armazenamento

| Funcionalidade | (Global) do Azure | Azure Stack |
| --- | --- | --- |
|Armazenamento de ficheiros|Com base na cloud partilhas de ficheiros SMB suportadas|Ainda não é suportado
|Encriptação do serviço de armazenamento do Azure para dados Inativos|encriptação AES de 256 bits|Encriptação de AES de 128 bits do BitLocker
|Tipo de conta de armazenamento|Contas de armazenamento de Blobs do Azure e para fins gerais|Para fins gerais apenas.
|Opções de replicação|Armazenamento localmente redundante, armazenamento georredundante, armazenamento georredundante com acesso de leitura e armazenamento com redundância de zona|Armazenamento localmente redundante.
|Armazenamento Premium|Totalmente suportado|Pode ser aprovisionado, mas sem limite de desempenho ou garantia.
|Managed disks|Premium e standard suportados|Suportada quando utiliza a versão 1808 ou posterior.
|Nome do blob|os 1024 carateres (2.048 bytes)|880 carateres (1,760 bytes)
|Tamanho máximo de blob de bloco|4.75 TB (100 MB X 50 000 blocos)|4.75 TB (100 MB x 50 000 blocos) para a atualização 1802 ou a versão mais recente. 50 000 x 4 MB (aproximadamente, 195 GB) para versões anteriores.
|Cópia do instantâneo incremental de blob de página|Premium e blobs de página do Azure standard suportados|Ainda não é suportado.
|Camadas de armazenamento para armazenamento de BLOBs|Camadas frequente, esporádica e de camadas de armazenamento de arquivo.|Ainda não é suportado.
Eliminação de forma recuperável para armazenamento de BLOBs|Pré-visualização|Ainda não é suportado.
|Tamanho máximo de blob de página|8 TB|1 TB
|Tamanho de página do blob de página|512 bytes|4 KB
|Tamanho da chave de linha e de chave de partição de tabela|os 1024 carateres (2.048 bytes)|400 caracteres (800 bytes)
|Instantâneo de blob|O número máximo de instantâneos de um blob não é limitado.|O número máximo de instantâneos de um blob é 1000.|

Também existem diferenças com métricas de armazenamento:

* Os dados de transação nas métricas de armazenamento não distinguem largura de banda de rede internos ou externos.
* Os dados de transação nas métricas de armazenamento não incluem o acesso à máquina virtual para os discos montados.

## <a name="api-version"></a>Versão de API

As seguintes versões são suportadas com o armazenamento do Azure Stack:

APIs de serviços de armazenamento do Azure:

a atualização 1802 ou mais recente:

 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [07 de 2015-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Versões anteriores:

 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

APIs de gestão dos serviços de armazenamento do Azure:

 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>Versões do SDK

O armazenamento do Azure Stack suporta as seguintes bibliotecas de cliente:

| Biblioteca de cliente | Uma versão suportada do Azure Stack | Ligação                                                                                                                                                                                                                                                                                                                                     | Especificação de ponto final       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | De 6.2.0 para 8.7.0.          | Pacote de Nuget:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | ficheiro App. config              |
| Java           | De 4.1.0 para 6.1.0           | Pacote maven:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Configuração de cadeia de ligação      |
| Node.js        | De 1.1.0 para 2.7.0           | Ligação do NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Por exemplo: executar "npm instalar azure-storage@2.7.0")<br> <br>Versão do Github:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Declaração de instância de serviço |
| C++            | De 2.4.0 para 3.1.0           | Pacote de Nuget:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Configuração de cadeia de ligação      |
| PHP            | De 0.15.0 para 1.0.0          | Versão do GitHub:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Instalar através do compositor (ver detalhes abaixo)                                                                                                                                                                                                                  | Configuração de cadeia de ligação      |
| Python         | De 0.30.0 para 1.0.0          | Versão do GitHub:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Declaração de instância de serviço |
| Ruby           | De 0.12.1 para 1.0.1          | Pacote do RubyGems:<br>Comuns:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Fila: https://rubygems.org/gems/azure-storage-queue/<br>Tabela: https://rubygems.org/gems/azure-storage-table/<br> <br>Versão do GitHub:<br>https://github.com/Azure/azure-storage-ruby/releases | Configuração de cadeia de ligação      |

## <a name="next-steps"></a>Passos Seguintes

* [Introdução às ferramentas de desenvolvimento do armazenamento do Azure Stack](azure-stack-storage-dev.md)
* [Introdução ao armazenamento do Azure Stack](azure-stack-storage-overview.md)
