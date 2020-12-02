---
description: 数据挖掘查询任务
title: 数据挖掘查询任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataminingquerytask.f1
- sql13.dts.designer.dmquerytask.miningmodel.f1
- sql13.dts.designer.dmquerytask.query.f1
- sql13.dts.designer.dmquerytask.output.f1
helpviewer_keywords:
- prediction queries [Integration Services]
- Data Mining Query task [Integration Services]
ms.assetid: f489348c-2008-4f66-8c2c-c07c3029439a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1af5f497be96f5c8b9808878aea9e5275c77ddf7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123552"
---
# <a name="data-mining-query-task"></a>数据挖掘查询任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  数据挖掘查询任务根据 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]内置的数据挖掘模型运行预测查询。 预测查询通过使用挖掘模型来创建对新数据的预测。 例如，预测查询可以预测夏季可能销售多少帆板，或生成可能购买帆板的预期客户列表。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供执行其他商业智能操作的任务，如运行数据定义语言 (DDL) 语句和处理分析对象。  
  
 有关其他商业智能任务的详细信息，请单击下列主题之一：  
  
-   [Analysis Services 执行 DDL 任务](../../integration-services/control-flow/analysis-services-execute-ddl-task.md)  
  
-   [Analysis Services 处理任务](../../integration-services/control-flow/analysis-services-processing-task.md)  
  
## <a name="prediction-queries"></a>预测查询  
 查询是数据挖掘扩展 (DMX) 语句。 DMX 语言是 SQL 语言的扩展，为对挖掘模型的操作提供支持。 有关如何使用 DMX 语言的详细信息，请参阅[数据挖掘扩展插件 (DMX) 引用](../../dmx/data-mining-extensions-dmx-reference.md)。  
  
 该任务可以查询根据同一挖掘结构生成的多个挖掘模型。 挖掘模型是使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 提供的某种数据挖掘算法生成的。 数据挖掘查询任务引用的挖掘结构可能包含使用不同算法生成的多个挖掘模型。 有关详细信息，请参阅[挖掘结构（Analysis Services - 数据挖掘）](/analysis-services/data-mining/mining-structures-analysis-services-data-mining)和[数据挖掘算法（Analysis Services - 数据挖掘）](/analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining)。  
  
 数据挖掘查询任务运行的预测查询可返回单行或数据集结果。 返回单行的查询称为单独查询。例如，预测夏季可能销售多少帆船的查询将返回一个数字。 有关返回单行的预测查询的详细信息，请参阅 [数据挖掘查询工具](/analysis-services/data-mining/data-mining-query-tools)。  
  
 查询结果将保存到表中。 如果数据挖掘查询任务指定名称的表已经存在，则任务可以在相同名称后追加数字来创建新表，也可以覆盖表内容。  
  
 如果结果包含嵌套，则在保存前将结果简化。 通过简化结果可将嵌套结果集更改为表。 例如，简化包含 **Customer** 列和 **Product** 嵌套列的嵌套结果时，将在 **Customer** 列中添加行，从而生成包含每个客户的产品数据的表。 例如，购买三个不同产品的客户将形成一个三行的表，每行都重复出现此客户，并在每行中包含不同的产品。 如果省略了 FLATTENED 关键字，则表将只包含 **Customer** 列，而且每个客户只占一行。 有关详细信息，请参阅 [SELECT (DMX)](../../dmx/select-dmx.md)。  
  
## <a name="configuration-of-the-data-mining-query-task"></a>配置数据挖掘查询任务  
 数据挖掘查询任务需要两个连接。 第一个连接是 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器，连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的实例或包含挖掘结构和挖掘模型的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目。 第二个连接是 OLE DB 连接管理器，连接到包含任务要向其写入的表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。 有关详细信息，请参阅 [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md) 和 [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md)。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
> [!NOTE]  
>  数据挖掘查询编辑器没有“表达式”页。 它使用 **“属性”** 窗口来访问工具，以便创建和管理数据挖掘查询任务的属性的属性表达式。  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-data-mining-query-task"></a>以编程方式配置数据挖掘查询任务  
 有关以编程方式设置这些属性的详细信息，请单击下列主题之一：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.DMQueryTask.DMQueryTask>  
  
