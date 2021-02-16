---
description: 监视运行包和其他操作
title: 监视运行包操作和其他操作 | Microsoft Docs
ms.custom: supportability
ms.date: 06/04/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isoperations.executions.f1
- sql13.ssis.ssms.isoperations.general.f1
ms.assetid: cbbcd79f-ab9b-46ec-84cb-4821c1d16b99
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2716b85dde5aaa7a7e2df15e87004c838b8137ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353020"
---
# <a name="monitor-running-packages-and-other-operations"></a>监视运行包和其他操作

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  您可以使用以下一个或多个工具监视 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包执行、项目验证和其他操作。 某些工具（如数据分流）只适用于部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的项目。  
  
-   日志  
  
     有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
-   报表  
  
     有关详细信息，请参阅 [Reports for the Integration Services Server](#reports)。  
  
-   视图  
  
     有关详细信息，请参阅[视图（Integration Services 目录）](../../integration-services/system-views/views-integration-services-catalog.md)。  
  
-   性能计数器  
  
     有关详细信息，请参阅 [性能计时器](../../integration-services/performance/performance-counters.md)。  
  
-   数据分流  

> [!NOTE]
> 本文介绍如何在一般情况下监视正在运行的 SSIS 包以及如何在本地监视正在运行的包。 还可运行和监视 Azure SQL 数据库中的 SSIS 包。 有关详细信息，请参阅[将 SQL Server Integration Services 工作负荷直接迁移到云](../lift-shift/ssis-azure-lift-shift-ssis-packages-overview.md)。
>
> 虽然也可以在 Linux 上运行 SSIS 包，但 Linux 上不提供任何监视工具。 有关详细信息，请参阅[使用 SSIS 在 Linux 上提取、转换和加载数据](../../linux/sql-server-linux-migrate-ssis.md)。

## <a name="operation-types"></a>操作类型  
 在 **服务器的** SSISDB [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中监视几种不同的操作类型。 每个操作可以具有多个与其关联的消息。 每个消息可划分为若干不同类型之一。 例如，消息可以是信息、警告或错误类型。 有关消息类型的完整列表，请参阅针对 Transact-SQL [catalog.operation_messages（SSISDB 数据库）](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md)视图的文档。 有关操作类型的完整列表，请参阅 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)。  
  
 使用九种不同的状态类型来指示操作的状态。 有关状态类型的完整列表，请参阅 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)视图。  

## <a name="active-operations-dialog-box"></a><a name="active_ops"></a>“活动操作”对话框
  使用 **“活动操作”** 对话框可以查看 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器上当前运行的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 操作的状态，例如部署、验证和包执行。 此数据存储在 SSISDB 目录中。  
  
 有关相关 [!INCLUDE[tsql](../../includes/tsql-md.md)] 视图的详细信息，请参阅 [catalog.operations（SSISDB 数据库）](../../integration-services/system-views/catalog-operations-ssisdb-database.md)、[catalog.validations（SSISDB 数据库）](../../integration-services/system-views/catalog-validations-ssisdb-database.md)和 [catalog.executions（SSISDB 数据库）](../../integration-services/system-views/catalog-executions-ssisdb-database.md)  
  
###  <a name="open-the-active-operations-dialog-box"></a><a name="open_dialog"></a> 打开“活动操作”对话框  
  
1.  打开 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]。  
  
2.  连接 Microsoft SQL Server 数据库引擎  
  
3.  在对象资源管理器中，展开 **Integration Services** 节点，右键单击 **SSISDB**，然后单击 **“活动操作”** 。  
  
### <a name="configure-the-options"></a>配置选项  
  
 类型  
 指定操作的类型。 下面是 **“类型”** 字段的可能值以及 Transact-SQL **catalog.operations** 视图的 operations_type 列中的相应值。  
  
|类型字段说明|operations_type 值|  
|-|-|  
|Integration Services 初始化|1|  
|操作清除（SQL 代理作业）|2|  
|项目版本清除（SQL 代理作业）|3|  
|部署项目|101|  
|还原项目|106|  
|创建和启动包执行|200|  
|停止操作（停止验证或执行）|202|  
|验证项目|300|  
|验证包|301|  
|配置目录|1000|  
  
 **Stop**  
 单击以停止当前正在运行的操作。  

## <a name="viewing-and-stopping-packages-running-on-the-integration-services-server"></a>查看和停止在 Integration Services 服务器上运行的包
  **SSISDB** 数据库在对用户不可见的内部表中存储执行历史记录。 不过，它通过您可以查询的公共视图公开您所需的信息。 它还提供存储过程，您可以调用这些存储过程以执行与包相关的常见任务。  
  
 通常，您在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中的服务器上管理 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]对象。 不过，您还可以查询数据库视图和直接调用存储过程，或者编写调用托管 API 的自定义代码。 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和托管 API 查询视图并调用存储过程以便执行其许多任务。 例如，您可以查看当前正在服务器上运行的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的列表，并且在需要时请求包停止运行。  
  
