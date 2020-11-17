---
title: Reporting Services 与可用性组
description: 了解如何配置 Reporting Services 以便用于 SQL Server 中的 Always On 可用性组。 不同方案支持的功能有所不同。
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: 260af6fa8615969a895425aa3d2145071b78eb72
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583942"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Reporting Services 与 AlwaysOn 可用性组 (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  本主题包含有关配置 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 以便用于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 中的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)](AG) 的信息。 使用 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 和 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的三种方案为用于报表数据源的数据库、报表服务器数据库和报表设计。 这三种方案支持的功能和所需的配置各不相同。  
  
 将 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 用于 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据源的一个重要好处是利用可读的辅助副本作为报表数据源，同时辅助副本为主数据库提供故障转移功能。  
  
 有关 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的一般信息，请参阅[适用于 SQL Server 2012 的 Always On 的常见问题解答](../../../sql-server/index.yml)。  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> 使用 Reporting Services 和 AlwaysOn 可用性组的要求  
 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 和 Power BI 报表服务器使用 .NET Framework 4.0，并支持将 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 连接字符串属性用于数据源。  
  
 若要结合使用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 与  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 及更早版本，你需要下载并安装 .Net 3.5 SP1 的修补程序。 该修补程序添加对 SQL Client for AG 功能的支持以及对连接字符串属性 **ApplicationIntent** 和 **MultiSubnetFailover** 的支持。 如果未在承载报表服务器的每台计算机上安装该修补程序，尝试预览报表的用户将看到类似以下内容的错误消息并将错误消息写入报表服务器跟踪日志：  
  
> **错误消息：** “关键字不受支持的 "applicationintent"”  
  
 当在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 连接字符串中包含一个 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 属性但是服务器无法识别该属性时，显示此消息。 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 用户界面中单击“测试连接”按钮以及预览报表（如果在报表服务器上启用远程错误）时，将显示所述的错误消息。  
  
 有关所需修补程序的详细信息，请参阅[将 SQL Server 2012 中对 AlwaysOn 功能的支持引入 .NET Framework 3.5 SP1 的 KB 2654347A 修补程序](https://go.microsoft.com/fwlink/?LinkId=242896)。  
  
 有关其他 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 要求的信息，请参阅[针对 AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)。  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置文件（如 RSreportserver.config），它们不包括在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 功能中。 如果手动更改某一报表服务器上的配置文件，将需要手动更新副本。  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> 报表数据源和可用性组  
 基于 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 的 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 数据源行为会根据管理员配置 AG 环境的方式不同而变化。  
  
 要将 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 用于报表数据源，您需要配置报表数据源连接字符串以使用可用性组“侦听器 DNS 名称” 。 支持以下数据源：  
  
-   使用 SQL Native Client 的 ODBC 数据源。  
  
-   SQL Client，已将 .Net 修补程序应用到报表服务器。  
  
 连接字符串还可以包含新的 AlwaysOn 连接属性，这些属性配置报表查询请求以将次要副本用于只读报表。 将辅助副本用于报表请求将减少读-写主副本上的负载。 下图显示包含三个副本 AG 配置的示例，其中 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据源连接字符串已使用 ApplicationIntent=ReadOnly 配置。 在此示例中，将报表查询请求发送到辅助副本而非主副本。  
  
 
  
 以下是示例连接字符串，其中 [AvailabilityGroupListenerName] 为创建副本时配置的 **侦听器 DNS 名称** ：  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 **用户界面中的** “测试连接” [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 按钮将验证是否可以建立连接，但是不验证 AG 配置。 例如，如果将连接字符串中包含的 ApplicationIntent 指向不是 AG 一部分的服务器，将忽略此额外参数， **“测试连接”** 按钮将只验证是否可以建立到指定服务器的连接。  
  
 创建和发布报表的方式将决定您在何处编辑连接字符串：  
  
-   **本机模式：** 为已发布到本机模式报表服务器的共享数据源和报表使用 [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)]。  
  
-   **SharePoint 模式：** 对于已发布到 SharePoint 服务器的报表，使用文档库内的 SharePoint 配置页。  
  
-   **报表设计：** 创建新报表时，请使用 [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] 或 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]。 有关详细信息，请参阅本主题中的“报表设计”部分。  
  
 **其他资源：**  
  
