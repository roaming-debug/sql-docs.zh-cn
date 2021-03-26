---
description: sys.dm_os_sys_info (Transact-SQL)
title: sys.dm_os_sys_info (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_sys_info_TSQL
- dm_os_sys_info
- dm_os_sys_info_TSQL
- sys.dm_os_sys_info
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_sys_info dynamic management view
- time [SQL Server], instance started
- starting time
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c6181968642860a24c94046eb12f3aed3ea9ae4e
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551648"
---
# <a name="sysdm_os_sys_info-transact-sql"></a>sys.dm_os_sys_info (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回一组有关计算机和有关 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] 可用资源及其已占用资源的有用杂项信息。  
  
> [!NOTE]
> 若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 `sys.dm_pdw_nodes_os_sys_info` 。  
  
|列名称|数据类型|说明和版本特定说明 |  
|-----------------|---------------|-----------------|  
|**cpu_ticks**|**bigint**|指定当前的 CPU 时钟周期计数。 CPU 时钟周期数是从处理器的 RDTSC 计数器获得的。 它是一个仅增加的数字。 不可为 Null。|  
|**ms_ticks**|**bigint**|指定自从计算机启动以来的毫秒数。 不可为 Null。|  
|**cpu_count**|**int**|指定系统中的逻辑 CPU 数。 不可为 Null。|  
|**hyperthread_ratio**|**int**|指定一个物理处理器包公开的逻辑内核数与物理内核数的比。 不可为 Null。|  
|**physical_memory_in_bytes**|**bigint**|**适用于：** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。<br /><br /> 指定计算机上的物理内存总量。 不可为 Null。|  
|**physical_memory_kb**|**bigint**|适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定计算机上的物理内存总量。 不可为 Null。|  
|**virtual_memory_in_bytes**|**bigint**|**适用于：** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。<br /><br /> 对用户模式进程可用的虚拟内存的数量。 通过使用 3 GB 开关，可以用它来确定是否 SQL Server 已启动。|  
|**virtual_memory_kb**|**bigint**|适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定对用户模式进程可用的虚拟地址空间的总量。 不可为 Null。|  
|**bpool_committed**|**int**|**适用于：** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。<br /><br /> 表示内存管理器中的已提交内存 (KB)。 不包括内存管理器中的保留内存。 不可为 Null。|  
|**committed_kb**|**int**|适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 表示内存管理器中的已提交内存 (KB)。 不包括内存管理器中的保留内存。 不可为 Null。|  
|**bpool_commit_target**|**int**|**适用于：** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。<br /><br /> 表示 SQL Server 内存管理器可以占用的内存量 (KB)。|  
|**committed_target_kb**|**int**|适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 表示 SQL Server 内存管理器可以占用的内存量 (KB)。 使用类似如下的不同输入计算目标量：<br /><br /> -包括其负载的系统的当前状态<br /><br /> -当前进程请求的内存<br /><br /> -计算机上安装的内存量<br /><br /> -配置参数<br /><br /> 如果大于 `committed_target_kb` `committed_kb` ，内存管理器将尝试获得额外内存。 如果小于 `committed_target_kb` `committed_kb` ，内存管理器将尝试减少提交的内存量。 `committed_target_kb`始终包括被盗和保留的内存。 不可为 Null。|  
|**bpool_visible**|**int**|**适用于：** [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。<br /><br /> 在进程虚拟地址空间中可直接访问的缓冲池中的 8 KB 缓冲区数。 当未使用地址窗口化扩展插件 (AWE)，缓冲区池已获取其内存目标 (bpool_committed = bpool_commit_target) 时，bpool_visible 的值与 bpool_committed 的值相等。当在 32 位版本的 SQL Server 上使用 AWE 时，bpool_visible 表示用于访问由缓冲池区分配的物理内存的 AWE 映射窗口的大小。 此映射窗口的大小由进程地址空间绑定，因此，可见数量将小于提交数量，并且通过为数据库页之外的其他用途而消耗内存的内部组件会进一步减少可见数量。 如果 bpool_visible 的值太低，则可能收到内存不足错误。|  
|**visible_target_kb**|**int**|适用于：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 与相同 `committed_target_kb` 。 不可为 Null。|  
|**stack_size_in_bytes**|**int**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建的每个线程的调用堆栈的大小。 不可为 Null。|  
|**os_quantum**|**bigint**|表示非抢先任务的量程（以毫秒数度量）。 量程 (秒) = `os_quantum` /CPU 时钟速度。 不可为 Null。|  
|**os_error_mode**|**int**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的错误模式。 不可为 Null。|  
|**os_priority_class**|**int**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程的优先级类。 可以为 NULL。<br /><br /> 32 = 正常（错误日志将指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正以正常的优先级基数 (=7) 启动）。<br /><br /> 128 = 高（错误日志将指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正以高优先级基数启动）。  (=13)。）<br /><br /> 有关详细信息，请参阅 [配置 priority boost 服务器配置选项](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md)。|  
|**max_workers_count**|**int**|表示可以创建的最大工作线程数。 不可为 Null。|  
|**scheduler_count**|**int**|表示在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中配置的用户计划程序数。 不可为 Null。|  
|**scheduler_total_count**|**int**|表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的计划程序总数。 不可为 Null。|  
|**deadlock_monitor_serial_number**|**int**|指定当前死锁监视序列的 ID。 不可为 Null。|  
|**sqlserver_start_time_ms_ticks**|**bigint**|表示上次启动时的 **ms_tick** 数字 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 与当前列进行比较 `ms_ticks` 。 不可为 Null。|  
|**sqlserver_start_time**|**datetime**|指定上次启动的本地系统日期和时间 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 不可为 Null。 <BR>许多其他 SQL Server Dmv 中的信息仅包括自上次数据库引擎启动以来的活动。 使用此列查找上次 [!INCLUDE[ssSDSfull](../../includes/ssdenoversion-md.md)] 启动时间。|
|**affinity_type**|**int**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 指定当前使用中的服务器 CPU 进程关联的类型。 不可为 Null。 有关详细信息，请参阅 [ALTER SERVER CONFIGURATION &#40;transact-sql&#41;](../../t-sql/statements/alter-server-configuration-transact-sql.md)。<br /><br /> 1 = MANUAL<br /><br /> 2 = AUTO|  
|**affinity_type_desc**|**varchar(60)**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 描述 `affinity_type` 列。 不可为 Null。<br /><br /> MANUAL = 已为至少一个 CPU 设置关联。<br /><br /> AUTO = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以自由地在 CPU 之间移动线程。|  
|**process_kernel_time_ms**|**bigint**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 内核模式下所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 线程所用的总时间（毫秒）。 该值可能会大于单处理器时钟，因为它包括服务器上所有处理器的时间。 不可为 Null。|  
|**process_user_time_ms**|**bigint**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 用户模式下所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 线程所用的总时间（毫秒）。 该值可能会大于单处理器时钟，因为它包括服务器上所有处理器的时间。 不可为 Null。|  
|**time_source**|**int**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于检索时钟时间的 API。 不可为 Null。<br /><br /> 0 = QUERY_PERFORMANCE_COUNTER<br /><br /> 1 = MULTIMEDIA_TIMER|  
|**time_source_desc**|**nvarchar(60)**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 描述 `time_source` 列。 不可为 Null。<br /><br /> QUERY_PERFORMANCE_COUNTER = [QueryPerformanceCounter](/windows/win32/api/profileapi/nf-profileapi-queryperformancecounter) API 检索时钟时间。<br /><br /> MULTIMEDIA_TIMER = 用于检索时钟时间的 [多媒体计时器](/previous-versions//ms713418(v=vs.85)) API。|  
|**virtual_machine_type**|**int**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是否正在虚拟环境下运行。  不可为 Null。<br /><br /> 0 = NONE<br /><br /> 1 = HYPERVISOR<br /><br /> 2 = OTHER|  
|**virtual_machine_type_desc**|**nvarchar(60)**|适用于：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 描述 `virtual_machine_type` 列。 不可为 Null。<br /><br /> 无 = 未 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在虚拟机中运行。<br /><br /> 虚拟机监控程序 = 正在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 由运行虚拟机监控程序的 OS 托管的虚拟机内运行 (使用硬件辅助虚拟化) 的主机操作系统。<br /><br /> OTHER = 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 由不使用硬件助理（如 Microsoft VIRTUAL PC）的操作系统托管的虚拟机中运行。|  
|**softnuma_configuration**|**int**|适用于：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 指定 NUMA 节点的配置方式。 不可为 Null。<br /><br /> 0 = OFF 表示硬件默认值<br /><br /> 1 = 自动软件 NUMA<br /><br /> 2 = 通过注册表手动进行软件 NUMA|  
|**softnuma_configuration_desc**|**nvarchar(60)**|适用于：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> OFF = 软 NUMA 功能已关闭<br /><br /> ON = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 自动确定适用于软件 numa 的 numa 节点大小<br /><br /> 手动 = 手动配置的软件 NUMA|
|**process_physical_affinity**|**nvarchar (3072)** |**适用于：** 从开始 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 。<br /><br />信息有待提供。 |
|**sql_memory_model**|**int**|**适用于：** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4、 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 及更高版本。<br /><br />指定用于分配内存的内存模型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 不可为 Null。<br /><br />1 = 常规内存模型<br />2 = 锁定内存页<br /> 3 = 内存中的大型页面|
|**sql_memory_model_desc**|**nvarchar(120)**|**适用于：** [!INCLUDE[sssql11](../../includes/sssql11-md.md)] SP4、 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 及更高版本。<br /><br />指定用于分配内存的内存模型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 不可为 Null。<br /><br />  =  传统 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用的是常规内存模型来分配内存。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户在启动过程中没有锁定页面的内存特权，则这是默认的 sql 内存模型。<br />  =  LOCK_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用锁定内存页来分配内存。 这是默认的 sql 内存管理器，在 SQL Server 启动期间 SQL Server 服务帐户拥有 "锁定内存页" 特权。<br />   =  LARGE_PAGES [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用内存中的大型页分配内存。 SQL Server 使用大型页面分配器仅 SQL Server 在服务器启动期间和跟踪标志834打开时，将内存分配给 Enterprise edition。|
|pdw_node_id|**int**|**适用于：** [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
|**socket_count** |**int** | **适用于：** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和更高版本。<br /><br />指定系统上可用的处理器插槽数。 |  
|**cores_per_socket** |**int** | **适用于：** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和更高版本。<br /><br />指定系统上每个套接字可用的处理器数。 |  
|**numa_node_count** |**int** | **适用于：** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 和更高版本。<br /><br />指定系统上可用的 NUMA 节点数。 此列包括物理 NUMA 节点以及软 NUMA 节点。 |  

## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
