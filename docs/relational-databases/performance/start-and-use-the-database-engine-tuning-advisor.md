---
title: 启动并使用数据库引擎优化顾问 | Microsoft Docs
description: 了解如何在 SQL Server 中启动和使用数据库引擎优化顾问来创建工作负载，优化数据库和创建 XML 输入文件。
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- sql13.dta.workload.f1
- sql13.dta.general.f1
- sql13.dta.advancedtuningoptions.f1
- sql13.dta.tuningoptions.f1
- sql13.dta.progress.f1
- sql13.dta.options.f1
helpviewer_keywords:
- Database Engine Tuning Advisor [SQL Server], starting
ms.assetid: a4e3226a-3917-4ec8-bdf0-472879d231c9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: da72d129bac65b1b9210e49d33782b257ea9e182
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236428"
---
# <a name="start-and-use-the-database-engine-tuning-advisor"></a>启动并使用数据库引擎优化顾问
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主题介绍如何在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]中启动和使用数据库引擎优化顾问。 有关如何查看和使用数据库优化结果，请参阅 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)。  
  
##  <a name="initialize-the-database-engine-tuning-advisor"></a><a name="Initialize"></a> 初始化数据库引擎优化顾问  
 第一次使用时，作为 **sysadmin** 固定服务器角色成员的用户必须初始化数据库引擎优化顾问。 这是因为必须在 **msdb** 数据库中创建多个系统表才能支持优化操作。 如果用户是 **db_owner** 固定数据库角色的成员，初始化还可以使他们能够优化数据库（他们拥有的数据库）中的表的工作负荷。  
  
 具有系统管理员权限的用户必须执行下列操作之一：  
  
