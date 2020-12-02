---
description: Excel 目标
title: Excel 目标 | Microsoft Docs
ms.custom: ''
ms.date: 04/02/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.exceldest.f1
- sql13.dts.designer.exceldestadapter.connection.f1
- sql13.dts.designer.exceldestadapter.mappings.f1
- sql13.dts.designer.exceldestadapter.erroroutput.f1
helpviewer_keywords:
- destinations [Integration Services], Excel
- Excel [Integration Services]
ms.assetid: 37c07446-1264-4814-b4f5-9c66d333bb24
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 85b62ff41c9b413c49ff6312e918f648fba430cd
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127292"
---
# <a name="excel-destination"></a>Excel 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Excel 目标将数据加载到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 工作簿中的工作表或范围中。  

> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)。
  
## <a name="access-modes"></a>访问模式  
 Excel 目标为数据加载提供了三种数据访问模式：  
  
-   表或视图。  
  
-   变量中指定的表或视图。  
  
-   SQL 语句的运行结果。 查询可以是参数化查询。  
  
## <a name="configure-the-excel-destination"></a>配置 Excel 目标  
 Excel 目标使用 Excel 连接管理器连接到数据源，而连接管理器指定要使用的工作簿文件。 有关详细信息，请参阅 [Excel Connection Manager](../../integration-services/connection-manager/excel-connection-manager.md)。  
  
 Excel 目标具有一个常规输入和一个错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了所有能以编程方式设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Excel 自定义属性](../../integration-services/data-flow/excel-custom-properties.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="excel-destination-editor-connection-manager-page"></a>Excel 目标编辑器（“连接管理器”页）
  使用 **“Excel 目标编辑器”** 对话框的 **“连接管理器”** 页可以指定数据源信息和预览结果。 Excel 目标将数据加载到 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 工作簿中的工作表或指定范围。  
  
> [!NOTE]  
>  Excel 目标的 **CommandTimeout** 属性未在 **“Excel 目标编辑器”** 中提供，但可以使用 **“高级编辑器”** 进行设置。 另外，某些快速加载选项仅在 **“高级编辑器”** 中提供。 有关这些属性的详细信息，请参阅 [Excel Custom Properties](../../integration-services/data-flow/excel-custom-properties.md)的“Excel 目标”部分。  
  
### <a name="static-options"></a>静态选项  
 **Excel 连接管理器**  
 从列表中选择现有的 Excel 连接管理器，或单击“新建”创建新连接。  
  
 **新建**  
 使用“Excel 连接管理器”对话框创建一个新连接管理器。  
  
 **数据访问模式**  
 指定从源选择数据的方法。  
  
|选项|说明|  
|------------|-----------------|  
|表或视图|将数据加载到 Excel 数据源中的工作表或指定范围。|  
|表名变量或视图名变量|在变量中指定工作表名称或范围名称。<br /><br /> **相关信息**：[在包中使用变量](../integration-services-ssis-variables.md)|  
|SQL 命令|使用 SQL 查询将数据加载到 Excel 目标中。|  
  
 **Excel 表的名称**  
 从下拉列表中选择 Excel 目标。 如果此列表为空，请单击 **“新建”**。  
  
 **新建**  
 单击“新建”将启动“创建表”对话框。 当您单击 **“确定”** 时，此对话框将创建 **“Excel 连接管理器”** 指向的 Excel 文件。  
  
 **查看现有数据**  
 使用“预览查询结果”  对话框预览结果。 预览最多可以显示 200 行。  
  
### <a name="data-access-mode-dynamic-options"></a>数据访问模式动态选项  
  
#### <a name="data-access-mode--table-or-view"></a>数据访问模式 = 表或视图  
 **Excel 表的名称**  
 从数据源中可用的工作表或指定范围列表中选择工作表或指定范围的名称。  
  
#### <a name="data-access-mode--table-name-or-view-name-variable"></a>数据访问模式 = 表名变量或视图名变量  
 **变量名称**  
 选择包含工作表名称或指定范围名称的变量。  
  
#### <a name="data-access-mode--sql-command"></a>数据访问模式 = SQL 命令  
 **SQL 命令文本**  
 输入 SQL 查询的文本，通过单击“生成查询”来生成查询，或通过单击“浏览”定位到包含查询文本的文件。  
  
 **生成查询**  
 使用“查询生成器”  对话框可直观地构造 SQL 查询。  
  
 **“浏览”**  
 使用“打开”  对话框可定位到包含 SQL 查询文本的文件。  
  
 **分析查询**  
 验证查询文本的语法。  
  
## <a name="excel-destination-editor-mappings-page"></a>Excel 目标编辑器（“映射”页）
  可以使用 **“Excel 目标编辑器”** 对话框的 **“映射”** 页，将输入列映射到目标列。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 使用拖放操作可以将表中的可用输入列映射到目标列。  
  
 **可用目标列**  
 查看可用目标列的列表。 使用拖放操作可以将表中的可用目标列映射到输入列。  
  
 **输入列**  
 查看从上表中选择的输入列。 可以通过使用 **“可用输入列”** 列表来更改映射。  
  
 **目标列**  
 查看每个可用的目标列，包括已映射或未映射的目标列。  
  
## <a name="excel-destination-editor-error-output-page"></a>Excel 目标编辑器（“错误输出”页）
  可以使用 **“Excel 目标编辑器”** 对话框的 **“高级”** 页指定错误处理选项。  
  
### <a name="options"></a>选项  
 **输入或输出**  
 查看数据源的名称。  
  
 **列**  
 查看在“Excel 源编辑器”对话框中“连接管理器”节点上选择的外部（源）列。  
  
 **错误**  
 指定发生错误时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **相关主题：** [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **截断**  
 指定发生截断时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **说明**  
 查看对错误的说明。  
  
 **将此值设置到选定的单元格**  
 指定发生错误或截断时应对所有选定单元格执行的操作：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
## <a name="see-also"></a>另请参阅  
 [使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)  
 [Excel 源](../../integration-services/data-flow/excel-source.md)   
[Excel 连接管理器](../connection-manager/excel-connection-manager.md)