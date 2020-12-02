---
description: 数据挖掘模型定型目标
title: 数据挖掘模型定型目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataminingmodeltrainingdest.f1
- sql13.dts.designer.dmmtrainingtransformation.connection.f1
- sql13.dts.designer.dmmtrainingtransformation.columns.f1
helpviewer_keywords:
- destinations [Integration Services], Data Mining Model Training
- Data Mining Model Training destination
- mining models [Analysis Services], training
- training mining models
ms.assetid: 6bc8cbe2-46af-4f7b-93d6-86779313c9d7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b70fe92da0c7efb0e59dbc89505ad623cad64432
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127238"
---
# <a name="data-mining-model-training-destination"></a>数据挖掘模型定型目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  数据挖掘模型定型目标将该目标接收到的数据通过数据挖掘模型算法传递，从而为数据挖掘模型定型。 如果模型是在同一数据结构上生成的，则一个目标可为多个数据挖掘模型定型。 有关详细信息，请参阅 [Mining Structure Columns](/analysis-services/data-mining/mining-structure-columns) 和 [Mining Model Columns](/analysis-services/data-mining/mining-model-columns)。  
  
## <a name="configuration-of-the-data-mining-model-training-destination"></a>数据挖掘模型定型目标的配置  
 如果目标结构以及在该结构上生成的模型的事例级别列具有内容类型 KEY TIME 或 KEY SEQUENCE，则输入数据必须基于此列排序。 例如，用 Microsoft 时序算法生成的模型使用内容类型 KEY TIME。 如果输入数据未排序，则对模型的处理可能失败。 如果数据需要排序，您可以事先在数据流中使用排序转换对数据进行排序。 此要求不适用于具有 KEY 内容类型的列。 有关详细信息，请参阅[内容类型（数据挖掘）](/analysis-services/data-mining/content-types-data-mining)和[排序转换](../../integration-services/data-flow/transformations/sort-transformation.md)。  
  
> [!NOTE]  
>  数据挖掘模型定型目标的输入必须经过排序。 若要对数据进行排序，您可以将数据挖掘模型定型目标的排序目标上游包括到该数据流中。 有关详细信息，请参阅 [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md)。  
  
 此目标有一个输入，没有输出。  
  
 数据挖掘模型定型目标使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接管理器连接到包含挖掘结构以及目标为其定型的挖掘模型的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 项目或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例。 有关详细信息，请参阅 [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md)。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [数据挖掘模型定型目标自定义属性](../../integration-services/data-flow/data-mining-model-training-destination-custom-properties.md)  
  
 有关如何设置属性的详细信息，请参阅 [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="data-mining-model-training-editor-connection-tab"></a>数据挖掘模型定型编辑器（“连接”选项卡）
  可以使用 **“数据挖掘模型定型编辑器”** 对话框的 **“连接”** 页选择要定型的挖掘模型。  
  
### <a name="options"></a>选项  
 **“ODBC 目标编辑器”**  
 从现有 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接的列表中进行选择，或者通过使用下面介绍的“新建”按钮创建新的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 连接。  
  
 **新建**  
 通过使用“添加 Analysis Services 连接管理器”对话框创建一个新连接。  
  
 **挖掘结构**  
 从可用挖掘结构的列表中进行选择，或者通过单击“新建”创建新的挖掘结构。  
  
 **新建**  
 通过使用 **数据挖掘向导** 创建新的挖掘结构和挖掘模型。  
  
 **挖掘模型**  
 查看与所选挖掘结构相关联的挖掘模型的列表。  
  
## <a name="data-mining-model-training-editor-columns-tab"></a>数据挖掘模型定型编辑器（“列”选项卡）
  可以使用 **“数据挖掘模型定型编辑器”** 对话框的 **“列”** 页，将输入列映射到挖掘结构中的列。  
  
## <a name="options"></a>选项  
 **可用输入列**  
 查看可用输入列的列表。 拖动输入列可将其映射到挖掘结构列。  
  
 **挖掘结构列**  
 查看挖掘结构列的列表。 拖动挖掘结构列可将其映射到可用输入列。  
  
 **输入列**  
 查看从上表中选择的输入列。 若要更改或删除映射选择，请使用 **“可用输入列”** 的列表。  
  
 **挖掘结构列**  
 查看每个可用的目标列，包括已映射或未映射的目标列。  
