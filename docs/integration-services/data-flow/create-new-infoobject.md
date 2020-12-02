---
description: 新建 InfoObject
title: 新建 InfoObject | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3587a633-1c0b-4d63-a22a-6b2b93923c3a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9cb9336b7999373b1f6af5bb5327a35f77fdbe7e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127236"
---
# <a name="create-new-infoobject"></a>新建 InfoObject

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“新建 InfoObject”** 对话框在 SAP Netweaver BW 系统中创建新的 InfoObject。  
  
 从 **“SAP BW 目标编辑器”** 的 **“连接管理器”** 页可以打开 **“创建 InfoObject”** 对话框。 若要了解有关 SAP BW 目标的详细信息，请参阅 [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“新建 InfoObject”对话框**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 SAP BW 目标。  
  
3.  在 **“SAP BW 目标编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
4.  在 **“连接管理器”** 页中，找到 **“创建 SAP BW 对象”** 分组框，执行以下步骤之一来创建 InfoObject：  
  
    1.  要直接创建 InfoObject，请选择 **“InfoObject”**，然后单击 **“创建”** 打开 **“新建 InfoObject”** 对话框。  
  
    2.  要在创建 InfoCube 的同时创建 InfoObject，请选择 **“InfoCube”**，然后单击 **“创建”**。 在 **“创建事务数据的 InfoCube”** 对话框中找到列表中某一行的 **“IObject”** 列，选择 **“创建”** 打开 **“新建 InfoObject”** 对话框。  
  
        > [!NOTE]  
        >  表中每一行表示包数据流中的一列。  
  
    3.  要在创建事务数据的 InfoSouce 的同时创建 InfoObject，请选择 **“InfoSource”**，然后单击 **“创建”**。 在 **“创建 InfoSource”** 对话框中，选择 **“事务数据”**，然后单击 **“确定”**。 在 **“创建事务数据的 InfoSource”** 对话框中找到列表中某一行的 **“IObject”** 列，选择 **“创建”** 打开 **“新建 InfoObject”** 对话框。  
  
        > [!NOTE]  
        >  表中每一行表示包数据流中的一列。  
  
    4.  要在创建主数据的 InfoSource 的同时创建 InfoObject，请选择 **“InfoSource”**，然后单击 **“创建”**。 在 **“创建 InfoSource”** 对话框中，选择 **“主数据”**，然后单击 **“确定”**。 在 **“创建主数据的 InfoSource”** 对话框中，单击 **“新建”** 打开 **“新建 InfoObject”** 对话框。  
  
 您也可在 **“新建 InfoObject”** 对话框的 **“属性”** 部分中单击 **“新建”** 来打开 **“新建 InfoObject”** 对话框。  
  
## <a name="general-options"></a>常规选项  
 **特征**  
 创建一个表示维度数据的 InfoObject。  
  
 **关键数字**  
 创建一个表示事实数据的 InfoObject。  
  
 **InfoObject 名称**  
 输入 InfoObject 的名称。  
  
 **简短说明**  
 输入 InfoObject 的简短说明。  
  
 **长说明**  
 输入 InfoObject 的详细说明。  
  
 **包含主数据**  
 指示 InfoObject 中包含属性、文本或层次结构形式的主数据。  
  
> [!NOTE]  
>   如果 InfoObject 表示维度数据且您已选择了 **“特征”** 选项，应选择此选项。  
  
 **允许小写字符**  
 允许 InfoObject 数据中使用小写字符。  
  
 **保存并激活**  
 保存并激活新的 InfoObject。  
  
## <a name="data-type-options"></a>数据类型选项  
 **CHAR - 字符串**  
 指示 InfoObject 包含字符数据。  
  
 **NUMC - 数值字符串**  
 指示 InfoObject 包含数值数据。  
  
 **DATS - 日期(YYYYMMDD)**  
 指示 InfoObject 包含日期数据。  
  
 **TIMS - 时间(HHMMSS)**  
 指示 InfoObject 包含时间数据。  
  
 **长度**  
 输入数据的长度。  
  
## <a name="text-options"></a>文本选项  
 **包含文本**  
 指示 InfoObject 包含文本。  
  
 **短文本**  
 指示 InfoObject 包含短文本。  
  
 **中等长度文本**  
 指示 InfoObject 包含中等长度文本。  
  
 **长文本**  
 指示 InfoObject 包含长文本。  
  
 **文本语言依赖**  
 指示文本依赖于语言。  
  
 **文本时间依赖**  
 指示文本依赖于时间。  
  
## <a name="attributes-section"></a>属性部分  
 **“属性”** 部分包含 InfoObject 的属性列表和用于在列表中添加和删除属性的选项。  
  
### <a name="attributes-list"></a>“属性”列表  
 **“属性”** 列表显示所创建的 InfoObject 的属性。 **“属性”** 列表包含以下列标题：  
  
 **InfoObject**  
 查看 InfoObject 的名称  
  
 **说明**  
 查看 InfoObject 的说明。  
  
 **InfoObject 类型**  
 查看 InfoObject 的类型。 下表列出了该类型的可能值。  
  
|值|说明|  
|-----------|-----------------|  
|CHA|特征|  
|KYF|关键数字|  
|UNI|单元|  
|TIM|时间特征|  
  
### <a name="attributes-options"></a>属性选项  
 使用以下选项添加和删除所创建的 InfoObject 的属性：  
  
 **添加**  
 添加现有的 InfoObject 作为属性。  
  
 要添加现有 InfoObject，请单击“添加”，然后使用 **“查找 InfoObject”** 对话框查找 InfoObject。 有关此对话框的详细信息，请参阅 [Look Up InfoObject](../../integration-services/data-flow/look-up-infoobject.md)。  
  
 **新建**  
 添加新的 InfoObject 作为属性。  
  
 要创建和添加新的 InfoObject，请单击“新建”，然后使用 **“新建 InfoObject”** 对话框的一个新实例来创建新的 InfoObject。  
  
 **删除**  
 从“属性”列表删除选择的 InfoObject。  
  
## <a name="see-also"></a>另请参阅  
 [“创建事务数据的 InfoCube”](../../integration-services/data-flow/create-infocube-for-transaction-data.md)   
 [“创建 InfoSource”](../../integration-services/data-flow/create-infosource.md)   
 [创建事务数据的 InfoSource](../../integration-services/data-flow/create-infosource-for-transaction-data.md)   
 [创建主数据的 InfoSource](../../integration-services/data-flow/create-infosource-for-master-data.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