### <a name="viewing-the-list-of-running-packages"></a>查看正在运行的包的列表  
 您可以在 **“活动操作”** 对话框中查看当前正在服务器上运行的包的列表。 有关详细信息，请参阅 [Active Operations Dialog Box](#active_ops)。  
  
 有关可用于查看正在运行的包列表的其他方法的信息，请参阅以下主题。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 若要查看正在服务器上运行的包的列表，请为其状态为 2 的包查询视图 [catalog.executions（SSISDB 数据库）](../../integration-services/system-views/catalog-executions-ssisdb-database.md)。  
  
 通过托管 API 以编程方式访问  
 请参阅 <xref:Microsoft.SqlServer.Management.IntegrationServices> 命名空间及其类。  
  
### <a name="stopping-a-running-package"></a>停止正在运行的包  
 您可以在 **“活动操作”** 对话框中请求正在运行的包停止。 有关详细信息，请参阅 [Active Operations Dialog Box](#active_ops)。  
  
 有关可用于停止正在运行的包的其他方法的信息，请参阅以下主题。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 若要停止正在服务器上运行的包，请调用存储过程 [catalog.stop_operation（SSISDB 数据库）](../../integration-services/system-stored-procedures/catalog-stop-operation-ssisdb-database.md)。  
  
 通过托管 API 以编程方式访问  
 请参阅 <xref:Microsoft.SqlServer.Management.IntegrationServices> 命名空间及其类。  
  
### <a name="viewing-the-history-of-packages-that-have-run"></a>查看已运行的包的历史记录  
 若要查看在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中已运行的包的历史记录，请使用 **“全部执行”** 报表。 有关“全部执行”  报表和其他标准报表的详细信息，请参阅 [Integration Services 服务器的报告](#reports)。  
  
 有关可用于查看正在运行的包的历史记录的其他方法的信息，请参阅以下主题。  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] access  
 若要查看与已运行的包有关的信息，请查询视图 [catalog.executions（SSISDB 数据库）](../../integration-services/system-views/catalog-executions-ssisdb-database.md)。  
  
 通过托管 API 以编程方式访问  
 请参阅 <xref:Microsoft.SqlServer.Management.IntegrationServices> 命名空间及其类。  

## <a name="reports-for-the-integration-services-server"></a><a name="reports"></a> Reports for the Integration Services Server
  在当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了标准报告，帮助你监视部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。 这些报告有助于您查看包状态和历史记录，并根据需要确定包执行失败的原因。  
  
 在每个报告页的顶部，后退图标会将您转到查看过的上一页，刷新图标会刷新在该页上显示的信息，打印图标则可以打印当前页。  
  
 有关如何将包部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的详细信息，请参阅[部署 Integration Services (SSIS) 项目和包](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。  
  
### <a name="integration-services-dashboard"></a>Integration Services 面板  
 **“Integration Services 面板”** 报告提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上所有包执行的概览。 对于已在服务器上运行的每个包，该面板允许您“放大”以便查找有关可能已发生的包执行错误的具体细节。  
  
 该报告显示以下信息部分。  
  
|部分|说明|  
|-------------|-----------------|  
|**执行信息**|显示过去 24 小时内处于不同状态（失败、正在运行、成功、其他）的执行次数。|  
|**包信息**|显示过去 24 小时内执行的包总数。|  
|**连接信息**|显示过去 24 小时内失败的执行中使用的连接。|  
|**包详细信息**|显示过去 24 小时内发生的已完成执行的详细信息。 例如，本节说明了失败的执行次数与执行总次数，执行的持续时间（秒）和过去三个月内执行的平均持续时间。<br /><br /> 通过单击 **“概述”** 、 **“所有消息”** 和 **“执行性能”** ，可以查看有关包的其他信息。<br /><br /> **“执行性能”** 报告显示上次执行实例的持续时间、开始和结束时间以及应用的环境。<br /><br /> **“执行性能”** 报告中包含的图表和关联的表显示过去 10 次成功执行包的持续时间。 该表还显示过去三个月内的平均执行持续时间。 在运行时可能会为这 10 次成功执行包应用不同的环境和不同的文字值。<br /><br /> 最后， **“执行性能”** 报告显示包数据流组件的活动时间和总时间。 活动时间指组件在所有阶段中用于执行的总共用时，而总时间指示组件占用的总时间。 如果上次包执行的日志记录级别设置为“性能”或“详细”，则该报告仅显示包组件的此信息。<br /><br /> **“概述”** 报告显示包任务的状态。 **“消息”** 报告显示包和任务的事件消息和错误消息，如报告开始和结束时间，以及写入的行数。<br /><br /> 还可以单击 **“概述”** 报告中的 **“查看消息”** ，导航到 **“消息”** 报告。 还可以单击 **“消息”** 报告中的 **“查看概述”** ，导航到 **“概述”** 报告。|  
  
 您可以通过单击 **“筛选器”** ，然后在 **“筛选设置”** 对话框中选择条件，对在任何页上显示的表进行筛选。 可用的筛选条件依赖于要显示的数据。 您可以通过在 **“筛选设置”** 对话框中单击排序图标，更改报告的排序顺序。  
  
### <a name="all-executions-report"></a>“所有执行”报告  
 **“所有执行”** 报告显示已在服务器上执行的所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 执行的摘要。 可以存在示例包的多个执行。 与 **“Integration Services 面板”** 报告不同，您可以配置 **“所有执行”** 报告以显示在日期范围内开始的执行。 该日期可以跨多天、多月或多年。  
  
 该报告显示以下信息部分。  
  
|部分|说明|  
|-------------|-----------------|  
|“筛选器”|显示应用于该报告的当前筛选器，如开始时间范围。|  
|执行信息|显示每个包执行的开始时间、结束时间和持续时间。您可以查看用于包执行的参数值列表，如使用“执行包”任务传递给子包的值。 若要查看参数列表，请单击“概述”。|  
  
 有关使用执行包任务使值可用于子包的详细信息，请参阅 [Execute Package Task](../../integration-services/control-flow/execute-package-task.md)。  
  
 有关参数的详细信息，请参阅 [Integration Services (SSIS) 包和项目参数](../../integration-services/integration-services-ssis-package-and-project-parameters.md)。  
  
### <a name="all-connections"></a>所有连接  
 **“所有连接”** 报告提供以下有关已失败的连接的信息，有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上已发生的执行的信息。  
  
 该报告显示以下信息部分。  
  
|部分|说明|  
|-------------|-----------------|  
|“筛选器”|显示应用于此报告的当前筛选器，如具有指定字符串的连接和 **“上次失败时间”** 范围。<br /><br /> 可以设置 **“上次失败时间”** 范围以仅显示在日期范围内发生的连接失败。 该范围可以跨多个天、月或年。|  
|详细信息|显示连接字符串、某个连接失败过程中的执行次数和上次连接失败的日期。|  
  
### <a name="all-operations-report"></a>“所有操作”报告  
 **“所有操作”** 报告显示已在服务器上执行的所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 操作的摘要，包括包部署、验证和执行以及其他管理操作。 与 Integration Services 面板一样，您可以将筛选器应用于表，以便缩小显示的信息的范围。  
  
### <a name="all-validations-report"></a>“所有验证”报告  
 **“所有验证”** 报告显示已在服务器上执行的所有 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 验证的摘要。 摘要中显示每次验证的信息，如状态、开始时间和结束时间。 每个摘要条目包含一个指向在验证期间生成的消息的链接。 与 Integration Services 面板一样，您可以将筛选器应用于表，以便缩小显示的信息的范围。  
  
### <a name="custom-reports"></a>自定义报告  
 可以将自定义报告（.rdl 文件）添加到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中“Integration Services 目录”节点下的 **SSISDB** 目录节点。 在添加报告前，请确认正在使用第三方命名约定以完全限定您引用的对象（如源表）。 否则， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 将显示错误。 命名约定为 \<database>.\<owner>.\<object>例如，SSISDB.internal.executions。  
  
> [!NOTE]  
>  将自定义报告添加到“数据库”节点下的 **SSISDB** 节点时，可以不使用 SSISDB 前缀。  
  
 有关如何创建和添加自定义报告的说明，请参阅 [Add a Custom Report to Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md)。  

## <a name="view-reports-for-the-integration-services-server"></a>查看 Integration Services 服务器的报告
  在当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]中， [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供了标准报告，帮助你监视部署到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  有关报表的详细信息，请参阅 [Integration Services 服务器的报表](#reports)。  
  
### <a name="to-view-reports-for-the-integration-services-server"></a>查看 Integration Services 服务器的报告  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，展开对象资源管理器中的 **“Integration Services 目录”** 节点。  
  
2.  右键单击“SSISDB”，单击“报表”，然后单击“标准报表”。  
  
3.  单击以下一个或多个选项以查看报告。  
  
    -   **Integration Services 面板**  
  
    -   **所有执行**  
  
    -   **所有验证**  
  
    -   **所有操作**  
  
    -   **所有连接**  

## <a name="see-also"></a>另请参阅  
 [项目和包的执行](../packages/deploy-integration-services-ssis-projects-and-packages.md)   
 [包执行的疑难解答报告](../troubleshooting/troubleshooting-reports-for-package-execution.md)  
