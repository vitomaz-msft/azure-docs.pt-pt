---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 7f5b829399004a58ba84dc9abc34b2c9dd544fbf
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227089"
---
1. Instalar dapl, rdmacm, ibverbs e mlx4

  ```bash
  sudo apt-get update

  sudo apt-get install libdapl2 libmlx4-1

  ```

2. Em /etc/waagent.Conf., ative o RDMA, removendo os comentários as seguintes linhas de configuração. Precisa de acesso de raiz para editar este ficheiro.
  
  ```
  OS.EnableRDMA=y

  OS.UpdateRdmaDriver=y
  ```

3. Adicione ou altere as seguintes definições de memória no artigo BDC no ficheiro /etc/security/limits.conf. Precisa de acesso de raiz para editar este ficheiro. Para fins de teste, pode definir memlock para ilimitado. Por exemplo: `<User or group name>   hard    memlock   unlimited`.

  ```
  <User or group name> hard    memlock <memory required for your application in KB>

  <User or group name> soft    memlock <memory required for your application in KB>
  ```
  
4. Instale a biblioteca do Intel MPI. Qualquer um dos [compre e baixe](https://software.intel.com/intel-mpi-library/) a biblioteca da Intel ou transferir o [versão de avaliação gratuita](https://registrationcenter.intel.com/en/forms/?productid=1740).

  ```bash
  wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/9278/l_mpi_p_5.1.3.223.tgz
  ```
 
 São suportados apenas os runtimes de 5.x do Intel MPI.
 
 Para obter os passos de instalação, consulte a [guia de instalação do Intel MPI biblioteca](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html).

5. Ative ptrace para processos de não-depurador não raiz (necessária para as versões mais recentes do Intel MPI).
 
  ```bash
  echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
  ```
