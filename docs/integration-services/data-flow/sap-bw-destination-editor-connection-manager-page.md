---
description: SAP BW 目标编辑器（“连接管理器”页）
title: SAP BW 目标编辑器（“连接管理器”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwdestination.connection.f1
ms.assetid: 04ae38f8-5287-45a3-826a-8aac5dd15a91
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ccd18ea2d13b643899b5492b0151984b275b8c80
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484546"
---
# <a name="sap-bw-destination-editor-connection-manager-page"></a>SAP BW 目标编辑器（“连接管理器”页）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **“SAP BW 目标编辑器”** 的 **“连接管理器”** 页，选择 SAP BW 目标要使用的 SAP BW 连接管理器。 在该页中，您还可选择用于向 SAP Netweaver BW 系统加载数据的参数。  
  
 若要了解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的 SAP BW 目标的详细信息，请参阅 [SAP BW 目标](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“连接管理器”页**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 目标的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 SAP BW 目标。  
  
3.  在 **“SAP BW 目标编辑器”** 中单击 **“连接管理器”** ，以打开编辑器的 **“连接管理器”** 页。  
  
## <a name="options"></a>选项  
  
> [!NOTE]  
>  如果您不知道配置目标所需的所有值，可能需要询问您的 SAP 管理员。  
  
 **SAP BW 连接管理器**  
 从列表中选择一个现有连接管理器，或通过单击“新建”  创建一个新连接。  
  
 **新建**  
 使用“SAP BW 连接管理器”对话框创建新的连接管理器。  
  
 **测试加载**  
 对要使用您选择的设置加载零行的加载进程进行测试。  
  
### <a name="infopackageinfosource-options"></a>InfoPackage/InfoSource 选项  
 您无需事先知道并输入这些值。 使用 **“查找”** 按钮查找和选择合适的 InfoPackage。 选定 InfoPackage 后，组件会为这些选项输入合适的值。  
  
 **InfoPackage**  
 输入 InfoPackage 的名称。  
  
 **InfoSource**  
 输入 InfoSource 的名称。  
  
 **类型**  
 输入标识 InfoSource 类型的单字符。 下表列出了可接受的单字符值。  
  
|值|说明|  
|-----------|-----------------|  
|**D**|事务数据|  
|**M**|主数据|  
|**T**|文本|  
|**H**|层次结构数据|  
  
 **逻辑系统**  
 输入 InfoPackage 关联的逻辑系统的名称。  
  
 **查找**  
 使用“查找 InfoPackage”对话框查找 InfoPackage。 有关此对话框的详细信息，请参阅 [Look Up InfoPackage](../../integration-services/data-flow/look-up-infopackage.md)。  
  
### <a name="rfc-destination-options"></a>RFC 目标选项  
 您无需事先知道并输入这些值。 使用 **“查找”** 按钮查找和选择合适的 RFC 目标。 选定 RFC 目标后，组件会为这些选项输入合适的值。  
  
 **网关主机**  
 输入服务器名称或网关主机的 IP 地址。 通常，名称或 IP 地址与 SAP 应用程序服务器相同。  
  
 **网关服务**  
 输入网关服务的名称，格式为“sapgwNN”，其中 **NN** 是系统编号。  
  
 **程序 ID**  
 输入与 RFC 目标关联的程序 ID。  
  
 **查找**  
 使用“查找 RFC 目标”对话框查找 RFC 目标。 有关此对话框的详细信息，请参阅 [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md)。  
  
### <a name="create-sap-bw-objects-options"></a>创建 SAP BW 对象选项  
 **选择 对象类型**  
 选择要创建的 SAP Netweaver BW 对象的类型。 可以选择下列其中一个类型：  
  
-   InfoObject  
  
-   InfoCube  
  
-   InfoSource  
  
-   InfoPackage  
  
 **创建**  
 创建选定类型的 SAP Netweaver BW 对象。  
  
|对象类型|结果|  
|-----------------|------------|  
|**InfoObject**|通过使用 **“创建新 InfoObject”** 对话框创建一个新的 InfoObject。 有关此对话框的详细信息，请参阅 [Create New InfoObject](../../integration-services/data-flow/create-new-infoobject.md)。|  
|**InfoCube**|使用 **“创建事务数据的 InfoCube”** 对话框创建新的 InfoCube。 有关此对话框的详细信息，请参阅 [Create InfoCube for Transaction Data](../../integration-services/data-flow/create-infocube-for-transaction-data.md)。|  
|**InfoSource**|使用 **“创建 InfoSource”** 对话框，然后使用 **“创建事务数据的 InfoSource”** 或 **“创建主数据的 InfoSource”** 对话框创建一个新的 InfoSource。 有关这些对话框的详细信息，请参阅 [Create InfoSource](../../integration-services/data-flow/create-infosource.md), [Create InfoSource for Transaction Data](../../integration-services/data-flow/create-infosource-for-transaction-data.md) 和 [Create InfoSource for Master Data](../../integration-services/data-flow/create-infosource-for-master-data.md)。|  
|**InfoPackage**|使用 **“创建 InfoPackage”** 对话框创建新的 InfoPackage。 有关此对话框的详细信息，请参阅 [Create InfoPackage](../../integration-services/data-flow/create-infopackage.md)。|  
  
## <a name="see-also"></a>另请参阅  
 [SAP BW 目标编辑器（“映射”页）](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)   
 [SAP BW 目标编辑器（“错误输出”页）](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)   
 [SAP BW 目标编辑器（“高级”页）](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)   
 [Microsoft Connector for SAP BW F1 帮助](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