-   [管理报表数据源](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   有关可用连接字符串属性的详细信息，请参阅 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)。  
  
-   有关可用性组侦听程序的详细信息，请参阅[创建或配置可用性组侦听程序 (SQL Server)](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)。  
  
 **注意事项：** 在从主要副本接收数据更改时，次要副本一般会有延迟。 以下因素可能影响主副本和辅助副本之间的更新滞后时间：  
  
-   辅助副本数目。 向配置中每增加一个辅助副本，延迟时间将增加。  
  
-   地理位置以及主副本和辅助副本之间的距离。 例如，辅助副本位于单独的数据中心与它们位于与主副本相同的大楼中相比，前者的延迟时间通常更长。  
  
-   每个副本的可用性模式的配置。 可用性模式确定主副本是否在辅助副本将事务写入磁盘之前，等待提交数据库上的事务。 有关详细信息，请参阅：[AlwaysOn 可用性组概述 (SQL Server)](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)的“可用性模式”部分。  
  
 将只读辅助副本作为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据源时，确保数据更新滞后时间满足报表用户的要求很重要。  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> 报表设计和可用性组  
 在 [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] 中设计报表或在 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]中创建报表项目时，用户可以配置报表数据源连接字符串以包含 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]提供的新连接属性。 对连接属性的支持取决于用户在何处预览报表。  
  
-   **本地预览：** [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] 和 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 使用 .NET Framework 4.0，并支持 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 连接字符串属性。  
  
-   **远程或服务器模式预览：** 在将报表发布到报表服务器后，或在 [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] 中使用预览后，你看到与以下内容相似的错误，指出你正在对报表服务器预览报表，且报表服务器上尚未安装用于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的 .Net Framework 3.5 SP1 修补程序。  
  
> **错误消息：** “关键字不受支持的 "applicationintent"”  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> 报表服务器数据库和可用性组  
 Reporting Services 和 Power BI 报表服务器为将 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 和报表服务器数据库结合使用提供了有限的支持。 可以在 AG 中将报表服务器数据库配置为副本的一部分；但是 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 在发生故障转移时不会自动将另一副本用于报表服务器数据库。 不支持使用 MultiSubnetFailover 与报表服务器数据库。  
  
 需要使用手动操作或自定义自动化脚本来完成故障转移和恢复。 在完成这些操作前，在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 故障转移后报表服务器的某些功能可能不能正常工作。  
  
> [!NOTE]  
>  规划报表服务器数据库的故障转移和灾难恢复时，建议您始终备份报表服务器加密密钥的副本。  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> SharePoint 模式和本机模式之间的区别  
 本节总结了 SharePoint 模式和本机模式的报表服务器与 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]交互方式的区别。  
  
 SharePoint 报表服务器将为您创建的每个 **服务应用程序创建** 3 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 个数据库。 创建服务应用程序时，SharePoint 模式下与报表服务器数据库的连接在 SharePoint 管理中心中配置。 这些数据库的默认名称包含与服务应用程序相关联的 GUID。 以下是 SharePoint 模式报表服务器的示例数据库名称：  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 本机模式报表服务器使用 **2** 个数据库。 以下是本机模式报表服务器的示例数据库名称：  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 本机模式不支持或使用警报数据库以及相关功能。 在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置管理器中配置本机模式的报表服务器。 对于 SharePoint 模式，将服务应用程序数据库名称配置为作为 SharePoint 配置一部分创建的“客户端访问点”的名称。 有关将 SharePoint 配置用于 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 的详细信息，请参阅 [为 SharePoint Server 配置和管理 SQL Server 可用性组](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))。  
  
