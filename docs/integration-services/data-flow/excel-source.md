---
description: Excel 源
title: Excel 源 | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.excelsource.f1
- sql13.dts.designer.excelsourceadapter.connection.f1
- sql13.dts.designer.excelsourceadapter.columns.f1
- sql13.dts.designer.excelsourceadapter.erroroutput.f1
helpviewer_keywords:
- Excel [Integration Services]
- sources [Integration Services], Excel
ms.assetid: e66349f3-b1b8-4763-89b7-7803541a4d62
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e00a786dfb9d9ffe55274822b276a2d2497efd82
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127184"
---
# <a name="excel-source"></a>Excel 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Excel 源从 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel 工作簿的工作表或范围中提取数据。  

> [!IMPORTANT]
> 有关连接到 Excel 文件的详细信息，以及从 Excel 文件加载数据或将数据加载到 Excel 文件的限制和已知问题，请参阅[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)。

## <a name="access-modes"></a>访问模式
 Excel 源提供了四种提取数据的数据访问方式：  
  
-   表或视图。  
  
-   变量中指定的表或视图。  
  
-   SQL 语句的运行结果。 查询可以是参数化查询。  
  
-   存储在变量中的 SQL 语句的运行结果。  
  
 Excel 源使用 Excel 连接管理器与数据源建立连接，连接管理器可指定要使用的工作簿文件。 有关详细信息，请参阅 [Excel Connection Manager](../../integration-services/connection-manager/excel-connection-manager.md)。  
  
 Excel 源有一个常规输出和一个错误输出。  
  
## <a name="excel-source-configuration"></a>Excel 源配置  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了所有能以编程方式设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Excel 自定义属性](../../integration-services/data-flow/excel-custom-properties.md)  
  
 有关循环遍历 Excel 文件中的某个组的信息，请参阅 [使用 Foreach 循环容器，循环遍历 Excel 文件和表](../../integration-services/control-flow/loop-through-excel-files-and-tables-by-using-a-foreach-loop-container.md)。  
  
## <a name="excel-source-editor-connection-manager-page"></a>Excel 源编辑器（“连接管理器”页）
  使用 **“Excel 源编辑器”** 对话框的 **“连接管理器”** 节点可以为源选择要使用的 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 工作簿。 Excel 源从现有工作簿中的工作表或指定范围中读取数据。  
  
> [!NOTE]  
>  Excel 源的 **CommandTimeout** 属性未在 **“Excel 源编辑器”** 中提供，但可以使用 **“高级编辑器”** 进行设置。 有关此属性的详细信息，请参阅 [Excel Custom Properties](../../integration-services/data-flow/excel-custom-properties.md)的“Excel 源”部分。  
  
### <a name="static-options"></a>静态选项  
 **“无缓存”**  
 从列表中选择现有的 Excel 连接管理器，或单击“新建”创建新连接。  
  
 **新建**  
 使用“Excel 连接管理器”对话框创建一个新连接管理器。  
  
 **数据访问模式**  
 指定从源选择数据的方法。  
  
|值|说明|  
|-----------|-----------------|  
|表或视图|从 Excel 文件的工作表或指定范围中检索数据。|  
|表名变量或视图名变量|在变量中指定工作表名称或范围名称。<br /><br /> **相关信息：** [在包中使用变量](../integration-services-ssis-variables.md)|  
|SQL 命令|使用 SQL 查询从 Excel 文件中检索数据。 |  
|变量中的 SQL 命令|在变量中指定 SQL 查询文本。|  
  
 **预览**  
 通过使用“数据视图”  对话框预览结果。 预览最多可以显示 200 行。  
  
### <a name="data-access-mode-dynamic-options"></a>数据访问模式动态选项  
  
#### <a name="data-access-mode--table-or-view"></a>数据访问模式 = 表或视图  
 **Excel 表的名称**  
 从 Excel 工作簿可用的工作表或指定范围中选择工作表或指定范围的名称。  
  
#### <a name="data-access-mode--table-name-or-view-name-variable"></a>数据访问模式 = 表名变量或视图名变量  
 **变量名称**  
 选择包含工作表名称或指定范围名称的变量。  
  
#### <a name="data-access-mode--sql-command"></a>数据访问模式 = SQL 命令  
 **SQL 命令文本**  
 输入 SQL 查询的文本，通过单击“生成查询”来生成查询，或通过单击“浏览”浏览至包含查询文本的文件。  
  
 **参数**  
 如果已经在参数化查询文本中使用 ? 作为参数占位符输入了参数化查询，请使用 **“设置查询参数”** 对话框将查询输入参数映射到包变量。  
  
 **生成查询**  
 使用“查询生成器”  对话框可直观地构造 SQL 查询。  
  
 **“浏览”**  
 使用“打开”  对话框可定位到包含 SQL 查询文本的文件。  
  
 **分析查询**  
 验证查询文本的语法。  
  
#### <a name="data-access-mode--sql-command-from-variable"></a>数据访问模式 = 变量中的 SQL 命令  
 **变量名称**  
 选择包含 SQL 查询文本的变量。  
  
## <a name="excel-source-editor-columns-page"></a>Excel 源编辑器（“列”页）
  可以使用“Excel 源编辑器”对话框的“列”页，将输出列映射到每个外部（源）列。  
  
### <a name="options"></a>选项  
 **可用外部列**  
 查看数据源中可用外部列的列表。 无法使用此表添加或删除列。  
  
 **“外部列”**  
 按任务读取外部（源）列的顺序查看这些列。 首先在上面讨论的表中清除所选择的列，然后以不同的顺序从列表中选择外部列，即可更改顺序。  
  
 **输出列**  
 为每个输出列提供唯一的名称。 默认值为所选外部（源）列的名称；不过，您也可以任选一个唯一的描述性名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
## <a name="excel-source-editor-error-output-page"></a>Excel 源编辑器（“错误输出”页）
  可以使用 **“Excel 源编辑器”** 对话框的 **“错误输出”** 页选择错误处理选项以及设置错误输出列的属性。  
  
### <a name="options"></a>选项  
 **输入或输出**  
 查看数据源的名称。  
  
 **列**  
 查看在“Excel 源编辑器”对话框中“连接管理器”页上选择的外部（源）列。  
  
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
  
## <a name="related-content"></a>相关内容  
[使用 SQL Server Integration Services (SSIS) 从 Excel 加载数据或将数据加载到 Excel 中](../load-data-to-from-excel-with-ssis.md)  
[Excel 目标](excel-destination.md)  
[Excel 连接管理器](../connection-manager/excel-connection-manager.md)