## <a name="data-mining-query-task-editor-mining-model-tab"></a>数据挖掘查询任务编辑器（“挖掘模型”选项卡）
  可以使用 **“数据挖掘查询任务”** 对话框的 **“挖掘模型”** 选项卡指定要使用的挖掘结构和挖掘模型。  
  
 若要了解有关在包中实现数据挖掘的详细信息，请参阅 [数据挖掘查询任务](../../integration-services/control-flow/data-mining-query-task.md) 和 [数据挖掘解决方案](/analysis-services/data-mining/data-mining-solutions)。  
  
### <a name="general-options"></a>常规选项  
 **名称**  
 为数据挖掘查询任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入数据挖掘查询任务的说明。  
  
### <a name="mining-model-tab-options"></a>挖掘模型选项卡选项  
 **Connection**  
 从列表中选择 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器，或单击“新建”创建新的连接管理器。  
  
 **相关主题：**  [Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)  
  
 **新建**  
 创建新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器。  
  
 **相关主题：** [“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)  
  
 **挖掘结构**  
 从列表中选择挖掘结构。  
  
 **挖掘模型**  
 选择建立在所选挖掘结构上的挖掘模型。  

## <a name="data-mining-query-task-editor-query-tab"></a>数据挖掘查询任务编辑器（“查询”选项卡）
  可以使用 **“数据挖掘查询任务”** 对话框的 **“查询”** 选项卡，基于挖掘模式创建预测查询。 在此对话框中还可以将参数和结果集绑定到变量。  
  
 若要了解有关在包中实现数据挖掘的详细信息，请参阅 [数据挖掘查询任务](../../integration-services/control-flow/data-mining-query-task.md) 和 [数据挖掘解决方案](/analysis-services/data-mining/data-mining-solutions)。  
  
### <a name="general-options"></a>常规选项  
 **名称**  
 为数据挖掘查询任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入数据挖掘查询任务的说明。  
  
### <a name="build-query-tab-options"></a>“生成查询”选项卡选项  
 **数据挖掘查询**  
 键入数据挖掘查询。  
  
 **相关主题：**  [数据挖掘扩展插件 (DMX) 参考](../../dmx/data-mining-extensions-dmx-reference.md)  
  
 **生成新查询**  
 使用图形工具创建数据挖掘查询。  
  
 **相关主题：** [Data Mining Query](../../integration-services/control-flow/data-mining-query.md)  
  
### <a name="parameter-mapping-tab-options"></a>“参数映射”选项卡选项  
 **参数名称**  
 根据需要更新参数名称。 通过在 **“变量名称”** 列表中选择变量，将参数映射到变量。  
  
 **“变量名称”**  
 在列表中选择变量以将其映射到参数。  
  
 **添加**  
 将参数添加到列表。  
  
 **删除**  
 选择一个参数，再单击“删除”。  
  
### <a name="result-set-tab-options"></a>“结果集”选项卡选项  
 **结果名称**  
 根据需要更新结果集的名称。 通过在 **“变量名称”** 列表中选择变量，将结果映射到变量。  
  
 通过单击 **“添加”** 添加结果之后，为该结果提供唯一的名称。  
  
 **“变量名称”**  
 在列表中选择变量以将其映射到结果集。  
  
 **结果类型**  
 指示是返回单行还是返回完整结果集。  
  
 **添加**  
 向列表中添加结果集。  
  
 **删除**  
 选择一个结果，再单击“删除”。  
## <a name="data-mining-query-task-editor-output-tab"></a>数据挖掘查询任务编辑器（“输出”选项卡）
  可以使用 **“数据挖掘查询任务编辑器”** 对话框的 **“输出”** 选项卡指定预测查询的目标。  
  
 若要了解有关在包中实现数据挖掘的详细信息，请参阅 [数据挖掘查询任务](../../integration-services/control-flow/data-mining-query-task.md) 和 [数据挖掘解决方案](/analysis-services/data-mining/data-mining-solutions)。  
  
### <a name="general-options"></a>常规选项  
 **名称**  
 为数据挖掘查询任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入数据挖掘查询任务的说明。  
  
### <a name="output-tab-options"></a>“输出”选项卡选项  
 **Connection**  
 从列表中选择连接管理器，或单击“新建”以创建新的连接管理器。  
  
 **新建**  
 创建新的连接管理器。 只能使用 ADO.NET 和 OLE DB 连接管理器类型。  
  
 **输出表**  
 指定预测查询要将其结果写入的表。  
  
 **删除并重新创建该输出表**  
 指示预测查询是否应通过删除表后再重新创建表来覆盖目标表中的内容。  
