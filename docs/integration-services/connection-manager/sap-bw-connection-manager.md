---
description: SAP BW 连接管理器
title: SAP BW 连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 06b166a1-a9df-48ea-a0e8-9b8d6979c0a1
f1_keywords:
- sql13.dts.designer.sapbwconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e09a6cdf6f8d863e57d2de0749e29b33c989eea
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91719375"
---
# <a name="sap-bw-connection-manager"></a>SAP BW 连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  SAP BW 连接管理器是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的连接管理器组件。 因此，SAP BW 连接管理器提供 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的源组件和目标组件所需的与 SAP Netweaver BW 版本 7 系统连接的能力。 （SAP BW 源组件和目标组件作为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 包的一部分，是唯一采用 SAP BW 连接管理器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 组件。）  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 当将 SAP BW 连接管理器添加到包中时，连接管理器的 **ConnectionManagerType** 属性被设为 **SAPBI**。  
  
## <a name="configuring-the-sap-bw-connection-manager"></a>配置 SAP BW 连接管理器  
 可以按下列方式配置 SAP BW 连接管理器：  
  
-   输入连接的客户端、用户名、密码和语言。  
  
-   选择连接单个应用程序服务器还是连接一组负载平衡服务器。  
  
-   如果是单个应用程序服务器，需提供主机和系统编号，如果是一组负载平衡的服务器，需提供消息服务器、组和 SID。  
  
-   启用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 组件的 RFC 函数调用自定义日志记录。 （此日志记录与可对 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包启用的可选日志记录是分开进行的。）要启用 RFC 函数调用日志记录，可以指定一个目录用来存储每次 RFC 函数调用前后创建的日志文件。 （此日志记录功能会创建许多个 XML 格式的日志文件。 这些日志文件还包含传输的所有数据行，因此可能会消耗大量的磁盘空间。）如果不选择日志目录，则不启用日志记录功能。  
  
    > [!IMPORTANT]  
    >  如果传输的数据包含敏感信息，日志文件中也会包含该敏感信息。  
  
-   使用您输入的值来测试连接。  
  
 如果您不知道配置连接管理器所需的所有值，可能需要询问您的 SAP 管理员。  
  
 有关演示如何配置和使用 SAP BW 连接管理器、源和目标的演练，请参阅白皮书 [将 SQL Server 2008 Integration Services 与 SAP BI 7.0 一起使用](/previous-versions/sql/sql-server-2008/dd299430(v=sql.100))。 此白皮书还说明如何在 SAP BW 中配置所需的对象。  
  
### <a name="using-the-ssis-designer-to-configure-the-source"></a>使用 SSIS 设计器配置源  
 有关可以在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的 SAP BW 连接管理器属性的详细信息，请单击以下主题：  
  
-   [SAP BW 连接管理器编辑器]()  
  
## <a name="sap-bw-connection-manager-editor"></a>SAP BW 连接管理器编辑器
  使用 **“SAP BW 连接管理器编辑器”** 指定用于连接 SAP Netweaver BW 版本 7 系统的属性。  
  
 SAP BW 连接管理器向 SAP BW 源或目标提供与 SAP Netweaver BW 7 系统的连接。 若要了解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的 SAP BW 连接管理器的详细信息，请参阅 [SAP BW 连接管理器](../../integration-services/connection-manager/sap-bw-connection-manager.md)。  
  
> [!IMPORTANT]  
>  针对 Microsoft Connector 1.1 for SAP BW 的文档假定您熟悉 SAP Netweaver BW 环境。 有关 SAP NetWeaver BW 的详细信息，或者有关如何配置 SAP Netweaver BW 对象和过程的信息，请参阅您的 SAP 文档。  
  
 **打开“SAP BW 连接管理器编辑器”**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含 SAP BW 连接管理器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在 **“控制流”** 选项卡上的“连接管理器”区域中执行以下步骤之一：  
  
    -   双击 SAP BW 连接管理器。  
  
         \- 或 -  
  
    -   右键单击 SAP BW 连接管理器，然后选择“编辑”。  
  
### <a name="options"></a>选项  
  
> [!NOTE]  
>  如果您不知道配置连接管理器所需的所有值，可能需要询问您的 SAP 管理员。  
  
 **客户端**  
 指定系统的客户端编号。  
  
 **语言**  
 指定系统使用的语言。 例如，指定 **“EN”** 表示英语。  
  
 **用户名**  
 指定将用于连接到系统的用户名。  
  
 **密码**  
 指定将与该用户名一起使用的密码。  
  
 **使用单一应用程序服务器**  
 连接到单一应用程序服务器。  
  
 若要连接到一组负载平衡的服务器，请改用“使用负载平衡”选项。  
  
 **主机**  
 如果连接到单一应用程序服务器，请指定主机名。  
  
> [!NOTE]  
>   只有在已选择 **“使用单一应用程序服务器”** 选项的情况下，此选项才可用。  
  
 **系统编号**  
 如果连接到单一应用程序服务器，请指定系统编号。  
  
> [!NOTE]  
>   只有在已选择 **“使用单一应用程序服务器”** 选项的情况下，此选项才可用。  
  
 **使用负载平衡**  
 连接到一组负载平衡的服务器。  
  
 要连接单一应用程序服务器，请使用 **“使用单一应用程序服务器”** 选项。  
  
 **消息服务器**  
 要连接一组负载平衡的服务器，请指定消息服务器的名称。  
  
> [!NOTE]  
>  只有在已选择“使用负载平衡”选项的情况下，此选项才可用。  
  
 **组**  
 要连接一组负载平衡的服务器，请指定服务器组名称。  
  
> [!NOTE]  
>  只有在已选择“使用负载平衡”选项的情况下，此选项才可用。  
  
 **SID**  
 要连接一组负载平衡的服务器，请指定连接的“系统 ID”。  
  
> [!NOTE]  
>  只有在已选择“使用负载平衡”选项的情况下，此选项才可用。  
  
 **日志目录**  
 启用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 组件的日志记录。  
  
 要启用日志记录，请指定一个目录用来存储每次 RFC 函数调用前后创建的日志文件。 （此日志记录功能会创建许多个 XML 格式的日志文件。 这些日志文件还包含传输的所有数据行，因此可能会消耗大量的磁盘空间。）  
  
> [!IMPORTANT]  
>  如果传输的数据包含敏感信息，日志文件中也会包含该敏感信息。  
  
 要指定日志目录，您可以手动输入目录路径，也可以单击 **“浏览”** 并浏览到要使用的日志目录。  
  
 如果不选择日志目录，则不启用日志记录功能。  
  
 **“浏览”**  
 浏览以选择一个用作日志目录的文件夹。  
  
 **测试连接**  
 使用您提供的值来测试连接。 单击 **“测试连接”** 后，将显示一个消息框，指示连接成功还是失败。  
  
## <a name="see-also"></a>另请参阅  
 [Microsoft Connector for SAP BW 组件](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
