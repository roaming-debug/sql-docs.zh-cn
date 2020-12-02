---
description: Analysis Services 处理任务
title: Analysis Services 处理任务 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.asprocessingtask.f1
- sql13.dts.designer.asprocessingtask.general.f1
- sql13.dts.designer.asprocessingtask.as.f1
helpviewer_keywords:
- Analysis Services Processing task
- processing objects [Integration Services]
ms.assetid: e5748836-b4ce-4e17-ab6b-617a336f02f4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2db38d2ad6c102b47b2a9f3349a3877a9d55a4c4
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123673"
---
# <a name="analysis-services-processing-task"></a>Analysis Services 处理任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务可负责处理 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象，如表格模型、多维数据集、维度和挖掘模型。  
  
 处理表格模型时，请记住以下事项：  
  
-   不能对表格模型执行影响分析。  
  
-   不公开表格模型的一些处理选项，如“处理碎片整理”和“处理重新计算”。 您可以使用执行 DDL 任务来执行这些功能。  
  
-   选项“处理索引”和“处理更新”不适用于表格模型，不应使用它们。  
  
-   对表格模型忽略批处理设置。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供了一些可执行商业智能操作的任务，如运行数据定义语言 (DDL) 语句和数据挖掘预测查询。 有关相关商业智能任务的详细信息，请单击以下主题之一：  
  
-   [Analysis Services 执行 DDL 任务](../../integration-services/control-flow/analysis-services-execute-ddl-task.md)  
  
-   [数据挖掘查询任务](../../integration-services/control-flow/data-mining-query-task.md)  
  
## <a name="object-processing"></a>对象处理  
 可以同时处理多个对象。 处理多个对象时，您需要定义处理批中所有对象时要应用的设置。  
  
 批中的对象可以按顺序处理或并行处理。 如果批中不包含必须按顺序处理的对象，则并行处理可加快处理的速度。 如果并行处理批中的对象，则可配置任务使其确定并行处理的对象数，也可以手动指定同时处理的对象数。 如果按顺序处理对象，则可通过将所有对象登记在一个事务中或对批中的每个对象使用单独的事务来设置批的事务属性。  
  
 在处理分析对象时，可能还要处理依赖于这些对象的对象。 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务还包含一个选项，可以在处理选定对象的同时，处理任意的相关对象。  
  
 通常，您应该在处理事实表之前处理维度表。 如果您试图在处理维度表之前处理事实表，则可能会遇到错误。  
  
 此任务还允许配置如何处理维度键中的错误。 例如，任务可忽略错误或在发生指定数量的错误后停止。 任务可使用默认错误配置，或者您可以构造自定义错误配置。 在自定义错误配置中，可以指定任务处理错误的方式和错误条件。 例如，可以指定发生第四个错误时停止运行任务，或指定任务处理 **Null** 键值的方式。 自定义错误配置还可以包含错误日志的路径。  
  
> [!NOTE]  
>  [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务只能处理使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具创建的分析对象。  
  
 此任务常与大容量插入任务或数据流任务结合使用，前者将数据加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表，后者实现将数据加载到表中的数据流。 例如，数据流任务可能具有数据流，该数据流从联机事务性数据库 (OLTP) 数据库中提取数据，并将数据加载到数据仓库中的事实数据表，然后调用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务来处理根据数据仓库构建的多维数据集。  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 处理任务使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器连接到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的实例。 有关详细信息，请参阅 [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md)。  
  
## <a name="error-handling"></a>错误处理  
  
## <a name="configuration-of-the-analysis-services-processing-task"></a>配置 Analysis Services 处理任务  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击以下主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-analysis-services-processing-task"></a>以编程方式配置 Analysis Services 处理任务  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   <xref:Microsoft.DataTransformationServices.Tasks.DTSProcessingTask.DTSProcessingTask>  
  
## <a name="analysis-services-processing-task-editor-general-page"></a>Analysis Services 处理任务编辑器（“常规”页）
  可以使用“Analysis Services 处理任务编辑器”对话框的“常规”页，对 Analysis Services 处理任务进行命名和说明 。  
  
### <a name="options"></a>选项  
 **名称**  
 为 Analysis Services 处理任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入 Analysis Services 处理任务的说明。  
  
## <a name="analysis-services-processing-task-editor-analysis-services-page"></a>Analysis Services 处理任务编辑器（Analysis Services 页）
  可以使用 **“Analysis Services 处理任务编辑器”** 对话框的 **Analysis Services** 页指定 Analysis Services 连接管理器，选择要处理的分析对象，以及设置处理选项和错误处理选项。  
  
 处理表格模型时，请记住以下事项：  
  
1.  不能对表格模型执行影响分析。  
  
2.  不公开表格模型的一些处理选项，如“处理碎片整理”和“处理重新计算”。 您可以使用执行 DDL 任务来执行这些功能。  
  
3.  提供的一些处理选项（如处理索引）不适用于表格模型，不应使用它们。  
  
4.  对表格模型忽略批处理设置。  
  
### <a name="options"></a>选项  
 **Analysis Services 连接管理器**  
 从列表中选择现有的 Analysis Services 连接管理器，或单击“新建”以创建新的连接管理器。  
  
 **新建**  
 创建新的 Analysis Services 连接管理器。  
  
 **相关主题：** [Analysis Services 连接管理器](../../integration-services/connection-manager/analysis-services-connection-manager.md)、[“添加 Analysis Services 连接管理器”对话框 UI 参考](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)  
  
 **对象列表**  
 
|属性|说明|  
|--------------|-----------------|  
|**Object Name**|列出指定对象的名称。|  
|**类型**|列出指定对象的类型。|  
|**处理选项**|从列表中选择处理选项。<br /><br /> **相关主题**：[处理多维模型 (Analysis Services)](/analysis-services/multidimensional-models/processing-a-multidimensional-model-analysis-services)|  
|**设置**|列出指定对象的处理设置。|  
  
 **添加**  
 将 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象添加到列表中。  
  
 **删除**  
 选择对象，再单击“删除”。  
  
 **影响分析**  
 对所选对象进行影响分析。  
  
 **相关主题：** [“影响分析”对话框（Analysis Services - 多维数据）](/analysis-services/analysis-services-overview?viewFallbackFrom=sql-server-ver15)  
  
 **批设置摘要**  
 
|属性|描述|  
|--------------|-----------------|  
|**处理顺序**|指定是按顺序处理对象还是按批处理对象；如果使用并行处理，则指定要并发处理的对象数。|  
|**事务模式**|指定按顺序处理时的事务模式。|  
|**维度错误**|指定在发生错误时的任务行为。|  
|**维度键错误日志路径**|指定记录错误的文件的路径。|  
|**处理受影响的对象**|指示是否还处理依赖对象或受影响的对象。|  
  
 **更改设置**  
 更改处理选项以及对维度键中错误的处理方式。  
  
 **相关主题：** [“更改设置”对话框（Analysis Services - 多维数据）](/analysis-services/analysis-services-overview?viewFallbackFrom=sql-server-ver15)