> [!NOTE]
>  SharePoint 模式的报表服务器使用 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 服务应用程序数据库和 SharePoint 内容数据库之间的同步过程。 将报表服务器数据库和内容数据库一起维护很重要。 您应考虑在同一可用性组中配置它们以便它们作为一个整体进行故障转移和恢复。 请考虑下列方案：  
> 
>  -   您还原或故障转移到一个内容数据库副本，该数据库未收到与报表服务器数据库所收到内容相同的最近更新。  
> -   [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 同步过程将检测内容数据库中项列表与报表服务器数据库中项列表的差异。  
> -   同步过程将删除或更新内容数据库中的项。  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> 为可用性组准备报表服务器数据库  
 以下是用于准备并将报表服务器数据库添加到 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]的基本步骤：  
  
-   创建可用性组并配置“侦听器 DNS 名称” 。  
  
-   **主要副本：** 将报表服务器数据库配置为单个可用性组的一部分，并创建包含所有报表服务器数据库的主要副本。  
  
-   **次要副本：** 创建一个或多个次要副本。 将数据库从主副本复制到辅助副本的常用方法为使用“RESTORE WITH NORECOVERY”将数据库恢复到每个辅助副本。 有关创建次要副本和验证数据同步是否正常工作的详细信息，请参阅[启动 AlwaysOn 辅助数据库的数据移动 (SQL Server)](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)。  
  
-   **报表服务器凭据：** 需要在针对主要副本创建的次要副本上，创建相应的报表服务器凭据。 具体的步骤取决于您在 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 环境中使用什么类型的身份验证以及 Window [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 服务帐户、Windows 用户帐户或 SQL Server 身份验证。 有关详细信息，请参阅[配置报表服务器数据库连接（SSRS 配置管理器）](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
  
-   将数据库连接更新为使用侦听程序 DNS 名称。 对于本机模式的报表服务器，在 **配置管理器中更改** “报表服务器数据库名称” [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 。 对于 SharePoint 模式，为 **服务应用程序更改** 数据库服务器名称 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 。  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> 完成报表服务器数据库的灾难恢复所需的步骤  
 在 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 故障转移到辅助副本后，需要完成以下步骤：  
  
1.  停止 SQL 代理服务实例，该实例由承载 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 数据库的主数据库引擎使用。  
  
2.  在作为新主副本的计算机上启动 SQL 代理服务。  
  
3.  停止报表服务器服务。  
  
     如果报表服务器处于本机模式，则使用 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置管理器停止报表服务器的 Windows 服务器。  
  
     如果将报表服务器配置为用于 SharePoint 模式，请在 SharePoint 管理中心中停止 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 共享服务。  
  
4.  启动报表服务器服务或 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] SharePoint 服务。  
  
5.  验证报表可以针对新主副本运行。  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> 发生故障转移时的报表服务器行为  
 当报表服务器数据库发生故障转移且您已更新该报表服务器环境以使用新主副本时，故障转移和恢复过程中会出现一些操作问题。 这些问题的影响各不相同，具体取决于故障转移时 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 的负载、 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] 故障转移到辅助副本所花的时间以及报表服务器管理员更新报表环境以使用新主副本所花的时间。  
  
-   由于重试逻辑和报表服务器无法在故障转移期间将预定工作标记为“已完成”，可能多次执行后台处理。  
  
-   在故障转移期间正常触发运行的后台处理将不执行，因为 SQL Server 代理无法将数据写入报表服务器数据库且该数据将不同步到新主副本。  
  
-   数据库故障转移完成且报表服务器服务重新启动后，将自动重新创建 SQL Server 代理作业。 在重新创建 SQL 代理作业前，将不处理与 SQL Server 代理作业关联的所有后台执行任务。 这包括 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 订阅、计划和快照。  
  
## <a name="see-also"></a>另请参阅  
 [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [AlwaysOn 可用性组 (SQL Server)](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [AlwaysOn 可用性组入门 (SQL Server)](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [将连接字符串关键字用于 SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)   
 [对高可用性、灾难恢复的 SQL Server Native Client 支持](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [关于对可用性副本的客户端连接访问 (SQL Server)](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
