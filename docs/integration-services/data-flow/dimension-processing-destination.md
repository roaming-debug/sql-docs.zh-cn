---
description: 维度处理目标
title: 维度处理目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dimensionprocessingdest.f1
- sql13.dts.designer.dimprocessingtransformation.connection.f1
- sql13.dts.designer.dimprocessingtransformation.mappings.f1
- sql13.dts.designer.dimprocessingtransformation.advanced.f1
helpviewer_keywords:
- Dimension Processing destination
- loading dimensions
- destinations [Integration Services], Dimension Processing
- dimensions [Analysis Services], processing
ms.assetid: 4c49bb95-7259-42f4-a785-bb6aaf5f8566
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 67a93e031afddb7533cea5251c4882da2760fbfe
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123502"
---
# <a name="dimension-processing-destination"></a>维度处理目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  维度处理目标加载并处理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 维度。 有关维度的详细信息，请参阅[维度（Analysis Services - 多维数据）](/analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-analysis-services-multidimensional-data)。  
  
 维度处理目标包含下列功能：  
  
-   用于执行增量处理、完全处理或更新处理的选项。  
  
-   用于指定维度处理是忽略错误还是在发生指定数量的错误后停止的错误配置。  
  
-   输入列到维度表中列的映射。  
  
 有关处理 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 对象的详细信息，请参阅[处理选项和设置 (Analysis Services)](/analysis-services/multidimensional-models/processing-options-and-settings-analysis-services)。  
  
## <a name="configuration-of-the-dimension-processing-destination"></a>维度处理目标的配置  
 维度处理目标使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器连接到包含目标所处理的维度的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的实例。 有关详细信息，请参阅 [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md)。  
  
 此目标有一个输入。 它不支持错误输出。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="dimension-processing-destination-editor-connection-manager-page"></a>维度处理目标编辑器（“连接管理器”页）
  可以使用“维度处理目标编辑器”对话框的“连接管理器”页面指定与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例之间的连接。  
  
### <a name="options"></a>选项  
 **“ODBC 目标编辑器”**  
 从列表中选择现有连接管理器，或单击“新建”创建新的连接管理器。  
  
 **新建**  
 通过使用“添加 Analysis Services 连接管理器”对话框创建一个新连接。  
  
 **可用维度列表**  
 选择要处理的维度。  
  
 **处理方法**  
 选择要应用于列表中选定维度的处理方法。 此选项的默认值为 **“完全”**。  
  
|值|描述|  
|-----------|-----------------|  
|**添加(增量式)**|对维度执行增量处理。|  
|**完整**|对维度执行完全处理。|  
|**更新**|对维度执行更新处理。|  
  
## <a name="dimension-processing-destination-editor-mappings-page"></a>维度处理目标编辑器（“映射”页）
  可以使用 **“维度处理目标编辑器”** 对话框的 **“映射”** 页，将输入列映射到维度列。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 使用拖放操作可以将表中的可用输入列映射到目标列。  
  
 **可用目标列**  
 查看可用目标列的列表。 使用拖放操作可以将表中的可用目标列映射到输入列。  
  
 **输入列**  
 查看从上表中选择的输入列。 可以通过使用 **“可用输入列”** 列表来更改映射。  
  
 **目标列**  
 查看每个可用目标列，而不管是否已对其进行映射。  
  
## <a name="dimension-processing-destination-editor-advanced-page"></a>维度处理目标编辑器（“高级”页）
  可以使用 **“维度处理目标编辑器”** 对话框中的 **“高级”** 页配置错误处理方式。  
  
### <a name="options"></a>选项  
 **使用默认错误配置**  
 指定是否使用 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 的默认错误处理方式。 默认情况下，此值为 **True**。  
  
 **键错误操作**  
 指定如何处理包含不可接受的键值的记录。  
  
|值|描述|  
|-----------|-----------------|  
|**ConvertToUnknown**|将不适用的键值转换为 **UnknownMember** 值。|  
|**DiscardRecord**|放弃记录。|  
  
 **忽略错误**  
 指定将忽略错误。  
  
 **出错时停止**  
 指定在出现错误时应停止处理。  
  
 **错误数**  
 指定应停止处理的错误阈值（如果选择了“出错时停止”）。  
  
 **出错时要执行的操作**  
 指定在达到错误阈值时执行的操作（如果选择了“出错时停止”）。  
  
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
 指定在将空键转换为 **UnknownMember** 值后所采取的操作。 默认情况下，此值为 **IgnoreError**。  
  
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
 键入错误日志的路径，或单击浏览 (...) 按钮以选择目标。  
  
 **浏览(...)**  
 选择错误日志的路径。  
  
## <a name="see-also"></a>另请参阅  
 [数据流](../../integration-services/data-flow/data-flow.md)   
 [Integration Services 转换](../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
