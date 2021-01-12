---
description: 分区处理目标
title: 分区处理目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.partitionprocessingdest.f1
- sql13.dts.designer.partprocessingtransformation.connection.f1
- sql13.dts.designer.partprocessingtransformation.mapping.f1
- sql13.dts.designer.partprocessingtransformation.advanced.f1
helpviewer_keywords:
- partitions [Analysis Services], processing
- Partition Processing destination [Integration Services]
- destinations [Integration Services], Partition Processing
ms.assetid: 36c592ff-3f78-4a58-b496-31c1c8eee131
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a3ee00006285ff8959ecba27624a491954e5a1bc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091637"
---
# <a name="partition-processing-destination"></a>分区处理目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  分区处理目标加载并处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 分区。 有关分区的详细信息，请参阅[分区（Analysis Services - 多维数据）](/analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data)。  
  
 分区处理目标包含下列功能：  
  
-   用于执行增量处理、完全处理或更新处理的选项。  
  
-   用于指定处理是忽略错误，还是在发生指定数量的错误后停止的错误配置。  
  
-   输入列到分区列的映射。  
  
 有关处理 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象的详细信息，请参阅[处理选项和设置 (Analysis Services)](/analysis-services/multidimensional-models/processing-options-and-settings-analysis-services)。  
  
> [!NOTE]  
>  此处所述的任何不适用于 Analysis Services 表格模型。  你无法将输入列映射到表格模型的分区列。 您可以改用 [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) 处理分区。  
  
## <a name="configuration-of-the-partition-processing-destination"></a>分区处理目标的配置  
 分区处理目标使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器连接到 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目或包含目标所处理的多维数据集和分区的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。 有关详细信息，请参阅 [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md)。  
  
 此目标有一个输入。 它不支持错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [分区处理目标自定义属性](../../integration-services/data-flow/partition-processing-destination-custom-properties.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="partition-processing-destination-editor-connection-manager-page"></a>分区处理目标编辑器（“连接管理器”页）
  使用“分区处理目标编辑器”对话框的“连接管理器”页面，可以指定与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例之间的连接。  
  
> [!NOTE]  
>  此处所述的任何不适用于 Analysis Services 表格模型。  你无法将输入列映射到表格模型的分区列。 您可以改用 [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) 处理分区。  
  
### <a name="options"></a>选项  
 **“ODBC 目标编辑器”**  
 从列表中选择一个现有连接管理器，或通过单击“新建”  创建一个新连接。  
  
 **新建**  
 通过使用“添加 Analysis Services 连接管理器”对话框创建一个新连接。  
  
 **可用分区列表**  
 选择要处理的分区。  
  
 **处理方法**  
 选择处理方法。 此选项的默认值为 **“完全”**。  
  
|值|描述|  
|-----------|-----------------|  
|添加(增量式)|对分区执行增量处理。|  
|完全|对分区执行完全处理。|  
|仅限数据|对分区执行更新处理。|  
  
## <a name="partition-processing-destination-editor-mappings-page"></a>分区处理目标编辑器（“映射”页）
  可以使用 **“分区处理目标编辑器”** 对话框的 **“映射”** 页，将输入列映射到分区列。  
  
> [!NOTE]  
>  此处所述的任何不适用于 Analysis Services 表格模型。  你无法将输入列映射到表格模型的分区列。 您可以改用 [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) 处理分区。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 使用拖放操作可以将表中的可用输入列映射到目标列。  
  
 **可用目标列**  
 查看可用目标列的列表。 使用拖放操作可以将表中的可用目标列映射到输入列。  
  
 **输入列**  
 查看从上表中选择的输入列。 可以通过使用 **“可用输入列”** 列表来更改映射。  
  
 **目标列**  
 查看每个可用的目标列，包括已映射或未映射的目标列。  
  
## <a name="partition-processing-destination-editor-advanced-page"></a>分区处理目标编辑器（“高级”页）
  可以使用 **“分区处理目标编辑器”** 对话框的 **“高级”** 页配置错误处理方式。  
  
> [!NOTE]  
>  此处所述的任何不适用于 Analysis Services 表格模型。  你无法将输入列映射到表格模型的分区列。 您可以改用 [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) 处理分区。  
  
### <a name="options"></a>选项  
 **使用默认错误配置**  
 指定是否使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的默认错误处理方式。 默认情况下，此值为 **True**。  
  
 **键错误操作**  
 指定如何处理包含不可接受的键值的记录。  
  
|值|描述|  
|-----------|-----------------|  
|**ConvertToUnknown**|将无法接受的键值转换为 Unknown 值。|  
|**DiscardRecord**|放弃记录。|  
  
 **忽略错误**  
 指定将忽略错误。  
  
 **出错时停止**  
 指定在出现错误时应停止处理。  
  
 **错误数**  
 如果选择了“出错时停止”，请指定应停止处理的错误阈值。  
  
 **出错时要执行的操作**  
 如果选择了“出错时停止”，请指定在达到错误阈值时要执行的操作。  
  
|值|描述|  
|-----------|-----------------|  
|**StopProcessing**|停止处理。|  
|**StopLogging**|停止记录错误。|  
  
 **找不到键**  
 指定在出现“找不到键”错误时执行的操作。 默认情况下，此值为 **ReportAndContinue**。  
  
|值|描述|  
|-----------|-----------------|  
|**IgnoreError**|忽略错误并继续处理。|  
|**ReportAndContinue**|报告错误并继续处理。|  
|**ReportAndStop**|报告错误并停止处理。|  
  
 **重复键**  
 指定在出现“重复键”错误时执行的操作。 默认情况下，此值为 **IgnoreError**。  
  
|值|描述|  
|-----------|-----------------|  
|**IgnoreError**|忽略错误并继续处理。|  
|**ReportAndContinue**|报告错误并继续处理。|  
|**ReportAndStop**|报告错误并停止处理。|  
  
 **空键转换为未知键**  
 指定在将空键转换为 Unknown 值后所采取的操作。 默认情况下，此值为 **IgnoreError**。  
  
|值|描述|  
|-----------|-----------------|  
|**IgnoreError**|忽略错误并继续处理。|  
|**ReportAndContinue**|报告错误并继续处理。|  
|**ReportAndStop**|报告错误并停止处理。|  
  
 **不允许空键**  
 指定在不允许空键而又遇到空键时执行的操作。 默认情况下，此值为 **ReportAndContinue**。  
  
|值|描述|  
|-----------|-----------------|  
|**IgnoreError**|忽略错误并继续处理。|  
|**ReportAndContinue**|报告错误并继续处理。|  
|**ReportAndStop**|报告错误并停止处理。|  
  
 **错误日志路径**  
 键入错误日志的路径，或者使用浏览按钮 **(...)** 选择目标。  
  
 **浏览(...)**  
 选择错误日志的路径。  
  
## <a name="see-also"></a>另请参阅  
 [Integration Services 错误和消息引用](../../integration-services/integration-services-error-and-message-reference.md)