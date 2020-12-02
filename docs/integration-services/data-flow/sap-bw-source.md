---
description: SAP BW 源
title: SAP BW 源 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 749afb64-3567-4dc9-8431-783d650c25db
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8d62e20117bf23511a1d261142a3618dcbd5095a
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195324"
---
# <a name="sap-bw-source"></a>SAP BW 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  SAP BW 源是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的源组件。 因此，SAP BW 源从 SAP Netweaver BW 版本 7 系统提取数据，并将这些数据提供给 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中的数据流。  
  
 此源具有一个输出和一个错误输出。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
> [!IMPORTANT]  
>  从 SAP Netweaver BW 提取数据要求额外的 SAP 许可。 请向 SAP 核实以便确认这些要求。  
  
 若要使用 SAP BW 源，必须执行以下任务：  
  
-   [准备 SAP Netweaver BW 对象](#bkmk_Prepare_Objects)  
  
-   [连接至 SAP Netweaver BW 系统](#bkmk_Connect_Database)  
  
-   [配置 SAP BW 源](#bkmk_Configure_Source)  
  
##  <a name="preparing-the-sap-netweaver-bw-objects-that-the-source-requires"></a><a name="bkmk_Prepare_Objects"></a> 准备源所需的 SAP Netweaver BW 对象  
 SAP BW 源要求 SAP Netweaver BW 系统中存在某些对象才能正常工作。 如果这些对象还不存在，必须按照下列步骤在 SAP Netweaver BW 系统中创建并配置这些对象。  
  
> [!NOTE]  
>  有关这些对象和配置步骤的更多详细信息，请参阅 SAP Netweaver BW 文档。  
  
1.  通过 SAP GUI 登录 SAP Netweaver BW，输入事务代码 SM59，创建一个 RFC 目标：  
  
    1.  对于“连接类型”，选择“TCP/IP”。  
  
    2.  对于 **“激活类型”**，选择 **“注册服务器程序”**。  
  
    3.  对于“与目标系统的通信类型”，选择“非 Unicode (非活动 MDMP 设置)”。  
  
    4.  分配适当的程序 ID。  
  
2.  创建 Open Hub 目标：  
  
    1.  转到 Administrator Workbench（事务代码 RSA1），在左窗格中选择“Open Hub 目标”。  
  
    2.  在中间窗格中，右键单击 InfoArea，然后选择“创建 Open Hub 目标”。  
  
    3.  对于 **“目标类型”**，选择 **“第三方工具”**，然后输入之前创建的 RFC 目标。  
  
    4.  保存并激活新的 Open Hub 目标。  
  
3.  创建数据传输进程 (DTP)：  
  
    1.  在 InfoArea 的中间窗格中，右键单击之前创建的目标，然后选择“创建数据传输进程”。  
  
    2.  配置、保存并激活 DTP。  
  
    3.  在菜单中单击 **“转到”**，然后单击 **“批管理器设置”**。  
  
    4.  对于串行处理，将 **“进程数”** 更新为 1。  
  
4.  创建进程链：  
  
    1.  配置进程链时，选择 **“使用元数据链或 API 启动”** 作为 **“启动进程”** 的 **“计划选项”**，然后添加之前创建的 DTP 作为下一节点。  
  
    2.  保存并激活进程链。  
  
     SAP BW 源可调用进程链来激活数据传输进程。  
  
##  <a name="connecting-to-the-sap-netweaver-bw-system"></a><a name="bkmk_Connect_Database"></a> 连接至 SAP Netweaver BW 系统  
 在连接 SAP Netweaver BW 版本 7 系统时，SAP BW 源会使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 包中的 SAP BW 连接管理器。 SAP BW 连接管理器是 SAP BW 源唯一可以使用的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 连接管理器。  
  
 有关 SAP BW 连接管理器的详细信息，请参阅 [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md)。  
  
##  <a name="configuring-the-sap-bw-source"></a><a name="bkmk_Configure_Source"></a> 配置 SAP BW 源  
 可以按下列方式配置 SAP BW 源：  
  
-   查找并选择用来提取数据的 Open Hub Service (OHS) 目标。  
  
-   选择以下方法之一作为数据提取方法：  
  
    -   触发进程链。 此情况下， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包会启动提取进程。  
  
    -   等待 SAP Netweaver BW 系统发出开始提取的通知。 此情况下，SAP Netweaver BW 系统会启动提取进程。  
  
    -   检索与某个特定请求 ID 关联的数据。 此情况下，SAP Netweaver BW 系统已将数据提取到内部表中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包只需读取这些数据。  
  
-   根据所选择的数据提取方法，提供以下额外信息：  
  
    -   对于“P - 触发进程链”选项，提供网关主机名称、网关服务名称、RFC 目标的程序 ID 和进程链的名称。  
  
    -   对于“W - 等待通知”选项，提供网关主机名称、网关服务器名称和 RFC 目标的程序 ID。 还可以指定超时时间（以秒为单位）。 超时是源收到通知前的最长等待时间。  
  
    -   对于“E - 仅提取”选项，提供请求 ID。  
  
-   指定字符串转换规则。 （例如，根据 SAP Netweaver BW 是否为 Unicode 转换所有字符串，或者将所有字符串转换为 **varchar** 或 **nvarchar**）。  
  
-   使用您选择的选项预览要提取的数据。  
  
 您还可以启用源 RFC 函数调用的日志记录功能。 （此日志记录与可对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包启用的可选日志记录是分开进行的。）配置源将要使用的 SAP BW 连接管理器时，会启用 RFC 函数调用的日志记录功能。 有关如何配置 SAP BW 连接管理器的详细信息，请参阅 [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md)。  
  
 如果您不知道配置源所需的所有值，可能需要询问您的 SAP 管理员。  
  
 有关演示如何配置和使用 SAP BW 连接管理器、源和目标的演练，请参阅白皮书 [将 SQL Server 2008 Integration Services 与 SAP BI 7.0 一起使用](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100))。 此白皮书还说明如何在 SAP BW 中配置所需的对象。  
  
### <a name="using-the-ssis-designer-to-configure-the-source"></a>使用 SSIS 设计器配置源  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的 SAP BW 源属性的详细信息，请单击以下主题之一：  
  
-   [SAP BW 源编辑器（“连接管理器”页）](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)  
  
-   [SAP BW 源编辑器（“列”页）](../../integration-services/data-flow/sap-bw-source-editor-columns-page.md)  
  
-   [SAP BW 源编辑器（“错误输出”页）](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)  
  
-   [SAP BW 源编辑器（“高级”页）](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)  
  
 在配置 SAP BW 源的同时，您还可使用各种对话框查找 SAP Netweaver BW 对象或预览源数据。 有关这些对话框的详细信息，请单击以下主题之一进行了解：  
  
-   [查找 RFC 目标](../../integration-services/data-flow/look-up-rfc-destination.md)  
  
-   [查找进程链](../../integration-services/data-flow/look-up-process-chain.md)  
  
-   [请求日志](../../integration-services/data-flow/request-log.md)  
  
-   [预览](../../integration-services/data-flow/preview.md)  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft Connector for SAP BW 组件](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
