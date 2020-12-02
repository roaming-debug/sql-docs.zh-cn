---
description: SAP BW 源编辑器（“列”页）
title: SAP BW 源编辑器（“列”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwsource.columns.f1
ms.assetid: c2ec8bb7-be9b-4783-ad88-32512de784b0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: dab03a43269739bbf77101f8ded9dac68ff2b8f3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88495738"
---
# <a name="sap-bw-source-editor-columns-page"></a>SAP BW 源编辑器（“列”页）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用“SAP BW 源编辑器”的“列”页将输出列映射到每个外部（源）列。  
  
 若要了解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的 SAP BW 源组件的详细信息，请参阅 [SAP BW 源](../../integration-services/data-flow/sap-bw-source.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
> [!IMPORTANT]  
>  从 SAP Netweaver BW 提取数据要求额外的 SAP 许可。 请向 SAP 核实以便确认这些要求。  
  
 **打开“列”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 源的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”选项卡上，双击“SAP BW 源”。  
  
3.  在 **“SAP BW 源编辑器”** 中单击 **“列”** ，以打开编辑器的 **“列”** 页。  
  
## <a name="options"></a>选项  
  
> [!NOTE]  
>  如果您不知道配置源所需的所有值，可能需要询问您的 SAP 管理员。  
  
 **可用外部列**  
 查看数据源中可用外部列的列表，然后选择要包括在数据流中的列。  
  
 若要在数据流中包括某列，请选中该列对应的复选框。 您选择这些复选框的顺序决定了列的输出顺序。 也就是说，您选择的第一个复选框将是第一个输出列，而选择的第二个复选框则将是第二个输出列，以此类推。  
  
 **“外部列”**  
 查看选定外部（源）列。 选定列将按照您在配置要使用来自此源的数据的下游组件时将看到的对应输出列的顺序显示。  
  
 要更改这些列的顺序，请在 **“可用外部列”** 列表中清除所有列的复选框。 然后按照您希望列显示的顺序选择列。  
  
 **输出列**  
 为每个输出列提供唯一的名称。 默认值是所选外部（源）列的名称。 但是，您可以输入任何唯一的描述性名称。 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器将显示列的 **输出列** 名称。  
  
## <a name="see-also"></a>另请参阅  
 [SAP BW 源编辑器（“连接管理器”页）](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)   
 [SAP BW 源编辑器（“错误输出”页）](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)   
 [SAP BW 源编辑器（“高级”页）](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
