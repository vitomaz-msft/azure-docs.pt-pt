---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: b06c57f50f759677770839ae06cb05e4de36f84c
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093283"
---
### <a name="prerequisites"></a>Pré-requisitos
* R [SendGrid](https://www.SendGrid.com/) conta 

Antes de poder utilizar a sua conta do SendGrid numa aplicação lógica, tem de autorizar a aplicação lógica para ligar à sua conta do SendGrid. Felizmente, pode fazer isso facilmente de na sua aplicação lógica no Portal do Azure. 

Eis os passos para autorizar a aplicação lógica para ligar à sua conta do SendGrid:

1. Para criar uma ligação ao SendGrid, no Estruturador da aplicação lógica, selecione **APIs geridas da Microsoft mostram** na lista pendente, em seguida, introduza *SendGrid* na caixa de pesquisa. Selecione o acionador ou ação que irá gostar a utilizar:  
   ![Passo 1 da SendGrid](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Se ainda não criou quaisquer ligações ao SendGrid antes, obter-lhe-á pedido para fornecer as credenciais do SendGrid. Estas credenciais serão utilizadas para autorizar a aplicação lógica para ligar a e acedam a dados da sua conta do SendGrid:  
   ![Passo 2 da SendGrid](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Observe que a ligação foi criada e agora está livre para continuar com os outros passos na sua aplicação lógica:  
   ![Passo 3 da SendGrid](./media/connectors-create-api-sendgrid/sendgrid-3.png)   