-   使用数据库引擎优化顾问图形用户界面连接到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]实例。 有关详细信息，请参阅本主题后面的 [启动数据库引擎优化顾问](#Start) 。  
  
-   使用 **dta** 实用工具优化第一个工作负荷。 有关详细信息，请参阅本主题后面的 [使用 dta 实用工具](#dta) 。  
  
##  <a name="start-the-database-engine-tuning-advisor"></a><a name="Start"></a> 启动数据库引擎优化顾问  
 可以用几种不同的方式启动数据库引擎优化顾问图形用户界面 (GUI)，以支持不同情况下的数据库优化。 启动数据库引擎优化顾问的其他方式包括：通过 **“开始”** 菜单启动，通过 **中的** “工具” [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]菜单启动，通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的查询编辑器启动，以及通过 **中的** “工具” [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]菜单启动。 第一次启动数据库引擎优化顾问时，该应用程序将显示一个 **“连接到服务器”** 对话框，您可以在该对话框中指定要连接的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
> [!WARNING]  
>  当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以单用户模式运行时，不要启动数据库引擎优化顾问。 如果试图在服务器处于单用户模式时启动该服务器，将返回错误，并且不会启动数据库引擎优化顾问。 有关单用户模式的详细信息，请参阅 [在单用户模式下启动 SQL Server](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)。  
  
#### <a name="to-start-database-engine-tuning-advisor-from-the-windows-start-menu"></a>通过 Windows“开始”菜单启动数据库引擎优化顾问  
  
1.  在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 **“Microsoft SQL Server”** 和 **“性能工具”** ，然后单击 **“数据库引擎优化顾问”** 。  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-management-studio"></a>在 SQL Server Management Studio 中启动数据库引擎优化顾问  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的“工具”菜单中，单击“数据库引擎优化顾问” 。  
  
#### <a name="to-start-the-database-engine-tuning-advisor-from-the-sql-server-management-studio-query-editor"></a>在 SQL Server Management Studio 查询编辑器中启动数据库引擎优化顾问  
  
1.  在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中打开 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]脚本文件。 有关详细信息，请参阅[查询和文本编辑器 (SQL Server Management Studio)](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md)。  
  
2.  在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本中选择一个查询，或选择整个脚本，右键单击选定的内容，再选择“在数据库引擎优化顾问中分析查询”。 此时将打开数据库引擎优化顾问图形用户界面，并将该脚本作为 XML 文件工作负荷导入。 可以指定会话名称和优化选项，以将选定的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询作为工作负荷进行优化。  
  
#### <a name="to-start-the-database-engine-tuning-advisor-in-sql-server-profiler"></a>在 SQL Server Profiler 中启动数据库引擎优化顾问  
  
1.  在 SQL Server Profiler 的 **“工具”** 菜单中，单击 **“数据库引擎优化顾问”** 。  
  
##  <a name="create-a-workload"></a><a name="Create"></a> 创建工作负荷  
 工作负荷是对要优化的一个或多个数据库执行的一组 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 数据库引擎优化顾问将分析这些工作负荷以便建议要使用的索引或分区策略来改善服务器的查询性能。  
  
 您可以通过使用以下方法之一创建工作负荷。  
  
-   将 [Query Store](../../relational-databases/performance/how-query-store-collects-data.md) 用作工作负荷。 这样，可以避免手动创建工作负荷。 有关详细信息，请参阅[使用 Query Store 中的工作负荷优化数据库](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md)。  

      ||  
      |-|  
      |**适用于**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本。|  

  
-   将计划缓存用作工作负荷。 这样，可以避免手动创建工作负荷。 有关详细信息，请参阅本主题后面的 [优化数据库](#Tune) 。  
  
-   使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的查询编辑器或您最喜欢的文本编辑器来手动创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本工作负荷。  
  
-   使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 创建跟踪文件或跟踪表工作负荷  
  
    > [!NOTE]  
    >  使用跟踪表作为工作负荷时，该表必须位于数据库引擎优化顾问正在优化的那台服务器上。 如果您创建的跟踪表位于其他服务器上，请将其移到数据库引擎优化顾问正在优化的服务器上。  
  
-   工作负荷也可以嵌入到 XML 输入文件，在此文件中您也可以为每一事件指定一个权重。 有关指定嵌入的工作负荷的详细信息，请参阅本主题后面的 [创建 XML 输入文件](#XMLInput) 。  
  
###  <a name="to-create-transact-sql-script-workloads"></a><a name="SSMS"></a> 创建 TRANSACT-SQL 脚本工作负荷  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中启动查询编辑器。 有关详细信息，请参阅[查询和文本编辑器 (SQL Server Management Studio)](../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md)。  
  
2.  在查询编辑器中键入您的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本。 此脚本应包含一组对想要优化的数据库执行的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。  
  
3.  使用 **.sql** 扩展名保存文件。 数据库引擎优化顾问 GUI 和命令行 **dta** 实用工具可以将此 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本用作工作负荷。  
  
###  <a name="to-create-trace-file-and-trace-table-workloads"></a><a name="Profiler"></a> 创建跟踪文件和跟踪表工作负荷  
  
1.  使用下列方法之一启动 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] ：  
  
    -   在 **“开始”** 菜单中，依次指向 **“所有程序”** 、 **Microsoft SQL Server**、 **“性能工具”** ，然后单击 **SQL Server Profiler**。  
  
    -   在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，单击 **“工具”** 菜单，然后单击 **“SQL Server Profiler”** 。  
  
2.  按照下面介绍的步骤，使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 的“优化”模板来创建跟踪文件或表：  
  
    -   [创建跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)  
  
    -   [将跟踪结果保存到文件 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-file-sql-server-profiler.md)  
  
         数据库引擎优化顾问假定工作负荷跟踪文件是滚动更新文件。 有关滚动更新文件的详细信息，请参阅 [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md)。  
  
    -   [将跟踪结果保存到表 (SQL Server Profiler)](../../tools/sql-server-profiler/save-trace-results-to-a-table-sql-server-profiler.md)  
  
         确保在将跟踪表用作工作负荷之前已经停止了跟踪。  
  
 建议使用 SQL Server Profiler 优化模板来为数据库引擎优化顾问捕获工作负荷。  
  
 如果要使用自己的模板，请确保捕获了以下跟踪事件：  
  
-   **RPC:Completed**  
  
-   **SQL:BatchCompleted**  
  
-   **SP:StmtCompleted**  
  
 也可以使用这些跟踪事件的 **Starting** 版本。 例如， **SQL:BatchStarting**。 但是，这些跟踪事件的 **Completed** 版本包括 **Duration** 列，它能使数据库引擎优化顾问更有效地优化工作负荷。 数据库引擎优化顾问不优化其他类型的跟踪事件。 有关这些跟踪事件的详细信息，请参阅 [Stored Procedures Event Category](../../relational-databases/event-classes/stored-procedures-event-category.md) 和 [TSQL Event Category](../../relational-databases/event-classes/tsql-event-category.md)。 有关使用 SQL 跟踪存储过程来创建跟踪文件工作负荷的信息，请参阅[创建跟踪 (Transact-SQL)](../../relational-databases/sql-trace/create-a-trace-transact-sql.md)。  
  
### <a name="trace-file-or-trace-table-workloads-that-contain-the-loginname-data-column"></a>包含 LoginName 数据列的跟踪文件或跟踪表工作负荷  
 数据库引擎优化顾问在优化过程中提交显示计划请求。 当包含 **LoginName** 数据列的跟踪表或跟踪文件被用作工作负荷时，数据库引擎优化顾问将模拟 **LoginName** 中指定的用户。 如果没有为此用户授予 SHOWPLAN 权限（该权限使用户能够为跟踪中包含的语句执行和生成显示计划），数据库引擎优化顾问将不会优化这些语句。  
  
##### <a name="to-avoid-granting-the-showplan-permission-to-each-user-specified-in-the-loginname-column-of-the-trace"></a>避免为跟踪的 LoginName 列中指定的每个用户授予 SHOWPLAN 权限  
  
1.  优化跟踪文件或跟踪表工作负荷。 有关详细信息，请参阅本主题后面的 [优化数据库](#Tune) 。  
  
2.  在优化日志中查看由于权限不足而没有优化的语句。 有关详细信息，请参阅 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)。  
  
3.  通过从未优化的事件中删除 **LoginName** 列来创建新的工作负荷，然后只将未优化的事件保存到新的跟踪文件或跟踪表中。 有关从跟踪中删除数据列的详细信息，请参阅[指定跟踪文件的事件和数据列 (SQL Server Profiler)](../../tools/sql-server-profiler/specify-events-and-data-columns-for-a-trace-file-sql-server-profiler.md) 或[修改现有跟踪 (Transact SQL)](../../relational-databases/sql-trace/modify-an-existing-trace-transact-sql.md)。  
  
4.  将不带 **LoginName** 列的新工作负荷重新提交到数据库引擎优化顾问。  
  
 数据库引擎优化顾问将优化新的工作负荷，因为跟踪中未指定登录信息。 如果某个语句没有相应的 **LoginName** ，数据库引擎优化顾问将通过模拟启动优化会话的用户（ **sysadmin** 固定服务器角色或 **db_owner** 固定数据库角色的成员）来优化该语句。  
  
##  <a name="tune-a-database"></a><a name="Tune"></a> 优化数据库  
 若要优化数据库，可以使用数据库引擎优化顾问 GUI 或 **dta** 实用工具。  
  
> [!NOTE]  
>  在使用跟踪表作为数据库引擎优化顾问的工作负荷之前，确保跟踪已停止。 数据库引擎优化顾问不支持将还在写入跟踪事件的跟踪表作为工作负荷使用。  
  
### <a name="use-the-database-engine-tuning-advisor-graphical-user-interface"></a>使用数据库引擎优化顾问图形用户界面  
 在数据库引擎优化顾问 GUI 上，可以利用计划缓存、工作负荷文件或工作负荷表来优化数据库。 可使用数据库引擎优化顾问 GUI 轻松查看您当前的优化会话结果和以前的优化会话结果。 有关用户界面选项的信息，请参阅本主题后面的 [用户界面说明](#UI) 。 有关使用数据库优化结果的详细信息，请参阅 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)。  

####  <a name="to-tune-a-database-by-using-the-query-store"></a><a name="PlanCache"></a> 使用 Query Store 优化数据库
有关详细信息，请参阅[使用 Query Store 中的工作负荷优化数据库](../../relational-databases/performance/tuning-database-using-workload-from-query-store.md)。
  
####  <a name="to-tune-a-database-by-using-the-plan-cache"></a><a name="PlanCache"></a> 使用计划缓存优化数据库  
  
1.  启动数据库引擎优化顾问，并登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例。 有关详细信息，请参阅本主题前面的 [启动数据库引擎优化顾问](#Start) 。  
  
2.  在 **“常规”** 选项卡上，在 **“会话名称”** 中键入一个名称以创建新的优化会话。 在启动优化会话之前，必须配置 **“常规”** 选项卡中的字段。 在启动优化会话之前，不必修改 **“优化选项”** 选项卡的设置。  
  
3.  选择 **“计划缓存”** 作为工作负荷选项。 数据库引擎优化顾问将从计划缓存中选择前 1,000 个事件用于分析。  
  
4.  选择要优化的数据库，或者可选择从 **“选择的表”** 中为每个数据库选择一个或多个表。 若要包括所有数据库的缓存项，请从 **“优化选项”** 中单击 **“高级选项”** ，然后选中 **“包括来自所有数据库的计划缓存事件”** 。  
  
5.  选中 **“保存优化日志”** 以保存优化日志的副本。 如果不希望保存优化日志的副本，请清除该复选框。  
  
     在分析之后，可以通过打开会话并选择 **“进度”** 选项卡来查看优化日志。  
  
6.  单击 **“优化选项”** 选项卡，从列出的选项中进行选择。  
  
7.  单击 **“开始分析”** 。  
  
     如果希望停止已经启动的优化会话，请在 **“操作”** 菜单上选择以下选项之一：  
  
    -   选择“停止分析（并提供建议）”将停止优化会话，并提示你选择是否希望数据库引擎优化顾问根据目前已完成的分析来生成建议。  
  
    -   选择 **“停止分析”** 将停止优化会话而不生成任何建议。  
  
> [!NOTE]  
>  不支持暂停数据库引擎优化顾问。 如果在单击“停止分析”或“停止分析（并提供建议）”工具栏按钮之后单击“开始分析”工具栏按钮，数据库引擎优化顾问将启动新的优化会话。    
  
#### <a name="to-tune-a-database-using-a-workload-file-or-table-as-input"></a>使用工作负荷文件或表作为输入来优化数据库  
  
1.  确定您希望数据库引擎优化顾问在分析过程中考虑添加、删除或保留的数据库功能（索引、索引视图、分区）。  
  
2.  创建工作负荷。 有关详细信息，请参阅本主题前面的 [创建工作负荷](#Create) 。  
  
3.  启动数据库引擎优化顾问，并登录到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅本主题前面的 [启动数据库引擎优化顾问](#Start) 。  
  
4.  在 **“常规”** 选项卡上，在 **“会话名称”** 中键入一个名称以创建新的优化会话。  
  
5.  选择一个 **“工作负荷文件”** 或 **“表”** ，然后在相邻的文本框中键入文件的路径或表的名称。  
  
     指定表的格式为  
  
    ```  
    database_name.schema_name.table_name  
    ```  
  
     若要搜索工作负荷文件或表，请单击 **“浏览”** 。 数据库引擎优化顾问假定工作负荷文件是滚动更新文件。 有关滚动更新文件的详细信息，请参阅 [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md)。  
  
     使用跟踪表作为工作负荷时，该表必须存在于数据库引擎优化顾问正在优化的同一台服务器上。 如果您创建的跟踪表在其他服务器上，则必须将其移到数据库引擎优化顾问准备优化的服务器上才能用作工作负荷。  
  
6.  选择要对其运行在步骤 5 中选择的工作负荷的数据库和表。 要选择表，请单击 **“所选表”** 箭头。  
  
7.  选中 **“保存优化日志”** 以保存优化日志的副本。 如果不希望保存优化日志的副本，请清除该复选框。  
  
     在分析之后，可以通过打开会话并选择 **“进度”** 选项卡来查看优化日志。  
  
8.  单击 **“优化选项”** 选项卡，从列出的选项中进行选择。  
  
9. 单击工具栏中的 **“开始分析”** 按钮。  
  
     如果希望停止已经启动的优化会话，请在 **“操作”** 菜单上选择以下选项之一：  
  
    -   选择“停止分析（并提供建议）”将停止优化会话，并提示你选择是否希望数据库引擎优化顾问根据目前已完成的分析来生成建议。  
  
    -   选择 **“停止分析”** 将停止优化会话而不生成任何建议。  
  
> [!NOTE]  
>  不支持暂停数据库引擎优化顾问。 如果在单击“停止分析”或“停止分析（并提供建议）”工具栏按钮之后单击“开始分析”工具栏按钮，数据库引擎优化顾问将启动新的优化会话。    
  
###  <a name="use-the-dta-utility"></a><a name="dta"></a> 使用 dta 实用工具  
 [dta 实用工具](../../tools/dta/dta-utility.md) 提供了一个命令提示符可执行文件，可以用来优化数据库。 该实用工具使您能够在批处理文件和脚本中使用数据库引擎优化顾问的功能。 **dta** 实用工具使用计划缓存项、跟踪文件、跟踪表和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本作为工作负荷。 它还将使用符合数据库引擎优化顾问 XML 架构的 XML 输入，有关该架构的详细信息，请访问此 [Microsoft 网站](https://go.microsoft.com/fwlink/?linkid=43100)。  
  
 在使用 **dta** 实用工具开始优化工作负荷之前，请考虑下列事项：  
  
-   使用跟踪表作为工作负荷时，该表必须存在于数据库引擎优化顾问正在优化的同一台服务器上。 如果在其他服务器上创建了跟踪表，则将该跟踪表移动到数据库引擎优化顾问正在优化的服务器。  
  
-   在使用跟踪表作为数据库引擎优化顾问的工作负荷之前，确保跟踪已停止。 数据库引擎优化顾问不支持将还在写入跟踪事件的跟踪表作为工作负荷使用。  
  
-   如果优化会话运行的时间比预计运行时间长，可以按 CTRL+C 停止优化会话并根据 **dta** 此时已完成的分析生成建议。 系统将提示您确定是否要生成建议。 再次按 Ctrl+C 停止优化会话，而不生成建议。  
  
 有关 **dta** 实用工具语法和示例的详细信息，请参阅 [dta Utility](../../tools/dta/dta-utility.md)。  
  
#### <a name="to-tune-a-database-by-using-the-plan-cache"></a>使用计划缓存优化数据库  
  
1.  指定 **-ip** 选项。 分析选定数据库的前 1000 个计划缓存事件。  
  
     在命令提示符下，输入以下内容：  
  
    ```  
    dta -E -D DatabaseName -ip -s SessionName  
    ```  
  
2.  若要修改用于分析的事件数，请指定“–n”选项。 以下示例将缓存项数提高到 2,000。  
  
    ```  
    dta -E -D DatabaseName -ip -n 2000-s SessionName1  
    ```  
  
3.  若要分析实例中的所有数据库的事件，请指定 **-ipf** 选项。  
  
    ```  
    dta -E -D DatabaseName -ip -ipf -n 2000 -s SessionName2  
    ```  
  
#### <a name="to-tune-a-database-by-using-a-workload-and-dta-utility-default-settings"></a>使用工作负荷和 dta 实用工具的默认设置优化数据库  
  
1.  确定您希望数据库引擎优化顾问在分析过程中考虑添加、删除或保留的数据库功能（索引、索引视图、分区）。  
  
2.  创建工作负荷。 有关详细信息，请参阅本主题前面的 [创建工作负荷](#Create) 。  
  
3.  在命令提示符下，输入以下内容：  
  
    ```  
    dta -E -D DatabaseName -if WorkloadFile -s SessionName  
    ```  
  
     其中， `-E` 指定优化会话使用的是可信连接（而不是登录 ID 和密码）， `-D` 指定要优化的数据库的名称。 默认情况下，实用工具会连接到本地计算机上的默认 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 （使用 `-S` 选项可以像下面过程中显示的那样指定远程数据库，或者指定命名实例。） `-if` 选项指定工作负荷文件（可以是 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本或跟踪文件）的名称和路径， `-s` 指定优化会话的名称。  
  
     此处显示的四个选项（数据库名称、工作负荷、连接类型和会话名称）是必需的。  
  
#### <a name="to-tune-a-remote-database-or-a-named-instance-for-a-specific-duration"></a>在特定的持续时间内优化远程数据库或命名实例  
  
1.  确定您希望数据库引擎优化顾问在分析过程中考虑添加、删除或保留的数据库功能（索引、索引视图、分区）。  
  
2.  创建工作负荷。 有关详细信息，请参阅本主题前面的 [创建工作负荷](#Create) 。  
  
3.  在命令提示符下，输入以下内容：  
  
    ```  
    dta -S ServerName\Instance -D DatabaseName -it WorkloadTableName   
    -U LoginID -P Password -s SessionName -A TuningTimeInMinutes  
    ```  
  
     其中， `-S` 指定远程服务器的名称和实例（而不是本地服务器上的命名实例）， `-D` 指定要优化的数据库的名称。 `-it` 选项指定工作负荷表的名称， `-U` 和 `-P` 指定登录远程数据库的登录 ID 和密码， `-s` 指定优化会话的名称， `-A` 指定优化会话的持续时间（分钟）。 默认情况下， **dta** 实用工具使用的优化持续时间为 8 小时。 如果希望数据库引擎优化顾问在时间长度不限的条件下优化工作负荷，请将 **选项指定为** 0 `-A` （零）。  
  
#### <a name="to-tune-a-database-using-an-xml-input-file"></a>使用 XML 输入文件优化数据库  
  
1.  确定您希望数据库引擎优化顾问在分析过程中考虑添加、删除或保留的数据库功能（索引、索引视图、分区）。  
  
2.  创建工作负荷。 有关详细信息，请参阅本主题前面的 [创建工作负荷](#Create) 。  
  
3.  创建 XML 输入文件。 有关详细信息，请参阅本主题后面的 [创建 XML 输入文件](#XMLInput) 。  
  
4.  在命令提示符下，输入以下内容：  
  
    ```  
    dta -E -S ServerName\Instance -s SessionName -ix PathToXMLInputFile  
    ```  
  
     其中， `-E` 指定可信连接， `-S` 指定远程服务器和实例或本地服务器上的命名实例， `-s` 指定优化会话的名称， `-ix` 指定用于优化会话的 XML 输入文件。  
  
5.  实用工具完成工作负荷的优化之后，可以使用数据库引擎优化顾问 GUI 查看优化会话的结果。 还有一种方法，可以使用 **-ox** 选项指定将优化建议写入 XML 文件。 有关详细信息，请参阅 [dta Utility](../../tools/dta/dta-utility.md)。  
  
##  <a name="create-an-xml-input-file"></a><a name="XMLInput"></a> 创建 XML 输入文件  
 如果是有经验的 XML 开发人员，您可以创建一些 XML 格式的文件， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可使用这些文件来优化工作负荷。 若要创建这些 XML 文件，请使用您最喜爱的 XML 工具编辑示例文件，或者通过 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 架构生成实例。  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 架构位于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的以下位置：  
  
 `C:\Program Files\Microsoft SQL Server\100\Tools\Binn\schemas\sqlserver\2004\07\dta\dtaschema.xsd` 
  
 此 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Microsoft 网站 [上也在线提供了](https://go.microsoft.com/fwlink/?linkid=43100&clcid=0x409)优化顾问 XML 架构。  
  
 单击此 URL 可打开一个包含许多 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] XML 架构的页面。 向下滚动页面，直至找到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问所在的行。  
  
#### <a name="to-create-an-xml-input-file-to-tune-workloads"></a>创建 XML 输入文件以优化工作负荷  
  
1.  创建工作负荷。 您可以通过使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]中的优化模板来使用跟踪文件或表，或创建可产生典型 [!INCLUDE[tsql](../../includes/tsql-md.md)] 工作负荷的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]脚本。 有关详细信息，请参阅本主题前面的 [创建工作负荷](#Create) 。  
  
2.  使用下列方法之一创建 XML 输入文件：  
  
    -   复制一个 [XML 输入文件示例 (DTA)](../../tools/dta/xml-input-file-samples-dta.md) 并将其粘贴到你最喜爱的 XML 编辑器。 更改值来为安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指定适当的参数，然后保存 XML 文件。  
  
    -   使用您最喜爱的 XML 工具，通过 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问 XML 架构生成一个实例。  
  
3.  创建 XML 输入文件之后，将它用作 **dta** 命令行实用工具的输入来优化工作负荷。 有关通过此实用工具使用 XML 输入文件的信息，请参阅本主题前面的 [使用 dta 实用工具](#dta) 部分。  
  
> [!NOTE]  
>  如果你要使用内联工作负荷（即在 XML 输入文件中直接指定的工作负荷），请使用[内联工作负荷的 XML 输入文件示例 (DTA)](../../tools/dta/xml-input-file-sample-with-inline-workload-dta.md) 示例。  
  
##  <a name="user-interface-descriptions"></a><a name="UI"></a> 用户界面说明  
  
### <a name="tools-menuoptions-page"></a>“工具”菜单/“选项”页  
 使用此对话框可以为数据库引擎优化顾问指定常规配置参数。  
  
 **启动时**  
 指定数据库引擎优化顾问在启动时应执行的操作：以不连接数据库的方式打开、显示“新建连接”对话框、显示新会话或者加载上次已加载的会话。  
  
 **更改字体**  
 指定由数据库引擎优化顾问表使用的显示字体。  
  
 **最近使用的列表中的项数**  
 指定在“文件”菜单的“最近使用的会话”和“最近使用的文件”下显示的会话数或文件数。  
  
 **记住我上次设置的优化选项**  
 在会话之间保留优化选项。 默认为选中状态。 如果清除此复选框，则总是使用数据库引擎优化顾问默认值启动。  
  
 **在永久删除会话之前询问**  
 在删除会话之前显示确认对话框。  
  
 **在停止会话分析之前询问**  
 在停止工作负荷分析之前，显示确认对话框。  
  
#### <a name="general-tab-options"></a>“常规”选项卡选项  
 在启动优化会话之前，必须配置 **“常规”** 选项卡中的字段。 在启动优化会话之前，无需修改 **“优化选项”** 选项卡的设置。  
  
 **“会话名称”**  
 指定会话的名称。 会话名称将名称与优化会话相关联。 此后，您可以通过引用此名称来查看优化会话。  
  
 **File**  
 为工作负荷指定 .sql 脚本或跟踪文件。 在关联的文本框中指定路径和文件名。 数据库引擎优化顾问假定工作负荷跟踪文件是滚动更新文件。 有关滚动更新文件的详细信息，请参阅 [Limit Trace File and Table Sizes](../../relational-databases/sql-trace/limit-trace-file-and-table-sizes.md)。  
  
 **表**  
 为工作负荷指定跟踪表。 在关联的文本框中指定跟踪表的完全限定名称，如下所示：  
  
```  
database_name.owner_name.table_name  
```  
  
-   确保在将跟踪表用作工作负荷之前已经停止了跟踪。  
  
-   该跟踪表必须位于数据库引擎优化顾问正在优化的同一台服务器上。 如果在其他服务器上创建了跟踪表，则将该跟踪表移动到数据库引擎优化顾问正在优化的服务器。  
  
 **“计划缓存”**  
 将计划缓存指定为工作负荷。 这样，可以避免手动创建工作负荷。 数据库引擎优化顾问将选择前 1,000 个事件用于分析。  
  
 **Xml**  
 除非您从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中导入了工作负荷查询，否则不会显示此项。  
  
 若要从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中导入工作负荷查询，请执行以下操作：  
  
1.  在查询编辑器中键入查询并突出显示该查询。  
  
2.  右键单击突出显示的查询，并单击“在数据库引擎优化顾问中分析查询”。  
  
 **查找工作负荷文件或查找工作负荷表**  
 选择“文件”或“表”作为工作负荷源时，请使用此浏览按钮选择目标。  
  
 **预览 XML 工作负荷**  
 查看从 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中导出的 XML 格式的工作负荷。  
  
 **用于工作负荷分析的数据库**  
 指定数据库引擎优化顾问在优化工作负荷时连接的第一个数据库。 优化开始之后，数据库引擎优化顾问连接到由工作负荷中包含的 `USE DATABASE` 语句所指定的数据库。  
  
 **选择要优化的数据库和表**  
 指定要优化的数据库和表。 若要指定所有数据库，请选中 **“名称”** 列标题中的复选框。 若要指定特定数据库，请选中数据库名称旁的复选框。 默认情况下，选定数据库的所有表都自动包括在优化会话中。 如果要使优化会话不包括某些表，请单击 **“选定的表”** 列中的箭头，再清除不希望优化的表旁边的复选框。  
  
 “所选表”下箭头  
 展开表列表以允许选择个别表进行优化。  
  
 **“保存优化日志”**  
 在会话期间创建日志并记录错误。  
  
> [!NOTE]  
>  数据库引擎优化顾问不会自动更新 **“常规”** 选项卡上所显示表的行信息。相反，它依赖于数据库中的元数据。 如果怀疑行信息过时，请对相关的对象运行 DBCC UPDATEUSAGE 命令。  
  
##### <a name="tuning-tab-options"></a>“优化”选项卡选项  
 使用 **“优化选项”** 选项卡可以修改常规优化选项的默认设置。 在启动优化会话之前，无需修改 **“优化选项”** 选项卡的设置。  
  
 **限制优化时间**  
 限制当前优化会话的时间。 提供更多优化时间可以提高建议质量。 为了确保获取最佳的建议，请不要选中此选项。  
  
> [!NOTE]  
> [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问在分析期间会占用系统资源。 使用 **“限制优化时间”** 会在对服务器上预期的高工作负荷进行优化之前停止优化。  
  
 **“高级选项”**  
 使用“高级优化选项”对话框可以配置有关最大空间、最大键列数和联机索引的建议设置。  
  
 **定义建议所用的最大空间(MB)**  
 键入数据库引擎优化顾问建议的供物理设计结构使用的最大空间量。  
  
 如果没有在此处输入值，则数据库引擎优化顾问将假定为以下空间限制中的较小者：  
  
-   当前原始数据大小的三倍，包括数据库中表的堆和聚集索引的总大小。  
  
-   所有已附连磁盘驱动器的可用空间加上原始数据的大小。  
  
 **“包括来自所有数据库的计划缓存事件”**  
 指定将分析来自所有数据库的计划缓存事件。  
  
 **每个索引的最大列数**  
 指定任一索引中可包括的最大列数。 默认值为 1023。  
  
 **所有建议均为脱机建议**  
 生成可能的最佳建议，但不建议联机创建任何物理设计结构。  
  
 **如果可能，则生成联机建议**  
 在创建 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句以实现建议时，即使可以使用更快的脱机方法，也选用可以在服务器上联机实现的方法。  
  
 **仅生成联机建议**  
 仅生成允许服务器保持联机的建议。  
  
 **结束时间**  
 提供 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问的结束日期和时间。  
  
 **索引和索引视图**  
 选中此框可包括添加聚集索引、非聚集索引和索引视图建议。  
  
 **索引视图**  
 只包括添加索引视图建议。 不会为聚集和非聚集索引提供建议。  
  
 **包括筛选的索引**  
 包括用来添加筛选索引的建议。 如果选择下列物理设计结构之一，则此选项可用：索引和索引视图、索引或非聚集索引  。  
  
 **索引**  
 只包括添加聚集和非聚集索引建议。 不会为索引视图提供建议。  
  
 **“非聚集索引”**  
 只包括对非聚集索引的建议。 不会为聚集索引和索引视图提供建议。  
  
 **仅计算现有 PDS 的使用率**  
 评估当前索引的效用，但不会为其他索引或索引视图提供建议。  
  
 **不分区**  
 不提供分区建议。  
  
 **完全分区**  
 包含分区建议。  
  
 **对齐分区**  
 将对齐新建议的分区以便于维护。  
  
 **不保留任何现有 PDS**  
 建议删除不必要的现有索引、视图和分区。 如果现有物理设计结构 (PDS) 对工作负荷有用， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问会建议不要删除该结构。  
  
 **仅保留索引**  
 保留所有现有索引，但建议删除不必要的索引视图和分区。  
  
 **保留所有现有 PDS**  
 保留所有现有索引、索引视图和分区。  
  
 **仅保留聚集索引**  
 保留所有现有聚集索引，但建议删除不必要的索引视图、分区和非聚集索引。  
  
 **保留对齐分区**  
 保留当前对齐的分区结构，但建议删除不必要的索引视图、索引和未对齐的分区。 建议的其他任何分区将与当前分区方案对齐。  
  
###### <a name="progress-tab-options"></a>“进度”选项卡选项  
 数据库引擎优化顾问开始分析工作负荷后，会显示数据库引擎优化顾问的 **“进度”** 选项卡。  
  
 如果希望停止已经启动的优化会话，请在 **“操作”** 菜单上选择以下选项之一：  
  
-   选择“停止分析（并提供建议）”将停止优化会话，并提示你选择是否希望数据库引擎优化顾问根据目前已完成的分析来生成建议。  
  
-   选择 **“停止分析”** 将停止优化会话而不生成任何建议。  
  
 **优化进度**  
 指示进度的当前状态。 其中包含已执行操作的数量，以及接收到的错误、成功和警告消息的数量。  
  
 **详细信息**  
 包含指示状态的图标。  
  
 **Action**  
 显示正在执行的步骤。  
  
 **Status**  
 显示操作步骤的状态。  
  
 **消息**  
 包含操作步骤返回的所有消息。  
  
 **优化日志**  
 包含与此优化会话有关的信息。 若要打印此日志，请右键单击此日志，再单击“打印”。  
  
## <a name="see-also"></a>另请参阅  
 [查看和使用数据库引擎优化顾问的输出](../../relational-databases/performance/view-and-work-with-the-output-from-the-database-engine-tuning-advisor.md)   
 [dta 实用工具](../../tools/dta/dta-utility.md)    
 [教程：数据库引擎优化顾问](../../tools/dta/tutorial-database-engine-tuning-advisor.md)
