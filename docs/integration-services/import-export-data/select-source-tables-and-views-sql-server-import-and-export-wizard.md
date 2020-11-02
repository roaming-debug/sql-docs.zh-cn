---
description: 选择源表和源视图（SQL Server 导入和导出向导）
title: 选择源表和视图（SQL Server 导入和导出向导）| Microsoft Docs
ms.custom: ''
ms.date: 04/02/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.impexpwizard.selectsourcetablesandviews.f1
ms.assetid: f60e1a19-2ea6-403c-89ab-3e60ac533ea0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 57485f68a1e9418e3d9d2402257599bf54e1ad7b
ms.sourcegitcommit: fb8724fb99c46ecf3a6d7b02a743af9b590402f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92439311"
---
# <a name="select-source-tables-and-views-sql-server-import-and-export-wizard"></a>选择源表和源视图（SQL Server 导入和导出向导）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  指定要复制整个表或提供查询之后， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 导入和导出向导会显示“选择源表和源视图”  。 在此页上，选择想要复制的现有表和视图。 然后将源表映射到新的或现有的目标表。 或者，还可查看单个列的映射并预览示例数据。

> [!TIP]
> 如果必须复制多个 SQL Server 数据库或 SQL Server 数据库对象（而非表和视图），请使用复制数据库向导，而不是使用导入和导出向导。 有关详细信息，请参阅 [使用复制数据库向导](../../relational-databases/databases/use-the-copy-database-wizard.md)。  
  
## <a name="screen-shot---if-youre-going-to-copy-tables"></a>屏幕截图 — 如果正准备复制表  
 以下屏幕截图显示一个示例，示例内容为之前在“指定表复制或查询”  页上选择“从一个或多个表或视图复制数据”  选项时，向导的“选择源表和视图”  页。 在列表中，可以看到数据源中可用的所有表和视图。
 
在此示例中，“源”  列表包含 AdventureWorks 示例数据库中的所有表。 所选的行显示用户要将 Sales.Customer  表从源复制到目标上的 Sales.CustomerNew  表。 
   
 ![屏幕截图显示了要复制表时的“导入和导出向导”的“选择表”页面。](../../integration-services/import-export-data/media/select-tables1.png "导入和导出向导的选择表页")
  
## <a name="screen-shot---if-you-provided-a-query"></a>屏幕截图 — 如果提供了查询  
 以下屏幕截图显示一个示例，内容为在“指定表复制或查询”页上选择了“编写查询以指定要传输的数据”选项时，向导的“选择源表和视图”页  。 “源”  列表仅包含一行，其中名为 `[Query]` 的项表示在“提供源查询”  页上提供的查询。
 
在此示例中，用户要将查询结果从源复制到目标上的 **Sales.CustomerNew** 表。  
    
 ![屏幕截图显示了已提供查询时的“导入和导出向导”的“选择表”页面。](../../integration-services/import-export-data/media/select-tables2.png "导入和导出向导的选择表页")  

## <a name="select-source-and-destination-tables"></a>选择源表和目标表 
**Source**  
使用这些复选框，可以从可用表和视图的列表中进行选择，以复制到目标。 默认情况下，可在不更改的情况下复制数据源中的数据。 如果创建新的目标表，会同时从数据源原样复制新表的架构，即列及其属性的列表。

如果提供查询，则列表将仅包含名为 `[Query]` 的项。 

**目标**  
 从列表中为每个源表或查询选择目标表，或输入希望向导创建的新表的名称。 如果选择现有目标表，则该表必须具有与源数据兼容的数据类型的列。  

> [!NOTE]
> 如果此时在向导中暂停操作以使用外部工具（如  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]）在目标数据库中手动创建新表，则新表不会立即出现在可用目标表的列表中。 若要刷新目标表的列表，请退回到“选择目标”  页，重新选择目标数据库以刷新可用表和视图的列表，然后再次前进到“选择源表和源视图”  页。  

## <a name="optionally-review-column-mappings-and-preview-data"></a>（可选）查看列映射和预览数据
**编辑映射**   
（可选）对所选表单击“编辑映射”以显示“列映射”对话框。 使用“列映射”  对话框可执行以下操作，
-   查看单个列在源与目标之间的映射。
-   通过对不想复制的列选择“忽略”  ，可以仅复制列的子集。

有关详细信息，请参阅 [列映射](../../integration-services/import-export-data/column-mappings-sql-server-import-and-export-wizard.md)。  

**预览**  
（可选）单击“预览”以在“预览数据”对话框中预览最多 200 行的示例数据 。 这会确认向导将复制你想要复制的数据。 有关详细信息，请参阅 [预览数据](../../integration-services/import-export-data/preview-data-dialog-box-sql-server-import-and-export-wizard.md)。  
  
预览数据后，你可能想更改之前在向导页面中选择的选项。 若要进行这些更改，请返回到“选择源表和源视图”  页，单击“后退”  返回到前面可以更改选项的页面。  

## <a name="select-source-and-destination-tables-for-excel"></a>选择 Excel 的源表和目标表

> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)。

### <a name="excel-source-tables"></a>Excel 源表
Excel 数据源的源表和视图的列表包括两种类型的 Excel 对象。
-   **工作表** 。 工作表名称后跟美元符号 ($) ，例如， **“Sheet1$”** 。
-   **命名区域。** 命名区域（如有）按名称列出。

如果想要加载来自特定的、未命名的单元格区域的数据（或将数据加载到其中）- 例如，来自或加载到 **[Sheet1$ A1: B4]** ，则必须编写查询。 后退到“指定表复制或查询”  页，选择“编写查询以指定要传输的数据”  。

### <a name="excel-destination-tables"></a>Excel 目标表
如果将数据导出到 Excel，可以通过以下三种方式之一来指定目标。
-   **工作表。** 若要指定工作表，请将 $ 字符附加到表名称的末尾，并用分隔符包围字符串 - 例如， **[Sheet1$]** 。
-   **命名区域。** 若要指定命名区域，只需使用区域名称 - 例如， **MyDataRange** 。
-   **未命名区域。** 若要指定未命名的单元格区域，请将 $ 字符附加到表名称的末尾，添加区域说明，并用分隔符包围字符串 - 例如， **[Sheet1$ A1: B4]** 。

> [!TIP]
> 使用 Excel 作为源或目标时，最好单击“编辑映射”  并在“列映射”  页查看数据类型映射。 

## <a name="whats-next"></a>下一步操作  
 选择现有表和视图以进行复制并将它们映射到其目标之后，下一页是“保存并运行包”  。 在此页上，你可以指定是否要立即运行复制操作。 根据配置，你还可保存向导创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包，以便对其进行自定义和以后重新使用。 有关详细信息，请参阅 [保存并运行包](../../integration-services/import-export-data/save-and-run-package-sql-server-import-and-export-wizard.md)。
 
 ## <a name="see-also"></a>另请参阅
[导入和导出向导的简单示例入门](../../integration-services/import-export-data/get-started-with-this-simple-example-of-the-import-and-export-wizard.md)  
[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)



