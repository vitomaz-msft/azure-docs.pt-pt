---
title: Importar um ficheiro do Power BI Desktop para o Azure Analysis Services | Documentos da Microsoft
description: Descreve como importar um ficheiro do Power BI Desktop (pbix) com o portal do Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3adf0c9c2e2b264904e66b82716447d634aaeee7
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209654"
---
# <a name="import-a-power-bi-desktop-file"></a>Importar um ficheiro do Power BI Desktop

Pode importar um modelo de dados no ficheiro do Power BI Desktop (pbix) para o Azure Analysis Services. Metadados de modelo, dados em cache e ligações de origem de dados são importadas. Relatórios e visualizações não são importadas. Importar dados de modelos do Power BI Desktop são no nível de compatibilidade 1400.

> [!IMPORTANT]
> Esta funcionalidade foi preterida. Pode ser removida ou alterada significativamente numa atualização futura. Recomenda-se a que descontinuar a utilizar esta funcionalidade em projetos novos e existentes para manter a compatibilidade com futuras atualizações. Para mais avançados modelo desenvolvimento e teste, é melhor usar o Visual Studio (SSDT) e o SQL Server Management Studio (SSMS).

**Restrições**   


- Se seu modelo de dados é criado no Power BI Desktop (2.60.5169.3201) de atualização de Julho de 2018 ou posterior, certifique-se que sem funcionalidades de pré-visualização estão ativadas. Funcionalidades de pré-visualização ainda não são suportadas no Azure Analysis Services. Se receber o seguinte erro ao importar, o ficheiro pbix tem funcionalidades de pré-visualização ativadas que ainda não são suportadas no Azure Analysis Services.

    ![Aviso do nível de compatibilidade](./media/analysis-services-import-pbix/aas-import-pbix-cl-warning.png)   
- Tem de ter permissões de administrador do servidor para importar a partir de um ficheiro pbix.
- O modelo de pbix pode ligar à **base de dados do Azure SQL** e **Azure SQL Data Warehouse** apenas origens de dados.
- O modelo de pbix não pode ter ao vivo ou ligações do DirectQuery. 


## <a name="to-import-from-pbix"></a>Para importar a partir do pbix

1. No seu servidor **descrição geral** > **Web designer**, clique em **aberto**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Na **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Na **novo modelo**, escreva um nome de modelo e, em seguida, selecione o ficheiro do Power BI Desktop.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. Na **importação**, localize e selecione o seu ficheiro.

     ![Ligue-se a caixa de diálogo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="change-credentials"></a>Alterar credenciais

Ao importar um modelo de dados de um ficheiro pbix, por predefinição, as credenciais utilizadas para ligar a uma origem de dados são definidas como ServiceAccount. Depois de um modelo foi importado de um pbix, pode alterar as credenciais através dos seguintes métodos:

- Utilização de Julho de 2018 (versão 17.8.1) ou versão posterior do SSMS para editar credenciais. Esta é a maneira mais fácil.
- Utilizar TMSL [Alter comando](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/alter-command-tmsl) sobre o [objeto de origens de dados](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/datasources-object-tmsl) para modificar a propriedade de cadeia de ligação. 
- Abrir o modelo no Visual Studio, edite as credenciais para a ligação de origem de dados e, em seguida, voltar a implementar o modelo.

Para alterar as credenciais com o SSMS. 

1. No SSMS, expanda a base de dados > **ligações**. 
2. Com o botão direito a ligação de base de dados e, em seguida, clique em **atualizar credenciais**. 

    ![Atualizar credenciais](./media/analysis-services-import-pbix/aas-import-pbix-creds.png)

3. Na caixa de diálogo credenciais, selecione um tipo de credencial e introduza as credenciais. Para a autenticação de SQL, selecione a base de dados. Para a conta de organização (OAuth), selecione a conta Microsoft.
    ![Editar credenciais](./media/analysis-services-import-pbix/aas-import-pbix-edit-creds.png)

A versão de Julho de 2018 do Power BI Desktop inclui uma nova funcionalidade para alterar as permissões de origem de dados. Sobre o **home page** separador, clique em **editar consultas**  > **definições da origem de dados**. Selecione a ligação de origem de dados e, em seguida, clique em **editar permissões**.


## <a name="see-also"></a>Consulte também

[Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
[Ligar ao Azure Analysis Services](analysis-services-connect.md)  
