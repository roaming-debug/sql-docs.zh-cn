---
title: 模板
titleSuffix: SQl Server Profiler
description: 了解 SQL Server Profiler 提供的预定义模板以及如何使用它们。 请参阅如何创建用户定义的模板并更改默认模板。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: b674e491-dc58-47a1-acdd-7028e9a201fc
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b29ba66b9cf0e05f1585ff4f37723cf8fffba669
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643369"
---
# <a name="sql-server-profiler-templates"></a>SQL Server Profiler 模板

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

可以使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 来创建模板，定义要包含在跟踪中的事件类和数据列。 定义并保存模板后，可以运行跟踪来记录每个选定事件类的数据。 您可以将一个模板用于多个跟踪；模板本身并不会执行。  

[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]提供了预定义的跟踪模板，使您可以轻松配置特定跟踪可能最需要的事件类。 例如，Standard 模板可帮助您创建通用跟踪，用于记录登录、注销、已完成的批处理和连接信息。 您可以使用此模板来运行跟踪而无需修改，也可以基于该模板创建具有不同事件配置的其他模板。  
  
> [!NOTE]  
>  除了通过预定义模板进行跟踪以外， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 还允许您从空模板（默认情况下不包含任何事件类）创建跟踪。 当计划的跟踪与任何预定义模板的配置都不相符时，使用空跟踪模板会十分有用。  
  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 可以跟踪各种服务器类型。 例如，您可以跟踪 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  但每种服务器可以包含的事件类会有所不同。 因此， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 为不同服务器保留不同的模板，并提供与所选服务器类型匹配的特定模板。  
  
## <a name="predefined-templates"></a>预定义模板  
 除了 Standard（默认）模板以外， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 还包含几个可监视特定类型的事件的预定义模板。 下表列出了预定义模板、其用途以及其捕获何种事件类的信息。  
  
|模板名称|模板用途|事件类|  
|-------------------|----------------------|-------------------|  
|SP_Counts|捕获一段时间内存储过程的执行行为。|**SP:Starting**|  
|Standard|创建跟踪的通用起点。 捕获所运行的全部存储过程和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理。 用于监视常规数据库服务器活动。|**审核登录**<br /><br /> **审核注销**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Completed**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL|捕获客户端提交给 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句及其发出时间。 用于调试客户端应用程序。|**审核登录**<br /><br /> **审核注销**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Duration|捕获客户端提交给 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句及其执行时间（以毫秒位单位），并按持续时间对其进行分组。 用于识别执行速度慢的查询。|**RPC:Completed**<br /><br /> **SQL:BatchCompleted**|  
|TSQL_Grouped|捕获提交给 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句及其发出时间。 信息按提交语句的用户或客户端分组。 用于调查某客户端或用户发出的查询。|**审核登录**<br /><br /> **审核注销**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SQL:BatchStarting**|  
|TSQL_Locks|捕获客户端与异常锁事件一起提交到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 语句。 用于排除死锁、锁超时和锁升级事件的故障。|**Blocked Process Report**<br /><br /> **SP:StmtCompleted**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:StmtCompleted**<br /><br /> **SQL:StmtStarting**<br /><br /> **Deadlock Graph**<br /><br /> **Lock:Cancel**<br /><br /> **Lock:Deadlock**<br /><br /> **Lock:Deadlock Chain**<br /><br /> **Lock:Escalation**<br /><br /> **Lock:Timeout (timeout>0)**|  
|TSQL_Replay|捕获重播跟踪所需的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的详细信息。 用于执行迭代优化，例如基准测试。|**CursorClose**<br /><br /> **CursorExecute**<br /><br /> **CursorOpen**<br /><br /> **CursorPrepare**<br /><br /> **CursorUnprepare**<br /><br /> **审核登录**<br /><br /> **审核注销**<br /><br /> **Existing Connection**<br /><br /> **RPC Output Parameter**<br /><br /> **RPC:Completed**<br /><br /> **RPC:Starting**<br /><br /> **Exec Prepared SQL**<br /><br /> **Prepare SQL**<br /><br /> **SQL:BatchCompleted**<br /><br /> **SQL:BatchStarting**|  
|TSQL_SPs|捕获有关执行的所有存储过程的详细信息。 用于分析存储过程的组成步骤。 如果您怀疑过程正在重新编译，请添加 **SP:Recompile** 事件。|**审核登录**<br /><br /> **审核注销**<br /><br /> **ExistingConnection**<br /><br /> **RPC:Starting**<br /><br /> **SP:Completed**<br /><br /> **SP:Starting**<br /><br /> **SP:StmtStarting**<br /><br /> **SQL:BatchStarting**|  
|优化|捕获有关存储过程和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 批处理执行的信息。 用于生成跟踪输出， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问可以将该输出用作工作负荷来优化数据库。|**RPC:Completed**<br /><br /> **SP:StmtCompleted**<br /><br /> **SQL:BatchCompleted**|  
  
 有关事件类的信息，请参阅 [SQL Server Event Class Reference](../../relational-databases/event-classes/sql-server-event-class-reference.md)。  
  
## <a name="default-template"></a>默认模板  
 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 自动指定 **Standard** 模板作为应用于任何新跟踪的默认模板。 但是，您可以将默认模板更改为其他任何预定义模板或用户定义模板。 若要更改默认模板，请在使用 **“跟踪模板属性”** 对话框的 **“常规”** 选项卡创建或编辑模板时，选中 **“用作所选服务器类型的默认模板”** 复选框。  
  
 若要导航到“跟踪模板属性”对话框，请在 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]“文件”菜单中，选择“模板”，然后单击“新建模板”或“编辑模板”。  
  
> [!NOTE]  
>  默认模板针对给定的服务器类型。 更改一个服务器类型的默认模板不会影响其他任何服务器类型的默认模板。 有关设置特定服务器的默认模板的详细信息，请参阅[设置跟踪定义默认值 (SQL Server Profiler)](../../tools/sql-server-profiler/set-trace-definition-defaults-sql-server-profiler.md)。  
  
## <a name="see-also"></a>另请参阅  
 [创建跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-template-sql-server-profiler.md)   
 [修改跟踪模板 (SQL Server Profiler)](./modify-trace-templates.md)   
 [导出跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/export-a-trace-template-sql-server-profiler.md)   
 [导入跟踪模板 (SQL Server Profiler)](../../tools/sql-server-profiler/import-a-trace-template-sql-server-profiler.md)  
  
