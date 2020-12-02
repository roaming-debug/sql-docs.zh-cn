---
description: 创建事务数据的 InfoSource
title: 创建事务数据的 InfoSource | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: ab5f23e2-cd4e-4507-83d9-ac5ef721c171
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5b37416ef8283f32909b216508defda671080e30
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127261"
---
# <a name="create-infosource-for-transaction-data"></a>创建事务数据的 InfoSource

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“创建事务数据的 InfoSource”** 对话框为 SAP Netweaver BW 系统中的事务数据创建一个新的 InfoSource。  
  
 从 **“SAP BW 目标编辑器”** 的 **“连接管理器”** 页可以打开 **“创建事务数据的 InfoSource”** 对话框。 若要了解有关 SAP BW 目标的详细信息，请参阅 [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“创建事务数据的 InfoSource”对话框**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 SAP BW 目标。  
  
3.  在 **“SAP BW 目标编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
4.  在 **“连接管理器”** 页的 **“创建 SAP BW 对象”** 组框中，选择 **“InfoSource”**，然后单击 **“创建”**。  
  
5.  在 **“创建 InfoSource”** 对话框中，选择 **“事务数据”** ，然后单击 **“确定”** 。  
  
## <a name="general-options"></a>常规选项  
 **InfoSource 名称**  
 输入新 InfoSource 的名称。  
  
 **简短说明**  
 输入新 InfoSource 的简短说明。  
  
 **长说明**  
 输入新 InfoSource 的详细说明。  
  
 **源系统**  
 选择与 InfoSource 相关联的源系统。  
  
 **保存并激活**  
 保存并激活该新 InfoSource。  
  
## <a name="infosource-transfer-structure-options"></a>InfoSource 传输结构选项  
 InfoSource 传输结构可供您将数据流列与 InfoSource 关联。  
  
 **PipelineElement**  
 显示数据流输出中与 SAP BW 目标连接的列。  
  
 **PipelineDataType**  
 显示数据流列的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型。  
  
 **Iobject - 搜索**  
 将现有的 InfoObject 与当前行中的数据流列关联。 要进行此关联，请单击 **“搜索”**，然后使用 **“查找 InfoObject”** 对话框选择现有的 InfoObject。 有关此对话框的详细信息，请参阅 [Look Up InfoObject](../../integration-services/data-flow/look-up-infoobject.md)。  
  
 选择现有的 InfoObject 后，组件会将选定的值填充在 **“InfoObject”** 和 **“类型”** 列中。  
  
 **Iobject - 新建**  
 创建一个新的 InfoObject 并将该新 InfoObject 与当前行中的数据流列关联。 要创建新的 InfoObject，请单击 **“新建”**，然后使用 **“新建 InfoObject”** 对话框创建 InfoObject。 有关此对话框的详细信息，请参阅 [Create New InfoObject](../../integration-services/data-flow/create-new-infoobject.md)。  
  
 创建新 InfoObject 后，组件会将新值填充在 **“InfoObject”** 和 **“类型”** 列中。  
  
 **Iobject - 删除**  
 删除 InfoObject 与当前行的数据流列之间的关联。 若要删除此关联，请单击 **“删除”**。  
  
 **InfoObject**  
 显示与数据流列相关联的 InfoObject 的名称。  
  
 **类型**  
 显示与数据流列相关联的 InfoObject 的类型。 下表列出了该类型的可能值。  
  
|值|说明|  
|-----------|-----------------|  
|CHA|特征|  
|UNI|单元|  
|KYF|关键数字|  
|TIM|时间特征|  
  
 **单位字段**  
 指定 InfoObject 将使用的单位。  
  
## <a name="see-also"></a>另请参阅  
 [“创建 InfoSource”](../../integration-services/data-flow/create-infosource.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
