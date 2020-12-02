---
description: SAP BW 目标
title: SAP BW 目标 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: a612ed91-b89b-4173-a0b1-0bce381e1e28
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d46d3598bb309029319c1b74b467983e681b96ac
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197042"
---
# <a name="sap-bw-destination"></a>SAP BW 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  SAP BW 目标是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的目标组件。 因此，SAP BW 目标将来自 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中数据流的数据加载到 SAP Netweaver BW 版本 7 系统。  
  
 此目标具有一个输入和一个错误输出。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 若要使用 SAP BW 目标，必须执行以下任务：  
  
-   [准备 SAP Netweaver BW 对象](#bkmk_Prepare_Objects)  
  
-   [连接至 SAP Netweaver BW 系统](#bkmk_Connect_Database)  
  
-   [配置 SAP BW 目标](#bkmk_Configure_Destination)  
  
##  <a name="preparing-the-sap-netweaver-bw-objects-that-the-destination-requires"></a><a name="bkmk_Prepare_Objects"></a> 准备目标所需的 SAP Netweaver BW 对象  
 SAP BW 目标要求 SAP Netweaver BW 系统中存在某些对象才能正常工作。 如果这些对象还不存在，必须按照下列步骤在 SAP Netweaver BW 系统中创建并配置这些对象。  
  
> [!NOTE]  
>  有关这些对象和配置步骤的更多详细信息，请参阅 SAP Netweaver BW 文档。  
  
1.  创建新的源系统：  
  
    1.  选择类型， **“第三方/分级 BAPI”**。  
  
    2.  对于“与目标系统的通信类型”，选择“非 Unicode (非活动 MDMP 设置)”。  
  
    3.  分配适当的程序 ID。  
  
2.  为 InfoSource 分配源系统。  
  
3.  创建一个 InfoPackage。  
  
     该 InfoPackage 是 SAP BW 目标所需要的最重要的对象。  
  
 您也可以创建为将数据加载到 SAP Netweaver BW 系统提供支持所需要的更多 InfoObject、InfoCube、InfoSource 和 InfoPackage。  
  
##  <a name="connecting-to-the-sap-netweaver-bw-system"></a><a name="bkmk_Connect_Database"></a> 连接至 SAP Netweaver BW 系统  
 为连接到 SAP Netweaver BW 版本 7 系统，SAP BW 目标会使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 包中的 SAP BW 连接管理器。 SAP BW 连接管理器是 SAP BW 目标唯一可以使用的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 连接管理器。  
  
 有关 SAP BW 连接管理器的详细信息，请参阅 [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md)。  
  
##  <a name="configuring-the-sap-bw-destination"></a><a name="bkmk_Configure_Destination"></a> 配置 SAP BW 目标  
 可以使用下列方式配置 SAP BW 目标：  
  
-   查找并选择用来加载数据的 InfoPackage。  
  
-   将数据流中的每个列映射到 InfoPackage 中相应的 InfoObject。  
  
-   通过配置 **PackageSize** 属性指定将同时传输的数据行数。  
  
-   选择等到 SAP Netweaver BW 系统完成数据加载。  
  
-   选择不触发 InfoPackage，但要等待 SAP Netweaver BW 系统已开始数据加载的通知。  
  
-   （可选）完成数据加载后触发其他进程链。  
  
-   （可选）创建目标所需的 SAP Netweaver BW 对象。 这包括创建 InfoObject、InfoCube、InfoSource 和 InfoPackage。  
  
-   使用您已选择的选项测试数据加载。  
  
 您还可以对目标的 RFC 函数调用启用日志记录功能。 （此日志记录与可对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包启用的可选日志记录是分开进行的。）配置目标将使用的 SAP BW 连接管理器时，会启用 RFC 函数调用的日志记录功能。 有关如何配置 SAP BW 连接管理器的详细信息，请参阅 [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md)。  
  
 如果您不知道配置目标所需的所有值，可能需要询问您的 SAP 管理员。  
  
 有关演示如何配置和使用 SAP BW 连接管理器、源和目标的演练，请参阅白皮书 [将 SQL Server 2008 Integration Services 与 SAP BI 7.0 一起使用](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100))。 此白皮书还说明如何在 SAP BW 中配置所需的对象。  
  
### <a name="using-the-ssis-designer-to-configure-the-destination"></a>使用 SSIS 设计器配置目标  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的 SAP BW 目标属性的详细信息，请单击以下主题之一进行了解：  
  
-   [SAP BW 目标编辑器（“连接管理器”页）](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)  
  
-   [SAP BW 目标编辑器（“映射”页）](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)  
  
-   [SAP BW 目标编辑器（“错误输出”页）](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)  
  
-   [SAP BW 目标编辑器（“高级”页）](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)  
  
 在配置 SAP BW 目标的同时，您还可使用各种对话框查找或创建 SAP Netweaver BW 对象。 有关这些对话框的详细信息，请单击以下主题之一进行了解：  
  
-   [查找 InfoPackage](../../integration-services/data-flow/look-up-infopackage.md)  
  
-   [新建 InfoObject](../../integration-services/data-flow/create-new-infoobject.md)  
  
-   [“创建事务数据的 InfoCube”](../../integration-services/data-flow/create-infocube-for-transaction-data.md)  
  
-   [查找 InfoObject](../../integration-services/data-flow/look-up-infoobject.md)  
  
-   [“创建 InfoSource”](../../integration-services/data-flow/create-infosource.md)  
  
-   [创建事务数据的 InfoSource](../../integration-services/data-flow/create-infosource-for-transaction-data.md)  
  
-   [“创建主数据的 InfoSource”](../../integration-services/data-flow/create-infosource-for-master-data.md)  
  
-   [创建 InfoPackage](../../integration-services/data-flow/create-infopackage.md)  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft Connector for SAP BW 组件](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
